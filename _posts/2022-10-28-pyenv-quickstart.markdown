---
title: Pyenv and Pyenv Virtualenvs Quickstart
published: true
description: Get a quick introduction to pyenv with the most used commands and features.
tags: python, pyenv, virtual, environments, virtualenv
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1666993346/articles/Python%20Env%20tutorial/background_python_envs.png

layout: post
date: 2022-10-28 16:34:51 -0500
categories: python pyenv virtual environments virtualenv
---
![Cover Image](https://res.cloudinary.com/dmrgfufa4/image/upload/v1666993346/articles/Python%20Env%20tutorial/background_python_envs.png)
###### Image obtained from DALL-E: *epic virtual pythons yellow and blue with a cyberpunk style*

Whenever I get a new computer, I need to get through the python versions management, so this is the guide with the most common commands I need to execute to get up and running.

For this, I will use [pyenv](https://github.com/pyenv/pyenv) and the [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) tools.

## Initial help

This should be the most common first step for most of the tools we use.

```shell
# Get the available commands for pyenv
pyenv --help
```

## Get The Installed Python Versions

This command will show us which versions we have already installed in our system. In the beginning, it should be only `system`, which is the default installation that comes with the macOS.

```shell
## List all the python versions available to pyenv
pyenv versions
```

## Install A New Version

First, we should run the `--help` command, to get the available subcommands available in the `install` option.

```shell
# Generic help & usage
pyenv install --help
```

Then, we need to decide which version we want to install. For that, we may want to first see all the available versions. To achieve this, we need to execute:

```shell
# List all available versions
pyenv install --list
```

Finally, we need to execute the installation command with the version we want. As an example, I will install the `3.9.14` version.

```shell
# Install selected version
pyenv install 3.9.14
```

To verify our installation, we may execute `pyenv versions` again, and the recent installation should appear there.

## Use The Recently Installed Python Version

To use the recently installed as the `global`, or default version, we need to execute:

```shell
# Sets the global Python version
pyenv global 3.9.14
```

## Create a Virtual Environment

The best practice for python projects, is to use a virtualenv per project, so we can have isolated dependencies between them. To create a new virtualenv we need to execute:

```shell
# Create a new virtual env with the global python version
# pyenv virtualenv YOUR_VIRTUAL_ENV_NAME
# e.g.
pyenv virtualenv python-graphql-client
```

## List All the Virtual Environments

A good way to verify that a virtual env has been created successfully is to list all the available virtual environments. To achieve this, we execute:

```shell
# Show available virtualenvs
pyenv virtualenvs
```

## Activate The Virtual Environment

To activate, and start using your virtual environment, you need to execute:

```shell
# Activate a virtualenv
# pyenv activate YOUR_VIRTUAL_ENV_NAME
# e.g.
pyenv activate python-graphql-client
```

## Deactivate The Virtual Environment

Once you're done using your virtual environment, you may want to deactivate it. To achieve this, you may run:

```shell
# The deactivation must be sourced
source deactivate
```

## Extra

If you want to avoid activating and deactivating manually, you can use [aactivator](https://github.com/Yelp/aactivator), which will activate and deactivate automatically based on the project you are placed.

## Conclusions

This is the basic stuff that works for me. If you find it useful, consider giving a like. Happy hacking!
