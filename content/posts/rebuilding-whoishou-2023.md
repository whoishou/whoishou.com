---
title: "WhoisHou 2.0: Rebuilding my website for 2023"
date: 2023-02-27
draft: false
categories: [Tech]
tags: [Web Development, Hugo, Static Website, AWS, Netlify, Markdown, Git, Github, SSG]
comments: true
featuredImage: "https://assets.whoishou.com/feature-webdev.png"
---

I decided going into 2023, that it was time I rebuilt whoishou.com into something closer to what I had originally envisioned when I first created my website in 2018.

I wanted a website that:

- Requires minimal ongoing maintenance
- Is secure
- Is easy to publish new content for
- Is responsive and mobile friendly

What I'd built didn't require much maintenance, was relatively secure (more on this shortly) but the accessibility and responsiveness wasn't where I wanted them to be.

I reviewed my website and identified the following that needed improvement:

- My website was vulnerable to multiple CVE's
- Mobile responsiveness was poor and needed to be improved
- My Hugo theme hadn't been updated since 2017
- The Hugo version I was running needed to be updated

## Vulnerabilities

I scanned my website with PageSpeed Insights which identified multiple CVE's on the version of Bootstrap and jQuery my website used.

![CVEs](https://assets.whoishou.com/tina-cms-vuln.jpg)

PageSpeed Insights is a tool provided by Google, it provides information on your websites performance and suggests optimization methods.

Both the jQuery and Bootstrap CVE's were a result of the theme I was using. Once I switched to something more up to date, I expected these to be patched. If you're wondering what a CVE is, feel free to have a read of my other [post](https://whoishou.com/cve-101) on that topic.

## Theme

Front end design has never been one of my strengths (or generally any form of artistic design), so I decided I'd have to use an existing template designed for Hugo.
I wanted to find something that was widely used and frequently updated. This would help mitigate similar issues I had with my old theme.

I came across a [python script](https://github.com/TrentSPalmer/hugo_themes_report) on GitHub written by Trent Palmer.

The script generates a report that helps you rank Hugo themes. It provides information such as how many stars the theme has and when the last commit/update was.

I used this to narrow down my options and eventually landed on the [LoveIt theme](https://github.com/dillonzq/LoveIt).

LoveIt is highly rated, has received recent updated, appears to be actively maintained and has additional features I'd like to incorporate. There are also many forked themes based on LoveIt, so if I need to migrate in the future, it should be a simpler process.

## Rebuilding

Around November, I started rebuilding the website from scratch. I created a dev branch on my GitHub repository which hosts the code for my website and used the new branch to rebuild.

My website is hosted on Netlify. Netlify automatically keeps track of new commits on my GitHub repository and rebuilds the website to deploy my new content.

By using a different branch, I could keep my previous website live, whilst I worked on the new one.

Previously I also used Forestry CMS to manage my content, it provides a graphical user interface to help write and manage content, but it just so happens that Forestry is being discontinued in April 2023 and its replacement TinaCMS doesn't officially support Hugo yet.

Whilst I considered looking at alternatives, my proficiency with Git and Markdown files has improved over the past few years, and it would be one more thing I'd have to monitor and update, so I'll forgo a CMS for my content.

Funnily enough, I received an email recently about a security vulnerability in TinaCMS.
![tina cms vulnerability](https://assets.whoishou.com/tina-cms-vuln.png)

## Enhancements

### Content Delivery Network

Most Hugo websites store their assets and media files statically. I didn't want to do this as I wanted my assets to be independent of my website itself. It will also be easier in the future if I need to migrate away from Hugo to a different platform.

So what I've done is upload all of my assets to an AWS S3 bucket and allowed public access to it. By default, S3 buckets do not allow for public access.

{{< admonition danger "Warning" true >}}
If you decide to allow public access to your S3 bucket, there are implications. Misconfigured and public S3 buckets have often been the cause of data breaches.
{{< /admonition >}}

Next I set up AWS CloudFront to provide a Content Deliver Network (CDN) for my assets.  
This achieves a few things:

- It speeds up load times of my images for visitors located in different parts of the world.
- It allows me to utilize the AWS free tier to keep costs associated with my website low.

I tested the configuration and verified it was functioning the way I wanted, then I ran some tests to see how much faster my content was loading via CDN.

On average with minimal testing, I found it to be about 25-30% faster.

Excellent  
![Excellent](https://assets.whoishou.com/excellent.webp)

### Custom CloudFront Domain

To make it easier when I am writing content, I wanted to use a custom domain name with CloudFront.

I configured assets.whoishou.com to point to the CDN, this was done by creating a new DNS entry with my domain registrar.

Now when I want to reference an image I can simply use assets.whoishou.com/name-of-the-file.format instead of having to remember what my distribution name is on CloudFront.

This covered most of the core functionality of the website.

### Giscus Comments

One feature I had wanted in the past was to allow visitors to leave comments with feedback or questions.

You have different options when it comes to comment functionality. I wanted something that wasn't invasive, didn't collect a lot of data but at the same time had a small barrier to entry to prevent spam.

I chose to use Giscus. It links directly with my repository on GitHub which has my websites code. This allows it to be centralized and more easily managed. Given that it uses GitHub Discussions, I don't need to host it, maintain it or update it which reduces the amount of maintenance I need to do.

## Workflow

If you're still with me, things might have started to sound a little complicated and confusing.

Could my website be simplified? Absolutely, so why I've made it so complicated?

Well it's primarily because I like to learn. By implementing all these features I've leveraged my website as a means to further develop my knowledge of different technologies.

I made a diagram which shows what happens when you visit whoishou.com or when I write new content.

![site workflow](https://assets.whoishou.com/site-workflow.jpg)

## Future Plans

My website is at a point where I'm pretty happy with it, but there is always room for more.

### Web Analytics

Currently I don't track any website metrics, I don't know what engagement is like when I publish new posts, where my viewers are or which channels are generating the most web traffic for me. Whilst I could spin up Google Analytics faster than it took to write this post, Google collects a lot of information, like A LOT, excessively if you ask me and I like to respect people privacy, so Google Analytics no Bueno.

An option I am exploring is using Plausible Analytics. It's a lightweight and open source alternative.

Plausible is a much more privacy focused service and I can implement it in a way that gathers only the basic data I'm looking to track. I'll most likely run this as a Docker container using Amazon Elastic Container Service (ECS)

### Image Transformation

When I upload my assets to S3 they are generally in different formats; JPEG, PNG, GIF.

I want to standardize this and utilize a newer file format for my assets.

An option I'm currently exploring is utilizing Amazons Lambda service to run a Function written with Python, that looks for new objects or images being uploaded to my S3 bucket and converts their file format. Most likely I'll use either WebP or AVIF, both are newer formats and more efficient which will further improve the speed at which my websites assets load.

---

I'll write posts on the implementation of Plausible and my Image Transformation process, so keep an eye out for those.

Otherwise! That's a wrap, thank you for reading, if you have any comments or questions, feel free to reach out, I'd be happy to chat.
