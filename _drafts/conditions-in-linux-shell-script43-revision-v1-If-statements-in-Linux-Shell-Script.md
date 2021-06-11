---
id: 178
title: If statements in Linux Shell Script
date: 2015-08-25T23:15:49+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/43-revision-v1/
permalink: /?p=178
---
If statements are important in any programming language. I had a bit of hard time while doing if statements in linux since they are different than those in a conventional programming language. First of all, an if statement to compare whether a value in a variable val1 is equal to 5 is like:
<code>val1=5</code>
<code>if [ $val1 -eq 5 ]</code>
<code>then</code>
<code>     <i>Code here to do some stuff</i></code>
<code>else</code>
<code>     <i>If above condition is false, then code here gets executed</i></code>
<code>fi</code>

Note that <b>spaces are important </b>when defining condition. $val1 refers to the variable and -eq simply implies equal to operator to compare if the $val1 is equal to 5.

Seems pretty easy? It is. Other conditional operators are:
<table border="1">
<tbody>
<tr align="center">
<td><b>Conditional Operator</b></td>
<td><b>Linux Equivalent</b></td>
</tr>
<tr>
<td>Greater than</td>
<td>-gt</td>
</tr>
<tr>
<td>Less than</td>
<td>-lt</td>
</tr>
<tr>
<td>Equal to</td>
<td>-eq</td>
</tr>
<tr>
<td>Not equal to</td>
<td>-ne</td>
</tr>
</tbody>
</table>
Now, if I want to compare strings, for instance, if variable answer is equal to "yes" then the code will be as follows:
<code>if [ "$val" = "yes" ]</code>
<code>then</code>
<code>     <i>Code here to do some stuff</i></code>
<code>fi</code>

Again, <b>spaces are important </b>here.
In here, '=' can easily be replaced by conventional conditional operators like '&gt;' or '&lt;' or '!=' etc.