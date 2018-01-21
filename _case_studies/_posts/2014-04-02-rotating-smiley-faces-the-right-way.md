---
title: How to turn text smiley faces the right way up with Smiley.js
category: Freebies
author: george
image: /images/blog/smileys-js.png
image_featured: true
permalink: /freebies/2014/04/02/rotating-smiley-faces-the-right-way.html
---
I recently released a small script I built last year. I took the time to clean it up and I thought you all might enjoy it.

I'm proud to announce the Smiley.js Jquery plugin. This tiny plugin can take some ordinary looking smiley faces and turn them into things that people actually recognise :D --> <span class="cheese">:D</span>.

## How do I use it?
CloudCannon is kindly giving the power of Smileys to everyone. If you want to use it just check out the <a target="_blank" href="https://github.com/GeorgePhillips/Smiley.js">GitHub repo</a> and add the files to your site.

### Step 1 - Add the happiness
Add the smileys.css file to the head.

{% highlight html %}
<link rel="stylesheet" href="smiley.css">
{% endhighlight %}

Add smileys.js to the end of the body after JQuery

{% highlight html %}
<script src="smileys.js"></script>
{% endhighlight %}

### Step 2 - Smilify
Just call smilify on any element as you would any other JQuery function.

{% highlight javascript %}
$("selector").smilify();
{% endhighlight %}

### Links

* <a target="_blank" href="http://cloudcannon.com/smileys/">Project Page</a>
* <a target="_blank" href="https://github.com/GeorgePhillips/Smiley.js">GitHub Repo</a>

Enjoy!
