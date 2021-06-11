---
id: 163
title: 'Displaying tabular data in Visual C# using ListView'
date: 2015-08-25T22:57:34+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/53-revision-v1/
permalink: /?p=163
---
ListView is extremely useful in displaying tabular data in C#. Yet, it is an easy to use control.
<ol>
	<li>Drag the control onto your form and note its name. In this instance, I am going to use ListView1 as name.</li>
	<li>You will have to add columns to it now. This is how you do it.
<code>ListView1.Columns.Add("ColumnName");</code>
If you have 5 columns, then you will have to do it five times.</li>
	<li>Now, you will have to have each row that you want to insert as a single dimension string array. Number of items in this array should be equal to number of added columns. If it's not, then rest of string elements will not show up.</li>
	<li>Add the string array to a ListViewItem. For that, you will have to create a ListViewItem first. ListViewItem's constructor would allow you to pass the string array to it. Do it by typing:
<code>ListViewItem li = new ListViewItem(stringarray);</code></li>
	<li>Now add the ListViewItem to the ListView by typing:
<code>ListView1.Items.Add(li);</code></li>
</ol>
That's it! Run your application and you'll have your table beautifully displayed in tabular format.