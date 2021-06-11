---
id: 541
title: 'CodeEval: Fizz Buzz (Java)'
date: 2017-01-02T08:32:16+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/01/02/539-revision-v1/
permalink: /?p=541
---
Finally managed to get some spare time in order to do this. Helped be clear my head a bit. Here's my quick 1 minute solution to CodeEval's fizz buzz problem in Java:
<pre class="lang:java decode:true ">/* Sample code to read in test cases:*/
import java.io.*;
public class Main {
    public static void main (String[] args) throws IOException {
        File file = new File(args[0]);
        BufferedReader buffer = new BufferedReader(new FileReader(file));
        String line;
        while ((line = buffer.readLine()) != null) {
            line = line.trim();
            final String[] inputStringArray = line.split(" ");
            final int divisor1 = Integer.parseInt(inputStringArray[0]);
            final int divisor2 = Integer.parseInt(inputStringArray[1]);
            final int countingLength = Integer.parseInt(inputStringArray[2]);
            
            for(int i = 1; i &lt;= countingLength; i++) {
                final StringBuilder responseBuilder = new StringBuilder();
                if(i % divisor1 == 0) responseBuilder.append("F");
                if(i % divisor2 == 0) responseBuilder.append("B");
                if(responseBuilder.length() == 0) responseBuilder.append(i);
                System.out.print(responseBuilder.toString() + " ");
            }
            
            System.out.println();
        }
    }
}</pre>
<!--more-->Upon submitting, CodeEval marked my solution as unique (go figure!) which was a bit weird. However, I thought I could do better. So, here's a second approach using lambdas:
<pre class="lang:java decode:true ">/* Sample code to read in test cases:*/
import java.io.*;
import java.util.Arrays;

public class Main {
    public static void main (String[] args) throws IOException {
        final File file = new File(args[0]);
        final BufferedReader buffer = new BufferedReader(new FileReader(file));
        String line;
        while ((line = buffer.readLine()) != null) {
            line = line.trim();
            final int[] inputs = Arrays.stream(line.split(" ")).mapToInt(Integer::parseInt).toArray();
            final int divisor1 = inputs[0];
            final int divisor2 = inputs[1];
            final int countingLength = inputs[2];
            
            for(int i = 1; i &lt;= countingLength; i++) {
                final StringBuilder responseBuilder = new StringBuilder();
                if(i % divisor1 == 0) responseBuilder.append("F");
                if(i % divisor2 == 0) responseBuilder.append("B");
                if(responseBuilder.length() == 0) responseBuilder.append(i);
                
                responseBuilder.append(" ");
                System.out.print(responseBuilder.toString());
            }
            
            System.out.println();
        }
    }
}</pre>
This takes slightly longer (3-5 ms) and uses more memory.