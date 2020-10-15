---
title: Build a Hugo Site and Deploy to Firebase
date: 2020-10-14T19:00:00.000+00:00
hero: "/images/hero-hugo-github-firebase.jpg"
excerpt: Step by step, build a hugo site.
authors:
- Will Elder

---
## Why Hugo?

On of the many benefits of working in tech is that you’re surrounded by smart people searching for alternatives — I got inspired by a company project that required a landing page to be easily editable, quickly hosted and lightweight. That's when I found [Hugo](https://gohugo.io/) — a general purpose website framework written in Go. As a [static site generator](https://gohugo.io/about/benefits/), Hugo is extremely fast to deploy.

## Let's Get To It

Hugo has a well-documented guide [here](https://gohugo.io/getting-started/quick-start/) to set things up, with the assumption that you are comfortable with [Git](https://git-scm.com/) and the command line. The example I'll use is on MacOS and the theme [Hugo ](https://themes.gohugo.io/minimo/)[Minimo](https://themes.gohugo.io/minimo/ "https://themes.gohugo.io/minimo/").

### 1. Install Hugo

Install Hugo on your machine using [Homebrew](https://brew.sh/)

```html
brew install hugo
```

### 2. Create a New Site

Navigate to the folder where you want your new site to live. For example, I would like to have mine in the `repos` folder

```html
cd ~/repos/
```

Use the command line below to create a new site.

```html
hugo new site your-new-site
```

Select a theme from [themes.gohugo.io](https://themes.gohugo.io/), I used [Hugo ](https://themes.gohugo.io/minimo/)[Minimo](https://themes.gohugo.io/minimo/ "https://themes.gohugo.io/minimo/"). You could also download it directly [`https://github.com/MunifTanjim/minimo/archive/master.zip`](https://github.com/MunifTanjim/minimo/archive/master.zip "https://github.com/MunifTanjim/minimo/archive/master.zip") and move it to `themes/`

```html
cd your-new-site
git init
git submodule add https://github.com/MunifTanjim/minimo.git themes/minimo
```