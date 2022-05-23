---
title: How I Built This Site
description: Follow the ramblings of a crazed developer trying to document the development of this site. It's not going to be a fun ride, but I'll try my best to take you on a journey with me as I build this site. Good Luck!
date: 2022-05-23
tags:
  - 11ty
  - scss
  - webperf
  - github
  - npm
layout: post
templateClass: u-padding-small
---

- [Intro](#intro)
- [Initialising the project](#initialising-the-project)
  - [Cloning an 11ty template project](#cloning-an-11ty-template-project)
  - [Updating root files](#updating-root-files)
  - [Modifying the directory structure](#modifying-the-directory-structure)
  - [Setting up the hosting](#setting-up-the-hosting)
- [Making it my own](#making-it-my-own)
  - [Cleaning up](#cleaning-up)
  - [Borrowing bits from html5boilerplate](#borrowing-bits-from-html5boilerplate)
  - [Borrowing bits from Harry Roberts @ CSS Wizardry](#borrowing-bits-from-harry-roberts--css-wizardry)
  - [Adding a GitHub status badge](#adding-a-github-status-badge)
- [Updating the build](#updating-the-build)
  - [Basic JS bundling](#basic-js-bundling)
  - [Basic SCSS support](#basic-scss-support)
  - [Creating custom NPM scripts](#creating-custom-npm-scripts)
  - [Testing](#testing)
- [Lets start creating some content](#lets-start-creating-some-content)
  - [Creating my first page](#creating-my-first-page)
  - [My first issue (Build missing CSS)](#my-first-issue-build-missing-css)
  - [Adding more content](#adding-more-content)
  - [Changing the order of the nav](#changing-the-order-of-the-nav)
  - [Cleaning up some more](#cleaning-up-some-more)
  - [Another issue (Missing page titles)](#another-issue-missing-page-titles)
- [Upgrading the build](#upgrading-the-build)
  - [Performing the upgrade](#performing-the-upgrade)
  - [Add packagelock.json to git](#add-packagelockjson-to-git)
  - [A bit more cleanup and maintenance](#a-bit-more-cleanup-and-maintenance)
  - [Updated page template to use base.njk](#updated-page-template-to-use-basenjk)
  - [Modified the way post tags are rendered](#modified-the-way-post-tags-are-rendered)

---

## Intro
This is my attempt to log the process of how this site was built. For now, it will serve as a place for me to quickly log the process as I work along. The goal is to eventually repurpose this as a proper blog post, well formatted and written in a way that is easy to follow. For now though, you'll just have to bare with the ramblings of a slightly mad developer sketching down notes in a journal like fashion. I'll make some mistakes, and be learning along the way. It's not going to be pretty, but I'll try my best to take you on a journey with me as I build this site. Good Luck!

**Lets Begin**

## Initialising the project
Quick precursor, prior to this project I had been using a static site generator I built myself using gulp, which had support for nunjucks, scss and js (via webpack). I'd used this to build various prototype and proeuction sites, but I wanted to add more to it and updating and maintaining it all was becoming a bit too much of a chore for me. I'd heard about other static site builders, looked at the spec for each one and landed on 11ty as my preferred choice. The reasons for this where that it supported a lot of the functionallity that I wanted to add to my site builder.

11ty provides a wide range of template projects that you can use as a starter. I looked through some of these and settled one one of the more basic ones. I wanted to use this as an oppurtunity to learn about 11ty, and as such I wanted a very minimal starter.

### Cloning an 11ty template project
```
git clone https://github.com/11ty/eleventy-base-blog.git .
rm -rg .git // I did this so that I could initialise my own git repository
```

### Updating root files
- Updated package.json
- Updated metadata.json
- Cleaned content from readme.md
- Updated travis.yml

### Modifying the directory structure
- Restructured website files
- Moved all 'src' files into a 'src' directory
- Moved 'css' and 'img' to directories to 'src/assets'
- Created a '_layouts' directory and moved layout files into here
- Renamed '_includes' to '_components'
- Updated template paths in all 'src' files
- .eleventy.js
- Updated path for layoutAlias
- Updated paths for 'css' and 'img' passthrough
- Updated path for 404 page
- Updated main dir paths
- Updated .gitignore

### Setting up the hosting
- Add CNAME file to project root
- Test build, worked.
- Added .github/workflows/deploy-and-build.yml
- 1st commit
- Pushed to remote, triggered action, built site and deployed to new branch... Result!

## Making it my own

### Cleaning up
- Removed travis.yml and netlify.yml

### Borrowing bits from html5boilerplate
- Implemented rootfiles from html5boilerplate
- Restructured _layouts/base.njk
- Added references to new rootfiles from html5boilerplate
- Updated eleventy.js to copy from src/_rootfiles/ to dist/

### Borrowing bits from Harry Roberts @ CSS Wizardry
- Restructured header to mirror config at csswizardry
- Added Google Analytics via method used on csswizardy
- Added performance logging to GA via method on csswizardry

### Adding a GitHub status badge
- Added github action status badge to readme.md

## Updating the build

### Basic JS bundling
- Updated 11ty config to copy js files from src/assets/js
- Added main.js to base.njk header

### Basic SCSS support
- Installed npm packages for processing scss/css
- Replaced css files in base.njk with new main.css compiled from scss

### Creating custom NPM scripts
- Created new npm scripts to clear 'dist' and build/watch/post-process scss/css

### Testing
- Add code testing with Mocha

## Lets start creating some content

### Creating my first page
Started by updating the content on my about page

### My first issue (Build missing CSS)
The built script was missing the parts to compile scss

Original
```
"build": "eleventy",
```

Modified
```
"build": "npm-run-all clean:dist clean:assets build:js build:scss build:site",
"build:site": "eleventy",
```

### Adding more content
I then added my first test blog post... victory
I then continued to update the content of the about page

### Changing the order of the nav
Modified the 'order' of pages in the front matter data

### Cleaning up some more
Noticed some files included in the build dir that didn't need to be there, these where coming from the `_rootfiles` directory (from html5boilerplate)

### Another issue (Missing page titles)
Noticed some pages where missing titles, so I went and added them to the pages front matter data, for some reason I can't seem to get it working on the tags page `/page-list`.

## Upgrading the build

### Performing the upgrade
Ran `npm outdated` to generate a list of all outdated packages
Ran `npm update` to update all packages
Tested and everything seemed fine initially

### Add packagelock.json to git
For some reason, this file was added to the `.gitignore` file, so I removed it and then committed the change.

### A bit more cleanup and maintenance
This project came with two stylesheets initially, I merged both of them together, deleted the redundant file and then update `layout.njk` to remove the reference.

### Updated page template to use base.njk
Originally most of the pages where using a layout file `home.njk` which I replaced with `base.njk`.

### Modified the way post tags are rendered
Made changes to `post.njk` and `postslists.njk` to change how tags are output.

