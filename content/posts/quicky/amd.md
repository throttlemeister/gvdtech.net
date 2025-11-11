---
title: "Firefox high CPU"
date: 2025-11-06T12:58:00+02:00
draft: false
tags: [ linux,  firefox ]
categories: [ linux, firefox ]
author: throttlemeister
---
# High CPU with firefox while using a custom new-tab page extension

## Introduction
Recently I started experiencing high CPU from firefox on my linux system. This is annoying, as it makes the fans spin up, and laptop run hot and performance to degrade. Eventually I narrowed the problem down to me using an extension to load a custom new-tab page, as I want to load my dashboard when opening firefox or create a new tab. Disabling this extension (and I have tried multiple) made the problem go away.

However, this left me with either a blank page or the firefox home for each new tab I open, not my nice dashboard.

This is how I fixed it.

## Solution 
After some digging on the web, I found the solution: we're going to be using the autoconfiguration feature built into firefox to allow companies to make custom deployments.

### Step 1 
Go to the directory in which your system has installed the binary. On my linux system this is `/usr/lib64/firefox/`. This may vary on your distribution or operating system. You can find where it is on your system by clicking the hamburger menu and going to 'Help' and then 'More troubleshooting information'

### Step 2 
From this directory navigate to the `defaults/pref` subdirectory and create a file called `autoconfig.js` with the following content:
```js
//
pref("general.config.filename", "autoconfig.cfg");
pref("general.config.obscure_value", 0);
pref("general.config.sandbox_enabled", false);
```

### Step 3 
Navigate back up to the installation directory (`cd ../../`) and create another file called `autoconfig.cfg` with the following content:

```
// IMPORTANT: Start your code on the 2nd line
try {
  // Use AboutNewTab.sys.mjs for recent Firefox versions (like v136+)
  ChromeUtils.importESModule("resource:///modules/AboutNewTab.sys.mjs").AboutNewTab.newTabURL = "https://MY_URL/";
} catch (e) {
  // Fallback for older versions (pre-v136)
  // var {classes:Cc,interfaces:Ci,utils:Cu} = Components;
  // Cu.import("resource:///modules/AboutNewTab.jsm");
  // AboutNewTab.newTabURL = "https://www.example.com/";
}
```

Change `MY_URL` to whatever page it is you want to show when opening a new tab. This can also be a local file, but make sure you prepend with `file://` instead of `https`.

### Step 4
If you done all this, all that is basically left is to close your firefox and open it again to activate the changes. If you didn't make any mistakes or typos, opning a new tab will show you your personalised url or file without having to use an extension.

## Final thoughts 
This solution works very well and saves you from having to deal with extensions bahaving badly. At the same time it is a bit of a hack, and I suggest you keep track of changes you make like this in case you ever need to troubleshoot firefox.

One final thing I want to mention, not using an extension to configure a custom new-tab page did make firefox operate noticeably faster. Keep your extensions down to what you actually need; it does help.
