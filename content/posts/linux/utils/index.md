---
title: "Top Utilities and Tools"
date: 2024-11-09
draft: false
description: "Utilities I use on Linux"
tags: ["utilities", "linux", "terminal"]
---
## Introduction
Over time, I have collected or installed a lot of tools and utilities that make my life in the terminal a little easier. Some help me visualize information on the terminal, others just simply make life easier by letting me achieve my goal quicker. Whatever it is, most of these I could not live without anymore. So here is a list of utilities I use and love.

## Fish
Fish, the friendly shell. This has to come first. For most of the tools after this, I created little fish functions to execute them as I want, and I will include them in their respective sections as well.

Fish allows for easy scripting of complex functions. It is extremely functional right out of the box. It has an abbreviation system that lets you quickly and intuitively shorten often used commands so you don't have to type them out each and every time. It even has a web-based configuration utility that you can use to graphically configure it to your liking.

But for me, the ease with which you can write your own functions and extend its functionality if king here.

Homepage: [Fish](https://fishshell.com/)

## eza
eza is a modern, maintained replacement for the venerable file-listing command-line program ls that ships with Unix and Linux operating systems, giving it more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git. And it’s small, fast, and just one single binary.

By deliberately making some decisions differently, eza attempts to be a more featureful, more user-friendly version of ls.

The way I use it in fish:

    function ls -d 'eza instead of ls'
      if type --quiet eza && test "$argv[1]" != "-ltr"
        eza --header --group-directories-first --git --icons=auto $argv
      else
        command ls --color=always $argv
      end
    end

    function ll
      if command -sq eza
        ls -laa -g $argv
      else
        command ls -la $argv
      end
    end

    function l
      ls $argv
    end

    function lt
       if command -sq eza
         ls -laa -snew -g $argv;
       else
         command ls -ltr $argv;
       end
     end

Homepage: [eza](https://github.com/eza-community/eza)

## ripgrep
Ripgrep is a replacement for the standard `grep` command. It can search recursively, respects `.gitignore` and skips hidden files or directories and binary files.

    # Defined in - @ line 1
    #
    # Let's check if we have ripgrep installed and if so
    # we use it, otherwise use standard grep with options
    function grep
      if command -sq rg
        rg  $argv;
      else
        command grep -n --color $argv;
      end
    end

Homepage: [Ripgrep](https://github.com/BurntSushi/ripgrep)

## bat
Bat is a, as the homepage tell you themselves, a "cat clone with wings". It supports syntax highlighting, it shows linenumbers, it has git integration, it shows non-printable characters and it automatically paginates the output. This makes it a lot more usable and readable than the regular cat, which just outputs flat ascii.

    function cat -d 'bat instead of cat'
      if type --quiet bat
        bat $argv
      else
        command cat $argv
      end
    end

Homepage: [Bat](https://github.com/sharkdp/bat)

## zoxide
Zoxide is a smarter `cd` command and my new best friend. It takes inspiration from z and autojump and the one cool thing it does is that it keeps a database of the directories you visit. You can then change directory by just typing the directory or part thereof, and it jumps right in no matter how deep or your current location. Also a simple `cd -` jumps you back to where you came from, making switching back and forth really quick. I don't think I can live without this little utility anymore.

    function cd -d 'Using zoxide as replacement for cd, if it exists'
        if command -sq zoxide
            z $argv
        else
            command cd $argv
        end
    end

Homepage: [zoxide](https://github.com/ajeetdsouza/zoxide?tab=readme-ov-file)

## advcp & advmv
Advanced Copy is a mod for the GNU cp and GNU mv tools which adds a progress bar and provides some info on what's going on. There is no real difference to the standard cp and mv commands, but I do very much appreciate that it gives feedback on progress. Why isn't this included by default?

In my fish config:

    function cp
      if command -sq advcp
        advcp -g $argv
      else
        command cp $argv
      end
    end

(same for mv)

Homepage: [advcp/mv](https://github.com/jarun/advcpmv)

## stow
From their homepage:
> GNU Stow is a symlink farm manager which takes distinct packages of software and/or data located in separate directories on the filesystem, and makes them appear to be installed in the same place. For example, /usr/local/bin could contain symlinks to files within /usr/local/stow/emacs/bin, /usr/local/stow/perl/bin etc., and likewise recursively for any other subdirectories such as .../share, .../man, and so on.

I like and use it to manage my dotfiles in a simple, intuitive way. I actually wrote an article about it, which you can read [here](https://www.crashdot.com/posts/linux/dotfiles/). 

    # Function for stow, to ingore .directory files and use 'dotfiles' special handling functionality
    #
    function dotf -d "Use stow with extra parameters"
        if command -sq stow
            stow -d ~/.dotfiles/ $argv --ignore=.directory --ignore=README.md --dotfiles
        else
            echo "Stow not installed. Please install before using."
        end
    end

Homepage: [GNU stow](https://www.gnu.org/software/stow/)

## fastfetch
Admit it, everybody loves neofetch. However, neofetch is no longer maintained and fastfetch does the same thing. Except it is written in C and as such it is blistering fast. It is also highly configurabla, just like its cousin.

    function ff -d 'fastfetch shortcut'
        if type --quiet fastfetch
            if test -n "$ALACRITTY_WINDOW_ID"
                fastfetch -l opensuse -c examples/6.jsonc $argv
            else
                fastfetch -l ~/ansible/files/twgrey.png --logo-type iterm --logo-padding-top 2 --logo-width 45 -c examples/6.jsonc $argv
            end
        else
            command neofetch $argv
        end
    end

I do a check on alacritty there, as it does not seem to like me using an image file for the distro logo. So I revert to a regular ASCII logo if it detects alacritty as the terminal emulator.

Homepage: [fastfetch](https://github.com/fastfetch-cli/fastfetch)

## neovim
Neovim is a hyperextensible vim-based editor. It is a drop-in for replacement for vim and is customizable to the extreme. I use it in combination with lazyvim, which is a plugin manager and allows for easy configuration and customization.

    function vi --wraps="neovim to vi"
        if command -sq nvim
            nvim $argv
        else
            command vi $argv
        end
    end

Homepage: [neovim](https://neovim.io/)
Homepage: [LazyVim](https://www.lazyvim.org/)
