---
title: Export from Org to Markdown in Spacemacs
published: true
description: This guide is the whole process to export from org into markdown within Spacemacs.
tags: spacemacs, org, export, markdown
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1602368674/articles/export%20from%20org%20to%20markdown%20in%20spacemacs/background.png
layout: post
date: 2020-10-10 17:43:23 -0500
categories: spacemacs org export markdown
---

Org-mode is one of the coolest features in Emacs and this case Spacemacs. It can be used to write documentation, personal notes, and also schedule your events in an awesome agenda.

In my case, I use Org files to write and export tutorials from one single file to three platforms, which are [Medium](https://medium.com/@roeeyn),[Dev.to](https://dev.to/roeeyn), and [my blog](https://roeeyn.github.io/roeeyn_blog/), using Spacemacs as my main editor. 

To be able to export to a Markdown from an Org file, you need to do the following:


# 1. Make sure you have the markdown layer installed

Having the [markdown layer](https://develop.spacemacs.org/layers/+lang/markdown/README.html#live-preview) installed will give you a better experience when editing markdown files.


# 2. Add the md export backend into Spacemacs

You need to add the `md` value into the export backends variable. The easiest way for me to do this is the following:

Inside Spacemacs type:

    M-x customize-option 

and then type `org-export-backends`.

It will bring a customization buffer in which you can move with the arrow keys, and select the `md` checkbox to enable it, and then press Enter in the `Appy and save` button.

![Modifying export backends](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602368202/articles/export%20from%20org%20to%20markdown%20in%20spacemacs/step1.png "Modifying export backends")


# 3. Verify the exportation

After applying and saving the new export backend, you can do a new export doing the following:

With an org file buffer open, type:

    , e e 

![Exporting menu](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602368203/articles/export%20from%20org%20to%20markdown%20in%20spacemacs/step2.png "Exporting menu")

This will open the export menu, and now we will see the new markdown option available. If you want to create a markdown file from your org file, inside the previous menu type:

    m m 

and this will create a new markdown file from your org file.


# Conclusions

From now on, whenever you want to export an org file to a markdown file, you will only need to follow step 3.

If you find this useful, please consider giving it a like.
Happy hacking!
