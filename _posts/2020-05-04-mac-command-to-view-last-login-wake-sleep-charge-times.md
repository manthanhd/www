---
id: 809
title: Mac command to view last login/wake/sleep/charge times
date: 2020-05-04T10:06:40+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=809
permalink: /2020/05/04/mac-command-to-view-last-login-wake-sleep-charge-times/
categories:
  - Findings
tags:
  - bash
  - command
  - macos
  - shell
format: status
---
<!-- wp:paragraph -->
<p>The pmset utility on mac is quite useful for viewing system logs. If you type:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>pmset -g log</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>you'll be able to see some cool system logs. However, as a timesaver, here's a no-bullshit command to view the following times:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>When mac went into sleep</li><li>When mac came out of sleep</li><li>Charge levels</li><li>Standby times</li></ul>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>pmset -g log|grep -e " Sleep  " -e " Wake  "</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Enjoy!</p>
<!-- /wp:paragraph -->