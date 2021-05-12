---
title: "Switching to Zsh and Oh-My-Zsh"
categories:
  - blog
tags:
  - Shell
  - Zsh
toc: true
---

Generally speaking, Zsh is a more powerful shell than Bash. 
It enables multiple features, customizations that are not possible
on Bash. 

However, given the difficulty set Zsh up, there's an alternative way
to configure your Zsh by using Oh-My-Zsh, a framework for Zsh.

## Install Zsh

In newer Mac OS systems, Zsh is included in the system already.
Run `zsh  --version` to confirm. Expected result is `zsh 5.4.2` or higher.

Otherwise, use `brew install zsh`.


## Swith default shell

```console
echo $SHELL # check current shell

#switch to zsh if current shell is bash
chsh -s $(which zsh)
```

## Install Oh-my-Zsh

```console
# install wget if it's not installed already
# brew install wget

# install OMZ
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Configure your zsh profile

All of the below configuration is done in `~/.zshrc` file. Open it with any editor of your choice to edit it.

### Migrate from Bash

There might be some variables defined in `~/.bash_profile`. In order to use these variables, you have two choices.

- Simply copy and paste the definition to the bottom of your `~/zshrc` file
- Add `source ~/.bash_profile` at the end of your `~/zshrc` file


### Plugins

Give your shortcuts or customized configuration for specific use, see [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) for more information on
zsh plugins.

```zsh
# ~/.zshrc

plugins = {git ruby} #use space/tabs/newline to seperate
```

### Themes

Pick your theme from [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes). The default theme is `rubbyrussel`.
```zsh
ZSH_THEME ="af-magic"
```

### Others

Check zshrc file for more custimization such as auto-updating.


