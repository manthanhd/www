---
id: 629
title: 'Fixing ubuntu desktop&#8217;s wifi issues on wake'
date: 2017-05-11T06:28:24+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/05/11/628-revision-v1/
permalink: /?p=629
---
For about more than half the time, when I wake my ubuntu laptop from sleep, I will lose wifi. When I say lose, I mean I lose the <code>wlan0</code> interface completely.

Now this is a problem because I quite like having wifi and normally there seems to be no way to turn it back on from that little wifi menu from the top right corner.

After doing some googling, I found a crude yet simple solution. Restart the <code>network-manager</code> service by running the following:

```
sudo service network-manager restart
```

&nbsp;