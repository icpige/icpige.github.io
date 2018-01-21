---
title: Have SSL on your CloudCannon website for free
category: Tutorials
author: mike
image:
image_featured: false
permalink: /tutorials/2014/12/09/have-ssl-on-your-cloudcannon-website-for-free.html
---

Today I’m going to show you how you can add SSL to your CloudCannon website in under 5 minutes. What’s even better, it’s free!

Before we get started let’s cover a few questions:

## What’s SSL?
SSL is a way of ensuring all the data passed between a website visitor and the server remains private. For websites with login passwords and private data, SSL is essential. For static websites it’s not so important as the website is already public. However, for visitors there’s an extra level of trust from seeing the padlock in their browser.

![Secure page](/images/blog/ssl/blurred.png)

## What’s CloudFlare?
CloudFlare is service which works as a reverse proxy for your website. What that means is when someone visits your website, the request will go to CloudFlare, then CloudFlare will download the website from the hosting provider and serve it to the user.

The advantage of doing this is CloudFlare does a number of performance and security enhancements so your website will load faster and be more immune to malicious attacks. They’ve also recently added free SSL support.

![CloudFlare](/images/blog/ssl/cloudflare-logo.png)

## Why should I care?
Google recently [announced](http://googlewebmastercentral.blogspot.co.nz/2014/08/https-as-ranking-signal.html) that it will prioritise sites that use SSL. Web designers should be approaching clients about SSL to ensure that their clients sites continue to rank well. Given that CloudFlare offers SSL for free AND you get the malicious traffic buffer of their CDN, using it is almost a no-brainer.

## Let’s add SSL!
First off, head over to [CloudFlare](https://www.cloudflare.com) and register a new account
![Registration](/images/blog/ssl/register.png)

Next you’ll be asked for your website address
![Add a website](/images/blog/ssl/add-website.png)


After this CloudFlare will take about a minute to scan your existing DNS records. Once it’s finished it, it’ll show you the DNS records it’s found. You need to verify these records are correct. For CloudCannon websites the DNS records should be fairly simple, usually the setup is:

* an A record for the root domain pointing to 184.169.135.34
* an A record for *.rootdomain pointing to 184.169.135.34
* perhaps an MX record if you have email set up for that domain

If you're setting up a new domain refer to our [documentation](http://cloudcannon.com/docs#setting_up_a_domain) under the "Use your Domain Registrar's DNS server" section and enter those details into CloudFlare.

Once you’re happy these are correct click the continue button.

![dns](/images/blog/ssl/dns.png)

Next up is configuring CloudFlare. I switched the plan to free and the performance to CDN + Full optimizations.
![Configure CloudFlare](/images/blog/ssl/settings.png)


The last step is updating your nameservers. To do this log in to your domain provider and change the nameservers to the ones that CloudFlare gives you.
![dns](/images/blog/ssl/update-ns.png)

That’s it! It’s so easy to get setup. Changing your DNS can take up to 48 hours to propagate and CloudFlare’s free SSL can take up to 24 hours to set up, so while there should be no downtime, it might be a day or two before you have SSL and CloudFlare serving your website.

Now I can access my site on [https://mikeneumegen.com](https://mikeneumegen.com) or [http://mikeneumegen.com](http://mikeneumegen.com) and everything loads super fast.

Give it a go and get in touch if you have any problems setting CloudFlare up. We’re always here to help.
