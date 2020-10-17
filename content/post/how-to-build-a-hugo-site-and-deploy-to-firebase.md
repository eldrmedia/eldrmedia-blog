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

### 3. Select a Theme

Select a theme from [themes.gohugo.io](https://themes.gohugo.io/), I used [Hugo ](https://themes.gohugo.io/minimo/)[Minimo](https://themes.gohugo.io/minimo/ "https://themes.gohugo.io/minimo/"). You could also download it directly [`https://github.com/MunifTanjim/minimo/archive/master.zip`](https://github.com/MunifTanjim/minimo/archive/master.zip "https://github.com/MunifTanjim/minimo/archive/master.zip") and move it to `themes/`

```html
cd your-new-site
git init
git submodule add https://github.com/MunifTanjim/minimo.git themes/minimo
```

Then, add the theme to the site configuration:

```html
echo 'theme = "minimo"' >> config.toml
```

### 4. Site Structure

Alright, now that you’re all set, it’s time to create a new post and understand how Hugo structures the website.

* Folders and files under `content/` will be rendered as web pages organised as follows:

```html
content/
     post/
          post-01.md
          post-02.md
     _index.md
     about.md
```

* Images are stored in `static/images` and can be added to your posts using [Markdown](https://guides.github.com/features/mastering-markdown/)

* The theme is normally organized as default layouts containing “partials” that enrich the content and layout options. Learn more about [Hugo particals](https://gohugo.io/templates/partials/#readout).

```html
layouts/
     _default/
          baseof.html
          list.html
          single.html
     partials/
     article/
     author/
     head/
     page/
```

* To generate a new post, you can either create a new `.md` file in `post/` or use `hugo new post/my-first-post.md`

* The newly generated post normally contains the following, and you can customise this by tweaking `archetypes/default.md`

```html
---
title: "My First Post"
date: 2020-10-17T22:02:43+11:00
hero: /images/hero-my-first-post.jpg
excerpt:
tags:
draft: true
---
This is my first post!
```


### 5. Start the Hugo server
With Hugo, changes such as adding new posts, CSS styling and etc. can be instantly viewed locally at `http://localhost:1313/` with the following:

```html
-- with draft enabled
hugo server -D
-- without draft
hugo server
```

### 6. Customization
Before making the site public, you probably want to make it more personal. config.toml is the good starting point, where you can change the URL, site title, pagination, add additional pages and more.

```html
baseURL = "http://www.example.com"
title = "Your New Site "
theme = "minimo"
Paginate = 5

[menu]
     [[menu.main]]
          identifier = "about"
          Name = "About"
          url = "/about/"
          weight = 0
```

### 7. Firebase
You can host your Hugo site virtually anywhere. I chose to use Firebase. You can find a list of more hosting options at the end. Before we begin, you'll need a Firebase account. If you don't, you can sign up for free with your Google account.

* Go to the [Firebase console](https://console.firebase.google.com/) and create a new project (unless you already have a project). You will need to globally install `firebase-tools` (node.js):

```html
npm install -g firebase-tools
```

* Log in to Firebase (setup on your local machine) using firebase login, which opens a browser where you can select your account.

```html
firebase login:ci
```

* Visit the URL provided, then log in using a Google account. You'll need to save this token to a private variable later.

* In the root of your Hugo project, initialize the Firebase project with the `firebase init` command:

```html
firebase init
```
From here:

* Choose Hosting in the feature question
* Choose the project you just set up
* Accept the default for your database rules file
* Accept the default for the publish directory, which is `public`
* Choose “No” in the question if you are deploying a single-page app

By this point you will have some new firebase files within your projecct directory. 

Your `.firebaserc` file should look something like this:

```html
{
  "projects": {
    "default": "your-project-name"
  }
}
```

Your `firebase.json` file should look something like this: 

```html
{
  "hosting": {
    "public": "public",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ]
  }
}
```

### 8. GitHub Actions
Now that we have Firebase installed and ready to go. We need to add one last step, GitHub Actions. Just to summerize, we will create a workflow that will make github do all the work to compile and push our static html files to Firebase.

* If you don't already, push your codebase to your repo. Navigate to GitHub, select your repo and go to "Actions". Click on the "New Workflow" button and then "set up a workflow yourself". 

* Name your new workflow, I chose "Deploy". 

* In the text area below copy and past the following:

```html
name: Deploy to Firebase

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  deploy_to_firebase_hosting:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout the repository
        uses: actions/checkout@master
      - name: Build Hugo
        uses: lowply/build-hugo@v0.75.1
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@master
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_CI_TOKEN }}
```

* Press the green "Start Commit" button to commit our new `deploy.yml` file to our repo.

* Next, we'll need to add a secrete to the repo. To do so, navigate to "Settings" and then "Secrets". Select "New Secret" and give it a name. I chose, `FIREBASE_CI_TOKEN`. Note, this will need to match the Firebase Token variable in our `deploy.yml` file (line 26). 

* Remember that Firebase CI token I told you to keep safe? Well, pull that up and paste that code in the "Value" text area. Save your new secret.

* And now, whenever you commit and push any changes to the repo, your workflow will activate and push your site to firebase. 