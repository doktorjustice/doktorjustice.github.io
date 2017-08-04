---
title: The beauty of automating little tedious things
subtitle: In which our protagonist reinvents the bycicle and becomes very proud of himself
header-id:E_8Zk_hfpcE
date: 2017-08-04
---

So, here’s an example why I love monkey with stuff that I tend to sometimes overstate as coding. Yesterday, when I woke up I didn’t know about the existence of _git hooks_ and what they are capabale of, but I felt something big was going to happen. Indeed, by the time I got bed in the evening I managed to automate the whole process of adding properly sized header images to my posts and credit them to the photographers.

This blog is a Jekyll static site hosted on GitHub Pages, which provides a fairly straightforward workflow for writing. I just scribble down the new post in Markdown (very efficient), add some details to the top like the title and subtitle (the so called ‘front-matter’), commit the changes to the local git repo, and `git push` to GitHub. All the rest is handled by GitHub, it automatically builds the site from the Jekyll assets and publish it, literally in seconds.

It’s good for me but also good for the readers, as the result is a static site, where everything is pre-rendered, no database queries, elaborate CMS engines and whatnots. Fast and easy.

There was only one hiccup in this flow, until yesterday, the header image. I love [Unsplash](https://unsplash.com) which offers wide range of amazing high-res photos for free. But I had to download, resize and save the chosen image, then add the path and author credits to the post, commit the photo and the changes, and push it to GitHub. 

Very inefficient to do all this manually, and not the most elegant way either, as Unsplash, unlike other photo providers, [encourages hotlinking](https://unsplash.com/documentation#hotlinking). 

> “Unlike most APIs, we prefer for the image URLs returned by the API to be directly used or embedded in your applications (generally referred to as hotlinking). By using our CDN and embedding the photo URLs in your application, we can better track photo views and pass those stats on to the photographer, providing them with context for how popular their photo is and how it’s being used.”

So in theory all I need is the proper url of the photo which I can include into my page’s markup. _’Something needs to be done!’_ - I said to myself in a dramatic voice. But how do I do it in an automated fashion, so each post gets its proper header with me putting the least amount of work into it?

First I looked into existing Jekyll plugins or writing one, but then it turned out that GitHub Pages runs the Jekyll build with the `--safe` tag, disabling every custom plugin. Then I found git hooks. (I was actually looking for compiling `.less` files into `.css` during build, and [found my hints here](https://www.benburwell.com/posts/less-file-compilation-for-jekyll-github-pages/), but that’s another story.)

As I learned [from the docs](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) git runs scripts from the `.git/hooks` directory at certain milestones of the proccess. Adding a proper `pre-commit` script let’s me inspect or change the contents of any file that is to be committed. The script can be anything, like shell, Ruby or my weapon of choice, Python. Great.

I’ve never automated anything with scripts in a build process before, but it had this _insider_ feeling, so I had to do it. And in fact it was faster than anticipated to write a Python script, that:

1. Checks the list of files to be committed, and picks the `.md` files from the `_posts` directory (if there’s any).
2. Extracts the `header-id` variable from the front-matter for each post.
3. Retrieves the url of the proper sized photo and the photographer’s name and username from the Unsplash API.
4. Replaces `header-id` with these new variables and overwrites the post.
5. Adds to the git staging area the modified post(s), before committing anything.

For most of you it’s not a big deal, and it’s surely not the nicest Python script ever written in the long history of the seven kingdoms of Westeros, but it works. So now I only have to chose my photo, copy its Unsplash id to the top of my post, and all the rest is automated. The photo is inserted before committing the changes and proper credits are given to the author in the post in an ‘Unsplash-compatible’ way (with a little tinkering with the templates of the page).

I have some other revolutionary ideas with the header images and other stuff on the blog, but for now I just want to enjoy the huge amount of precious time I saved myself with this hack.

So here's the code:

``` python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import subprocess
import re
import requests


def get_img_data(id):
    '''Get photo details from Unsplash API by photo id'''
    url = 'https://api.unsplash.com/photos/{}'.format(id)
    params = {
        'w': 1900,
        'client_id': 'XXXXXXX'
    }
    r = requests.get(url, params=params)

    json_result = r.json()
    img_url = json_result['urls']['custom']
    author_name = json_result['user']['name']
    author_profile_url = json_result['user']['username']

    return (img_url, author_name, author_profile_url)


def update_front_matter(path):
    '''Read in .md file, loop through the first line and replace header-id with image details'''
    lines = []
    updated = False
    # Open post for reading
    with open(path, 'r') as post:
        dash_counter = 0
        lines = post.readlines()
        for idx, line in enumerate(lines):
            tokens = line.strip().replace(' ', '').split(':')
            if tokens[0] == '---':
                # Front-matter starts and ends with '---'
                dash_counter += 1
            elif tokens[0] == 'header-id':
                id = tokens[1]
                details = get_img_data(id)

                # New lines to add to front-matter
                url_line = 'header-url: {}\n'.format(details[0])
                photo_author = 'photo-author: {}\n'.format(details[1])
                photo_profile = 'photo-author-profile: {}\n'.format(details[2])

                # Remove header-img line and add new lines
                lines.pop(idx)
                lines.insert(idx, url_line)
                lines.insert(idx, photo_author)
                lines.insert(idx, photo_profile)
                updated = True

            if dash_counter == 2:
                # Stop if second '---' is found
                break
    if updated:
        # Open post for writing
        # (There should be a simpler way to avoid opening it twice)
        with open(path, 'w') as post:
            post.writelines(lines)
    return updated


if __name__ == '__main__':
    # Get the list of all (staged, unstaged, untracked) files to be committed
    result = subprocess.run('git status --porcelain', shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    if result.returncode == 0:
        filelist = result.stdout.decode('UTF-8').strip().split('\n')
        for entry in filelist:
            file_path = entry.split(' ')[1]
            if re.match('_posts/.*\.md', file_path):
                # Only continue with *.md files
                update = update_front_matter(file_path)
                if update:
                    print('--> {} front-matter updated'.format(file_path))
                    # Add newly updated post to staging area
                    subprocess.run('git add {}'.format(file_path), shell=True)
    elif result.returncode == 1:
        print('Error when calling pre-commit hook:')
        print(result.stderr)
```
