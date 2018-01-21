---
title: Google's transforming material icons in HTML and CSS
category: Deconstructions
author: george
image: /images/blog/delightful-details.png
image_featured: true
permalink: /deconstructions/2014/12/05/material-design-delightful-details.html
---

This week I am deconstructing the Delightful Details from [Google's Material Design](http://www.google.com/design/spec/material-design/introduction.html). It's not built in HTML and CSS but that's not important. What's important is that we can.

![An image version of Google's Delightful Details](/images/blog/delightful-details.png)

This week I am deconstructing the Delightful Details from [Google's Material Design](http://www.google.com/design/spec/material-design/introduction.html).

<video crossorigin="anonymous" preload="metadata" loop autoplay poster="/video/animation-delightfuldetails.png" style="margin-bottom: 35px;">
    <source src="/video/animation-delightfuldetails.mp4" type="video/mp4">
    <source src="/video/animation-delightfuldetails.mkv" type="video/x-matroska">
</video>

<link rel="stylesheet" href="/css/delightful-details.css"  />

It's not built in HTML and CSS but that's not important. What's important is that we can. If you haven't read [Google's Material Design Spec](http://www.google.com/design/spec/material-design/introduction.html) I highly recommend it. It's a well crafted design spec with a lot of good information. I have followed the different android specs over the years but this one excites me as it includes information for larger screens.

### What is it?
This is the finished clone of the [Delightful Details](http://www.google.com/design/spec/animation/delightful-details.html). Note that it isn't exactly the same but it proves that it can be done in HTML and CSS. You click on each section to toggle between icons.

<div class="material-grid">
    <section class="material-green">
        <div class="material-icon hamburger" data-icon="hamburger">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-amber">
        <div class="material-icon play" data-icon="play">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-blue">
        <div class="material-icon failed-loader" data-icon="failed-loader">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-red">
        <div class="material-icon plus-one" data-icon="plus-one">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight html %}
<div class="material-grid">
    <section class="material-green">
        <div class="material-icon hamburger">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-amber">
        <div class="material-icon play">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-blue">
        <div class="material-icon failed-loader">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-red">
        <div class="material-icon plus-one">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>
{% endhighlight %}

Now let's break it down.

#### The Grid
This is just a square grid cut in four with different colours. My colours differ slightly as I chose to use the 500 strength colours from the material [colour palette](http://www.google.com/design/spec/style/color.html#color-color-palette).

<div class="material-grid">
    <section class="material-green">
    </section>

    <section class="material-amber">
    </section>

    <section class="material-blue">
    </section>

    <section class="material-red">
    </section>
</div>

{% highlight html %}
<div class="material-grid">
    <section class="material-green">
    </section>

    <section class="material-amber">
    </section>

    <section class="material-blue">
    </section>

    <section class="material-red">
    </section>
</div>
{% endhighlight %}

Here's the CSS for the grid, the [Roboto font](http://www.google.com/fonts/specimen/Roboto) will come in handy later. The <code>user-select</code> will allow us to click the elements without selecting them.

{% highlight css %}
.material-grid {
    width: 360px;
    height: 360px;
    font-family: "Roboto", Arial;
}

.material-grid section {
    width: 50%;
    height: 50%;
    float: left;
    position: relative;
    overflow: hidden;
    cursor: pointer;
    user-select: none;
}
{% endhighlight %}

Here's another nice effect for clicking the grid section. [My inspiration](http://www.google.com/design/spec/animation/responsive-interaction.html#responsive-interaction-user-input) comes from the material design spec again; it adds a circular active effect on click. Try clicking any of the grids on this page (you will need to hold it for a bit).

{% highlight css %}
@keyframes materialResponse {
    0% {
        width: 0;
        height: 0;
        margin: 0;
        background: rgba(255,255,255,0.1);
    }
    100% {
        width: 250%;
        height: 250%;
        margin: -125%;
        background: rgba(255,255,255,0.4);
    }
}

.material-grid section:before {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    border-radius: 999px;
    cursor: pointer;
}

.material-grid section:active:before {
    animation: materialResponse 0.4s ease;
    z-index: 2;
}

.material-green { background: #4CAF50; }
.material-amber { background: #FFC107; }
.material-blue { background: #2196F3; }
.material-red { background: #F44336; }
{% endhighlight %}

#### The Icons
To recreate the icons in the grids I have chosen to use a div with three span tags. With these elements I will create the following icons:

* The infamous hamburger icon
* A back arrow
* Pause
* Play
* Stop
* The animated loader (a failed attempt)
* +1 Icon
* +2 Icon

By creating all of these with the same elements I can ensure that I can smoothly transition between any of them. Each one having it's own class to add to the <code>.material-icon</code> element I will break them down in the order listed above (also the order I built them).

{% highlight html %}
<div class="material-icon">
    <span class="first"></span>
    <span class="second"></span>
    <span class="third"></span>
</div>
{% endhighlight %}

Here is the base CSS used to construct my icons:

{% highlight css %}
.material-icon {
    width: 70px;
    height: 70px;
    margin: 55px;
    position: relative;
}

.material-icon span,
.material-icon {
    transition: all 500ms ease;
}

.material-icon span {
    background-color: #fff;
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
}
{% endhighlight %}

#### The infamous hamburger icon
The hamburger is an on going debate in the UX community but I leave that for another day. All I need to do for this icon is create three horizontal bars of equal height. I will move the second span to the middle and the third to the bottom.

<div class="clearfix">
    <section class="material-standalone-section material-green">
        <div class="material-icon hamburger">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.hamburger span {
    height: 10px;
    -webkit-transform: translateY(29px);
}

.hamburger .second {
    -webkit-transform: translateY(0);
}

.hamburger .third{
    -webkit-transform: translateY(60px);
}
{% endhighlight %}

Now you may be asking, why not use absolute positioning here? By using transforms we get some fantastic performance gains as well as sub-pixel positioning on our transitions. [Paul Irish](https://twitter.com/paul_irish) breaks this down really well in [his article on absolute positioning vs translate](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/).

#### A back arrow
I will build the back arrow to be an arrow facing right, then rotate the whole container 180 degrees. This will give we the same effect as the video and allow me to focus each animation separately. The first span is exactly the same so I will combine that selector. The second and third are rotated 45 degrees and need to be moved into place. <em>You can click this one to see the animation.</em>

<div class="clearfix">
    <section class="material-standalone-section material-green">
        <div class="material-icon arrow" data-icon="arrow">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
/* This part is added to the hamburger css so they have the same height and positioning. */
.hamburger span, .arrow span {
    height: 10px;
    -webkit-transform: translateY(29px);
}

/* I also need to move it back a bit to keep it centered. */
.arrow {
    -webkit-transform: rotate(180deg) translateX(-3px);
}

.arrow .second,
.arrow .third {
    -webkit-transform: rotate(-45deg) translateY(56px);
    width: 40px;
}

.arrow .second {
    -webkit-transform: rotate(45deg) translateY(-15px) translateX(40px);
}
{% endhighlight %}

That's the green section all done. Let's move on to the amber section.

#### Pause
The pause is just as easy as the hamburger; It's just two vertical bars. I decided to rotate it 90deg and have two horizontal bars for the animation to play.

<div class="clearfix">
    <section class="material-standalone-section material-amber">
        <div class="material-icon pause" data-icon="pause">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.pause {
    -webkit-transform: rotate(90deg);
}

.pause span {
    height: 25px;
}

.pause .second {
    -webkit-transform: translateY(45px);
}
{% endhighlight %}

#### Play...
This one took me a while to think about. You can probably imagine me trying to stick three rectangles together to create a triangle. <strong>Hmmm... I'll come back to this one after stop.</strong>

<div class="clearfix">
    <section class="material-standalone-section material-amber">
        <div class="material-icon not-play" data-icon="not-play">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

#### Stop
This one was obvious everyone who knows CSS knows how to create a coloured square. I did however use two half blocks so the animation to pause would look like it's splitting from the middle.

<div class="clearfix">
    <section class="material-standalone-section material-amber">
        <div class="material-icon stop">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.stop {
    -webkit-transform: rotate(270deg);
}

.stop span {
    height: 35px;
}

.stop .second {
    height: 45px;
    -webkit-transform: translateY(25px);
}
{% endhighlight %}

#### Play Again
After doing stop and looking at the original animation closely I realised that I needed something else to achieve this. I employed the same technique from my [Facebook content placeholder](/deconstructions/2014/11/15/facebook-content-placeholder-deconstruction.html) post to mask the icon into a triangle shape.

<div class="clearfix">
    <section class="material-standalone-section material-amber">
        <div class="material-icon play" data-icon="play">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-standalone-section material-amber visible-masks">
        <div class="material-icon play" data-icon="play">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

Here's my mask, no extra elements required. I default it to sitting on top and bottom so my earlier icons are not effected.

{% highlight css %}
.material-icon:before,
.material-icon:after {
    background: transparent;
    width: 0;
    height: 0;
    display: block;
    position: relative;
    width: 100px;
    height: 40px;
    z-index: 1;
    transform: translateY(-50px);
    transition: all 500ms ease;
    color: transparent;
}

/* Colour the masks the same colour as the grids */
.material-green,
.material-green .material-icon:before,
.material-green .material-icon:after { background: #4CAF50; }

.material-amber,
.material-amber .material-icon:before,
.material-amber .material-icon:after { background: #FFC107; }

.material-blue,
.material-blue .material-icon:before,
.material-blue .material-icon:after { background: #2196F3; }

.material-red,
.material-red .material-icon:before,
.material-red .material-icon:after { background: #F44336; }
{% endhighlight %}

Here's the additional CSS for the play icon. It rotates the container so that it has the animation on change and rotates our masks into place to create a triangle.

{% highlight css %}
/* We want to use the square and mask it so we might as well use the same CSS as the stop */
.stop span, .play span {
    height: 35px;
}

.stop .second, .play .second {
    height: 45px;
    -webkit-transform: translateY(25px);
}

.play {
    -webkit-transform: rotate(180deg);
}

.play:before {
    -webkit-transform: translateY(-24px) translateX(-17px) rotate(-26deg);
}

.play:after {
    -webkit-transform: translateY(11px) translateX(-22px) rotate(26deg);
}
{% endhighlight %}

That's the amber section all done. Let's move on to the blue loader section.

#### The animated loader <small>a failed attempt</small>
I'm going to be honest, I was on a bit of a high getting the last two parts done. The loader did not seem like a huge leap as I could use all the same tactics and pair it with an infinite CSS animation. As you can see I gave up getting the animations anything close to perfect:

<div class="clearfix">
    <section class="material-standalone-section material-blue">
        <div class="material-icon failed-loader">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.stop {
    -webkit-transform: rotate(270deg);
}

.stop span {
    height: 35px;
}

.stop .second {
    height: 45px;
    -webkit-transform: translateY(25px);
}
{% endhighlight %}

I will break each step down anyway. Since releasing this post, [a good article using SVG and CSS animations](http://david.ingledow.co.uk/blog/google-material-designs-animated-loading-spinner-svg-and-css/) has popped up to implement the loader.

<div class="clearfix">
    <section class="material-standalone-section material-blue">
        <div class="material-icon loader-step-one">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-standalone-section material-blue">
        <div class="material-icon loader-step-two">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>

    <section class="material-standalone-section material-blue">
        <div class="material-icon loader-step-three">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
    <section class="material-standalone-section material-blue">
        <div class="material-icon failed-loader loader-only-trailer">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
    <section class="material-standalone-section material-blue">
        <div class="material-icon failed-loader">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
@keyframes spin {
    from {transform:rotate(0deg);}
    to {transform:rotate(360deg);}
}

@keyframes leader {
    0% { transform: rotate(0); }
    12% {
        transform: rotate(90deg);
        border-bottom-color: transparent;
    }
    12.00001% {
        border-bottom-color: #fff;
    }
    25% {
        transform: rotate(180deg);
        border-bottom-color: #fff;
    }
    37.5% {
        transform: rotate(180deg);
        border-bottom-color: #fff;
        border-right-color: transparent;
    }
    37.50001%, 50% {
        transform: rotate(180deg);
        border-bottom-color: transparent;
        border-right-color: transparent;
    }

    /* --- */
    67.5% {
        transform: rotate(270deg);
        border-bottom-color: transparent;
        border-right-color: transparent;
    }
    67.50001% {
        transform: rotate(270deg);
        border-bottom-color: #fff;
        border-right-color: transparent;
    }
    75%, 87.5% {
        transform: rotate(360deg);
        border-bottom-color: #fff;
        border-right-color: transparent;
    }
    87.50001%, 100% {
        transform: rotate(360deg);
        border-bottom-color: transparent;
        border-right-color: transparent;
    }
}

@keyframes trailer {
    0% { transform: rotate(0); }
    25% { transform: rotate(0); }
    37.5% {
        transform: rotate(90deg);
    }
    50% {
        transform: rotate(180deg);
        border-bottom-color: transparent;
        border-right-color: transparent;
    }
    75% { transform: rotate(180deg); }
    87.5% {
        transform: rotate(270deg);
    }
    100% {
        transform: rotate(360deg);
        border-bottom-color: transparent;
        border-right-color: transparent;
    }
}

.failed-loader {
    animation: spin 3s infinite 0.5s linear;
}

.failed-loader span {
    width: 58px;
    height: 58px;
    border: 6px solid #fff;
    background: transparent;
    border-radius: 70px;
    border-top-color: transparent;
    border-bottom-color: transparent;
    border-right-color: transparent;
    animation: trailer 1.5s linear 0.5s infinite;
}

.failed-loader .first {
    animation: leader 1.5s linear 0.5s infinite;
}

.failed-loader .third {
    display: none;
}
{% endhighlight %}

Here are the steps I took to recreate the loader. To get the general shape I created a div with border on three sides (1st version) then added a border radius (2nd version). The third step was to add a constant rotation to the outer element (3rd version). The next image is a bit weird on it's own; it rotates one of the spans and while adding and removing one of the borders. This image paired with another span rotating behind it gives the growing and shrinking look.

If I were to build it again I would have had a full circle in white and used the masks from before. The masks would use the same technique to grow and shrink. This would allow the white segment to get much smaller like in the video. This was not possible in the previous version as the minimum size was set by one of the edges.

#### +1 and +2 Icon
I was finally up to my last segment and I was cut down by the mighty loader. I decided to start with the plus. Very similar to techniques before but I overlap them instead of putting them parallel. For the +2 icon I can rotate the two elements of the plus 90 degrees.

<div class="clearfix">
    <section class="material-standalone-section material-red no-mask">
        <div class="material-icon plus-one" data-icon="plus-one">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.plus-one .first, .plus-two .first,
.plus-one .second, .plus-two .second {
    width: 30px;
    height: 6px;
    -webkit-transform: translateY(32px);
}

.plus-one .second,
.plus-two .first {
    -webkit-transform: translateY(32px) rotate(90deg);
}

.plus-two .second {
    -webkit-transform: translateY(32px) rotate(180deg);
}
{% endhighlight %}

For the number, I mentioned earlier that the [Roboto font](http://www.google.com/fonts/specimen/Roboto) will come in handy. I figured the easiest way to add the numbers was to make the content of the masks "1" and "2". Then all I need to do is position the masks to the right of the plus.

<div class="clearfix">
    <section class="material-standalone-section material-red">
        <div class="material-icon plus-one" data-icon="plus-one">
            <span class="first"></span>
            <span class="second"></span>
            <span class="third"></span>
        </div>
    </section>
</div>

{% highlight css %}
.material-icon:before { content: "1"; }
.material-icon:after { content: "2"; }

.plus-one:before,
.plus-two:after {
    color: #fff;
    background: transparent !important;
    font-size: 52px;
    line-height: 70px;
    font-weight: bold;
    transform: translateY(-5px) translateX(30px);
}

.plus-two:after {
    transform: translateY(-45px) translateX(35px);
}
{% endhighlight %}

Once I added the CSS to position it I noticed a really nice effect of the masks fading back to their original positions. That wasn't an intended effect but I liked it so much I kept it like that. It feels like the plus is a level and it is pulling the numbers in and out.

For anyone looking for the original number change animation, here is [a codepen](http://codepen.io/lbebber/pen/Ecdsb) by [Lucas Bebber](https://twitter.com/lucasbebber) which does exactly that.

### Why would I ever use this?
Android used to have a rule, maybe they still do, that for every negative experience a user has there must be three positive reactions to make up for it. These are just a few examples that will truly delight a user and these are the things they remember. Using things like this will show the user how the interface is changing rather relying on education.

### That's it
This Deconstruction took me a bit longer than expected which is why there wasn't one last week. I hope to use some of these techniques soon. I am thinking of writing a tutorial to demonstrate custom controls on HTML5 videos which will feature the play, pause and stop. Definitely check out [Google's Material Design Spec](http://www.google.com/design/spec/material-design/introduction.html), it's full of great designs and guidelines to think about. If you have any questions or you fixed my loading symbol add a comment below.

<script type="text/javascript">
window.onload = function(){
    var switches = {
        "hamburger": "arrow",
        "arrow": "hamburger",

        "stop": "pause",
        "pause": "play",
        "play": "stop",

        "plus-one": "plus-two",
        "plus-two": "plus-one"
    };

    $(".material-grid section, .material-standalone-section").on("click", function (argument) {
        var $el = $(this).find(".material-icon"),
            icon = $el.data("icon"),
            newIcon = switches[icon];

        if (newIcon) {
            $el.removeClass(icon).addClass(newIcon).data("icon", newIcon);
        }
    });
}
</script>
