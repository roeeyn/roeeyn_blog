---
title: Basic Security For Your Brand New Server
published: true
description: This is a basic checklist of what to do when you're going to configure a brand new server to maintain it secure.
tags: security, servers, ubuntu
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1601958827/articles/basic%20security%20for%20a%20brand%20new%20server/server-90389_1280.jpg
layout: post
date: 2020-10-03 23:55:41 -0500
categories: security servers ubuntu
---

Whenever I get a brand new server for whatever reason, I always forget what to do as I only have in mind the project that this server will use. This is why I created a basic security checklist for me, to set up the basic stuff.

Note: Most of the times I work with Debian based servers, so this checklist will assume that.


# 1. Update and Upgrade

As you're receiving the server for the first time, it may not be necessary to update, but it is always recommended. Also, you may want to upgrade and remove the unused packages.

For this, you can throw a oneliner:

    apt update -y && apt upgrade -y && apt dist-upgrade -y && apt autoremove -y


# 2. Change the root password

Most of the time the first interaction will be through `ssh`, using the default root password. This is mostly insecure as the server will be publicly available and some bored guy will have enough time to brute force the password.

Run this command to change the password (logged as root):

    passwd 

I will suggest to logout with `exit` and then login again to verify that everything is ok, but it is optional.


# 3. Add another user

Having your main user to be root is not the best of the ways to handle operations. I recommend adding another user that will carry all the usual operations that do not require root access. This will prevent that if the server is compromised, at least they will not have root access.

To create a new user, run this command:

    # I will use pedrito, but you can use whatever username you want
    adduser pedrito 


# 4. Reconfigure SSH Server

It is a good idea to regenerate the ssh keys, even if they are new. This can be done by reconfiguring the `open-ssh` server.

For reconfiguring the ssh server run:

    # Reconfigure
    dpkg-reconfigure openssh-server
    # Restart the server
    systemctl restart ssh


# 5. Disable password authentication in sshd<sub>config</sub>

Having password authentication is a bad idea because as I said before, attackers will have time to brute-force passwords and we don't want that. That's why we will disable the password auth for ssh and will make available only the pub keys.

For disabling password auth:

    sudo vim /etc/ssh/sshd_config

Inside the file we need to change the following:

-   PubkeyAuthentication yes
-   PasswordAuthentication no
-   ChallengeResponseAuthentication no
-   UsePAM no
-   PermitRootLogin no
-   PasswordAuthentication no

Then, we need to copy our ssh pubkey in the `authorized_keys` file.

    # inside local machine
    cat ~/.ssh/id_rsa.pub | pbcopy

    # inside server
    mkdir -p /home/pedrito/.ssh
    vim authorized_keys

Inside the `authorized_keys` file, you should paste whatever you copy from the `id_rsa.pub` JUST AS IT IS.

Then, you should change the ownership of the `.ssh` directory, to the normal user.

    chown -R pedrito:pedrito /home/pedrito/.ssh

Before restarting the system, you may want to give your user some privileges, this can be done with `visudo`

    visudo 

And inside the file:

    pedrito  ALL=(ALL) NOPASSWD:ALL 

Finally, restart the ssh service executing:

    systemctl restart ssh 


# 6. Validate

Validate that you can log in with your ssh keys, by doing:

    ssh pedrito@ipaddress 

This time it will log in automatically because we have the ssh keys. If we try on another computer, it shouldn't let us log in. Also, the only user available for login should be `pedrito`, i.e. if we try to login as `root` it should throw another error and not proceed.


# Conclusions

This is the basic stuff that works for me. If you find it useful consider giving a like. Happy hacking!


