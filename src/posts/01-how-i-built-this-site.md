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

## Intro<!-- omit in toc -->
This is my attempt to log the process of how this site was built. For now, it will serve as a place for me to quickly log the process as I work along. The goal is to eventually repurpose this as a proper blog post, well formatted and written in a way that is easy to follow. For now though, you'll just have to bare with the ramblings of a slightly mad developer sketching down notes in a journal like fashion. I'll make some mistakes, and be learning along the way. It's not going to be pretty, but I'll try my best to take you on a journey with me as I build this site. Good Luck!

**Lets Begin...**

---

- [Initialising the project](#initialising-the-project)
  - [Cloning an 11ty template project](#cloning-an-11ty-template-project)
  - [Updating root files](#updating-root-files)
    - [Updated package.json](#updated-packagejson)
    - [Updated metadata.json](#updated-metadatajson)
    - [Cleaned content from readme.md](#cleaned-content-from-readmemd)
    - [Updated travis.yml](#updated-travisyml)
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
- [Adding tasty-css](#adding-tasty-css)
  - [Adding tasty-css as a dependency](#adding-tasty-css-as-a-dependency)
  - [Autoprefixing and minifying](#autoprefixing-and-minifying)
  - [Linting scss](#linting-scss)
  - [Include tasty-css on the pages](#include-tasty-css-on-the-pages)
  - [Adding tasty-css to npm](#adding-tasty-css-to-npm)
- [More maintenance](#more-maintenance)
  - [Updating packages (again)](#updating-packages-again)
  - [Removed unused css](#removed-unused-css)
  - [Added svg icons](#added-svg-icons)
    - [Added icons to tags](#added-icons-to-tags)
    - [Added icons to footer](#added-icons-to-footer)
  - [Fixed minor layout styling issues](#fixed-minor-layout-styling-issues)
  - [Added styling for pre and code elements](#added-styling-for-pre-and-code-elements)
- [Created this post](#created-this-post)

---

## Initialising the project
Quick precursor, prior to this project I had been using a static site generator I built myself using gulp, which had support for nunjucks, scss and js (via webpack). I'd used this to build various prototype and proeuction sites, but I wanted to add more to it and updating and maintaining it all was becoming a bit too much of a chore for me. I'd heard about other static site builders, looked at the spec for each one and landed on 11ty as my preferred choice. The reasons for this where that it supported a lot of the functionallity that I wanted to add to my site builder.

11ty provides a wide range of template projects that you can use as a starter. I looked through some of these and settled one one of the more basic ones. I wanted to use this as an oppurtunity to learn about 11ty, and as such I wanted a very minimal starter.

### Cloning an 11ty template project
```
git clone https://github.com/11ty/eleventy-base-blog.git .
rm -rg .git // I did this so that I could initialise my own git repository
```

### Updating root files

#### Updated package.json
Because I cloned this project initially, I needed to modify the `package.json` to include the correct data for this project, as you would if you ran `npm init` and followed the guided process.

#### Updated metadata.json
The `metadata.json` file is used to add some basic data to the site, such as the url, page title, language etc. Again, this data needed to be modified to suit this project.

#### Cleaned content from readme.md
I didn't want to use the `readme.md` from the cloned project, so I empited the contents out ready for me to write my own.
#### Updated travis.yml
There where some project specific data in the `travis.yml` file, which I also updated to match this project.
### Modifying the directory structure
I wasn't keen on the original directory structure of the project. I like to keep my `src` and `dist` directories seperate from each other, wheras this project didn't use a `src` directory and instead served things from the root.

I also prefer to have my assets contained within an `assets` directory where I can store my imgs, fonts, css and js files.

The directory structure had a directory within `src` called `includes` which would be used to store template files, and within here there was a subdirectory called `layouts`. I was not keen on this so I decided to move layouts to the `src` directory, and rename `includes` to `components` as this is more similar of a structure to the gulp based system I had built myself. These changes meant I had to edit a lot of the references to these directories within the other template files.

The next task was to make some modifications to the `eleventy.js` file. This file is used by 11ty as a configuration for the build. I  cannot remember all the changes made here at the time of writing, but I will update this section once I have verified what work was done here.

Finally, I updated the `.gitignore` file, I believe part of my reason for doing this was to stop 11ty from including certain files within the build. Again, I cannot remember this for certain just yet, but will update this post once I have reviewed the work done on the previous step.

<!--
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
- -->

### Setting up the hosting
The next task was to configure the hosting. I knew I wanted to host the site via GitHub pages. The main reason for this was as a learning excercise. I'd seen it done previously for project sites but had never gotten round to putting it into practice. I also wanted to use this oppurtunity to experiment with GitHub actions as this is another area that I hadn't had the oppurtunity to practice. It would be been very easy to set up a simple vhost on one of my servers and upload the projects `dist` directory via FTP or SSH, but I've done that plenty of times before, this time I wanted automation to handle all that. Updating the site should be as simple as commiting a change to the GitHub repo.

- Add CNAME file to project root
- Test build, worked.
- Added .github/workflows/deploy-and-build.yml
- 1st commit
- Pushed to remote, triggered action, built site and deployed to new branch... Result!

## Making it my own

### Cleaning up
As I was using GitHub actions, I didn't really need either the `travis.yml` or `netlify.yml` files, so I deleted them
<!-- - Removed travis.yml and netlify.yml -->

### Borrowing bits from html5boilerplate
I've liked using the html5boilerplate as a starting point for projects. It doesn't do much, but makes for a very clean jumpoff point that can be moulded into anything, whilst covering a lot of points that I would probably forget about (such as `site.webmanifest`). 

I started by cloning the repository into a sub-directory of the `src` called `_rootfiles`, and again ran `rm -rf .git` to clear out the initial git repository.

I then updated `_layouts/base.njk` to include some of the document structure from the html5boilerplate `index.html`.

Finally, I updated `eleventy.js` to copy the contents of `_rootfiles` into the root of the `dist` directory during the build.

<!-- 
- Implemented rootfiles from html5boilerplate
- Restructured _layouts/base.njk
- Added references to new rootfiles from html5boilerplate
- Updated eleventy.js to copy from src/_rootfiles/ to dist/
-->

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

## Adding tasty-css

### Adding tasty-css as a dependency
Added tasty-css as a dependency via the GitHub repo

### Autoprefixing and minifying
Installed postcss and some plugins to faciliate in autoprefixing and minifying css

### Linting scss
Installed stylelint via npm

### Include tasty-css on the pages
Updated the head to include tasty-css
Modifed tasty.scss to include additional partials

### Adding tasty-css to npm
It became clear I'd need to be able to version control tasty-css as it's in very early stages of development. I had to learn how to release the package to npm,

## More maintenance

### Updating packages (again)
Updated various packages to their latest version

### Removed unused css
Commented out all tasty-css partials that are not currently in use

### Added svg icons

#### Added icons to tags

#### Added icons to footer

### Fixed minor layout styling issues

### Added styling for pre and code elements

## Created this post
All the writing in this post before this point was written 'after the fact'. From this point forward, the plan is to maintain this doc as I go along. There are parts I need to go back and edit, and I will do that eventually, but the most important part at this stage is to make sure that anything done from here onwards is done on this post, at the time it happens.