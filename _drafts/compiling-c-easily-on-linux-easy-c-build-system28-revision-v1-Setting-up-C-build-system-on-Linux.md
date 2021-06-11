---
id: 154
title: Setting up C++ build system on Linux
date: 2015-08-25T22:47:49+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/28-revision-v1/
permalink: /?p=154
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