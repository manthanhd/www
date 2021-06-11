---
id: 669
title: Generating random bytes in Java
date: 2017-09-01T08:06:27+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/09/01/667-revision-v1/
permalink: /?p=669
---
I recently needed to generate a bit of randomness in Java in order to produce a secret. Java comes built in with <code>Random</code> and <code>SecureRandom</code> classes which can help you do this properly but as with all things, there are multiple ways of doing things.

Two of these stood out to me.

<strong>Generate a long random number as <code>String</code>, convert to hex and then convert it to bytes.</strong>
<pre class="lang:java decode:true">SecureRandom random = new SecureRandom();
byte[] randomBytes = Long.toHexString(random.nextLong()).getBytes();</pre>
You could potentially improve this method by not just converting the long number to hex string but also maybe base64 encoding it. You could potentially go further by adding additional entropy to it by filling in random bytes in random indices.

But as it stands above, the benefit of this method is that it is very quick to run. This may vary based on your random seed but I have found the above method to consistently produce byte arrays of size 14-16. This might be good enough in most cases - especially due to the fact that its fast, but in times where you might need very high amounts of entropy, the second approach might suit you.

My personal discontent with this method is that the bytes it produces will be chunked by the length of each individual hex character due to the fact that it is coming from a hex string. This, arguably is not very random. Arguably because although every individual hex characters are themselves in random order, the bytes generated off of those will have identifiable chunks representing each hex character. The second approach resolves this problem in a simpler way.

Another issue I have with this approach is that the total size is limited to the maximum value a Long can support (2<sup>63</sup>-1). The size of Long data structure limits the length of hex string that gets generated which in turn limits total number of bytes produced.

The approach below resolves most of these issues.

<strong>Generate a array of random size composed of bytes and then let <code>SecureRandom</code> fill in those bytes.</strong>
<pre class="lang:java decode:true">SecureRandom random = new SecureRandom();
int byteArraySize = Math.abs(random.nextInt());
byte[] randomBytes = new byte[byteArraySize];
random.nextBytes(randomBytes);</pre>
Here, we're creating a byte array of random size and then using secure random to fill that byte array in with random bytes. Its simple and elegant. However, as with all things, this has pros and cons of its own.

In my experiments, I have found the range of the byte array to be truly random. In a test that I ran, once it created a byte array of size 8890 while in another time, it created an array so large, I lost my patience and had to quit the process.

This poses a problem where your program could take a long time to generate your secret. Furthermore, it could potentially even go out of stack memory if the array becomes too large.

You can resolve this issue by setting a bound to the <code>random.nextInt</code> which is being used to determine the size of the byte array. I cannot tell you what size here is most optimal because it really depends on the capabilities of the processor you're running on, your stack size as well as the algorithm you're using to initialise <code>SecureRandom</code>. An implementation limiting the size of the byte array may look like following:
<pre class="lang:java decode:true">SecureRandom random = new SecureRandom();
int byteArraySize = Math.abs(random.nextInt(20));
byte[] randomBytes = new byte[byteArraySize];
random.nextBytes(randomBytes);</pre>
Here the size of the byte array will be between 0 and 20.

Also, please note that without the <code>Math.abs</code> the value for <code>byteArraySize</code> could be negative. If you initialise your array with a negative number for size, you will get <code>NegativeArraySizeException</code>.