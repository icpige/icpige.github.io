---
title: Publishing Workflows for Jekyll Editors
author: mike
category: Tips
image: /images/blog/git-workflows/banner.png
image_featured: true
---


Our main goal at CloudCannon is to make collaboration between developers and non-technical editors seamless. To an extent we've achieved this with editing a Jekyll site; editors can update HTML, Markdown, front matter, blog posts, collections and data files without knowing anything about Jekyll or HTML but what about Git? Recently we've been working to achieve the same level of integration for Git workflows. In this post I'm going over some of the new workflows CloudCannon supports.

## Two way Git syncing

This is a feature we've supported for a while but it's worth mentioning as it's at the core of what we're trying to achieve. You can sync a repository from GitHub or Bitbucket to a site on CloudCannon. Developers work locally using their own tools, editors update on CloudCannon, and everything stays in sync through a central Git repository.

![two way Git syncing](/images/blog/git-workflows/2-way-syncing.svg)

## Staging sites

You might want to preview new content or changes on a live testing website before pushing it to your production site. In CloudCannon, staging sites are achieved using Git branches.

We recommend you set up a CloudCannon site which syncs with the master branch on your repository. Editors can update this site without worrying about messing up the live site. They can see their changes on a live site with a testing domain. For the live site you create another CloudCannon site which syncs with a production branch in your repository. When changes from the staging site need be pushed to production you simply merge master into production.

![two way Git syncing](/images/blog/git-workflows/staging-sites.svg)

## Build options and environments

Build options allow you to customise the build differently for each environment. For example, on your staging site you can publish draft posts so editors can preview them on the live site. On your production site you probably want them hidden. To achieve this you can enable the `--drafts` flag in **Site Settings** / **Build** on your staging site.

![build settings](/images/blog/git-workflows/build-settings.png){: .screenshot}

`--limit-posts` is another useful option for your staging site. If you have a site with thousands of posts your editors will spend minutes waiting for it to build after making a change. For the staging site you can limit the number of posts that get published to drastically decrease build time.

Jekyll environments are a way to switch on/off features for particular environments. I've found the most common usecase for this is to only output the Google Analytics snippet in production. You can set the environment using the `JEKYLL_ENV` environment variable. Locally you can do this on the command when you run Jekyll:

```
JEKYLL_ENV=production bundle exec jekyll serve
```

On CloudCannon you can do this in your **Site Settings** / **Build**.

Then in can access the current environment in liquid using `jekyll.environment`. To only output the Google Analytics snippet in production you would do this:

{% raw %}
```
{% if jekyll.environment == "production" %}
  (function(i,s,o,g,r,a,m){i["GoogleAnalyticsObject"]=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,"script","//www.google-analytics.com/analytics.js","ga");

    ga("create", key , param);
    ga("require", "linkid", "linkid.js");
    ga("send", "pageview");
  </script>
{% endif %}
```
{% endraw %}

## Merging

Merging allows editors to perform a merge from one branch to another in CloudCannon. This is typically for pushing a staging branch to production but there's nothing stopping you from using it with more complex workflows. To set this up go to **Site Settings** / **Storage Providers**. Add the publishing branch you want to merge into and make sure Publishing Mode is set to Merge. Once that is set up, Editors will have a publish button when they're editing the site. When they press this it will perform the merge to the publishing branch.

![merge](/images/blog/git-workflows/merge.png){: .screenshot}

## Pull Requests

Pull Requests are similar to merging but instead of performing a merge it creates a Pull Request. When a Pull Request is created, CloudCannon waits for any tests to run on the PR and then shows a merge button to the editor if everything passed. Using Pull requests gives you an extra safety net as you can run your own testing scripts on a continuous integration service. To set this up on CloudCannon following the Merging instructions but change the Publishing Mode to Pull Request.

## Wrap up

Looking forward we're considering ways we can give editors an even deeper Git experience. We're asking ourselves:

* How can we support hundreds of editors updating content on single site?
* How can we provide enough information so a non-technical editor can confidently merge a PR and know what is changing?
* How can we empower editors to try radical new ideas without fearing they'll break the site if they need to undo them?

If we can answer these questions, we will enable workflows that have never before been possible with a CMS.