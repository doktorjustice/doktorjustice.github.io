---
title: What we really pay for bitcoin
subtitle: Even if you don’t own it, you still bear the costs
photo-author-profile: littlewitch
photo-author: Witch Kiki
header-url: https://images.unsplash.com/photo-1414638032997-ce7a3a24fd8d?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1900&fit=crop&s=5e61934bd6fbf4ac6d67e5a9d1a0f36f
date: 2017-08-30
category: blockchain
---

This is how the _oh-so-liberating_ modern crypto revolution really looks like: economic slavery in [a broke ghost city](http://www.china.org.cn/business/2013-08/10/content_29680298.htm) in Inner Mongolia run by a faceless corporation with seemingly unlimited money, thriving on subsidized fossil energy and zero public oversight.

<iframe width="640" height="360" src="https://www.youtube.com/embed/yGeim9E24Wk?rel=0" frameborder="0" allowfullscreen></iframe>

Besides the grim distopian landscape one important aspect grabbed my attention [in the article](https://qz.com/1054805/what-its-like-working-at-a-sprawling-bitcoin-mine-in-inner-mongolia/): the extreme energy consumption of the blockchain. The issue is well documented already, but it gets heavier with every new peak, and it is still not known widely enough.

*(<small>I found the story via [Index](http://index.hu/tech/2017/08/29/a_fiuk_a_bitcoinbanyaban_dolgoznak/). Very thorough translation, they even managed to include the mix-up of electricity-related measurements from the original.)</small>*

## The most inefficient money you can get

Here is the fact: blockchain consumes disproportionately large amounts of energy for validating and processing transactions (also known as mining). This is true for not only bitcoin, but all the most popular blockchains out there, and sadly it is not just a _’bug’_, but a _‘feature’_ rooted deeply in the design.

According to Quartz, this particular Chinese mining facility uses 40MW of power capacity 24/7/365, while it controls about 4 percent of the bitcoin mining market. That means that the whole bitcoin blockchain should require about 1000MW, half of the current nominal capacity of our Paks nuclear plant, which covers roughly 40 percent of Hungarian electricity need.

This translates to an annual electricity consumption of the whole blockchain of about 8,8 TWh. Or maybe even double of that. This [Bitcoin Energy Index data](https://digiconomist.net/bitcoin-energy-consumption) claims that the overall annual energy need of the bitcoin blockchain is around 16 TWh, which roughly equals the annual consumption of countries like Lebanon, Tunisia or Cuba (and that’s bitcoin in itself, not counting ether and the others).

![](/img/posts/lebanon.png)

<small>*This is the per capita consumption, multiply it by the popoulation of 5.6 million people and divide by 1000 million, to get TWh. Source: [Word Bank via Google](https://www.google.com/search?q=lebanon+electricity+consumption&oq=lebanon+electricity+con&gs_l=psy-ab.3.0.0j0i22i30k1.3378.5535.0.6672.7.7.0.0.0.0.225.686.4j1j1.6.0....0...1.1.64.psy-ab..1.6.684.VkFtJbeFCkQ)*</small>

I don’t know which number is more precise, the disparity may come from a number of estimation errors, and due to its decentralized nature there is no reliable central source of information on the consumption of the blockchain. Nevertheless Digiconomist gives a rather [credible description of their methodology](https://digiconomist.net/bitcoin-energy-consumption#assumptions), so I tend to believe their numbers, at least on the order of magnitude level. 

_(**Update:** In the meantime I found another detailed analysis, and the author [vigorously dissects](http://blog.zorinaq.com/serious-faults-in-beci/) Digiconomist’s calculations, claiming that the actual number is lower by 40-50%, putting the consumption closer to my initial 8-9 TWh.)_

This gets really shocking when comparing the average energy need of a single transaction on the blockchain with the same on the transactional network of Visa. Even if the estmations are rough at the edges, the difference is striking: bitcoin transactions consume 30 times more energy than their peers on the Visa network.

![](/img/posts/btc_visa.png)

Or as [Motherboard puts it](https://motherboard.vice.com/en_us/article/ypkp3y/bitcoin-is-still-unsustainable): a single transaction on the blockchain takes almost a day worth of electricity consumption of an American household (which is already several times more than we use in Eastern Europe). Or about 150 rounds of washing with a washing machine. For a single transaction.

<div>
<div class='tableauPlaceholder' id='viz1504099900756' style='position: relative'><noscript><a href='https:&#47;&#47;motherboard.vice.com&#47;en_us&#47;article&#47;ypkp3y&#47;bitcoin-is-still-unsustainable'><img alt='Embodied Energy: bitcoin transaction vs. household appliances, per-use ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;BT&#47;BTCvscommonhouseholdappliances&#47;Sheet1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='site_root' value='' /><param name='name' value='BTCvscommonhouseholdappliances&#47;Sheet1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;BT&#47;BTCvscommonhouseholdappliances&#47;Sheet1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1504099900756');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</div>

<small>*Source: [Motherboard](https://motherboard.vice.com/en_us/article/ypkp3y/bitcoin-is-still-unsustainable)*</small>

By the way, bitcoin is [currently valued at 75 billion dollars](https://www.cnbc.com/2017/08/15/bitcoin-price-market-cap.html) in total, in the ball-park of companies like Netflix, Adobe, PayPal and American Express (though it does not really make sense to compare them this way). Definitely not small for a company, but still tiny little for a _’global money’_, and especially small for the energy wasted on it.

![](/img/posts/market_cap.png)

*<small>Source: [Finviz](http://finviz.com/screener.ashx?v=152&f=geo_usa&o=-marketcap&r=61)</small>*

## Why is it so inefficient?

This waste is related to the inherent logic of the so-called _Proof of Work_ mechanism. Simply put this mechanism requires miners to work simultaneously on the same problem, racing for the reward of finding the solution of a complex mathematical challenge, that is the key of validating latest transactions and adding a new block to the chain. 

But only one can be a winner for a single block, and every drop of energy spent by the others is wasted. So the energy consumption is the cost of the redundancy, which is the key of the decentralization in this concept. The problem is that the bearer of the costs is not the same who earns the revenues.

*(Sidenote: this is somewhat ironic, because the ascent of modern societies was based on the **reduction** of redundancy and focusing on specialization (eg. administration, trading, banking, medicine, sciences, etc.), enabled by machines capable of harnessing fossil energy. Now crypto craze pushes us to spend million times more energy **for** redundancy in the hope of undoing some of the ’errors’ of this process, like the centralized monetary system.)* 

## New algorithms to the rescue

Luckily, not all is lost, at least not from an academic point of view. There are several different theoretical solutions for the blockchain energy efficiency issue in the form of different block validation algorithms. For example, the proposed _’Proof of Stake’_ mechanism on the Ethereum network would be based on the existing _’owners’_ of the tokens and not the energy-intensive work of miners. (Here is a rather lengthy and [somewhat heavy description](https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ).)

The question is that how is it possible to transition from one fundamental principle to another without breaking the network where dollar billions are on the stake, and miners transport computer hardware on Jumbo Jets (additional carbon footprint), causing global shortage and price spike on these markets (additional external costs).

But even if bitcoin and other current blockchains ultimately hit some kind of scalability wall, blockchains should stay with us for a long time hopefuly in an evolved and more efficient form. Until then let’s not pretend we are already there.

## Sidenote: Bitcoin vs. bitcoin

> There is no uniform convention for bitcoin capitalization. Some sources use Bitcoin, capitalized, to refer to the technology and network and bitcoin, lowercase, to refer to the unit of account. The Wall Street Journal, The Chronicle of Higher Education, and the Oxford English Dictionary advocate use of lowercase bitcoin in all cases, a convention which this article follows. ([Wikipedia](https://en.wikipedia.org/wiki/Bitcoin))

For the sake of simplicity I am with WSJ on this matter.