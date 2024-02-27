+++
title = 'TIOBE Language Rankings: Separating Hype from Reality'
date = 2024-02-21T05:22:01+04:00
draft = false
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
tags = ["TIOBEIndex", "programming languages", "software development"]
categories = ["General", "News"]
keywords = ["TIOBE index analysis", "TIOBE index metrics", "TIOBE index flaws", "TIOBE index search engine queries", "TIOBE index language popularity", "Interpreting TIOBE index", "TIOBE index context ignorance", "TIOBE index language fluctuations", "TIOBE index language rankings", "TIOBE index language adoption rates", "TIOBE index market share", "TIOBE index programming language trends", "TIOBE index software development landscape", "TIOBE index Golang rise", "TIOBE index Golang surge", "TIOBE index Golang growth", "TIOBE index Python ranking", "TIOBE index language community impact", "TIOBE index language ranking analysis", "TIOBE index language selection", "TIOBE index language ranking interpretation"]
description = "Ranking wars have been with us all the time. There's a never ending paradigm of \"Best frameworks for X\" or similar questions, but one thing that has been with us from 2000 to this day is - TIOBE Index."
coverImage = "/images/tiobe.webp"
coverImageAltText = "A meme where person wants to use some programming language but TIOBE stops them"
+++

{{<img src="/images/tiobe.webp" align="center" alt="A meme where person wants to use some programming language but TIOBE stops them">}} <br>

Ranking wars have been with us all the time. There's a never ending paradigm of "Best frameworks for X" or similar questions, but one thing that has been with us from 2000 to this day is - [TIOBE](https://www.tiobe.com/tiobe-index/) Index. With the recent news, which is Golang moving up the ladder in the same index, new hype was launched. There's nothing particularly wrong with TIOBE existing, it can cause some damage to beginners interpreting such lists. I could say it even for myself, I remember when I first started, I used to search a lot of the similar material and being influenced by them, I'm pretty sure many of readers could recognize themselves in this behavior. On the other hand, I'm pretty sure that simply saying _This is bad_ is not enough, which is why I decided to put my thoughts in an article, so let's dive into what's wrong with all of these hypetrain lists.

## Metrics used for TIOBE

Primary metric used for constructing TIOBE ranking are search engine queries and I don't think it's correctly representing language ranking. While it certainly can display language's popularity, it's not reflecting language's usage statistics. Languages with stronger online communities may rank higher regardless of their actual adoption or usefulness. Not that I'm against Golang or TIOBE, but the weight given to TIOBE is just too much. You could find a dozen of articles where X language is praised just because of having high ranking in TIOBE index. Right now, if you look at it, Python is ranked #1, but that doesn't mean that you should use it for any kind of work you have and there's the next problem I wanted to talk about.

## Ignorance of context

As I've already mentioned, main metric used for TIOBE ranking are search engine queries. With this said, it's safe to consider that it fails to account for the nuanced contexts in which these languages are used. One-size-fits-all approach can be misleading, cause it misses out on crucial factors such as, performance, project requirements and developer preferences. We've already mentioned Python, so let's continue with it, it won't be the swiss knife of programming languages just because TIOBE ranks it as #1 and it shouldn't be, it's fine. The problem is not the ranking itself, it's how we interpret it. Just make sure to double-check your decisions which may be highly influenced by some ranking, as misguided decisions could have a dramatic effect on your project's success.

> misguided decisions could have a dramatic effect on your project's success.

## Fluctuations

Now as we already explained how the ranking works, it's also crucial to say that it fluctuates too much. It might not be visible so much for languages that have been in this world for decades, but it still does. At this moment we're not even talking about market adoption, language maturity or job requirements, we're talking communities. Say for example, X language's community decides to go quiet for a bit of time and another language's community is like, we're alive, we're going to be posting every day about how awesome our language is. Does it really change anything on the market? If Brainf\*\*\*'s community was as alive as Python's, would it change anything? No, cause the language is not applicable to real world needs, but the TIOBE rankings would be jumping like crazy.

> If Brainf\*\*\*'s community was as alive as Python's, would it change anything? No, cause the language is not applicable to real world needs.

## Conclusion

This is not the type of articles I was used to be doing, but I've been seeing it so much since Golang went to #8 on the ranking, I felt like I should've took this hot take. Only value this article could provide, would probably be for beginners in this industry to not trust any ranking blindfoldedly. It's crucial to always do your own research, think about your project's needs and only then, decide which language or framework to use.
