---
title: How to Install Spacemacs in MacOS Catalina
published: true
description: This is a quick guide of how to install Spacemacs into MacOS Catalina
tags: spacemacs, macos
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1602365469/articles/Install%20Spacemacs%20in%20MacOS%20Catalina/Spacemacs_screenshot.png
layout: post
date: 2020-10-10 17:45:23 -0500
categories: spacemacs macos
---

I am a fan of Spacemacs, and whenever I have to install it on a new computer, I have to go to the documentation and scroll for a while. This is why I created this quick guide with all the steps I usually follow.


# 1. Install Emacs

As Spacemacs is based in emacs, we need to install it first. As we're using macOS, the recommended way of doing this is using the brew cask:

    brew cask install emacs 


# 2. Install Source Code Pro font

This is the font suggested for the editor, and we can do it by using the `cask/fonts` tap from homebrew

    brew tap homebrew/cask-fonts 
    brew cask install font-source-code-pro


# 3. Clone the Spacemacs Project

Clone the Spacemacs project, it doesn't matter where, we're going to delete it at the end (after copying it).

    git clone https://github.com/syl20bnr/spacemacs


# 4. Checkout develop branch

As the `main` or `master` branch is a little outdated, I always use the `develop` branch for a better experience.

So let's get into the folder

    cd spacemacs

And then change the branch that we're going to copy

    git checkout develop 
    # Just to make sure everything is updated
    git pull origin develop


# 5. Copy spacemacs

We need to copy the Spacemacs folder into the `~/.emacs.d`. For this, we need to get outside the Spacemacs folder and execute

    cp -a spacemacs ~/.emacs.d


# 6. Giving permissions

As MacOS Catalina has new security features, we need to give ****Full Disk Access**** to the `/usr/bin/ruby` script which is the one that launches Spacemacs.

For this, go to `System Preferences`, and then into `Security and Privacy`.

![MacOS Security and Privacy](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602356666/articles/Install%20Spacemacs%20in%20MacOS%20Catalina/step1.png "MacOS Security and Privacy")

Then, go into `Privacy -> Full Disk Access`, and then unlock the lock to enable changes. After that, go and click the add button.

![Full Disk Access](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602356667/articles/Install%20Spacemacs%20in%20MacOS%20Catalina/step2.png "Full Disk Access")

Then you need to search for the `/usr/bin/ruby` script. The easiest way to do this is with the KeyBoard shortcut

    SHIFT + CMD + G  

Which will open a search window and then there we can type `/usr/bin/ruby`.

![Full Disk Access](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602356843/articles/Install%20Spacemacs%20in%20MacOS%20Catalina/step3.png "Full Disk Access")

We can click accept and lock again to prevent further changes.


# 7. Verify the access

If you open Spacemacs and the hit

    SPC f f 

you should be able to go now to the desktop and any folder you want.

![Working File access Spacemacs](https://res.cloudinary.com/dmrgfufa4/image/upload/v1602357529/articles/Install%20Spacemacs%20in%20MacOS%20Catalina/step4.png "Working File access Spacemacs")

If this is useful for you, please consider giving it a like.
Happy hacking!
