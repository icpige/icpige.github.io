---
title: Speed up your Jekyll builds
author: mike
category: Tips
image: /images/blog/build-performance/banner.png
image_featured: true
---


Having a short Jekyll build time helps you iterate faster while developing and goes a long way to improving the experience for editors on CloudCannon. In this post, we're going over how to identify bottlenecks in your Jekyll build and tips on how to address them.

## Jekyll version

Let's start with the easiest way to improve build performance, update your Jekyll version! In the last year, Jekyll has made great strides in decreasing build time. Keeping an eye on the [Jekyll news page](https://jekyllrb.com/news/) and making sure you're using the latest version will go a long way to keeping your build time down.

## Exclude folders

Developers often use tools like Bower and npm to manage JavaScript and CSS libraries. These tools are great but they often produce files which aren't required when building your site. You can add these files to the `exclude:` key in your `_config.yml`. This prevents Jekyll having to copy them around every build.

## Environment

Your production site might need to be translated into multiple languages and have everything compressed to ensure fast load times but you can avoid these while developing. Turning these off for development along with limiting the number of posts output with `--limit_posts` and not switching on `--lsi` will have a massive impact on build performance.

## Liquid

Liquid is often the a culprit in slow build times. The first port of call for optimising Liquid is using the Liquid profiler:

```bash
bundle exec jekyll build --profile
```

This prints out a report of where Jekyll is spending its time rendering Liquid:

```
Filename                         | Count |    Bytes |  Time
---------------------------------+-------+----------+------
_layouts/default.html            |   222 | 3849.23K | 2.766
_layouts/post.html               |    52 | 1191.25K | 0.896
_includes/footer.html            |   222 |  536.36K | 0.596
_includes/navigation.html        |   222 |   87.61K | 0.345
_includes/listings.html          |    65 |  516.76K | 0.274
cheat-sheet.html                 |     1 |  146.32K | 0.224
_includes/social-icon.html       |   275 |  524.88K | 0.213
_includes/render_cheat.html      |     1 |  136.16K | 0.193
_includes/relative-src.html      |   222 |   36.21K | 0.173
_includes/right-navigation.html  |   222 |  119.24K | 0.124
sitemap.xml                      |     1 |    9.11K | 0.078
_includes/document-icon.html     |   295 |  186.81K | 0.078
_layouts/archive.html            |    12 |   54.95K | 0.077
_includes/search.html            |   222 |  129.86K | 0.076
_includes/share-section.html     |    52 |   48.80K | 0.063
feed.xml                         |     1 |   81.00K | 0.053
_includes/featured-icon.html     |    21 |  219.76K | 0.053
index.html                       |     1 |   36.62K | 0.047
```

The profiler gives us a baseline we can use to optimise individual files. Let's take `_includes/footer.html` from the example above. It's an include that iterates over a data file array:

{% raw %}

```html
<footer>
  <ul class="footer-links">
    {% for footer_item in site.data.footer %}
      <li>
        <a href="{{ footer_item.link }}">{{ footer_item.name }} </a>
     </li>
    {% endfor %}
  </ul>
</footer>
```

{% endraw %}

This Liquid for loop looks innocent enough but it's included on every page on the site. If there are 1000 pages Jekyll has to execute this for loop 1000 times. The easiest way to optimise this is to output static HTML and avoid using Liquid:

```html
<footer>
  <ul class="footer-links">
      <li>
        <a href="https://facebook.com/CloudCannon">
          Facebook
        </a>
      </li>
      <li>
        <a href="https://twitter.com/CloudCannon">
          Twitter
        </a>
      </li>
      <li>
        <a href="https://www.youtube.com/channel/UC8CXR0-3I70i1tfPg1PAE1g">
          YouTube
        </a>
      </li>
      <li>
        <a href="/feed.xml">
          RSS
        </a>
      </li>
  </ul>
</footer>
```

This is much faster but makes the site harder to maintain. We need the best of both worlds, the speed of static while having the flexibility of Liquid. We can achieve this with a Jekyll plugin. The plugin will generate the footer the first time it's run then cache the result for subsequent requests.

Ben Balter has solved this for us with his [jekyll-include-cache](https://github.com/benbalter/jekyll-include-cache) plugin. To install add `jekyll-include-cache` to your `Gemfile` then run `bundle install`. Instead of calling {% raw %}`{% include footer.html %}`{% endraw %} we call {% raw %}`{% include_cached footer.html %}`{% endraw %}. This took the execution time of this file from 0.596 to 0.001.

The footer is easy to cache as is exactly the same on every page. Let's look at something that isn't the same on every page, the main navigation. `_includes/navigation.html` iterates over a data file, outputs a link and name then adds an&nbsp;`active` class if the link is the current page:

{% raw %}

```html

<nav>
  {% for nav_item in site.data.navigation %}
    <a href="{{ nav_item.link }}" {% if nav_item.link == page.url %}class="active"{% endif %}>
      {{ nav_item.name }}
    </a>
  {% endfor %}
</nav>
```

{% endraw %}

If we included this using `include_cached` the `active` class will be on the same link on every page as it will execute it once then use that version for subsequent includes. We need to move the active page logic outside the include so we can cache it properly:

{% raw %}

```html
<nav>
  {% for nav_item in site.data.navigation %}
    <a href="{{ nav_item.link }}">{{ nav_item.name }}</a>
  {% endfor %}
</nav>
```

{% endraw %}

From here we could rely on JavaScript/JQuery to add the active class:

```javascript
$(function() {
  $('nav a[href^="/' + location.pathname.split("/")[1] + '"]').addClass('active');
});
```

Or we could add a class to identify each `nav` item and a class to body in `_layouts/default.html` to identify the current page. The HTML output of the about page would look something like this:

```html
...
<body class="about">
  <nav>
    <a href="/home/" class="home">
      Home
    </a>
    <a href="/about/" class="about">
      About
    </a>
    <a href="/contact/" class="contact">
      Contact
    </a>
  </nav>
...
```

We can use those classes to highlight the current page:

```css
.home .home, .about .about, .contact .contact { // Styles for active link
  color: red;
  text-decoration: underline;
}
```

If you're hosting on CloudCannon, a class of `cc-active` is automatically added to any links which point to the current page. This allows you to simply add a style for `.cc-active` in your CSS for active link highlighting.

## Gems

While it can be tempting to add every Jekyll plugin under the sun to your site, they can have a big impact on your build performance. The best way to understand the impact is to profile before and after adding a plugin. If you identify a slow plugin there are a few workarounds to consider:

### Do you actually need the plugin?

When I started using Jekyll I thought pagination was essential for any blog, however, our analytics told a different story. I realised that very few people click through the pagination pages, they're simply a way for search engines to find content. Instead of using pagination now we have a [blog page](/blog/) which has our ten most recent posts and an&nbsp;[archived page](/archived/) which has the rest. No plugins necessary.

### Can you do this in frontend instead?

One of the great things about Jekyll is you can have a piece of content which is used in different forms around your site. With a plugin like [jekyll-picture-tag](https://github.com/robwierzbowski/jekyll-picture-tag) you can also apply this logic to images. For example, you might want to generate thumbnails on the fly for a series of photo gallery images. Instead of doing this in Jekyll using a plugin you can use a 3rd party so it doesn't slow down your build. [Imgix](https://www.imgix.com/), [Cloudinary](https://cloudinary.com/) and [weserv](https://images.weserv.nl/) are all great candidates for doing this. You just need to tweak your image source so it's loaded from one of these services:

```html
<img src="//images.weserv.nl/?url=mywebsite.com/cloud.jpg&w=300">
```

### Can you do this with a post-processing tool?

People have come up with ways to minify HTML using a layout, have a full asset pipeline inside Jekyll and perform other post-processing tasks. I would argue that while it's nice to have one tool do everything, they sit outside the scope of what Jekyll should be trying to do. [Grunt](https://gruntjs.com/) and [Gulp](https://gulpjs.com/) will perform much faster for these tasks and already have a huge library of scripts you can use.

## Conclusion

Jekyll has come a long way in decreasing building time. Knowing some of the constraints and working around them should give you speedy build times. If you have any other build performance tips and tricks leave them in the comments below!