---
title:  "A neovim config in 1 file"
layout: post
categories: media
---

Previously, I used LunarVim a lot, but it had a big install process and I would always run into errors with different versions of neovim and plugin updates.
I saw some starter templates for making your own config but they all had a bunch of files (usually 10+) and a lot of code.

Then I read [this article about making a config in 1 file]([url](https://techne98.com/blog/single-file-neovim-config/)) and started working off that.

At this point I've implemented every feature (that I used) from LunarVim, and I'm on to adding more. I've noticed that the less custom code you have, the more resilient your config is to plugin updates (since you're mostly using defaults anyways).
[The file is 196 lines as of Jan 2026.]([url](https://github.com/wang-edward/onefile-nvim/blob/main/init.lua))

<img width="1028" height="1117" alt="Screenshot 2026-01-28 at 12 00 01 AM" src="https://github.com/user-attachments/assets/6e5a4066-f88c-4932-a483-a45f6738e818" />
