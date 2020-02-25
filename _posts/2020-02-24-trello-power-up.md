---
title: How I solved my own problem with code
subtitle: And created my first ever public "product"
date: 2020-02-24
category: coding
header-url: https://images.unsplash.com/photo-1511306162219-1c5a469ab86c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1650&q=80
photo-author: Nicolas Hoizey
photo-author-profile: nhoizey
---

_It's been a while since I wrote anything here. Two years. I guess it's not a coincidence that my son is turning two in a few weeks..._

In 2020 I completely surprised myself by actually finishing one of my numerous pet projects: I wrote [a Trello Power-Up](https://trello.com/power-ups/5e14aa34ce4580518945bf9b/text-to-cards) called Text to Cards. This is my first publicly available "software product" ever. And the greatest thing: I solved a problem for myself. Exciting times.

(_"What's a Power-Up?" - you might ask. Simply put these are sort of "plugins" on Trello, that enhance the platform by adding some new functionality or introducing integrations with other services. Read more [here](https://help.trello.com/article/1094-what-are-power-ups)._

_"What's Trello?" - you might also ask. In that case please checkout [Trello.com](https://trello.com)_)

## What is Text to Cards?

> [Text to Cards](https://trello.com/power-ups/5e14aa34ce4580518945bf9b/text-to-cards) is a simple Power-Up for Trello that helps transforming your longform texts into neat little Trello cards. It will extract card names and descriptions from your text and add the proper labels, members and due date to the card. <br><br> You can create dozens of cards with just one click.  ([Text to Cards docs](https://somiandras.gitbook.io/text-to-cards/))

It's not the most revolutionary software ever written in the history of mankind, but probably a neat tool for someone having the same issue as me: trying to create multiple cards on Trello from a single, long(ish) text without a hundred rounds of copy-pasting and clicking around.

The idea came back in the days when I was managing my team at KBC - I even started to code up a prototype 4 years ago -, where during our regular weekly discussions we spent disproportionately long time with trying to enter the right thing to the right part of the right cards. This can completely derail an otherwise productive discussion.

Nowadays I tend to spend quite some time on calls with people I work with, and I usually just type up a few notes in my text editor to stay in the flow. You guessed it: most of the time these notes end up on my Trello boards as cards.

## How does it work?

Instead of adding these cards one by one now I can simply copy my notes in the Power-Up, spice it up with a few "hints" to mark the cards (less than 30 seconds of extra work), and _boom_, my cards are ready.

And there is even a live preview of the soon-to-be created cards:

![screenshot](https://text-to-cards.netlify.com/preview.png)

Text to Cards uses the following hints to extract cards:

- `::card title` to find cards and separate their title from description
- `@username` or `@initials` for adding members
- `#label` for adding labels
- `$due: YYYY-MM-DD` for adding a due date

So this - completely made up - meeting memo:

```
::Create docs for the power-up

@andrassomi will set up a Gitbook site with a few simple pages
to document the power-up. $due: 2020-02-10

#Docs #Product


::Add board members and available labels to main window

@andrassomi - Board members and labels should appear somewhere on the #UI to make text editing easier.

```

will become these cards:

![screenshot of cards](https://blobs.gitbook.com/assets%2F-M--b8JLQtIxcG9fkDVz%2F-M-6YoNcXC09W8cXUXcp%2F-M-1V0TK1lWz_kgJnuiu%2Fscreenshot1.png?generation=1580674797272740&alt=media)

I even created a small documentation page, [check it out for the detailed functionality](https://somiandras.gitbook.io/text-to-cards/).

## How did I create the Power-Up?

While I use Python all the time in my work and I even tend to consider myself a more experienced Pythonista, this Power-Up project is fully client-side Javascript (Power-Ups are just websites that Trello loads in an iframe).

So I had to make myself somewhat familiar (again) with the extremely confusing modern JS ecosystem and some other interesting tools:

- **webpack**: much nicer than my last memories from a few years earlier, but I still wasn't always sure I knew what I was doing (but hey, it just worked!).
- **Vue.js**: still comfortably easy, but for me (the outsider) the ecosystem is starting to feel almost as intimidating as other giant frameworks which I deliberately avoid now.
- **jest**: I think this is my first project where tests became really relevant and increased my productivity (not sure it has anything to do with the test runner, or I just happened to test the right things).
- **Netlify**: The greatest and most pleasant surprise of all. This service - even in the free tier - is amazing, it just builds and deploys my project straight from the Github repo, with (almost) zero configuration.
- **ngrok**: Another magical tool, which exposes my local development server to a proper external url. It was important as I did not have any other means to try out Trello functionality, but to actually look at the Power-Up on Trello. Without ngrok it means committing and deploying the code you want to try out (not nice).
- (And let's not forget **VS Code**, my go-to code editor which served me extremely well during this experiment as well.)

I'm not telling you there wasn't any frustration along the way, but I still somehow managed to create this little piece software (and plan to further work on it, especially if someone actually starts using it).

Check out the [Power-Up on Trello](https://trello.com/power-ups/5e14aa34ce4580518945bf9b/text-to-cards).

## More to come?

I enjoyed this project so much. Maybe 2020 will be the year of actually putting some other stuff out there. We'll see.

--------

_**Disclaimer:** Just to make sure it's clear for everyone: I am not affiliated with Trello in any form._