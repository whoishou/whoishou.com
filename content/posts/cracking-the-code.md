---
title: "Cracking the Code: A Dive into File Signatures and Encoding Techniques"
date: 2018-12-26
draft: false
categories: [Cyber,Tech]
tags: [File Signature, Encoding Techniques, File Format, File Identification, Hexadeciminal, ASCII, Unicode, ZIP, Cracking, Code]
comments: true
featuredImage: "https://assets.whoishou.com/feature-cracking-the-code.jpg"
---
As an aspiring information security professional, I was recently browsing job postings to get a sense of where the industry is headed and what skills I should be focused on developing.

I came across a security analyst role that caught my attention. The job posting had an interesting challenge that read: "For extra kudos, solve the following and include the solution in your cover letter and included the following string:

> UEsDBAoACQAAAM1rV0xjL16LGAAAAAwAAAAPABw
> Ad2hhdHNpbnNpZGUudHh0VVQJAAPBfI9a1HyPWnV4
> CwABBAAAAAAEAAAAALvAF23HELDMWWvH/sMUFqMab
> C8PE+ZY/lBLBwhjL16LGAAAAAwAAABQSwECHgMKAA
> kAAADNa1dMYy9eixgAAAAMAAAADwAYAAAAAAABAAA
> ApIEAAAAAd2hhdHNpbnNpZGUudHh0VVQFAAPBfI9a
> dXgLAAEEAAAAAAQAAAAAUEsFBgAAAAABAAEAVQAAAHEAAAAAAA==

I was curious to know what this string was, and what skills the company was seeking to validate and i never miss an opportunity for a challenge so..

Challenge Accepted

If you want to give it a go yourself, pause here before reading on.

## Decoding the string

At first glance, the string looked like it could be an encoded with Base64. The string is made up of the characters: a – z, A – Z, 0 -9, + and / which are used by Base64. Additionally, there is also some padding on the end of the string represented by the == which fulfills Base64’s length requirements. This [Wikipedia article](https://en.wikipedia.org/wiki/Base64) explains Base64 in more depth.

So let's decode it!

I like to use a tool called [Cyber Chef](https://gchq.github.io/CyberChef/), which is provided by the [Government Communications Headquarters (GCHQ)](https://www.gchq.gov.uk/) an intelligence and security organisation in the UK. It's a web app that has a number of different tools covering encryption, encoding, compression, data analysis, etc.

Decoding from Base64 resulted in:

> PK..
> .	...ÍkWLc/^.............whatsinside.txtUT	..Á|.ZÔ|.Zux.............»À.mÇ.°ÌYkÇþÃ..£.l/..æXþPK..c/^.........PK....
> .	...ÍkWLc/^.....................¤.....whatsinside.txtUT...Á|.Zux.............PK..........U...q.....

seemingly random characters, but on closer inspection we can see whatsinside.txt embedded within it.

***

## File Signatures

So what's inside the text file? I tried using a function called Magic in CyberChef, that attempts to automatically detect the properties of the data, but that provided no leads.

That's when a friend suggested that it was a zip file., I hadn't thought of that, but that would make sense. I asked them why they had suggested it was a zip file, and they said they just did.

There should be a way to identify that the string is a zip file, I did some research into this and came across a Wikipedia article about [file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures) (also referred to as [magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)))

File formats when viewed as text are meaningless (most of the time), but on some occasions, the file signature has recognisable text which would allow you to discern what the file format is. The string starts with PK. PK refers to Phil Katz, the developer of the ZIP file format.
PK can also refer to other file formats built on the ZIP format such as docx, jar and apk.

So I saved the string as a ZIP file, and there it was a ZIP archive with a text file called whatsinside.txt

![password needed](https://assets.whoishou.com/password_needed.png)

***

## What's the password?

It can't be too easy right? So what's the password?
Given that this came from a job listing, I didn’t have much to go off, I went over the job listing again, thinking that a clue might have been left somewhere in the content.
I wasn't able to identify anything specifically, so I decided on a different approach.

If there are no hints to be found, then it has to be something simple or easily crack-able. I thought of compiling a list of the words inside the job description, then attempting to brute force the password using something like John the Ripper.

Instead, I opted for a ZIP file password remover I found online

I uploaded the ZIP file, and it turns out the password is…

> password

![Door kick gif](https://assets.whoishou.com/door-kick.gif)

Go figure.. I should have just guessed that to begin with.. maybe i should write a script that tries a number of common passwords as a starting point for next time.

I inputted the password and the text file opened!

![whats inside](https://assets.whoishou.com/whats_inside.png)

So what's bobbytables? its a reference to an xkcd comic called [The Exploits of a Mom.](https://xkcd.com/327/)

![exploits of a mom](https://assets.whoishou.com/exploits_of_a_mom.png)

Brilliant! My curiosity has been satiated, and the challenge helped me develop my understanding of file signature and encoding methods.

If you are interested in giving it a go, I've created a similar challenge, try it out with the string below:

>UEsDBBQAAAAIAOddMVaN2/znPgAAAF8AAAAIAAAAbmljZS50eHRNjNERgDAIxVbJCG2ljzqOd9b9RwDxx79cXsCEJuq4ofWyjzLGaFjKE2304EdlFeQ0v8Oc9q/syOtDwk27AlBLAQIUABQAAAAIAOddMVaN2/znPgAAAF8AAAAIAAAAAAAAAAAAAAAAAAAAAABuaWNlLnR4dFBLBQYAAAAAAQABADYAAABkAAAAAAA=

If you manage to crack it, need a hint, or if you know of other similar challenges, feel free to leave a comment below or reach out to me via email.
