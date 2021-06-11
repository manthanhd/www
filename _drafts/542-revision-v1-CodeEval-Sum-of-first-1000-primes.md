---
id: 543
title: 'CodeEval: Sum of first 1000 primes'
date: 2017-01-02T08:53:22+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/01/02/542-revision-v1/
permalink: /?p=543
---
Sum of first 1000 primes
<pre class="lang:java decode:true">/* Sample code to read in test cases:*/
import java.io.*;
public class Main {
    private static final int MAX_PRIME_COUNT = 1000;
    public static void main (String[] args) throws IOException {
        int primeSum = 2;
        int primeCount = 1;
        int number = 3;
        while(primeCount &lt; MAX_PRIME_COUNT) {
            boolean prime = true;
            for(int i = 2; i &lt; number; i++) {
                if(number % i == 0) prime = false;
            }
            
            if(prime){
              primeSum += number;
              primeCount++;
            }
            
            number++;
        }
        System.out.println(primeSum + "");
    }
}
</pre>
&nbsp;

&nbsp;