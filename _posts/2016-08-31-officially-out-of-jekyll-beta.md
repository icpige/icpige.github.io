---
title: Officially Out of Jekyll Beta
category: Announcements
author: ross
image: /images/blog/officially-out-of-jekyll-beta/logos@2x.png
image_featured: true
icons: true
---

Today marks a huge milestone for CloudCannon. We've finished the Jekyll beta, making Jekyll and plugins available to all users on every plan. The latest release also includes some requested features and more Jekyll configuration.

![CloudCannon and Jekyll logos](/images/blog/officially-out-of-jekyll-beta/logos.png){: srcset="/images/blog/officially-out-of-jekyll-beta/logos.png 800w, /images/blog/officially-out-of-jekyll-beta/logos@2x.png 1600w"}

***

### Plugins

Every new site created on CloudCannon from today is automatically built with Jekyll and supports custom plugins. To prevent breaking changes, existing sites need to create a `_config.yml` for CloudCannon to build with Jekyll. New and existing static sites that do not use Jekyll are still supported in full.

<i class="material-icons">extension</i> [Full documentation for Jekyll plugins](https://docs.cloudcannon.com/building/plugins/)
{: .list-item-with-icon}

***

### Versions

We've added support for all the latest versions of Jekyll, and have consolidated the way versions are set. The version is now set through a `Gemfile`. The default version for new Jekyll sites is **3.2.1**. You can always check what version of Jekyll is active on the *Status* section.

<i class="material-icons">history</i> [Full documentation for Jekyll versions](https://docs.cloudcannon.com/building/versions/)
{: .list-item-with-icon}

***

### Configuration

Jekyll has a number of command line options for builds. Setting these in the new *Site Settings* / *Build* section gives you more control and helps reproduce your local environment. The existing CloudCannon Optimisations and Jekyll Environment options have been moved to this section.

The most notable options are `baseurl`, `source`, and Jekyll's other custom directory options. These options are now supported in full as command line options and specified in `_config.yml`. The CloudCannon editing interface reads the `baseurl` and custom directories and reacts seamlessly.

<i class="material-icons">settings</i> [Full documentation for build configuration](https://docs.cloudcannon.com/building/configuration/#image-elements)
{: .list-item-with-icon}

![Site Settings Build Interface](/images/blog/officially-out-of-jekyll-beta/configuration.png){: .screenshot srcset="/images/blog/officially-out-of-jekyll-beta/configuration.png 800w, /images/blog/officially-out-of-jekyll-beta/configuration@2x.png 1600w"}

***

### Additional Features

To complement Jekyll's custom directories, we've created `uploads_dir` to change the location that images uploaded in the editor are stored. By default it's set to **uploads**. For consistency with Jekyll, the path is relative to the Jekyll `source` if set.

<i class="material-icons">photo</i> [Full documentation for image editable regions](https://docs.cloudcannon.com/editing/editable-regions/#image-elements)
{: .list-item-with-icon}

For those of you battling targeted spam email, we've added Google reCAPTCHA support in addition to our honeypot prevention.

<i class="material-icons">email</i> [Full documentation for contact forms](https://docs.cloudcannon.com/hosting/contact-forms/)
{: .list-item-with-icon}

![reCAPTCHA Example](/images/blog/officially-out-of-jekyll-beta/captcha.gif)

***

Thank you to everyone who provided feedback over the beta, you've shaped many of the features in our Jekyll support. We have plenty of new features and improvements planned and look forward to your continued feedback.
