---
id: 835
title: 'Who&#8217;s using my macbook camera?'
date: 2020-08-03T11:01:27+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2020/08/03/834-revision-v1/
permalink: /?p=835
---
<!-- wp:paragraph -->
<p>So the other day I was working away on my Macbook Pro and suddenly, out of the blue, the light next to my camera came on. Now, this is a new macbook so I don't have my little sliding window sticker that blocks my webcam yet. Even if I did have it, I'd still be equally worried. So I started digging. The only apps I had open at the time were:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Firefox</li><li>BlueJeans (video conferencing software)</li><li>Intellij IDEA</li><li>Visual Studio Code</li><li>iTerm</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Basically the Software Engineer's toolkit.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>After a little digging on the Internet, I found a few commands that could help me. I have conveniently combined them all into this single command:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>lsof | grep -e "AppleCamera" -e "iSight" -e "VDC"</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This gave  me the following:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>➜  ~ lsof | grep -e "AppleCamera" -e "iSight" -e "VDC"
firefox   1956 mdave  txt       REG                1,5      424176 1152921500312438057 /System/Library/Frameworks/CoreMediaIO.framework/Versions/A/Resources/VDC.plugin/Contents/MacOS/VDC</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Hmm so its Firefox? Weird because usually its very good with permissions. Most of my tabs were DuckDuckGo, StackOverflow, Jira, Confluence, Gmail and a few other "trusty" company web pages. This couldn't be it?</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I did some digging around in settings but no luck. Finally, I brought down the hammer and took away the permissions to Camera (and Microphone for safety) from the System Preferences. And viola!</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>➜  ~ lsof | grep -e "AppleCamera" -e "iSight" -e "VDC"
firefox   1956 mdave  txt       REG                1,5      424176 1152921500312438057 /System/Library/Frameworks/CoreMediaIO.framework/Versions/A/Resources/VDC.plugin/Contents/MacOS/VDC
➜  ~ lsof | grep -e "AppleCamera" -e "iSight" -e "VDC"
➜  ~</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>No more naughty webcam access! I guess if I need it again, I'll just give it permissions explicitly through the System Preferences. Or perhaps I can side-install another firefox browser when I need to grant it permissions to view my webcam - the number of websites that I use that need this permission are very small in number and I can live with using a dedicated web browser for it.</p>
<!-- /wp:paragraph -->