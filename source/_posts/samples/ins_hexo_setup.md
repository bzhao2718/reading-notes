---
title: Hexo Setup Instructions
date: 2021-11-24 13:52:01
---

[TOC]

# Some References

Here is doc [next theme](https://theme-next.js.org/).

And the [classless-hexo](https://github.com/fiatjaf/classless-hexo) theme.

[hexo renderer pandoc](https://github.com/wzpan/hexo-renderer-pandoc).

## Setup pluggins

Here is a [tutorial of setting up Hexo next theme](https://jdhao.github.io/2017/02/26/hexo-install-use-issue/).

Generate ssh keys.

Here are some commands used to install some pluggins

This [tutorial mentions setup MathJax to enable copy latex code](https://yanghaowang.github.io/posts/20201125-LaTex-Display-in-Hexo).

# Hexo Init Setup

## 1. Install Prerequisite Tools

This step is needed only when these tools are not installed.

### Install NVM and Node

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install node --lts
nvm use node --lts
```

### Install Hexo

```bash
npm install -g npm
npm install -g hexo-cli
```

### Other Utilities

Install and set up **Git and Github** and the following:

**Pandoc**

## 2. Init A Hexo Blog

To **create a folder** for the blog, use the following commands:

```bash
hexo init hexo-blog # this will create a hexo-blog folder in the current directory
```

This will throw a warning:

`WARN  Failed to install dependencies. Please run 'npm install' in ".../your/hexo-blog" folder.`

Go into the newly created folder and **instal dependencies**:

```bash
cd hexo-blog
npm install
```

Now make sure the blog can run successfully on the localhost:

```bash
hexo clean && hexo generate && hexo server
```

Make sure the localhost port `4000` is not in use since the hexo uses port `4000`. The following info show show `Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.` if everything works properly. Go to the link and check it’s running.

Optionally, **copy the sample test files** into the `_posts_` folder. Some files in the samples folder may require the `hexo-renderer-pandoc` plugin, so **replace the default renderer with pandoc**:

**pandoc renderer: **  [wzpan/hexo-renderer-pandoc: A pandoc-markdown-flavor renderer for hexo.](https://github.com/wzpan/hexo-renderer-pandoc) 

```bash
# replace the default renderer
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

Test the blog on localhost again.

It is possible to customize the pandoc renderer settings in the `_config.yml` file, e.g.:

```yaml
# renderer pandoc
pandoc:
    extensions:
      - '-implicit_figures'
    filters:
      # - pandoc-crossref
    extra:
      - citeproc:
      - filter: pandoc-crossref
      - bibliography: "source/referenceAll.bib"
    template:
    meta:
    # mathEngine: katex
    # by default, pandoc uses mathjax
    # also, using pandoc as the markdown renderer will pose some issues related to post tags
    ## extensions 'implicit_figures' don't use caption, solve the duplicate caption issue when fancybox = true in butterfly.
    
```



**Example** (reading-notes):

```bash
hexo init reading-notes
cd reading-notes
npm install
```

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```



# Github Repo Setup And Deployment

## Create a GitHub repo. 

## Git init blog folder and connect to the GitHub repo

In the blog root directory, init it as a git repo:

```bash
git init
```

Then add connect it to the GitHub repo:

```bash
git remote add origin https://github.com/bzhao2718/reading-notes.git
```

A few options to play with:

Fetch the readme.md file `git pull origin master`, make some modifications as a test and push to the repo:

```bash
git push --set-upstream origin master
```

Also, create a `gitignore` file if there is not one, but it should include one when running the hexo init.

Create a new branch to store the files and serve the blog. 

**Example**:

```bash
git checkout -b reading-notes
git checkout -b ghpages-reading-notes
```

Now switch back to `reading-notes` branch, add files and push it to the GitHub repo:

```bash
git add .
git commit -m 'a message'
git push --set-upstream origin reading-notes
# git push --set-upstream origin ghpages-reading-notes
```

## Deploy With GitHub Pages

> Deploy instructions:  [One-Command Deployment | Hexo](https://hexo.io/docs/one-command-deployment.html) 
>
> I think `ssh` keys are already set up at this point.
>
> The command `hexo deploy` is used to deploy the site to the server.

First download the dependency  [hexojs/hexo-deployer-git: Git deployer plugin for Hexo.](https://github.com/hexojs/hexo-deployer-git) 

```bash
npm install hexo-deployer-git --save
```

Then go to the GitHub repo page and go to settings. Click the Pages option on the left and select the source to the branch that will serve the blog.

Save and will get a info like 

```
 Your site is ready to be published at https://bzhao2718.github.io/reading-notes/
```

Then config the deploy settings in the `_config.yml` file:

```yaml
deploy:
  type: 'git'
  repo: https://github.com/bzhao2718/reading-notes
  branch: ghpages-reading-notes
```

Now deploy it:

```bash
hexo clean && hexo deploy
```

Once one, go to the blog URL (this will be the url when setting the GitHub Pages source, like the one given above `https://bzhao2718.github.io/reading-notes/`).

# Some Basic Plugins

`npm install hexo-related-popular-posts --save` seems to cause errors.

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
npm i --save hexo-wordcount
npm install hexo-pangu --save
npm install --save hexo-filter-mathjax
npm install hexo-generator-searchdb –save
npm install --save hexo-filter-link-post
npm i hexo-filter-nofollow --save
npm install hexo-generator-sitemap --save
npm install hexo-reference-plus
npm install hexo-deployer-git --save

```

Also, add specific settings in the config file. See the `ins_hexo_butterfly file` or copy the current theme config file.

Cross reference to other files (anchor). (Already added by the above commands).

Use this plugin:  [tcatche/hexo-filter-link-post: Transfer relative post link in markdown file to post link. 将文件里的通过相对路径引用的 markdown 文件转为对应的文章的链接](https://github.com/tcatche/hexo-filter-link-post) 

```
npm install hexo-filter-link-post --save
[Referrence: bar](./bar.md#title)
# also supports Task lists {#task_list_id}
then refer to it using the manualy set id.
```

# Butterfly Theme

## Install And Setup

Step 1:  [Butterfly 安裝文檔(一) 快速開始 | Butterfly](https://butterfly.js.org/posts/21cfbf15/) 

```bash
npm i hexo-theme-butterfly
# To upgrade：under the root directory run: npm update hexo-theme-butterfly
```

In the `_config.yml`:

```yaml
theme: butterfly
```

> Make sure `pug` and `stylus` are installed if the basic plugins step is skipped. Run `npm install hexo-renderer-pug hexo-renderer-stylus --save`.

Create a `_config.butterfly.yml` file under the blog root directory, and copy the contents of the butterfly config file to this file. Or use the `cp` or `mv` command to copy or move the file to the root directory:

```bach
cp node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
# mv node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```

Now modify the `_config.butter.yml` file a little bit, e.g., uncomment some menu options, etc. 

Note that the `_config.yml` is different, it’s for the hexo blog.

Create some new pages:

```bash
hexo new page tags
hexo new page categories
hexo new page link
hexo new page archives
```

Also, create a `.nojekyll` file since GitHub Pages use `jekyll` themes as default.

```bash
vim .nojekyll
```

Now test it on localhost and GitHub Pages.

### Add hexo Next theme

Instructions from this post:  [next-theme/hexo-theme-next: 🎉 Elegant and powerful theme for Hexo.](https://github.com/next-theme/hexo-theme-next) 

Modify theme config file:

```
# Installed through npm
cp node_modules/hexo-theme-next/_config.yml _config.next.yml
# Installed through Git
cp themes/next/_config.yml _config.next.yml

```

With this way, all your configurations locate in main site config file. You don't need to edit theme config file or create any new files. But you need to **[keep up indentation](https://theme-next.js.org/docs/troubleshooting.html#Keep-Up-Indentation)** within `theme_config` option.

MathJax

```
npm un hexo-renderer-marked
npm i hexo-renderer-pandoc

```

To enable mathjax, go the the next theme setting :

```
math:
  # Default (false) will load mathjax / katex script on demand.
  # That is it only render those page which has `mathjax: true` in front-matter.
  # If you set it to true, it will load mathjax / katex script EVERY PAGE.
  every_page: true

  mathjax:
    enable: true
    # Available values: none | ams | all
    tags: none
```

Remember to set the every_page to true if not included in the source file. Also set the mathjax: enable to true as well.
