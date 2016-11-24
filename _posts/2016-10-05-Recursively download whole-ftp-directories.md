---
layout: post_ks
---

# Recursively download whole-ftp-directories

- How to recursively download all files in a directory, with keeping the ftp's directory structure?

First, I want to use python spider to do this. Python script parse links and download links.  
But it will be some complicated, you need to judge which link is available.

I googled a lot of pages, and never found a simple way to do this.

Until I met one page, it said  that wget can do this, and only one command:

    wget -m ftp://username:password@ip.of.old.host

> -m means mirror.

It's really powerful and graceful.
