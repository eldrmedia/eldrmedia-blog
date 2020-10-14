+++
authors = []
date = 2020-10-14T19:00:00Z
excerpt = ""
hero = ""
title = "Build a Hugo Site and Deploy to Firebase"

+++
## Why Hugo?

On of the many benefits of working in tech is that you’re surrounded by smart people searching for alternatives — I got inspired by a company project that required a landing page to be easily editable, quickly hosted and lightweight. That's when I found [Hugo](https://gohugo.io/) — a general purpose website framework written in Go. As a [static site generator](https://gohugo.io/about/benefits/), Hugo is extremely fast to deploy.

## Let's Get To It

### 1. Install Hugo

Install Hugo on your machine using [Homebrew](https://brew.sh/)

    brew install hugo

### 2. Create a New Site

Navigation to your folder where you want your new site to live. For example, I would like to have mine under repos folder

    cd ~/repos/

Use the command line below to create a new site.

    hugo new site your-new-site