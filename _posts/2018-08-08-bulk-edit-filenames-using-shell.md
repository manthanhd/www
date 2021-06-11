---
id: 751
title: Bulk edit filenames using shell
date: 2018-08-08T14:18:02+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=751
permalink: /2018/08/08/bulk-edit-filenames-using-shell/
categories:
  - Findings
tags:
  - automation
  - linux
  - shell
  - unix
---
for f in *-defaults.properties; do mv "$f" "$(echo "$f" | sed s/-defaults//)"; done