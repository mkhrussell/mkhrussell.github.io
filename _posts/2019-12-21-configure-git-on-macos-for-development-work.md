---
title: "Configure Git on macOS for Your Work"
excerpt: "A first time git setup guide for developers on macOS. In this guide I demonstrated how you can install Homebrew and then use that to install other tools.
Then, I configured default editor for git, so that I can use it for editing commit message and git config. I also showed, how to set VS Code as a diff tool and merge tool for Git."
date: 2019-12-21T13:21:17+00:00
author: Kamrul Hasan
classes: wide
categories:
  - blog
tags:
  - git
  - macOS
  - git-gui
  - gitk
---
## Install Homebrew (package manager) for macOS
I believe almost all iOS developers use Homebrew (*brew* command line package manager) for installing dev-tools needed for their daily work. I am also going to install it using below command, then use it to install other necessary tools.

```zsh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
## Use open source version of Git instead of Apple's one
Second thing is to install latest version of Git on the machine. Here, I am also installing *tcl-tk* as Apple deprecated built-in version in *macOS Catalina*. By installing *tcl-tk*, you will able to use *git gui* and *gitk* in your macOS.
```zsh
brew install git

brew install tcl-tk
brew link tcl-tk --force
```

Now, check your git version: 
```zsh
git --version
```
For my case:
```git version 2.24.1```

## Set Your Git Identity
Set your name and email in Git configuration.
```zsh
git config --global user.name "Kamrul Hasan"
git config --global user.email mkhrussell@gmail.com
```

## Set Visual Studio Code (VS Code) as your editor for Git
Before setting VS Code as default editor for Git, you have to install Shell Command for VS Code. Follow guide for how to [launch VS Code from the command line](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line).

After installing *code*, set this as git editor using:
```zsh
git config --global core.editor 'code --new-window --wait'
```

## Set VS Code as Merge Tool and Diff Tool for Git

```zsh
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --new-window --wait $MERGED'

git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --new-window --wait --diff $LOCAL $REMOTE'
```

## Check Git Config
Now, check all the configs:

```zsh
git config --list --show-origin
```

Output should be like below:
```
file:/usr/local/etc/gitconfig   credential.helper=osxkeychain
file:/Users/kamrul/.gitconfig   user.name=Kamrul Hasan
file:/Users/kamrul/.gitconfig   user.email=mkhrussell@gmail.com
file:/Users/kamrul/.gitconfig   core.editor=code --new-window --wait
file:/Users/kamrul/.gitconfig   merge.tool=vscode
file:/Users/kamrul/.gitconfig   mergetool.vscode.cmd=code --new-window --wait $MERGED
file:/Users/kamrul/.gitconfig   diff.tool=vscode
file:/Users/kamrul/.gitconfig   difftool.vscode.cmd=code --new-window --wait --diff $LOCAL $REMOTE
```

## Edit Global Git Config
You can also edit the Git config with your editor.

```zsh
git config --global --edit
```
![git-config-edit.png](/assets/images/configure-git-on-macos/git-config-edit.png)

{% capture notice-text %}
1. [Homebrew](https://brew.sh/)
2. [Visual Studio Code on macOS](https://code.visualstudio.com/docs/setup/mac)
3. [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
4. [Using VSCode as git mergetool and difftool](https://medium.com/faun/using-vscode-as-git-mergetool-and-difftool-2e241123abe7)
{% endcapture %}
<div class="notice--info">
  <h4>References:</h4>
  {{ notice-text | markdownify }}
</div>