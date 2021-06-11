---
id: 503
title: Getting started with chat bots using Talkify
date: 2016-11-11T12:28:41+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/11/11/496-revision-v1/
permalink: /?p=503
---
Chat bots have always seemed so complex. They process natural language from text so it must be hard right? After all, how can you make sense of loose words into computer instructions and then back? It must be hard.

Well, it is hard. Kinda. A lot has already been solved around natural language processing so the amount you have to do to get started has reduced by significant amount. Tools are already there, you just have to use them.

I used those tools and still found it to be difficult. I wanted to make the process of building chat bots as easy as getting started with web development. So, I built <a href="https://github.com/manthanhd/talkify">Talkify</a>.

Takify is an Open Source framework for building chat bots. It is written in node.js and here's how you can build your very own chat bot in a couple of minutes.

We'll be building a chat bot that I like to call "sidekick". This is a simple bot that tells you knock knock and chuck norris jokes.<!--more-->

The template for this already exists on my <a href="https://github.com/manthanhd/talkify-example-sidekick">GitHub</a> so lets just clone it for now. Assuming you have git command line installed, run:
<pre class="lang:sh decode:true">git clone https://github.com/manthanhd/talkify-example-sidekick.git</pre>
This will create a folder called <span class="lang:default decode:true crayon-inline ">talkify-example-sidekick</span> within your current working directory. Go into that folder using the <span class="lang:sh decode:true crayon-inline ">cd</span> command.

You'll now need to install module dependencies. This should take a couple of seconds depending on your internet connection. Run the following command to tell npm to do this for you.
<pre class="lang:sh decode:true">npm install</pre>
Now if you list your current directory (using <span class="lang:sh decode:true crayon-inline">ls</span> if using linux/mac or <span class="lang:sh decode:true crayon-inline ">dir</span> for windows), you'll notice two JavaScript files. These are <span class="lang:default decode:true crayon-inline">index.js</span> and <span class="lang:default decode:true crayon-inline">cli.js</span>. The <span class="lang:default decode:true crayon-inline ">index.js</span> file contains the main bot code while the <span class="lang:default decode:true crayon-inline ">cli.js</span>  provides an interface to talk the bot via the command line interface. For this tutorial, lets run the <span class="lang:default decode:true crayon-inline ">cli.js</span> file using node.
<pre class="lang:sh decode:true">node cli.js</pre>
This should give you a prompt.
<pre class="lang:sh decode:true">C:\Users\manthanhd\Documents\projects\sidekick&gt;node cli.js
 BOT&gt; Ready.
YOU&gt;</pre>
Give it a try. Ask the bot to tell you a knock knock joke. Or ask it to tell you a Chuck Norris joke. You should get responses like so:
<pre class="lang:sh decode:true">YOU&gt; tell me a knock knock joke
 BOT&gt;  Knock, knock.
 BOT&gt;  Who’s there?
 BOT&gt;  Rufus.
 BOT&gt;  Rufus who?
 BOT&gt;  Rufus the most important part of your house.
YOU&gt; tell me a chuck norris joke
 BOT&gt;  There are only two things that can cut diamonds: other diamonds, and Chuck Norris.</pre>
If you keep asking for knock knock jokes, it will eventually run out. Try it!

Awesome work! You've setup the bot correctly!

Now lets look under the hood inside the <span class="lang:default decode:true crayon-inline ">index.js</span> to better understand whats going on. First couple of lines in this file are a bunch of requires.
<pre class="lang:js decode:true">const knockKnockJokes = require('knock-knock-jokes');
const cnApi = require('chuck-norris-api');

const talkify = require('talkify');</pre>
This is like an "import" statement where its initialising libraries like the <span class="lang:default decode:true crayon-inline">knock-knock-jokes</span>, <span class="lang:default decode:true crayon-inline">chuck-norris-api</span> and <span class="lang:default decode:true crayon-inline">talkify</span>.

The next couple of lines are importing template functions from Talkify for us to use later on in the code.
<pre class="lang:js decode:true ">const Bot = talkify.Bot;

const BotTypes = talkify.BotTypes;

const SingleLineMessage = BotTypes.SingleLineMessage;

const TrainingDocument = BotTypes.TrainingDocument;
const Skill = BotTypes.Skill;</pre>
If the previous require statements were loading libraries, this is equivalent to loading specific things that we will be needing from those libraries.

In the next line we are initialising the bot itself using the previously loaded Bot function.
<pre class="lang:js decode:true ">const bot = new Bot();</pre>
The constructor for Bot can accept an optional configuration object but to keep things simple, we're just going to use default configuration.

It might look like a lot is happening in the following lines but in essence, we're simply training the bot to learn a couple of words and associate them with topics. To do that, we are using the <span class="lang:default decode:true crayon-inline ">trainAll</span> function of the bot and are passing in an array of objects as well as a callback function. Each of these objects are of <span class="lang:default decode:true crayon-inline">TrainingDocument</span> type.
<pre class="lang:js decode:true ">bot.trainAll([
    new TrainingDocument('knock_joke', 'knock'),
    new TrainingDocument('knock_joke', 'knock knock'),

    new TrainingDocument('chuck_norris_joke', 'chuck norris'),
    new TrainingDocument('chuck_norris_joke', 'chuck'),
    new TrainingDocument('chuck_norris_joke', 'norris'),
    new TrainingDocument('chuck_norris_joke', 'chuck norris joke'),
], function () {
    console.log(' BOT&gt; Ready.');
});</pre>
As you can see, each <span class="lang:default decode:true crayon-inline">TrainingDocument</span> object accepts two parameters. The first parameter is the topic name and the second is text that the bot will learn to associate with the previously mentioned topic.

The callback function that is passed to the <span class="lang:default decode:true crayon-inline ">trainAll</span> function is executed when the training for the bot has finished. Here, we're just printing out a message indicating that the bot is ready.

Next, we're defining two skill objects by utilising the <span class="lang:default decode:true crayon-inline ">Skill</span> constructor.
<pre class="lang:js decode:true">const kJokeSkill = new Skill('my_knock_knock_joke_skill', 'knock_joke', function (context, request, response) {
    if (!context.kJokes) {
        context.kJokes = [];
    }

    let newJoke = knockKnockJokes();
    let counter = 0;
    while(counter &lt; 11 &amp;&amp; context.kJokes.indexOf(newJoke) !== -1) {
        newJoke = knockKnockJokes();
        counter++;
    }

    if(counter === 11) {
        return response.send(new SingleLineMessage('Sorry I am out of knock knock jokes. :('));
    }

    context.kJokes.push(newJoke);
    return response.send(new SingleLineMessage(newJoke));
});

const cJokeSkill = new Skill('my_chuck_norris_joke_skill', 'chuck_norris_joke', function(context, request, response) {
    return cnApi.getRandom().then(function(data) {
        return response.send(new SingleLineMessage(data.value.joke));
    });
});</pre>
As you can see, this accepts three parameters, unique name of the skill, topic to associate the skill with and a function to execute as part of that skill.

This function is called an <span class="lang:default decode:true crayon-inline ">apply</span> function and it must accept at least three parameters. These are context, request and response.

The context parameter is the context that the bot loads for you automatically. This is request driven context and is associated with an end user.  Think of this as how a web session works.

The request parameter contains metadata about the request like the actual raw sentence in the request, topic resolution confidence level etc.

The response parameter contains a bunch of methods that you can use to set states and respond to the user. More information on this entire section is available in the <a href="https://github.com/manthanhd/talkify/blob/master/wiki/SKILLS.md">Skills wiki page</a>.

Within the skill functions we are using the knock knock joke and the chuck norris library to get jokes that the bot can respond with.

Next two lines is where we are making the bot aware of the skills by adding them to it.
<pre class="lang:js decode:true">bot.addSkill(kJokeSkill);
bot.addSkill(cJokeSkill);

module.exports = bot;</pre>
The last line exports the bot so that it can be used by other files within this project. We are doing this so that we can separate the bot logic from the logic of getting the raw request from the user and then responding with it. The latter logic in this case can be found from <span class="lang:default decode:true crayon-inline ">cli.js</span> file.

This means that you can quite easily replace the <span class="lang:default decode:true crayon-inline ">cli.js</span> file with a web hook from Facebook messenger, Apple iMessage, Skype or even a web request from an iPhone app that works with Siri to enable voice.

So that's it. That was a quick whistle stop guide to writing your own first chat bot. Hope you enjoyed it.

The project is available on my GitHub page: <a href="https://github.com/manthanhd/talkify">https://github.com/manthanhd/talkify</a>

If you want to request a feature or file a bug, please feel free to raise an issue on the GitHub repository link above. I am also open to contributors so feel free to fork the repository and contribute.

Just for reference, here's the NPM package link: <a href="https://www.npmjs.com/package/talkify">https://www.npmjs.com/package/talkify</a>