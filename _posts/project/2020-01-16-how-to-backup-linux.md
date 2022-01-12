---
layout: post
title: How to backup automatically with Timeshift
category: project
description: 01-16 | never forget backup.
---

Timeshift is a simple application for newer to backup and restore Linux system.

## Install in elementary or Ubuntu

+ sudo apt-add-repository ppa:teejee2008/ppa
+ sudo apt-get update
+ sudo apt-get install timeshift

## Backup

+ I selected RSYNC as snapshot type and chose to include /user and /root directories.

+ You can also use other filters to select what you want to backup.

## Restore

+ Available snapshots will be shown in window.

+ Timeshift will create auto-snapshots every 5 days by default, if you want to, you can also create backups manually.
