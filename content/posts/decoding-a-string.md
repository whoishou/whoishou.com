---
title: "Decoding a string"
date: 2018-12-26
draft: false
categories: [cyber]
comments: true
featuredImage:
---
# Decoding a string

So I was recently browsing Seek which is an Australian job searching website. I was looking to see where the Industry is headed and what skills I should be focusing on developing for a career in Information Security.

I came across a security analyst role that had the following:

For extra kudos, solve the following and include the solution in your cover letter:

> UEsDBAoACQAAAM1rV0xjL16LGAAAAAwAAAAPABw
> Ad2hhdHNpbnNpZGUudHh0VVQJAAPBfI9a1HyPWnV4
> CwABBAAAAAAEAAAAALvAF23HELDMWWvH/sMUFqMab
> C8PE+ZY/lBLBwhjL16LGAAAAAwAAABQSwECHgMKAA
> kAAADNa1dMYy9eixgAAAAMAAAADwAYAAAAAAABAAA
> ApIEAAAAAd2hhdHNpbnNpZGUudHh0VVQFAAPBfI9a
> dXgLAAEEAAAAAAQAAAAAUEsFBgAAAAABAAEAVQAAAHEAAAAAAA==

I was curious to know what this was, and what skills the company was seeking to validate.

Challenge Accepted

If you want to give it a go yourself, pause here before reading on.

## To decode or ~~not~~ to decode?

Looking at the string provided (a string is just a sequence of characters), it looks like it might have been encoded with Base64. The string is made up of the characters: a – z, A – Z, 0 -9, + and / which is used by Base64.

There is also some padding on the end of the string represented by the == which fulfils Base64’s length requirements. This [Wikipedia article](https://en.wikipedia.org/wiki/Base64) explains Base64 in more depth.

So lets decode it!

I like to use a tool called [Cyber Chef](https://gchq.github.io/CyberChef/), its provided by the [Government Communications Headquarters (GCHQ)](https://www.gchq.gov.uk/) an intelligence and security organisation in the UK. Its a web app that has a number of different tools covering encryption, encoding, compression, data analysis etc.

Decoding from Base64 resulted in:

> PK..
> .	...ÍkWLc/^.............whatsinside.txtUT	..Á|.ZÔ|.Zux.............»À.mÇ.°ÌYkÇþÃ..£.l/..æXþPK..c/^.........PK....
> .	...ÍkWLc/^.....................¤.....whatsinside.txtUT...Á|.Zux.............PK..........U...q.....

whatsinside.txt, looks like it has a file embedded in it?

***

## File Signatures

I tried to determine how to access the text file or what was in it, but kept running into dead ends. I tried using a function called Magic in CyberChef, that attempts to automatically detect the properties of the data, but that provided no leads.

A friend suggested that it was a zip file.

That makes sense but how did they know that? they said they just did…
Hmm… there must be a way to identify that this is a zip file without guessing or having come across something like this before right?

Right! I did some research on how you would be able to identify different file types and came across a Wikipedia article about [file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures) (also referred to as [magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)))

File formats when viewed as text are meaningless (most of the time), but on some occasions, the file signature has recognisable text which would allow you to discern what the file format is. The string starts with PK. PK refers to Phil Katz, the developer of the ZIP file format.
PK can also refer to other file formats built on the ZIP format such as docx, jar and apk.

So I saved the string as a ZIP file, opened the archive and lo and behold there lied the whatsinside.txt text file.

![password needed](https://whoishou-media.s3.amazonaws.com/password_needed.png)

***

## What's the password?

Given that this came from a job listing I didn’t have much to go off in terms of identifying what the password was.
I went over the job listing again, thinking that a clue might have been left somewhere in the content.
Alas that search was in vain so I decided on a different approach.

If there are no hint to be found, then it has to be something simple or easily crack-able. I thought of compiling a list of the words inside the job description, then attempting to brute force the password using something like John the Ripper.

Instead i opted for a ZIP file password remover i found online

I uploaded the ZIP file and it turns out the password is… 

> ### password
<br>

![Door kick gif](https://assets.whoishou.com/door-kick.gif)

Go figure.. I should have just guessed that to begin with..  
(note to self, make a python script that'll brute force common passwords)

I opened the text file and lo behold the mystery was unveiled.

![whats inside](https://assets.whoishou.com/whats_inside.png)

So what's bobbytables? its a reference to an xkcd comic called [The Exploits of a Mom.](https://xkcd.com/327/)


![exploits of a mom](https://assets.whoishou.com/exploits_of_a_mom.png)

Brilliant! Curiosity satiated!

If you know of or come across similar challenges, let me know via twitter or send me an email
