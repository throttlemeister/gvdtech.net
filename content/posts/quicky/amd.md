---
title: "AMD + KDE Crashes"
author: throttlemeister
date: 2025-11-06T12:58:00
tags: [ linux, AMD, kwin, firefox ]
draft: false
---
## AMD + KDE Crashes

For a while I had some issues on KDE. If firefox was running, and my display went to sleep kwin and firefox would crash upon waking up. Bit annoying. Even when firefox was not running, at times crashes would happen when waking up the screen.

After updating to KDE 6.5.1, this got worse to the point of almost everytime the system would hang or crash. It appears these sort of issues are not uncommon on systems with an AMD graphics card.

## Possible solution

I have found a possible solution by adding `amdgpu.dc=1` to the kernel boot parameters. It has been a day (as I am writing this) and no crash has happened since. Not even while leaving firefox open. Time will have to tell if it is indeed related to this setting.
