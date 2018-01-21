---
title: How to create an animated pickup in CSS and HTML
category: Deconstructions
author: george
image:
image_featured: false
permalink: /deconstructions/2014/12/19/css-truck-deconstruction.html
---

This week I came across the [7mml](http://7mml.org/) website and I loved the loaded symbol. An animated car with a scrolling background. It was rather sluggish so I looked at how it was built. It was actually built using a large background image with JavaScript changing the background position.

### What is it?
I am going to rebuild the car/pickup in HTML, SVG and CSS. I was going to build it all in CSS but as it is a complicated shape SVG is probably better. Here's my version:

<div class="truck-container">
	<div class="truck">
		<div class="body">
			<img src="/images/blog/truck-under.svg" width="500" height="136">
			<div class="shine"></div>
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>

		<div class="speed-thingy"></div>
		<div class="speed-thingy second"></div>
		<div class="shadow"></div>

		<div class="wheel back"></div>
		<div class="wheel front"></div>
	</div>
</div>

{% highlight html %}
<div class="truck">
    <div class="body">
        <img src="truck-under.svg">
        <div class="shine"></div>
        <img src="truck-frame.svg" class="frame">
    </div>

    <div class="speed-thingy"></div>
    <div class="speed-thingy second"></div>
    <div class="shadow"></div>

    <div class="wheel back"></div>
    <div class="wheel front"></div>
</div>
{% endhighlight %}

Let's break it down.

#### The Wheels
I started building the car using just HTML and CSS so theses are circles built using border-radius. I used the :before and :after pseudo-elements too for the inner circles. You may be asking "Why did you not use a border on the inner circle?" - if I used border I would not be able to use percentage widths. Then I added an infinite spin animation which works well because of the slightly inconsistent size;

<div class="truck-container">
	<div class="truck">
		<div class="body" style="visibility: hidden">
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>

		<div class="wheel back"></div>
		<div class="wheel front"></div>
	</div>
</div>

{% highlight css %}
@keyframes spin {
    0% { transform: rotate(0); }
    100% { transform: rotate(360deg); }
}

.wheel,
.wheel:after,
.wheel:before {
    bottom: -20%;
    width: 12.8%;
    height: 46.38%;
    border-radius: 100%;
    background: #1e2327;
}

.wheel {
    left: 21.5%;
    -webkit-animation: spin 0.4s infinite linear;
    animation: spin 0.4s infinite linear;
}

.wheel.front {
    left: 82%;
}

.wheel:after,
.wheel:before {
    content: "";
    position: absolute;
    display: block;
    top: 20%;
    left: 20%;
    width: 60%;
    height: 60%;
}

.wheel:before {
    background: #aaa;
}

.wheel:after {
    top: 40%;
    left: 40%;
    width: 20%;
    height: 20%;
}
{% endhighlight %}

#### The Body

I started building the body with HTML elements and CSS but felt I was using the wrong tool for the job. I decided to use SVG instead. There are actually two images to be exact and I overlay one on the other with a slight rotation and vertical transition to give it a bit of character.

<div class="truck-container">
	<div class="truck">
		<div class="body">
			<img src="/images/blog/truck-under.svg" width="500" height="136" class="frame">
		</div>
	</div>
</div>

<div class="truck-container">
	<div class="truck">
		<div class="body">
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>
	</div>
</div>

{% highlight css %}
@keyframes bobbing {
    0%{
        transform: rotate(0) translateY(0);
    }
    100%{
        transform: rotate(0.1deg) translateY(5px);
    }
}

.body {
    width: 100%;
    position: relative;
    transform-origin: right center;
    animation: bobbing 0.2s infinite ease-in-out forwards alternate;
}

.frame {
    position: relative;
}

.truck img {
    width: 100%;
    height: auto;
    top: 0;
    left: 0;
    margin: 0;
}
{% endhighlight %}

#### The Shine
This uses the same effect as the [Facebook content placeholder](http://cloudcannon.com/deconstructions/2014/11/15/facebook-content-placeholder-deconstruction.html). It has the bobble animation as it's placed inside the <code>.body</code> of the truck. The first image is to show the gradient and the shape which is achieved using transforms and a linear gradient. The second one has an animation applied.

<div class="truck-container">
	<div class="truck">
		<div class="body" style="width:500px; height:136px; position: relative;">
			<div class="shine not-animated" style="border: 1px solid #000;"></div>
		</div>
	</div>
</div>

<div class="truck-container">
	<div class="truck">
		<div class="body" style="width:500px; height:136px; position: relative;">
			<div class="shine" style="border: 1px solid #000;"></div>
		</div>
	</div>
</div>

{% highlight css %}
@keyframes shine {
    0%{
        opacity: 0.1;
        background-position: 50px top;
    }
    30% { opacity: 1; }
    50%, 100%{
        opacity: 0.2;
        background-position: -500px 0;
    }
}

.shine {
    /* Position it in the gap */
    width: 67%;
    height: 37%;
    left: 9%;
    top: 6%;

    /* Shape of the div to fit the windows */
    transform: skew(33deg, 1deg);
    border-radius: 56px 100px 20px 0;

    /* The background */
    background: linear-gradient(to right, transparent 76%, rgba(255,255,255,0.8) 78%, rgba(255,255,255,0.8) 96%, transparent 98%);
    background-repeat: no-repeat;
    background-size: cover;

    /* The animation */
    animation: shine 3s infinite ease forwards;
}
{% endhighlight %}


#### The Speed Things
These are simple shrinking and growing divs.

<div class="truck-container">
	<div class="truck">
		<div class="body" style="visibility:hidden">
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>

		<div class="speed-thingy"></div>
		<div class="speed-thingy second"></div>
	</div>
</div>

{% highlight css %}
@keyframes thingy {
    0%{
        transform: translateX(0);
        width: 0;
        opacity: 0;
    }
    10%{
        width: 10%;
        opacity: 1;
    }
    30%, 100%{
        transform: translateX(-250px);
        width: 0;
        opacity: 0;
    }
}

.speed-thingy {
    position: absolute;
    width: 20px;
    border-radius: 10px;
    height: 5%;
    background: #fff;
    top: -10%;
    left: 50%;
    animation: thingy 3s infinite ease forwards;
}

.speed-thingy.second {
    top: 50%;
    left: 30%;
    animation-delay: 0.1s;
}
{% endhighlight %}

#### A Shadow to top it all off
This makes the bobbing effect seem more realistic and shows a platform for the wheels to sit on. We want it to stay still at the front so we change the transform origin to the front. Once that's done we scale it ever so slightly with timing to match the body.

<div class="truck-container">
	<div class="truck">
		<div class="body" style="visibility:hidden">
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>

		<div class="shadow"></div>
	</div>
</div>

{% highlight css %}
@keyframes shadow {
    0%{
        transform: scale(1);
    }
    100%{
        transform: scale(1.01);
    }
}

.shadow {
    width: 90%;
    border-radius: 100%;
    height: 20px;
    bottom: -25%;
    left: 5%;
    background: rgba(0,0,0,0.1);
    animation: shadow 0.2s infinite ease-in-out forwards alternate;
    transform-origin: right center;
}
{% endhighlight %}

#### All Together Again

<div class="truck-container">
	<div class="truck">
		<div class="body">
			<img src="/images/blog/truck-under.svg" width="500" height="136">
			<div class="shine"></div>
			<img src="/images/blog/truck-frame.svg" width="500" height="136" class="frame">
		</div>

		<div class="speed-thingy"></div>
		<div class="speed-thingy second"></div>
		<div class="shadow"></div>

		<div class="wheel back"></div>
		<div class="wheel front"></div>
	</div>
</div>

### Why would I ever use this?
Using CSS animations rather than sprite sheets will give you:

- A crisp look on all screen resolutions
- Seamless tweening between states.
- Flexibility to reuse work (It's much easier to change code then recreate images)

### That's it
I have altered the original quite a bit and the key with these animations was subtlety. Each part adds up to a make it look like it is traveling forward even without the background animations. Going back to [Tim's one word idea](http://cloudcannon.com/operations/2014/12/03/this-is-operations.html), for animations it would be to "subtlety" or "reactive". Animations should not surprise the user, they should feel natural and help convey a real world metaphor. If an animation is off continued use will annoy the user not improve their experience. Hope you enjoyed this deconstruction, comment below if you have any questions.


<style>
	@-webkit-keyframes shine {
		0%{
			opacity: 0.1;
			background-position: 50px top;
		}
		30% { opacity: 1; }
		50%, 100%{
			opacity: 0.2;
			background-position: -500px 0;
		}
	}

	@keyframes shine {
		0%{
			opacity: 0.1;
			background-position: 50px top;
		}
		30% { opacity: 1; }
		50%, 100%{
			opacity: 0.2;
			background-position: -500px 0;
		}
	}

	@-webkit-keyframes bobbing {
		0%{
			-webkit-transform: rotate(0) translateY(0);
		}
		100%{
			-webkit-transform: rotate(1deg) translateY(5px);
		}
	}

	@keyframes bobbing {
		0%{
			-webkit-transform: rotate(0) translateY(0);
			transform: rotate(0) translateY(0);
		}
		100%{
			-webkit-transform: rotate(0.1deg) translateY(5px);
			transform: rotate(0.1deg) translateY(5px);
		}
	}

	@-webkit-keyframes spin {
		0%{
			-webkit-transform: rotate(0);
		}
		100%{
			-webkit-transform: rotate(360deg);
		}
	}

	@keyframes spin {
		0%{
			-webkit-transform: rotate(0);
			transform: rotate(0);
		}
		100%{
			-webkit-transform: rotate(360deg);
			transform: rotate(360deg);
		}
	}

	@-webkit-keyframes thingy {
		0%{
			-webkit-transform: translateX(0);
			width: 0;
			opacity: 0;
		}
		10%{
			width: 10%;
			opacity: 1;
		}
		30%, 100%{
			-webkit-transform: translateX(-250px);
			width: 0;
			opacity: 0;
		}
	}

	@keyframes thingy {
		0%{
			-webkit-transform: translateX(0);
			transform: translateX(0);
			width: 0;
			opacity: 0;
		}
		10%{
			width: 10%;
			opacity: 1;
		}
		30%, 100%{
			-webkit-transform: translateX(-250px);
			transform: translateX(-250px);
			width: 0;
			opacity: 0;
		}
	}

	@-webkit-keyframes shadow {
		0%{
			-webkit-transform: scale(1);
		}
		100%{
			-webkit-transform: scale(1.01);
		}
	}

	@keyframes shadow {
		0%{
			-webkit-transform: scale(1);
			transform: scale(1);
		}
		100%{
			-webkit-transform: scale(1.01);
			transform: scale(1.01);
		}
	}

	.truck-container {
		background: #44dbae;
		margin: 20px 0;
		padding: 20px;
	}

	.truck {
		width: 90%;
		max-width: 500px;
		margin: 100px auto;
		position: relative;
	}

	.truck * {
		position: absolute;
	}

	.wheel,
	.wheel:after,
	.wheel:before {
		bottom: -20%;
		width: 12.8%;
		height: 46.38%;
		border-radius: 100%;
		background: #1e2327;
	}

	.wheel {
		left: 21.5%;
		-webkit-animation: spin 0.4s infinite linear;
		animation: spin 0.4s infinite linear;
	}

	.wheel.front {
		left: 82%;
	}

	.wheel:after,
	.wheel:before {
		content: "";
		position: absolute;
		display: block;
		top: 20%;
		left: 20%;
		width: 60%;
		height: 60%;
	}

	.wheel:before {
		background: #aaa;
	}

	.wheel:after {
		top: 40%;
		left: 40%;
		width: 20%;
		height: 20%;
	}

	.body {
		width: 100%;
		position: relative;
		-webkit-transform-origin: right center;
		-ms-transform-origin: right center;
		transform-origin: right center;
		-webkit-animation: bobbing 0.2s infinite ease-in-out forwards alternate;
		animation: bobbing 0.2s infinite ease-in-out forwards alternate;
	}

	.frame {
		position: relative;
	}

	.truck img {
		width: 100%;
		height: auto;
		top: 0;
		left: 0;
		margin: 0;
	}

	.shine {
		width: 67%;
		height: 37%;
		background: #222;
		left: 9%;
		top: 6%;
		-webkit-transform: skew(33deg, 1deg);
		-ms-transform: skew(33deg, 1deg);
		transform: skew(33deg, 1deg);
		border-radius: 56px 100px 20px 0;
		background: -webkit-gradient(linear, left top, right top, color-stop(76%, transparent), color-stop(78%, rgba(255,255,255,0.8)), color-stop(96%, rgba(255,255,255,0.8)), color-stop(98%, transparent));
		background: -webkit-linear-gradient(left, transparent 76%, rgba(255,255,255,0.8) 78%, rgba(255,255,255,0.8) 96%, transparent 98%);
		background: linear-gradient(to right, transparent 76%, rgba(255,255,255,0.8) 78%, rgba(255,255,255,0.8) 96%, transparent 98%);
		background-repeat: no-repeat;
		-webkit-background-size: cover;
		background-size: cover;

		-webkit-animation: shine 3s infinite ease forwards;
		animation: shine 3s infinite ease forwards;
	}

	.speed-thingy {
		position: absolute;
		width: 20px;
		border-radius: 10px;
		height: 5%;
		background: #fff;
		top: -10%;
		left: 50%;
		-webkit-animation: thingy 3s infinite ease forwards;
		animation: thingy 3s infinite ease forwards;
	}

	.speed-thingy.second {
		top: 50%;
		left: 30%;
		-webkit-animation-delay: 0.1s;
		animation-delay: 0.1s;
	}

	.shadow {
		width: 90%;
		border-radius: 100%;
		height: 20px;
		bottom: -25%;
		left: 5%;
		background: rgba(0,0,0,0.1);
		-webkit-animation: shadow 0.2s infinite ease-in-out forwards alternate;
		animation: shadow 0.2s infinite ease-in-out forwards alternate;
		-webkit-transform-origin: right center;
		-ms-transform-origin: right center;
		transform-origin: right center;
	}

	.not-animated {
		-webkit-animation: none;
		animation: none;
	}
</style>
