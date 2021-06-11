---
id: 758
title: Bulk edit filenames using shell
date: 2018-08-08T14:18:02+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2018/08/08/751-revision-v1/
permalink: /?p=758
---
<p>Unix/linux command to remove specific matching text from all files. In this case, I had a list of tiles that had <code>-defaults.properties</code> suffix. I wanted to rename them all so that they had just <code>.properties</code> suffix instead. For example, if a file was named <code>connection-defaults.properties</code>, I wanted it to be renamed to <code>connection.properties</code>. </p>
<p>As usual, rather than wasting my time and doing it manually 100 times, I set myself to find an automated way of doing it. And sure enough, I found it!</p>
<p>Here it is:</p>

<!-- wp:code -->
<pre class="wp-block-code"><code>for f in *-defaults.properties; do mv "$f" "$(echo "$f" | sed s/-defaults//)"; done</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The above command finds all files that end with <code>-defaults.properties</code>. For each file that match that criteria, we then rename it to a name that has <code>-defaults</code> stripped out. To strip out the name, we use <code>sed</code> command and to rename we use <code>mv</code> command.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Tested on mac running <a href="https://ohmyz.sh/">oh-my-zsh</a>.</p>
<!-- /wp:paragraph -->