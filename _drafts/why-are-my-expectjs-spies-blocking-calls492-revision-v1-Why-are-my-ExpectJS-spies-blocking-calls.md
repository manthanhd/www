---
id: 494
title: Why are my ExpectJS spies blocking calls?
date: 2016-10-19T21:30:55+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/10/19/492-revision-v1/
permalink: /?p=494
---
While I love writing tests in JavaScript, it is sometimes incredibly painful to debug through the asynchronous test code. Today, a weirdness with ExpectJS happened. I had the following test code:
<pre class="lang:js decode:true">var contextStore = {
    put: function (id, context, callback) {
        return callback(undefined, context);
    },

    get: function (id, callback) {
        return callback(undefined, undefined);
    }
};

var contextStore_putSpy = expect.spyOn(contextStore, 'put');
var contextStore_getSpy = expect.spyOn(contextStore, 'get');

var bot = new Bot({contextStore: contextStore});

return bot.resolve(123, "Hello. Hi", function (err, messages) {
    if (err) return done(err);

    expect(contextStore_putSpy).toHaveBeenCalled();
    expect(contextStore_getSpy).toHaveBeenCalled();
    return done();
});</pre>
Simple right? Nope. Some how my callback was not being called which was preventing the test from finishing which in turn caused a test failure. I spent good amount of time debugging step by step and finally found the issue.

My fake contextStore was not being called. Hmm. How can this happen? I've defined functions that do respond asynchronously and have set spies on them as well. If anything, my spies should tell me that the function has been called!

Nothing could be further away from the truth! Well, you see, spies work differently in Java than in JavaScript. While using Mockito with Java, I've found that Spies never block the calls on the object that they are spying on unless explicitly told to. However, this behaviour different in JavaScript. ExpectJS blocks the calls to spies unless told otherwise!

At the end, solution was simple. I changed lines 11 and 12 to:
<pre class="lang:js decode:true ">var contextStore_putSpy = expect.spyOn(contextStore, 'put').andCallThrough();
var contextStore_getSpy = expect.spyOn(contextStore, 'get').andCallThrough();</pre>
Boom! And it worked!