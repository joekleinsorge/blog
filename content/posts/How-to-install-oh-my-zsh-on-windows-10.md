---
title: 'How to Install Oh My Zsh on Windows 10'
date: 2021-11-22T21:04:41-05:00
draft: false
toc: true
cover: img/OMZLogo_BnW.png
tags:
  - zsh
  - windows terminal
---

## Why do I want this?

[Oh My Zsh](https://ohmyz.sh/) will make you look cool to everyone you share your desktop with! More practically, it has a bunch of features and improvements over Bash, here are some of the major ones:

- Automatic cd: Just type the name of the directory
- Recursive path expansion: For example “/u/lo/b” expands to “/usr/local/bin”
- Spelling correction and approximate completion: If you make a minor mistake typing a directory name, ZSH will fix it for you
- Plugin and theme support: ZSH includes many different plugin frameworks

## Steps

### Install Windows Terminal

This has been a favorite of mine for the last couple years. The multiple tabs along with the ability to handle multiple types of shells and command line tools make it the best way to use the CLI in Windows.

The easiest way to install it is from the built in Microsoft Store, just search for it with "Windows Terminal"

{{< image src="/img/windowsTerminal.JPG" alt="Windows Terminal Screenshot" style="border-radius: 8px;" >}}

### Install Ubuntu WSL

Just like the Windows Terminal, the easiest way to install the Ubuntu WSL is through the Microsoft Store.

{{< image src="/img/ubuntu.JPG" alt="Ubuntu WSL Screenshot" style="border-radius: 8px;" >}}
If you see "x packages can be upgraded. Run 'apt list --upgradable' to see them."

### Update Ubuntu

You should now see Ubuntu under Profiles in the Windows Terminal.
{{< image src="/img/terminalProfile.JPG" alt="Terminal Profile Screenshot" style="border-radius: 8px;" >}}

In the shell, get the latest updates

```shell
sudo apt-get update
```

### Install zsh

```shell
sudo apt install zsh
```

### Install Oh My Zsh

Install Oh My Zsh with the following command:

```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Once it's done, close and re-open the Ubuntu WSL terminal and you'll be greeted with a prettier shell.

{{< image src="/img/zsh.JPG" alt="zsh Screenshot" style="border-radius: 8px;" >}}

### Customize it (optional)

If you feel like customizing your shell some more, head over [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) and find a theme you like.

To change it to the one you want, open up the Zsh config file.

```shell
vi ~/.zshrc
```

Find `ZSH_THEME="robbyrussel"` and replace it with the theme of your choice.

## Conclusion

Thanks for reading, enjoy Oh My Zsh and start exploring all the available [plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)!
