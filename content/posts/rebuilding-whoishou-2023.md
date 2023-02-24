---
title: "WhoisHou 2.0: Rebuilding my website for 2023"
date: 2023-02-25
draft: true
categories: [Tech]
tags: [Web Development, Hugo, Static Website, AWS, Netlify, Markdown, Git, Github, SSG]
comments: true
featuredImage: "https://assets.whoishou.com/feature-webdev.png"
---

I decided going into 2023, that it was time I rebuilt whoishou.com into something closer to what I had originally envisioned when I first created my website in 2018.

I wanted a website that:

- Requires minimal ongoing maintenance
- Is secure
- Has easy accessibility for publishing new content
- Is responsive and mobile friendly

What I had built didn't require much maintenance, was relatively secure (more on this shortly) but the accessibility and responsiveness weren't where I wanted them to be.

Having reviewed my website I identified the following points that needed improvement:

- My website was vulnerable to multiple CVE's
- Mobile responsiveness was poor and needed to be improved, this ties into the theme
- My Hugo theme hadn't been updated since 2017.
- The Hugo version I was running needed to be updated

## Vulnerabilities

I scanned my website with PageSpeed Insights which identified multiple CVE's on the version of Bootstrap and jQuery my website used.

![CVEs](https://assets.whoishou.com/tina-cms-vuln.jpg)

PageSpeed Insights is a tool provided by Google, it provides information on your websites performance and suggests optimization methods.

Both the jQuery and Bootstrap CVE's were a result of the theme I was using. Once I switched to something more up to date, I expected these to be patched. If you're wondering what a CVE is, feel free to have a read of my other post on that topic.

## Theme

Front end design has never been one of my strengths (or any sort of artistic design in general), so I looked at what templates were available for Hugo. I wanted to find something that was widely used and frequently updated. This would ensure I don't run into similar issues with further vulnerabilities in the future.

I came across a [python script](https://github.com/TrentSPalmer/hugo_themes_report) on Github written by Trent Palmer.

The script generates a report that helps you rank Hugo themes. It provides information such as how many stars the theme has and when the last commit/update was.

I used this to narrow down my options and eventually landed on the [LoveIt theme](https://github.com/dillonzq/LoveIt).

LoveIt is highly rated, has received recent updated and appears to be actively maintained. It also has a number of forks I could alternate to in case LoveIt itself stopped being maintained as well as support for additional features I wanted to incorporate with my website.

## Rebuilding

Around November, I started rebuilding the website from scratch. I created a dev branch on my Github repository hosting my website and used this new branch to rebuild the website.

This allowed me to start working on the new website without making any changes to the old one that was live.

Previously I was using Forestry CMS to manage my content, it provides a graphical user interface to help write and manage content, but it just so happens that Forestry is being discontinued in April 2023 and its replacement TinaCMS doesn't officially support Hugo yet.

My proficiency with Git and Markdown files has improved over the past few years, so I made the decision to forgo a CMS to manage my content.

Funnily enough, I received an email recently about a security vulnerability in TinaCMS.
![tina cms vulnerability](https://assets.whoishou.com/tina-cms-vuln.jpg)

I'm glad I've decided against a CMS, its one less factor ill need to monitor and update in order to maintain my sites' security.


## Enhancements

Moved assets to AWS
Setup CloudFront to provide CDN for assets
Utilized assets.whoishou.com to make it simpler to reference assets and media files.
This was done by using a DNS entry to point assets.whoishou.com to my Cloudfront distribution.

Added Giscus comments, this is something I had wanted in the past but has some reservations with using Disqus and other services that provide commment functionality.
 
I chose Giscus as its linked directly with the repo that houses my websites code, its all in one place and it didn't have the same privacy concerns that other comment services have

## Workflow

If you're still with me, things might have started to sound a little complicated and confusing.
Could my website be simplified? Absolutely,
Why so complex? So i can learn

I wanted to utilize different cloud technologies and leveraged my website in a way which would allow me to learn these things.

I made a diagram which shows what happens when you visit whoishou.com or when I write new content.

![site workflow](https://assets.whoishou.com/site-workflow.jpg)

## Future Plans

- Deploying Plausible for web analytics
I want to use Plausible instead of something like Google Analytics, even though the latter is much simpler and doesn't require hosting on my end, I prefer not to have excessive data from both my site and its visitors sent to Google. Plausible is a much more privacy focused service and I can implement it in a way that gathers only the basic data I'm looking to track without violating the privacy of my visitors.

- Deploying Lambda function to convert image file format, determine Webp or AVIF
In order to further automate and utilize newer file formats for my assets, I'm writing a Python function that will look for new objects being uploaded to S3 and converting their format. I haven't decided on whether I want to use WebP or AVIF yet but once I do Moving assets to AWS
I'll write up another post about how i built and implemented it.

Well that's a wrap, if you have any comments or questions, feel free to reach out, I'd be happy to answer your questions.
