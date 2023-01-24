+++ 
draft = false
date = 2023-01-24T21:55:57+01:00
title = "How to set up simple website with hugo"
description = "Description on how did I set up this website"
slug = ""
authors = [ "Marian Sabat" ]
tags = [ "web" ]
categories = []
externalLink = ""
series = []
+++

Here is the problem, I want a website where I can put what ever I want.
I could write it from scratch but I don't have time nor desire to do that.
Not to mention that my design and css skill is abysmal.
So obvious answer is to use some tool that will create web for you. One 
of such tools is Hugo.

This is step by step how have I created this website.

## 1. Install Hugo
I have two computers, both are running linux. My main pc is running Manjaro
and other one has Mint. Installing Hugo on Manjaro was very simple. Just run
`sudo pacman -S hugo`. As it turns out, Hugo package available on Mint is 
very old and doesn't fulfill my needs. But I've found better one on snap.
So I've installed Hugo on Mint with `sudo snap install hugo`. Because
this is a snap package, every Hugo command needs to be preceded with
`snap run`. I won't be writing it everywhere, just know that on Mint I had to
run it through snap (i.e. `snap run hugo ...`).

After installing you can check that everything is ok by checking installed
version `hugo version`.

## 2. Create new project
Now to the fun part, creating project. To do that simply run
```
hugo new site myWeb
cd myWeb
git init
```
This will create new hugo project for you and set up git repository.

## 3. Get theme
However, site is still unusable. What you need is hugo theme. I will not
be going over how to create your custom theme. I might write article 
about it in future. But for start you can use any already existing theme.
There are tousands out there just search for one. I found one that
I like, [hugo-coder](https://github.com/luizdepra/hugo-coder). So add it 
to `themes` directory in your project (just folow instructions in github).
You can do it with

`git submodule add https://github.com/luizdepra/hugo-coder.git themes/hugo-coder`

Other thing is to configure it. You need to update file `config.toml`.
Just use example from their [documentation](https://github.com/luizdepra/hugo-coder/blob/main/docs/configurations.md#complete-example).

## 4. Run website
Now you are ready to run everything. Hugo comes with server build in, so
you can easily try that everything works as it should. Use command

`hugo server`

This will run server with your website. Also it is looking for any changes,
so when you will be updating any file, after save all changes will be rendered
in real time.

## 5. Adding content
Contents of your website should be stored to `content` directory. That include
all page content like text. To create new page, you can use `hugo new` 
with path to new file that will be created. E.g. if you want to create new blog 
post you can do `hugo new posts/first_post.md`. This will create new
markdown file under `content/posts/` with name `first_post.md`. You can edit 
this file and you should see new article appear on you site under Blog.

## 6. Go public with github pages
Last thing is to set up github action to automaticaly deploy newest version
of page when pushing to remote repository on main branch.

Inside project root directory create new directory `.github/workflows`.
And create new file that will reside in this directory. Name it `gh-pages.yml`.

Content:
```
name: github pages

on:
  push:
    branches:
      - main  # Set a branch that will trigger a deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

This action will deploy contents of `pulic` directory into branch `gh-pages`.
So for everything to work correctly, make sure that in setting under 'Pages'
there is set up branch for build and deployment as `gh-pages`.

Before pushing to remote, don't forget to run command `hugo`.
