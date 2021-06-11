---
id: 28
title: Setting up C++ build system on Linux
date: 2013-05-01T18:20:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/05/01/compiling-c-easily-on-linux-easy-c-build-system/
permalink: /2013/05/01/compiling-c-easily-on-linux-easy-c-build-system/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/05/compiling-c-easily-on-linux-easy-c.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/867719114871805812
categories:
  - Findings
tags:
  - build system
  - c
  - linux
---
I was developing some C++ stuff on my Linux virtual machine and wanted to create something that would make it easy for me to compile C++ code. So I wrote up some bash code and added it into .bashrc as a function. Here's the code:
<pre>function cpprun(){
echo "Checking if output directory exists..."
if [ ! -d "./output" ]; then
echo "Creating output directory..."
mkdir "./output"
echo "Output directory created."
elif [ -f "./output/cpprun.o" ]; then
echo "Backing up previous output..."
mv ./output/cpprun.o ./output/cpprun.bak.o
echo "Backup finished."
fi

echo "Running g++ compile..."
g++ -o ./output/cpprun.o *.cpp
if [ -f "./output/cpprun.o" ]; then
echo "Compile finished."
echo "Running output..."
echo "-----------------"
./output/cpprun.o
echo "-----------------"
echo "Run finished."
else
echo "Compile failed."
fi
}
</pre>
<div></div>
<div>Add this to your .bashrc script and then you should be able to compile your C++ code by typing cpprun in the base directory.</div>