---
ID: 109
post_title: >
  Recursively download
  whole-ftp-directories
author: Kyshel
post_date: 2016-10-05 10:05:43
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/recursively-download-whole-ftp-directories/
published: true
---
How to recursively download all files in a directory, with keeping the ftp's directory structure?
First, I want to use python spider to do this. Python script parse links and download links.
But it will be some complicated, you need to judge which link is available.

I googled a lot of pages, and never found a simple way to do this.
Until I met one page, it said  that wget can do this, and only one command:

wget -m ftp://username:password@ip.of.old.host

-m means mirror.
And below is manual:
Turn on options suitable for mirroring.  This option turns on recursion and time-stamping, sets infinite recursion depth and keeps FTP directory listings.  It is currently equivalent to -r -N  -l inf --no-remove-listing.

It's really powerful and graceful.