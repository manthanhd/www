---
id: 44
title: Variables in Linux Shell
date: 2012-02-13T18:16:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/02/13/variables-in-linux-shell-script/
permalink: /2012/02/13/variables-in-linux-shell-script/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/02/variables-in-linux-shell-script.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/2389385571981277318
categories:
  - Findings
tags:
  - linux
  - shell
---
Well, this has been a bit tricky for me so I decided to put it on my blog so that it can be helpful to you guys.

First of all, <b>How to define a variable?</b>

<code> </code>
<code>count=0</code>

There! I've just defined a variable count with 0 as its initial value.
Now, <b>How do you print value of a variable?</b>

<code>echo "$count"</code>

or

<code>echo $count</code>

Next. <b>How do you perform calculations on a variable?</b>
Okay. So assume that you have two variables called val1 and val2 with values 3 and 4 respectively. Now, you want val3 to have the addition of val1 and val2. So,

<code> </code>
<code>val1=3</code>
<code>val2=4</code>
<code>val3=`expr $val1 + $val2`</code>

Similarly, to do subtraction:

<code>val4=`expr $val1 - $val2`</code>


Note that spaces are very important here.

Now, if you wish to assign value returned by a command to a variable, then it works by doing:

<code>wcount=`wc -w myfile.txt`</code>

anything between ` and ` will be executed as linux command and the value returned by that command will be assigned back to the variable to left.