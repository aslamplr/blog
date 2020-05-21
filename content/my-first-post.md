+++
title = "How I setup this blog site?"
date = 2019-09-05T09:15:08.000Z
draft = false
category = "blog"
+++
> **UPDATE** (on 22-May-2020): This blog was written while this site was initially setup using **[Hugo](http://gohugo.io/)**. This site was recently migrated to **[Zola](http://getzola.org/)**. The title of this post is also changed from ***"How I setup this blog site?"*** to ***"How to setup a blog site using hugo?"***

This is supposed to be my first blog post. The truth is, this is my second blog post. I was wondering what I will write down in my first blog post other than about the "Video Resnet" which I already had in mind before starting to setup this site.

> **This post is for you,** If you like this blog site and want to setup one for you just like this one. You are looking for a static site generator. And if you are willing to write your blog posts in `markdown` format (other format are supported by this setup like `html` and so but I won't discuss all that in this post). And bit of ~~expertise~~ familiarity with `git` and `github`. And willing to get your hands dirty in the command line. And if you love terminals. I ❤️ terminal windows!

<!--more-->

## Static site generators?

Most of the blog sites you see in the web are of two categories. Either (i) dynamic CMS (ii) static CMS. Dynamic CMS are complex setup compared to static setup. An example for a dynamic CMS would be Wordpress. I know people love Wordpress, even I like Wordpress. But there are a lot of cogs when it comes to a Wordpress site. Because it requires a database, then a PHP server and all.

Static CMS, is different as the word says everything is few static files. Your blog post is a simple file written in markdown or html. As simple as that. You don't need to worry about a lot of other stuffs like databases, migration etc.

[Why use a static site generator?](https://learn.cloudcannon.com/jekyll/why-use-a-static-site-generator/) provides a good enough explanation on static site generators.

Now that I have decided on static site generators. The next question is what tools I am going to use for my blog site.

## Tools?

I have the following things to be satisfied for any setup that I choose.

* Able to write my blog posts in `markdown` format.
* Not be bothered about the underlying technology and engine (React, Ruby, PHP etc.).
* Simple deployment steps.
* Theming ability. And availability of free to use themes.
* Customizability of themes without a lot of complexity.

When the inception of the blog site occurred in my mind. I had the github.io url of my blog site in my mind (https://aslamplr.github.io). The next thing I was thinking was `Jekyll`. I am not comfortable with Ruby on rails. But I am good with ReactJS. Ideal choice for those who love react is `Gatsby`. I wasn't sure which one I should choose. I was predetermined in some abstract sort of way, that I should be using `Jekyll` mostly because of the internet. Mostly everyone who uses static blogging sites claim to use `Jekyll` and I wasn't sure what is the way forward. 

I searched for "Jekyll vs ..." google came to my help. Auto complete my query "Jekyll vs hugo vs gatsby". Now that is the moment I knew there exists `hugo`. And from a short read of this article [Static Site Generators Overview: Gatsby vs. Hugo vs. Jekyll](https://dzone.com/articles/static-site-generators-overview-gatsby-vs-hugo-vs). I knew that I should be using `hugo` for my blog site, no matter what. It ticks mostly every criteria that I have above.

All CHECK!

* Able to write my blog posts in `markdown` format.
* Not be bothered about the underlying technology and engine (React, Ruby, PHP etc.).    `hugo` is written in `golang` but you do not required to know anything about `golang` or setup the toolchain yourself.
* Simple deployment steps.
* Theming ability. And availability of free to use themes.   There are in fact a lot of themes available
* Customizability of themes without a lot of complexity.

Now what?

## Let's go "hugo"

I am using an `ubuntu` machine at work and a `Macbook Air` at home. So I should be able to setup with ease on both machines. Let's checkout the "Getting started" guide on hugo's website (https://gohugo.io/getting-started/installing/). Wow! They have a cross-platform binary distribution. That is good what else they have. Homebrew for macOS and Linuxbrew for linux. Bottles are good, let's pour them. So `brew` it is.

I installed `hugo` using brew following instructions on the "Install Hugo" guide (https://gohugo.io/getting-started/installing/). 

```
$ brew install hugo
```

Git is a pre-requisite. I already had Git installed on my machines. 

```
$ hugo version
Hugo Static Site Generator v0.57.2/extended linux/amd64 BuildDate: unknown
```

## Now what?

I head straight to the "Quick Start" guide (https://gohugo.io/getting-started/quick-start/).

Created a sample site and see what I could do with hugo. Play with it, tried different stuffs. 

The Hugo theme site (https://themes.gohugo.io/), that is where I headed next. I want to see what I have in store. What are the possibilities of theming.

> Trust me, at this point I know only nothing about hugo themes or hugo itself. Or how I can customize it. 

This is a confusing task, to select a theme. I want something minimal and something intuitive and familiar. Don't want a lot of noise and glitters.

Anyway I am going to write about deep learning, coding, web and stuff. Or at least that is what I think I am going to post in this blog at this point in time. So the theme should be good enough for the purpose thats it.

I shortlisted few of the themes which I like.

* Academic
* Paperback
* Book
* Hugo Dusk
* Hyde (The famous fast.ai website is using this one!)
* Minimo
* Coder
* Hugo Classic
* Hyde Hyde (The one that I choose)
* Mediumish (Wanted this one but felt like bloated)

Settled for "Hyde Hyde" (https://themes.gohugo.io/hyde-hyde/).

It is a fork of "Hyde" theme. I like the simplicity of "Hyde" theme. I wanted something similar. 

Instead of installing the theme directly onto your project it is better to submodule it. And again for the sake of customizability I suggest forking the repository in Github and using the fork in your project. This way you get the flexibility of tweaking the layout and stuff.

## Next steps

**Created a new hugo site**

```
hugo new site infinite-study # you should put your blog site name instead of `infinite-study`

# Let's go inside our hugo blog project
cd infinite-study

# Initialize a git repository
git init
```

**Now what?** Let's add our `hide-hide` theme from the forked repo.

For those of you who what to fork the theme login to Github and goto the theme repo - https://github.com/htr3n/hyde-hyde. And click the "Fork" button. Select the account that you want to fork the repo into (This is in case you have organization memberships). Now get the git remote url of your repo. Mine is `git@github.com:aslamplr/hyde-hyde.git`

Head back to the terminal window.

```
git submodule add -b master git@github.com:aslamplr/hyde-hyde.git themes/hyde-hyde

# This should clone the theme repo onto `themes/hyde-hyde` directory in your blog project tree.
```

Now, let's add the theme to `config.toml` file. The file should be available in the root of your blog project.

Mine look like this after the edit.

```toml
## Basic Configuration
baseURL = "https://aslamplr.github.io/"
languageCode = "en-us"

title = "Infinite Study"
theme = "hyde-hyde"

## Site Settings
[params]
    author = "Aslam Ahammed"
    title = "Infinite Study"
    description = "My deep dive into deep learning and AI"
    authorimage = "/img/logo.png"
    dateformat = "Monday, Jan 2, 2006"

    # sidebar, copyright & licence
    copyright = "@aslamplr"
    licence = "CC BY-SA 4.0"
    licenseURL = "https://creativecommons.org/licenses/by-sa/4.0"
    showBuiltWith = true

    highlightjs = true
    hughlightjsstyle = "github"

## Social Accounts
[params.social]
    github = "aslamplr"
    twitter = "aslamplr"

## Main Menu
[[menu.main]]
    name = "Posts"
    weight = 100
    identifier = "posts"
    url = "/posts/"
[[menu.main]]
    name = "About Me"
    identifier = "about"
    weight = 300
    url = "/about/"
```

**Let's create the first post**

```
hugo new posts/my-first-post.md
```

Wow now I have a file `content/posts/my-first-post.md` with the following content in it.

```markdown
---
title: "My First Post"
date: 2019-09-05T14:45:08+05:30
draft: true
---
```

I have added some content to test everything. And run the hugo server.

```
hugo server -D
```

Now head to http://localhost:1313/ if everything is in order. You should see a beautiful website with the Hyde Hyde theme.

**Well and good, now what? don't you want to deploy your site.**

There are a whole bunch of options to deploy your hugo site. Since I am going to deploy on Github as github pages. I head over to "Hosting on GitHub" (https://gohugo.io/hosting-and-deployment/hosting-on-github/) guide on hugo website.

From there it's real easy just follow along and you should be ready to deploy your content. Before running the `./deploy.sh` make sure that you have made the `draft: false` in your post if you want to get it published.

My hyde-hyde theme fork - https://github.com/aslamplr/hyde-hyde My blog repo - https://github.com/aslamplr/blog My GitHub pages repo - https://github.com/aslamplr/aslamplr.github.io

I don't have a discuss plugin or comments feature as of now. I will be working on enabling those soon. Till then you don't have a better way of contacting me other than emails.

My email - aslamplr(at)gmail(dot)com (don't spam bomb me please!)

Thank you for reading my post! I appreciate you spend your time reading my post. Please email me in case you followed my post and are stuck with your setup or anything related to this. I am happy to help you out or point you to the right direction.

Thank you!