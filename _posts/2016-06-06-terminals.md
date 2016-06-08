---
layout: post
section-type: post
title: Achieving Terminal Velocity
category: Technology
tags: [ 'software', 'coding']
---

I always enjoy watching others give technical presentations because it seems every time I notice a tool or technique that I haven't seen before.  

Recently I was watching Brandon Phillips [presentation on etcd](https://www.youtube.com/watch?v=z6tjawXZ71E) and was intrigued by his use of TMUX.  I'd heard of TMUX a few times and it was on my list of things to learn so I decided to bite the bullet.  

This post will cover TMUX along with terminal applications and emulators across several OS platforms that I've found valuable over the years.

## Cross Platform

### A Note about package managers
Pretty much every OS these days has some some sort of package manager.  If you are still installing software by hand, consider scripting your environment setup/updates through your OS's package manager.

Here are some popular ones:

* Windows: [Chocolatey](https://chocolatey.org/), usage: `choco install <package>`
    * Note: Unless you are attempting to use the newly announced [support for bash in Windows 10](http://www.hanselman.com/blog/DevelopersCanRunBashShellAndUsermodeUbuntuLinuxBinariesOnWindows10.aspx), you also need some kind of unix emulator for windows, I cover these in more detail below.
* OSX: [Homebrew](http://brew.sh/), usage: `brew install <package>`
* Ubuntu/Debian: `sudo apt-get install <package>`
* RHEL/CoreOS: `sudo yum install <package>`

If you are using a configuration management tool like Chef, Puppet, or Ansible to configure your local environment please make a comment about your experience below.

### TMUX - The Terminal Multiplexer

[TMUX](https://tmux.github.io/) can be downloaded from the site or installed by your package manager (i.e. `brew install tmux`)

Here I am running TMUX inside of an iTerm tab, for the best of both worlds!

![TypeScript](/img/terminals/tmux.png){: .center-image }

#### What is great about TMUX?

Probably the most significant aspect is that you can create persistent sessions, this means:

1. You can login to a remote server, spawn one or more terminal panes, and execute commands.  If you get disconnected or logout from that machine your tmux session continues to run in the background.
1. Two or more people can connect to a single session.  This means you can have a poor mans screen share inside of the terminal.  However, this should be used with caution if one use has escalated privileges.

#### How do I use this thing?!

Just remember the **`ctrl+b` prefix**! This is the key binding that you must enter before giving every command.

There are many [cheat sheets](https://gist.github.com/andreyvit/2921703) available, as well as very good man page documentation.

There are two great presentation available on YouTube, a [shorter one that covers TMUX](https://www.youtube.com/watch?v=BHhA_ZKjyxo) and a longer one that is also [great and covers VIM](https://www.youtube.com/watch?v=5r6yzFEXajQ)

If you're working on a desktop environment be sure to **enable mouse support** by adding this line to your `~/.tmux.conf`
file:

```
# Enable mouse control (clickable windows, panes, resizable panes)
set -g mouse on
```

### A Sea of Shells

For years I've worked out of the **Bourne Again Shell (bash)**.  Recently I was at a meetup and was taken back by the presenters command prompt which featured a wealth of information about his git repo.  Turns out he was using **Oh My ZSH** along with several plugins from the long list of options.

#### Oh-My-ZSH!

If you're not ready to jump to ZSH, but still want the git nicities I came across **[Oh My Git](https://github.com/arialdomartini/oh-my-git)** while I was configuring my system.

In either case, you will end up with a terminal that looks something like this:

![TypeScript](/img/terminals/zsh-git-cmd.png){: .center-image }

Notice the circle and plus markers for when there were changes pending and scheduled for commit.  The red 'x' icon is added anytime the previous command terminates with a non-zero code.

Below are some important steps I encountered when getting setup with Oh-My-Zsh.  If you're setting up your mac environment from ground zero, feel free to borrow from my own [script on GitHub](https://github.com/codecounselor/scripts/blob/master/bootstrap-osx.sh)

1. Make sure you have the latest Python 2 version installed and the PIP is up to date
1. Install powerline-status with pip
1. configure your terminal to use a powerline font

This is all included in the script.  This is tailored and tested only on OSX, but in theory it should work on Linux environments as well.  The relevant parts are:

```zsh
brew install git zsh-completions

#Get the latest Python 2 Version
brew install python
brew linkapps python
pip install --upgrade pip

#Setup Oh My ZSH
if [ ! -d ~/.oh-my-zsh ] ; then
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
  pip install powerline-status
  mkdir ~/Fonts && cd ~/Fonts
  git clone git@github.com:powerline/fonts.git powerline
  cd powerline && ./install.sh
else
  echo "Oh My ZSH Already installed..."
fi

```

You will probably want to customize your `~/.zshrc` file with plugins and themes based on your own requirements.  Mine is available in the same github folder.

[Zsh Completions](https://github.com/zsh-users/zsh-completions/blob/master/zsh-completions-howto.org) are a great feature but they can affect performance.  Enable the ones you use on a regular basis.

*Important:* You will need to configure iTerm to use a powerline font or the special symbols will not be rendered on the command prompt.

A lot of folks prefer the Menlo font, you can get that here: https://github.com/abertsch/Menlo-for-Powerline

## OSX Only

### iTerm
If you're an mac user and you're not already using iTerm download it now! (`brew install cask iterm2`)
iTerm is great in and of itself, besides supporting tabs (`⌘+T`) it also supports splitting panes vertically (`⌘+D`)  and horizontally (`⌘+⇧+D`) .

## Linux Only

### Terminator

For years when I worked daily on a RHEL system I couldn't live without [Terminator](http://gnometerminator.blogspot.com/p/introduction.html).  I don't think there is much functional difference between this and TMUX, so the choice is yours.  

See the homepage for installation instructions.  The man pages lists all of the shortcuts.

## Windows Only

I rarely spend time developing on windows, but when I have to I tend to use **[ConEmu](https://conemu.github.io/)** along with **Git Bash** or **[Babun](http://babun.github.io/)**.

These can be installed with `choco install -y conemu git babun`

**ConEmu** is a terminal emulator that allows you to select whatever shell you want.  This could be the windows command prompt, powershell, git-bash, babun, or anything else you choose to install.

**Babun** is a shell built on top of Cygwin and it installs a copy of cygwin in your user directory.  Refer to the main site for more information.

**Git-Bash** comes with Git for windows and includes a lot of the unix utilities (i.e. cat, grep, find, etc) that will make you a more productive developer on windows.  Most of the time this will be sufficient unless you need a more complete unix environment.  If thats the case you might want to consider running Vagrant or Docker.

# Until Next Time...
Perhaps a future post will cover VIM, **please share any tools or techniques that you've found beneficial in the comments!**
