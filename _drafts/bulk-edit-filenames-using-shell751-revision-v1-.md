---
id: 752
date: 2018-07-12T01:18:03+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2018/07/12/751-revision-v1/
permalink: /?p=752
---
for f in *-defaults.properties; do mv "$f" "$(echo "$f" | sed s/-defaults//)"; done