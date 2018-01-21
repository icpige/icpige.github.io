---
title: Powerful Templates
author: mike
category: Features
image:
image_featured: false
permalink: /features/2013/08/21/powerful-templates.html
---
You guys asked for it, and here it is. We're so excited to announce the brand new templating feature in CloudCannon.

Making changes to the structure of your website can involve editing every single HTML page. Now you can include a file into your HTML pages making __updating a breeze__.

![Includes](/images/blog/includes.png)

We've made the syntax as simple as possible. Simply adding `<!-- include header.html -->` to an HTML page is all you need to save yourself some __serious time__. See our docs for a more detailed explanation.

We've also launched a few smaller features to make __your life easier__. Adding an underscore to the beginning of a file name (e.g. \_header.html) makes the file inaccessible to visitors and hides it from your clients in the client editor. This is great if you have templating files lying around you don't want anyone to see.

Highlighting the current page is much more __straightforward__. CloudCannon automatically adds a class of `cc-active` to any `<a>` tags which point to the current page. Using some CSS and the `cc-active` class you can highlight the active page (Especially useful when you're using includes).
