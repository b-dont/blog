+++
title = "Shell, Yes!"
description = "How to write shell configs that work for you, at any experience level."
template = "blog_post.html"
date = "2023-05-19"
draft = true
[taxonomies]
tags = ["bash", "zsh", "shell", "linux"]
+++

If you've used the Linux command line for any meaningful amount of time, and you've become comfortable with the basics, you're probably familiar with the term "shell". Webster's Dictionary defines.. sorry I had to do it. I'm not going to get into what a shell is, as much as the basics of how you can configure your shell here. If you'd like to learn more about what shells _are_, then the [Wikipedia page](https://en.wikipedia.org/wiki/Shell_(computing)) is a great place to start.
<!-- more -->

For any Linux enthusiast, administartor, developer, or systems engineer, the terminal is the meat and potatoes of a good chunk of day-to-day work. Your shell can be a powerfull tool to make this work easier, and sometimes more fun to work with.

### Anatomy
I'm going to break down shell configurations into several different files (more details in [this awesome Unix StackExchange answer](https://unix.stackexchange.com/a/71258)); the `rc`, `env`, `profile`, and `aliases` files, which are going to categorize our config snippets. The `rc` file is going to carry our shell's interactive behavior, the `env` file is going to contain our exported environment variables, the `profile` will carry our login shell configurations, and our `aliases` file will carry, you guessed it, our aliases. These files are often referred to as "dot files" (because they're usually hidden files that are pre-fixed with a "`.`") and they will be referred to as such in this post from here on out.

There are a few other shell-specific config files I'll mention later, but will not dive into. I'll link the documentation for these files (I use zsh, so these docs and this post will be focused there) so you can dive into the liturature yourself and play around with them.

### The `rc` file
An `rc` file (or a [`RUNCOM` file](https://www.baeldung.com/linux/rc-files)) is a file or directory designated to hold configurations for an interactive shell. Modern shells usually come with an `rc` file when installed with several example configurations lines and inline comments explaining them. 

This is the best place to start your configurations, and there are plenty of experienced users out there who run their shell with all of its configs in this file alone. The `rc` is (probably?) the most important config file for your shell.

### The `env` file
The `env` (environment) file contains our exported variables. The zsh `.zshenv` file is always [sourced](https://ss64.com/bash/source.html). We want variables that other programs will see or use in here. Some examples (from the previous StackExchange answer above) include your `$PATH`, `$EDITOR`, and `$PAGER` environment variables.

### The `profile`
In zsh, this would be the `.zprofile` file and is sourced before the `.zshrc` file. Here, we want to place our configs for login shells. This is an alternative to the `login` file (or `.zlogin`), which is sourced _after_ the `rc`.
### The `aliases` file
### Some Other Files
### Managing Your Dotfiles
### Some Cool Config Tricks
### Done