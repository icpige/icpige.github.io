---
title: How to create a super simple Christmas landing page
category: Deconstructions
author: george
image:
image_featured: false
permalink: /deconstructions/2014/12/23/super-simple-christmas-landing-page.html
---

This week I felt like doing something Christmas related, it seemed quite relevant. Instead of enjoying the summer sun that a New Zealand Christmas offers I decided to build an experimental landing page to share. Hope you enjoy it.

### What is it?

We wanted a neat way to send our users a present for the holiday season. The obvious way to give a present is in a box that they can open. Who opens presents in a 2D space? Maybe Mario but I don't think he's one of our users. Here is [my Christmas Landing equipped with a 3D present fully animated](http://christmas.cloudcannon.com/).

<iframe src="http://christmas.cloudcannon.com/" height="740"></iframe>

* [GitHub Repository](https://github.com/CloudCannon/Best-Christmas-Landing-Page)
* [Hosted version of landing page](http://christmas.cloudcannon.com/) - Built this by connecting the git repository to a CloudCannon website making everything updatable.

Let's break it down.

#### The Layout
The new version of [Skeleton responsive boilerplate](http://getskeleton.com/) came out recently so I decided to use that. It gives a base which doesn't get in the way and it is basically a better browser default. This allows me to extend it really easily, pairing their grid system with our repeatable regions means I can set up the layout and hand it off. Check it out, I highly recommend it.

#### The Snow Background
The Snow background adds a really nice effect. It uses a set of background images paired with CSS animations to make them move separately from one another. [A full tutorial is available on designmodo.com](http://designshack.net/articles/css/make-it-snow-on-your-website-with-css-keyframe-animations/).

<div class="snow"></div>

{% highlight css %}
@keyframes snow {
    0% {
        background-position: 0px 0px;
    }
    100% {
        background-position: 500px 1000px, 400px 400px, 300px 300px;
    }
}

body {
    background-color: #eee;
    background-image: url('snow.png'), url('snow2.png'), url('snow3.png');

    animation: snow 20s linear infinite;
}
{% endhighlight %}

#### The Box
The Box uses 3D transformations to align all the edges. I originally based the code off [a tutorial found by googling "CSS Cube"](http://desandro.github.io/3dtransforms/docs/cube.html). It goes in depth about the 3D transformations. One very important attribute to note is <code>transform-style: preserve-3d;</code>, this tells the browser to act in a 3D space. If your transforms and transitions are looking very flat then this is probably what is missing. FYI, colours used are from [the Google Material Design colour pallete](http://www.google.com/design/spec/style/color.html#color-color-palette).

<section class="cube-wrapper moved">
  <div class="cube">
    <figure class="front"></figure>
    <figure class="back"></figure>
    <figure class="right"></figure>
    <figure class="left"></figure>
    <figure class="top"></figure>
    <figure class="bottom"></figure>
  </div>
</section>

{% highlight html %}
<section class="cube-wrapper">
    <div class="cube">
        <figure class="front"></figure>
        <figure class="back"></figure>
        <figure class="right"></figure>
        <figure class="left"></figure>
        <figure class="top"></figure>
        <figure class="bottom"></figure>
        <figure class="shadow"></figure>
    </div>
</section>
{% endhighlight %}

This box will live in two states, 'open' and 'closed'. I want the difference between the states to be a single class on the parent element. This way I can toggle between the states with just:

{% highlight javascript %}
$(".cube-wrapper").click(function () {
    $(this).toggleClass("open");
});
{% endhighlight %}

The closed state has 4 segments on the top all pointed inwards. This is just four divs occupying the four different quadrants of the parent square. I did it this way instead of adding more parent figures to keep things modular.

<section class="cube-wrapper">
  <div class="cube">
    <figure class="front"></figure>
    <figure class="back"></figure>
    <figure class="right"></figure>
    <figure class="left"></figure>
    <figure class="top">
      <div class="lower lower-left"></div>
      <div class="lower lower-right"></div>
      <div class="upper upper-left"></div>
      <div class="upper upper-right"></div>
    </figure>
    <figure class="bottom"></figure>
  </div>
</section>

{% highlight html %}
<figure class="top">
    <div class="lower lower-left"></div>
    <div class="lower lower-right"></div>
    <div class="upper upper-left"></div>
    <div class="upper upper-right"></div>
</figure>
{% endhighlight %}

{% highlight css %}
.cube .top div {
	width: 50%;
	height: 100%;
	position: absolute;

	font-size: 60px;
	transform-origin: left center;

	border: 1px solid #3E2723;
	box-sizing: border-box;
}

.cube .top .lower {
	height: 50%;
	width: 100%;
	line-height: 96px;
}

.open .cube .top .lower {
	z-index: 2;
}

.cube .top .upper-right {
	transform-origin: right center;
	transform: translateZ(0px) translateX(98px);
	background: #795548;
}

.cube .top .lower-right {
	transform-origin: bottom center;
	transform: translateZ(-1px) translateY(98px);
	background: #8D6E63;
}

.cube .top .upper-left {
	transform-origin: left center;
	transform: translateZ(0px);
	background: #8D6E63;
}

.cube .top .lower-left {
	transform-origin: top center;
	transform: translateZ(-1px);
	background: #795548;
}
{% endhighlight %}

This CSS places the 4 flaps in the correct position using transforms. The Z translations are very important here. The upper flaps need to be 1px above the others to prevent any overlapping effects. Another important part are the transform origins, these say where the animations should originate from.

For the open state we want the elements to flip across from the outside edge which is where we have set the transform origin.

<section class="cube-wrapper open">
  <div class="cube">
    <figure class="front"></figure>
    <figure class="back"></figure>
    <figure class="right"></figure>
    <figure class="left"></figure>
    <figure class="top">
      <div class="lower lower-left"></div>
      <div class="lower lower-right"></div>
      <div class="upper upper-left"></div>
      <div class="upper upper-right"></div>
    </figure>
    <figure class="bottom"></figure>
  </div>
</section>

{% highlight css %}
.open .cube .top .upper-right {
	transform: translateZ(-1px) translateX(98px) rotateY(150deg);
	background: #795548;
}

.open .cube .top .lower-right {
	transform: translateZ(-1px) translateY(98px) rotateX(-150deg);
	background: #A1887F;
}

.open .cube .top .upper-left {
	transform: translateZ(-1px) rotateY(-150deg);  
	background: #A1887F;
}

.open .cube .top .lower-left {
	transform: translateZ(-1px) rotateX(150deg);
	background: #795548;
}
{% endhighlight %}

We have translated the elements to be all on the same Z plane. I chose 150 degrees very intentionally, if it gets too close to 180 degrees the browser can't decide which way to rotate the flaps. The inverse rotation through the box didn't look that realistic so I decided to use 150 degrees.

#### A Shadow to top it all off
Quite a simple effect gives the whole scene a lot more depth. I replicated the bottom edge of the box, extended it and changed the background.

<section class="cube-wrapper">
  <div class="cube">
    <figure class="front"></figure>
    <figure class="back"></figure>
    <figure class="right"></figure>
    <figure class="left"></figure>
    <figure class="top">
      <div class="lower lower-left"></div>
      <div class="lower lower-right"></div>
      <div class="upper upper-left"></div>
      <div class="upper upper-right"></div>
    </figure>
    <figure class="bottom"></figure>
    <figure class="shadow"></figure>
  </div>
</section>

<section class="cube-wrapper open">
  <div class="cube">
    <figure class="front"></figure>
    <figure class="back"></figure>
    <figure class="right"></figure>
    <figure class="left"></figure>
    <figure class="top">
      <div class="lower lower-left"></div>
      <div class="lower lower-right"></div>
      <div class="upper upper-left"></div>
      <div class="upper upper-right"></div>
    </figure>
    <figure class="bottom"></figure>
    <figure class="shadow"></figure>
  </div>
</section>

{% highlight css %}
.cube .shadow {
	transform: rotateX(-90deg) translateZ(101px) translateX(0);
	width: 355px;
	background: rgba(0,0,0,0.1);
	border: 0;
	padding: 2px;
}

.open .cube .shadow {
	transform: rotateX(-90deg) translateZ(101px) translateX(100px);
}
{% endhighlight %}

#### The Animations
This is where a great looking box comes alive. Everything to this point has been built using transforms and a single class change. This means that all the hard work has been done and we can just roll over some CSS transitions.

First we want to add the transitions to the original state. This will allow us to synchronise the different parts when the box is closing.

{% highlight css %}
.cube {
	transition: transform 1s ease 0,
				background 1s ease 0;
}

.cube .top div,
.cube .shadow {
	transition: transform 1s ease 1s,
				background 1s ease 1s;
}


.cube .top .lower {
	transition: transform 1s ease 0,
				background 1s ease 0,
				z-index 0 ease 0;
}
{% endhighlight %}

Second we want to add the transitions to the <code>.open</code> state. This will allow us to synchronise the different parts when the box is opening.

{% highlight css %}
.open .cube {
	transition: transform 1s ease 2s,
				background 1s ease 2s;
}

.open .cube .top div,
.open .cube .shadow {
	transition: transform 1s ease 0s,
				background 1s ease 0s;
}

.open .cube .top .lower {
	transition: transform 1s ease 1s,
				background 1s ease 1s,
				z-index 0 ease 1s;
}
{% endhighlight %}

You can try these out by clicking any box on this page.

#### All Together Again

<iframe src="http://christmas.cloudcannon.com/" height="740"></iframe>

* [GitHub Repository](https://github.com/CloudCannon/Best-Christmas-Landing-Page)
* [Hosted version of landing page](http://christmas.cloudcannon.com/)

### Why would I ever use this?
This shows just how much you can achieve with CSS; 3D transformations, animations and handy boilerplates lets you get a lot done in a very short amount of time. Also it's Christmas, and who doesn't like to give out presents.

### That's it
This landing page was a great afternoon project, plugging together a whole lot of different tutorials into a really useful resource. If you like it feel free to contribute to the [GitHub Repository](https://github.com/CloudCannon/Best-Christmas-Landing-Page). I'd love to add some other branches with different holidays. If you have any questions or want to spread some cheer be sure to comment below. Merry Christmas.

<script type="text/javascript">
window.onload = function () {
	document.querySelector(".cube-wrapper").addEventListener("click", function (e) {
		e.target.classList.toggle("open");
	});
}
</script>

<style>
iframe {
	width: 100%;
	display: block;
	border: 1px solid #aaa;
}

@keyframes snow {
 0% {background-position: 0px 0px;}
 100% {background-position: 500px 1000px, 400px 400px, 300px 300px;}
}

@-moz-keyframes snow {
 0% {background-position: 0px 0px;}
 100% {background-position: 500px 1000px, 400px 400px, 300px 300px;}
}

@-webkit-keyframes snow {
0% {background-position: 0px 0px;}
 100% {background-position: 500px 1000px, 400px 400px, 300px 300px;}
}

@-ms-keyframes snow {
 0% {background-position: 0px 0px;}
 100% {background-position: 500px 1000px, 400px 400px, 300px 300px;}
}

.snow {
    background-color: #eee;
    background-image: url('/images/blog/deconstructions/snow.png'), url('/images/blog/deconstructions/snow2.png'), url('/images/blog/deconstructions/snow3.png');

    -webkit-animation: snow 20s linear infinite;
    -moz-animation: snow 20s linear infinite;
    -ms-animation: snow 20s linear infinite;
    animation: snow 20s linear infinite;

    width: 100%;
    height: 300px;
}

.cube-wrapper {
	width: 200px;
	height: 200px;
	position: relative;
	margin: 160px auto 20px auto;
	-webkit-perspective: 500px;
	-ms-perspective: 500px;
	perspective: 500px;
	cursor: pointer;
}

.cube-wrapper p {
	position: absolute;
	bottom: -70px;
	width: 100%;
	text-align: center;
	color: #aaa;
	-webkit-transition: all 0.5s ease 3s;
	transition: all 0.5s ease 3s;
}

.cube-wrapper.open p {
	-webkit-transform: translateY(50px);
	-ms-transform: translateY(50px);
	transform: translateY(50px);
	-webkit-transition: all 0.5s ease 0;
	transition: all 0.5s ease 0;
	opacity: 0;
}

.cube-wrapper.open {
	/*-webkit-animation: none;*/
}

.open-message {
	visibility: hidden;
	-webkit-transform: translateY(50px) scale(0.2);
	-ms-transform: translateY(50px) scale(0.2);
	transform: translateY(50px) scale(0.2);
	-webkit-transform-origin: bottom center;
	-ms-transform-origin: bottom center;
	transform-origin: bottom center;
	text-align: center;
	z-index: 3;
	position: relative;
	background: rgba(255, 255, 255, 0.9);
	padding: 10px;
	max-width: 700px;
	margin: -100px auto 0;
	opacity: 0;

	-webkit-transition: all 0.5s ease 0;

	transition: all 0.5s ease 0;
}

.cube-wrapper.open + .open-message  {
	visibility: visible;
	-webkit-transform: translateY(0);
	-ms-transform: translateY(0);
	transform: translateY(0);
	opacity: 1;
	-webkit-transition: all 0.5s ease 3s;
	transition: all 0.5s ease 3s;
}

.cube {
	width: 100%;
	height: 100%;
	position: absolute;
	-webkit-transform-style: preserve-3d;
	-ms-transform-style: preserve-3d;
	transform-style: preserve-3d;
	-webkit-transform: translateZ( -200px ) rotateZ(0) rotateX(-20deg) rotateY(45deg) translateX(-40px) translateY(-120px);
	-ms-transform: translateZ( -200px ) rotateZ(0) rotateX(-20deg) rotateY(45deg) translateX(-40px) translateY(-120px);
	transform: translateZ( -200px ) rotateZ(0) rotateX(-20deg) rotateY(45deg) translateX(-40px) translateY(-120px);
	cursor: pointer;
}

.cube figure {
	width: 196px;
	height: 196px;
	display: block;
	position: absolute;
	border: 2px solid #3E2723;
	top: 0;
	left:0;

	line-height: 196px;
	font-size: 120px;
	text-align: center;
	-webkit-transform-style: preserve-3d;
	-ms-transform-style: preserve-3d;
	transform-style: preserve-3d;
	cursor: pointer;
}

.cube .front    { -webkit-transform: rotateY(   0deg ) translateZ(100px); -ms-transform: rotateY(   0deg ) translateZ(100px); transform: rotateY(   0deg ) translateZ(100px); background: #795548; }
.cube .back     { -webkit-transform: rotateX( 180deg ) translateZ(100px); -ms-transform: rotateX( 180deg ) translateZ(100px); transform: rotateX( 180deg ) translateZ(100px); background: #5D4037; }
.cube .right    { -webkit-transform: rotateY(  90deg ) translateZ(100px); -ms-transform: rotateY(  90deg ) translateZ(100px); transform: rotateY(  90deg ) translateZ(100px); background: #4E342E;  }
.cube .left     { -webkit-transform: rotateY( -90deg ) translateZ(100px); -ms-transform: rotateY( -90deg ) translateZ(100px); transform: rotateY( -90deg ) translateZ(100px); background: #8D6E63; }
.cube .top     { -webkit-transform: rotateX(  90deg ) translateZ(100px); -ms-transform: rotateX(  90deg ) translateZ(100px); transform: rotateX(  90deg ) translateZ(100px); }
.cube .bottom   { -webkit-transform: rotateX( -90deg ) translateZ(99px); -ms-transform: rotateX( -90deg ) translateZ(99px); transform: rotateX( -90deg ) translateZ(99px); background: #4E342E; }
.cube .shadow   { -webkit-transform: rotateX( -90deg ) translateZ(101px) translateX(0); -ms-transform: rotateX( -90deg ) translateZ(101px) translateX(0); transform: rotateX( -90deg ) translateZ(101px) translateX(0); width: 355px; background: rgba(0,0,0,0.1); border: 0; padding: 2px; }

.cube .present   { margin: 0; width: 150px; height: 60px; -webkit-transform: rotateX( -90deg ) rotateZ(29deg) translateZ(280px) translateY(-76px) translateX(60px); -ms-transform: rotateX( -90deg ) rotateZ(29deg) translateZ(280px) translateY(-76px) translateX(60px); transform: rotateX( -90deg ) rotateZ(29deg) translateZ(280px) translateY(-76px) translateX(60px); border: 0; }

.cube .top div {
	width: 50%;
	height: 100%;
	position: absolute;

	font-size: 60px;
	-webkit-transform-origin: left center;
	-ms-transform-origin: left center;
	transform-origin: left center;

	border: 1px solid #3E2723;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}

.cube .top .lower {
	height: 50%;
	width: 100%;
	line-height: 96px;
}

.open .cube .top .lower {
	z-index: 2;
}

.cube .top div,
.cube .shadow {
	-webkit-transition: -webkit-transform 1s ease 2s,
				background 1s ease 2s;
	transition: transform 1s ease 2s,
				background 1s ease 2s;
}


.cube .top .lower {
	-webkit-transition: -webkit-transform 1s ease 1s,
				background 1s ease 1s,
				z-index 0 ease 1s;
	transition: transform 1s ease 1s,
				background 1s ease 1s,
				z-index 0 ease 1s;
}

.open .cube .top div,
.open .cube .shadow {
	-webkit-transition: -webkit-transform 1s ease 0s,
				background 1s ease 0s;
	transition: transform 1s ease 0s,
				background 1s ease 0s;
}


.open .cube .top .lower {
	-webkit-transition: -webkit-transform 1s ease 1s,
				background 1s ease 1s,
				z-index 0 ease 1s;
	transition: transform 1s ease 1s,
				background 1s ease 1s,
				z-index 0 ease 1s;
}


.cube {
	-webkit-transition: -webkit-transform 1s ease 1s,
				background 1s ease 1s;
	transition: transform 1s ease 1s,
				background 1s ease 1s;
}

.open .cube {
	-webkit-transition: -webkit-transform 1s ease 2s,
				background 1s ease 2s;
	transition: transform 1s ease 2s,
				background 1s ease 2s;
}

.cube .present {
	-webkit-transition: -webkit-transform 1s ease 0,
				background 1s ease 0;
	transition: transform 1s ease 0,
				background 1s ease 0;
}


.open .cube .present {
	-webkit-transition: -webkit-transform 1s ease 2.5s,
				background 1s ease 2.5s;
	transition: transform 1s ease 2.5s,
				background 1s ease 2.5s;
}


.cube .top .upper-right {
	-webkit-transform-origin: right center;
	-ms-transform-origin: right center;
	transform-origin: right center;
	-webkit-transform: translateZ(0px) translateX(98px);
	-ms-transform: translateZ(0px) translateX(98px);
	transform: translateZ(0px) translateX(98px);
	background: #795548;
}

.cube .top .lower-right {
	-webkit-transform-origin: bottom center;
	-ms-transform-origin: bottom center;
	transform-origin: bottom center;
	-webkit-transform: translateZ(-1px) translateY(98px);
	-ms-transform: translateZ(-1px) translateY(98px);
	transform: translateZ(-1px) translateY(98px);
	background: #8D6E63;
}

.cube .top .upper-left {
	-webkit-transform-origin: left center;
	-ms-transform-origin: left center;
	transform-origin: left center;
	-webkit-transform: translateZ(0px);
	-ms-transform: translateZ(0px);
	transform: translateZ(0px);
	background: #8D6E63;
}

.cube .top .lower-left {
	-webkit-transform-origin: top center;
	-ms-transform-origin: top center;
	transform-origin: top center;
	-webkit-transform: translateZ(-1px);
	-ms-transform: translateZ(-1px);
	transform: translateZ(-1px);
	background: #795548;
}

.open .cube .top .upper-right {
	-webkit-transform: translateZ(-1px) translateX(98px) rotateY(150deg);
	-ms-transform: translateZ(-1px) translateX(98px) rotateY(150deg);
	transform: translateZ(-1px) translateX(98px) rotateY(150deg);
	background: #795548;
}

.open .cube .top .lower-right {
	-webkit-transform: translateZ(-1px) translateY(98px) rotateX(-150deg);
	-ms-transform: translateZ(-1px) translateY(98px) rotateX(-150deg);
	transform: translateZ(-1px) translateY(98px) rotateX(-150deg);
	background: #A1887F;
}

.open .cube .top .upper-left {
	-webkit-transform: translateZ(-1px) rotateY(-150deg);
	-ms-transform: translateZ(-1px) rotateY(-150deg);
	transform: translateZ(-1px) rotateY(-150deg);  
	background: #A1887F;
}

.open .cube .top .lower-left {
	-webkit-transform: translateZ(-1px) rotateX(150deg);
	-ms-transform: translateZ(-1px) rotateX(150deg);
	transform: translateZ(-1px) rotateX(150deg);
	background: #795548;
}

.open .cube .shadow {
	-webkit-transform: rotateX( -90deg ) translateZ(101px) translateX(100px);
	-ms-transform: rotateX( -90deg ) translateZ(101px) translateX(100px);
	transform: rotateX( -90deg ) translateZ(101px) translateX(100px);
}

.open.moved .cube {
	-webkit-transform: translateZ(-200px) rotateZ(0) rotateX(-70deg) rotateY(45deg) translateX(-40px) translateY(-120px);
	-ms-transform: translateZ(-200px) rotateZ(0) rotateX(-70deg) rotateY(45deg) translateX(-40px) translateY(-120px);
	transform: translateZ(-200px) rotateZ(0) rotateX(-70deg) rotateY(45deg) translateX(-40px) translateY(-120px);
}

.open .cube .present {
	-webkit-transform: rotateX(60deg) rotateY(-25deg) rotateZ(38deg) translateY(-125px) translateZ(160px) translateX(41px);
	-ms-transform: rotateX(60deg) rotateY(-25deg) rotateZ(38deg) translateY(-125px) translateZ(160px) translateX(41px);
	transform: rotateX(60deg) rotateY(-25deg) rotateZ(38deg) translateY(-125px) translateZ(160px) translateX(41px);
}
</style>
