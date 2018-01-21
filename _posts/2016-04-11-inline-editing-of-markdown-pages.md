---
title: Inline editing of Markdown pages
category: Features
author: george
image: /images/blog/inline-editing/markdown-editable@2x.png
image_featured: false
---

This week we announce a long awaited feature, inline editing of Markdown files. This feature means that posts, collection items or pages written in Markdown are editable in the Visual Editor automatically. The ability to edit posts in context has been requested since the introduction of blogging.

This feature works for all existing and new sites. The only requirement of this feature is that the `{% raw %}{{ content }}{% endraw %}` block is the only child of its parent element. For example:


{% highlight html %}
{% raw %}
<div>{{ content }}</div>
{% endraw %}
{% endhighlight %}

The controls available to the editable region are decided by the parent element. `span` has less controls than a `p` which has less controls than a `div`. See our [Editable Regions documentation](https://docs.cloudcannon.com/editing/editable-regions/) for more information.

![CloudCannon inline editing of our blog](/images/blog/inline-editing/markdown-editable.png){: .screenshot srcset="/images/blog/inline-editing/markdown-editable.png 800w, /images/blog/inline-editing/markdown-editable@2x.png 1600w"}


The Content Editor is still available for sites that are not set up for inline editing. Existing sites need to trigger a build to access the new feature.

---

If you need help or have any feedback you can always contact support.
Alternatively, let us know in the comments below.
