---
layout: post
title: "Why Octopress?"
date: 2012-05-27 19:04
comments: true
categories: blogging wordpress octopress
---
For years I was running Openswitch on <a href="http://wordpress.org">WordPress</a>, and even released a theme for it which found such popularity that it was <a href="http://theme.wordpress.com/themes/unsleepable/">adopted as a stock theme</a> on WordPress.com. Then, I switched over to <a href="http://textpattern.com">Textpattern</a>; its markup was more simple, and was easier to customize to my liking.  Unfortunately, both of those content management systems left me wanting more... and less. This led me to <a href="https://github.com/mojombo/jekyll">Jekyll</a> and ultimately to <a href="http://octopress.org">Octopress</a>.  Let me explain.
<!-- more -->

## Octopress gives me more
<strong>Control</strong>.  We all want more of it when it comes to our websites and Octopress delivers.  Leveraging the flexability that comes with Github and Jekyll, it takes content management and delivery out of the hands of a database and puts it in your direct control. It may not be as easy to set up and deploy as WordPress, but the loss of ease is more than made up for in the sheer power it holds.

<strong>Geek cred</strong>.  Much like street cred, the geek version of this clout carries weight with only a small group of people.  I am one such person.

<strong>Security</strong>.  There's a great threat in modern content management systems: hacking.  Losing control of your website is a constant threat with all major CMS's today.  Jekyll and Octopress sidestep this issue completely by _not_ being a CMS. Jekyll lives on my personal computer, and furthermore the end-user has no means by which to interract with my web server at all.

In fact, every time I deploy Openswitch to my web server it's done via SSH through rsync and authentication is done with RSA keys.  An attacker would either have to hack the physical web server or my physical compupter to gain access to Openswitch.

<strong>Freedom</strong>.  No longer do I have to worry about my CMS getting hacked.  Also, I don't have to think about how well my website is load-optimized or how much strain it's putting on the server; which brings me to my next point.

## Octopress gives me less
<strong>Overhead</strong>.  Leveraging Jekyll means my website consists of flat .html files.  All the building is done locally on my Macbook and then deployed to my web server.  This means that when people visit Openswitch the server has an extremely easy job, it just serves a flat file.  Pages load lightning fast because there is no interraction with a database, there are no queries.

<strong>Spending</strong>.  I'm hosting Openswitch with <a href="http://nearlyfreespeech.net">Nearly Free Speech</a>.  With NFS I'm billed for the resources I use.  Since there is no database there is very little load on the server.  I'm literally able to host Openswitch for pennies a day.


