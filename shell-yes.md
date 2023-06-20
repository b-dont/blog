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
I'm going to break down shell configurations into several different files (more details in [this awesome Unix StackExchange answer](https://unix.stackexchange.com/a/71258)); the `rc`, `env`, and `aliases` files, which are going to categorize our config snippets. The `rc` file is going to carry our shell's interactive behavior, the `env` file is going to contain our exported environment variables, and our `aliases` file will carry, you guessed it, our aliases. These files are often referred to as "dot files" (because they're usually hidden files that are pre-fixed with a "`.`") and they will be referred to as such in this post from here out.

There are a few other shell-specific config files I'll mention later, but will not dive into. I'll link the documentation for these files (I use zsh, so these docs and this post will be focused there) so you can dive into the liturature yourself and play around with them.

### The `rc` file
An `rc` file (or a [`RUNCOM` file](https://www.baeldung.com/linux/rc-files)) is a file or directory designated to hold configurations for an interactive shell. Modern shells usually come with an `rc` file when installed with several example configurations lines and inline comments explaining them. 

This is the best place to start your configurations, and there are plenty of experienced users out there who run their shell with all of its configs in this file alone. The `rc` is (probably?) the most important config file for your shell.

Let's start configuring this thing. We'll continue using `zsh` as our example here, and will pull the important lines from the generated `zshrc` that comes with a `zsh` install.

```bash
# .zshrc

HIST_STAMPS="mm/dd/yyyy"
ENABLE_CORRECTION="false"

if [ -f ~/.aliases ]; then
	. ~/.aliases
fi

eval "$(dircolors -p | \
    sed 's/ 4[0-9];/ 01;/; s/;4[0-9];/;01;/g; s/;4[0-9] /;01 /' | \
    dircolors /dev/stdin)"
```

We'll break this down line-by-line. The first line here describes the time stamp formatting for our shell history. The second line sets command-line autocorrection. The `if` statement checks for our `.aliases` config file, and makes sure `zsh` sees it. and the last `eval` block removes the background colors of directories when running `ls` commands.

Now we'll take a look at the contents of `.aliases` we dropped into our `.zshrc`.

### The `aliases` file
This one will be brief, as everyone's alieses will be very different from everyone elses (for the most part). Usually, most of my aliases are `ssh` commands to quickly jump to other hosts, but there's a few others that I use as well. Create a `.aliases` file and drop the following in it:

```bash
# .aliases

# grep
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'

# cat
alias cat='bat --theme Nord -p'

# ls 
alias ll='ls -Al'
alias la='ls -A'
alias ls='ls --color=auto'
alias l='ls -CF'

# dir
alias dir='dir --color=auto'
alias vdir='vdir --color=auto'

# opening files in Helix; replace `hx` with your favorite text editor
alias ali='hx ~/.aliases'
alias zrc='hx ~/.zshrc'

# source config files
alias arc='source ~/.aliases'
alias src='source ~/.zshrc'

# system
alias upd='sudo dnf upgrade -y && flatpak update'
```

These are pretty straight forward. We use the `alias` keyword followed by a key-value pair describing our alias and the command it represents. The `ls`, `grep`, and `dir` aliases here can be copied and pasted without issue; the `cat` alias will require [`bat`](https://github.com/sharkdp/bat), a fancy `cat` alternative. The rest are shortcuts to open config files, source them, and one to run system updates on Fedora.

Zsh will drop read these as though they were directly in the `.zshrc`. I like to keep the aliases seperate, it feels more organized to me, but there's no issue at all with dropping them straight into the `rc` if you like everything in one place. They should both work the same.

### The `env` file
The `env` (environment) file contains our exported variables. The zsh `.zshenv` file is always [sourced](https://ss64.com/bash/source.html). We want variables that other programs will see or use in here. Some examples (from the previous StackExchange answer above) include your `$PATH`, `$EDITOR` and `$PAGER` environment variables.

Here's some basic environment variables you can drop in the `.zshenv` file:
```bash
# .zshenv

export DOTFILES=$HOME/.dotfiles
export DOCKER=podman
export PATH=$HOME/bin:/usr/local/bin:$PATH
export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
if [[ -n $SSH_CONNECTION ]]; then
  export EDITOR='vim'
else
  export EDITOR='hx'
fi
```

Ok, so first we create a `DOTFILES` variable that describes the location of our dotfiles. Next, I prefer to use Podman over Docker, so for convenience, I have the `$DOCKER` env variable point to Podman instead. Next is a line that adds `$HOME/bin` and `/usr/local/bin` to `$PATH`, then finally we set `$LANG` to `en_US.UTF-8`.

The `if` block here at the end, we set `$EDITOR` to Helix if we're not on a `ssh` connection, and Vim if we are connected via `ssh`.

### The `profile`
In zsh, this would be the `.zprofile` file and is sourced before the `.zshrc` file. Here, we want to place our configs for login shells. This is an alternative to the `login` file (or `.zlogin`), which is sourced _after_ the `rc`.

### Managing Your Dotfiles
So you've got all these files now, but how do we organize and manage them? If you've consolidated everything to just the `.zshrc`, then there's no issues with just dropping it in your home directory and leaving it at that, but if you expect your configs to grow and need a way to organize them, keep backups, and maybe use some version control then you can place them all in a git repository and use a sym-link manager like [GNU Stow](https://www.gnu.org/software/stow/). 

We'll start by taking a look at stow and building out our directory structure that it can use to manage our dotfiles. Create a directory in your `$HOME` called `.dotfiles` and create a `zsh` subdirectory inside it, then move (`mv`, not `cp`) the dotfiles we've worked on to that directory, like so:

```bash
~/.dotfiles
└── zsh
    ├── .aliases
    ├── .zshenv
    └── .zshrc
```
This `.dotfiles` directory will be our root directory when using `stow`, and inside this directory we'll be storing our `stow` "packages", which we'll be calling `stow` on to install. In this example, we'll be creating a `zsh` package which will install the three config files we've created to the target directory we give to `stow`. By default, the target directory is the directory above our `stow` root directory, which is our home directory in this case. If you want to change the target directory, there's a flag we can use, which I'll get into next. First, let's install this `zsh` package with `stow`. Run the following command in your `.dotfiles` directory.

```
stow -S zsh
```

This will call stow on our `zsh` package, and install our dotfiles in the directory above our `.dotfiles` directory, which is `$HOME`. Stow uses symbolic links, and "installs" by creating the sym-linked in the target directory. This is the same as if you would run `ln -s /home/btp/.dotfiles/zsh/.zshrc /home/btp/` for each of these files. Because these are soft symbolic links, removing them from the home directory will not affect the files in our `.dotfiles` location. 

Now consider the following package:
```bash
.dotfiles
├── alacritty
│   └── .config
│       └── alacritty
│           └── alacritty.toml
└── zsh
    ├── .aliases
    ├── .zshenv
    └── .zshrc
```

In this case, we've added a config package for the terminal Alacritty. Stow will check the fill path under the package, and mirror it in our target location. When running the `stow` command on this package, it will add the full config path to `$HOME/.config/alacritty/alacritty.toml`, which is exactly where we want it.

You can layer your Stow root directory however you want, but when you install packages, you'll need to navigate to the directory that holds the package directory itself; Stow will not accept a path to a package.
```bash
# This will *not* work
stow -S .dotfiles/zsh
```
I keep my `.dotfiles` in a git repository, and hold configs for multiple operating systems there, then navigate to the relivant OS directory for installs, like so.
```bash
.
├── linux
│   ├── alacritty
│   ├── awesome
│   ├── helix
│   ├── nvim
│   ├── p10k
│   ├── powershell
│   ├── tmux
│   └── zsh
├── macos
│   ├── alacritty
│   ├── helix
│   ├── nvim
│   ├── p10k
│   ├── powershell
│   ├── tmux
│   └── zsh
└── wsl
    ├── helix
    ├── p10k
    ├── powershell
    ├── tmux
    └── zsh
```

### Cool Config Tricks
Here's a couple of neat config snippets I've picked up that you may also find useful.

```bash
# .zshrc

# Auto activate venv when entering a venv directory
function precmd {
    # Set up the venv
    if ! typeset -f deactivate >/dev/null; then
        for activate in ./venv/bin/activate ./.venv/bin/activate; do
            if [ -e "$activate" ]; then
                . "$activate"
            fi
        done
    fi
}

# Launch your shell with tmux, except when over ssh
if ! [[ -n $SSH_CONNECTION ]]; then
  if [ "$TMUX" = "" ]; then tmux; fi
fi
```

### Conclusion
I tried to keep the scope of this post pretty limited to avoid confusion, and to avoid a massive post. This topic is very subjective, so take these examples as just that, examples. I've found what works for me when building and managing my config files in a scaleable way, and I hope this has helped you as well. Enjoy.