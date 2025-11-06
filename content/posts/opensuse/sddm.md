---
title: "Sddm"
date: 2025-10-06T20:04:30+02:00
draft: false
description: "SDDM on Wayland"
tags: [ linux, opensuse, wayland ]
categories: [ linux, opensuse ]
author: throttlemeister
---
# Running SDDM on Wayland

## Introduction

By default, on OpenSUSE, SDDM runs on Xorg and as root. As Xorg is not the safest option anymore today, it is better to run SDDM on Wayland and not having to deal with the potential security risks. This should article describes how to disable SDDM on Xorg and start it using Wayland.

## Creating the necessary configuration file

Location: `/etc/sddm.conf.d/`

Create the file `10-wayland.conf` with the following content:

```
[General]
DisplayServer=wayland
```

## Stop and disable the default displaymanager script

```shell
systemctl stop display-manager-legacy
systemctl disable display-manager-legacy
```

> [!NOTE]
> You need to do this from a tty, so switch to it using `CTRL-ALT-F3`. If you don't, stopping the displaymanager will likely kill your session.. :)

## Enable and start sddm

```shell
systemctl enable sddm
systemctl start sddm
```

> [!NOTE]
> Same as above; do this from a tty. When you start, you will immediately dropped back into the GUI and see the normal graphical login screen

## Conclusion

Now you should be running SDDM under Wayland.
