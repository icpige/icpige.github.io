---
title: Editor Links
category: Features
author: ross
image: /images/blog/editor-links/blog-posts@2x.png
image_featured: false
---

Editor Links are a new CloudCannon feature to add in-app navigation around the editing interface.
Use them to create edit buttons on blog posts and collections items in the editor for your clients.

The links are anchor tags with an `href` attribute specific to CloudCannon.
The `href` is prefixed with *cloudcannon*, followed by a colon and the URL you wish to link to.
For example, the following links to the *Posts* page for your clients:

{% highlight html %}
<a href="cloudcannon:collections/_posts">Open Posts</a>
{% endhighlight %}

This link navigates to *https://app.cloudcannon.com/editor#/site/123/**collections/_posts***, the interface for managing blog posts.

### Generating Editor Links

Jekyll generates the paths you need in a number of situations. Adding an edit button to each blog post in a list is as easy as:

{% highlight html %}
{% raw %}{% for post in site.posts %}
  ...
  <a href="cloudcannon:collections/{{ post.path }}">Edit Post</a>
  ...
{% endfor %}{% endraw %}
{% endhighlight %}

This could generate *https://app.cloudcannon.com/editor#/site/123/**collections/_posts/2016-01-16-my-blog-post.md***,
navigating to the editor for that blog post.

![CloudCannon blog posts with edit buttons](/images/blog/editor-links/blog-posts.png){: .screenshot srcset="/images/blog/editor-links/blog-posts.png 800w, /images/blog/editor-links/blog-posts@2x.png 1600w"}

Our [documentation](https://docs.cloudcannon.com/editing/editor-links/) includes instructions for **Editor Links on collection items** and **hiding them in the live site**.

---

One of our customers requested this feature to create buttons to edit products from the Visual Editor.
We'd love to hear how you use it in the comments below.
