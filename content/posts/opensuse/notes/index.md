+++
title = 'Quick reference notes'
date = 2024-11-30T22:58:17+01:00
draft = true
tags = ["opensuse", "linux", "notes"]
+++
## Introduction
This is a page where I store quick notes and commands I sometimes need but always forget and end up having to search for on the web, which is not the most economic way to spend my time. They are generally not very complicated, but not used quite enough to always memorize. Me lazy..

This is a living document. Things will be added as I go.

## Update-alternatives
Installing neovim on OpenSUSE does not make it recognized automatically as an alternative for vi. We will need to add it manually for it to be used and set by default.

    update-alternatives --install /usr/bin/vi vi /usr/bin/nvim 110
