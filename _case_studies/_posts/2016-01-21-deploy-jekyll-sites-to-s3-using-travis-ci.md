---
title: Deploy Jekyll sites to S3 using Travis CI
category: Tutorial
author: mike
image: /images/blog/travis-ci/travis-ci.png
image_featured: true
---

CloudCannon consolidates editing and reliable hosting into a single package. Alternatively, you can use external hosting solutions and keep editing in CloudCannon. To demonstrate this workflow, we will use [Amazon S3](http://aws.amazon.com/s3/), a great platform to host static and Jekyll websites. The uptime is [99.9% guaranteed](http://aws.amazon.com/s3/sla/), it scales indefinitely and it's cheap.

This tutorial shows you how to automatically deploy changes from CloudCannon/GitHub to S3 using [Travis CI](https://travis-ci.org/).

## Setup

To begin, [sign up for Travis CI](https://travis-ci.org/).

![Travis CI Homepage](/images/blog/travis-ci/travis-ci.png){:.screenshot}

Travis CI requires access to your GitHub account.

![GitHub authentication](/images/blog/travis-ci/travis-auth.png){:.screenshot}

Click the add button next to My Repositories.

![Travis CI Dashboard](/images/blog/travis-ci/my-repo.png){:.screenshot}

Enable the repository with your Jekyll site.

![Repository list](/images/blog/travis-ci/enable-repo.png){:.screenshot}

## Scripts

Travis CI needs a [configuration script](https://docs.travis-ci.com/user/customizing-the-build/) in your repository describing how to build and deploy your site.

The script will set up Ruby:

{% highlight yaml %}
language: ruby
rvm:
  - 2.1
{% endhighlight %}

Install the `jekyll` and `s3_website` gems. `s3_website` copies the website files to S3.

{% highlight yaml %}
install: gem install jekyll -v 2.4.0 && gem install s3_website
{% endhighlight %}

Build the site using Jekyll:

{% highlight yaml %}
script: jekyll build
{% endhighlight %}

Then copy the generated site to S3:

{% highlight yaml %}
after_success: s3_website push
{% endhighlight %}

Copy the script to `.travis.yml` in your repository:

{% highlight yaml %}
language: ruby
rvm:
  - 2.1
install: gem install jekyll -v 2.4.0 && gem install s3_website
script: jekyll build
after_success: s3_website push
{% endhighlight %}

`s3_website` needs a configuration file describing how and where to deploy the site. Your S3 Secret Key is private so it's a bad idea to put it in your public repository. Instead, you can set and use an environment variable in Travis CI.

Copy the script to `s3_website.yml` and change `s3_bucket` to the name of your S3 bucket:

{% highlight yaml %}
s3_id: <%= ENV['S3_ACCESS_KEY_ID'] %>
s3_secret: <%= ENV['S3_SECRET_KEY'] %>
s3_bucket: your.bucket.com
{% endhighlight %}

## Environment Variables

The final step is to set environment variables for the S3 Access Key and Secret Key.

Go to settings and add your Amazon S3 credentials:

![Travis CI environment variables](/images/blog/travis-ci/settings.png){:.screenshot}

Every time you make a change to the repository in GitHub (including edits from [CloudCannon](http://cloudcannon.com)), Travis CI rebuilds the site and deploys it to S3.

## Summary

[Travis CI](https://travis-ci.org/) is flexible enough to keep the easy-to-use editing in CloudCannon and host your site anywhere.
