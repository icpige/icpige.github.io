---
title: The ops team
category: Operations
author: tim
image:
image_featured: false
permalink: /operations/2014/12/03/this-is-operations.html
---

Hi I’m Tim, the guy keeping all the servers running. I quietly joined the team a couple of months ago and although I can’t point to anything and go ‘I made that’, I have been working hard on improving things behind the scenes. Someone needs to keep things chugging along while everyone is working on the main event:

>Behind the curtain  
>hearing the muffled applause  
>the ops team smiles  

![Makerbot](/images/blog/operations/makerbot.jpg)

While this series won’t be quite as exciting, especially graphically, as the other blog posts, the underlying system can be elegant in its own way. I am going to dive in to our infrastructure in future posts but I thought I would start off with something a bit more abstract.

There is often an endless list of features on your todo list. Major items that need to be started, ideas from customer feedback, polish on what's already there or even things that would just be cool to try. To decide what to work on it is great to have a vision statement to work from, and we’ve got one over in [the about page](/about/) which is awesome. It can also help to try and distil your area into one word, ‘what am I aiming for with these features’ . I asked the guys what their one word was and got ‘easy’, ‘magical’ and ‘editing’. So continue to expect magically easy editing.

Down in the depths of operations we don’t want too much excitement. You are trusting us with your websites and your clients, its a big responsibility. So my word for operations is robust.
This means the pipeline is about rock solid servers, safe and consistent deploys and comprehensive monitoring.

In future posts I will be covering how our infrastructure is becoming more robust. We made our developer, staging and production environments consistent using [Docker](https://www.docker.com/). Our [CoreOS](https://coreos.com/) clusters mean that we aren’t worrying about individual servers. Centralised logging gives us an overview of the whole system from one dashboard. Continuous integration gives us fast and painless deploys.

One of my mentors in my first job had a motto ‘Be brave’ and I think it fits well with robust. It doesn't mean be foolhardy or a cowboy, rather, how can we set everything up so that we don’t need to be afraid. Scared of making changes? Use version control so you can always go back to a known state. Worried that your new code will break something? Add more unit tests so you can be confident the behaviour is the same. Deploys are large and scary so they get put off? Automate the steps until they are routine. I have tried to keep this in mind so that when something scary comes up the response is ‘how can we fix it so it isn’t scary’. You might not be able to do it right away but it is always good to have a road map to where you want to end up.

Well that wraps up my introduction, have a think about your own word and be brave.
