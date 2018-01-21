---
title: Lightweight Social Share Buttons
category: Tutorials
author: ross
image: /images/blog/lightweight-social-share-buttons/buttons@2x.png
image_featured: true
---

Social share buttons are commonplace on a range of websites. Many social networks provide embed code to build them quickly, however these solutions load a number of additional requests and resources. This increases the load time for your pages.

Social networks also provide a share URL as an alternative to the embedded buttons. Linking to these with a few `<a>` tags is much more lightweight than the embedded solution. They can also be styled to match your branding. Here are the buttons and everything you need to recreate them for your site:

{% assign url = page.url | prepend: site.url | cgi_escape %}

<div class="well">
<a class="share-button facebook" href="https://www.facebook.com/sharer/sharer.php?u={{ url }}" target="_blank">{% include svgs/social-icon.html icon="facebook" %} Share on Facebook</a>

<a class="share-button twitter" href="https://twitter.com/intent/tweet?url={{ url }}&amp;text={{ page.title | markdownify | strip_html | cgi_escape }}%20on%20{{ site.title | cgi_escape }}&amp;via=CloudCannon" target="_blank">{% include svgs/social-icon.html icon="twitter" %} Tweet this</a>

<a class="share-button google-plus" href="https://plus.google.com/share?url={{ url }}" target="_blank">{% include svgs/social-icon.html icon="google-plus" %} Share on Google+</a>

<a class="share-button pinterest" href="https://pinterest.com/pin/create/button/?url={{ url }}&description={{ page.title | cgi_escape }}&media={{ page.image | prepend: site.url | cgi_escape }}" target="_blank">{% include svgs/social-icon.html icon="pinterest" %} Save on Pinterest</a>

<a class="share-button linkedin" href="https://www.linkedin.com/shareArticle?url={{ url }}&title={{ page.title | cgi_escape }}&source=CloudCannon&mini=true" target="_blank">{% include svgs/social-icon.html icon="linkedin" %} Share on LinkedIn</a>

<a class="share-button tumblr" href="
https://tumblr.com/widgets/share/tool?canonicalUrl={{ url }}&tags=jekyll,webdev,webdesign&caption={{ page.title | cgi_escape}}" target="_blank">{% include svgs/social-icon.html icon="tumblr" %} Share on Tumblr</a>

<a class="share-button reddit" href="https://reddit.com/submit?url={{ url }}&title={{ page.title | cgi_escape }}&resubmit=true" target="_blank">{% include svgs/social-icon.html icon="reddit" %} Submit to Reddit</a>

<a class="share-button hacker-news" href="https://news.ycombinator.com/submitlink?u={{ url }}&t=={{ page.title | cgi_escape | replace: "+", "%20" }}" target="_blank">{% include svgs/social-icon.html icon="hacker-news" %} Submit to Hacker News</a>

<a class="share-button designer-news" href="https://www.designernews.co/submit?url={{ url }}&title={{ page.title | cgi_escape }}" target="_blank">{% include svgs/social-icon.html icon="designer-news" %} Submit to Designer News</a>

<a class="share-button email" href="mailto:?subject={{ page.title | cgi_escape }}&body={{ "Check this out: " | cgi_escape | append: url }}" target="_blank">{% include svgs/social-icon.html icon="email" %} Share via Email</a>
</div>

<!-- ![Social icons on a phone](/images/blog/lightweight-social-share-buttons/social-icon-grid.jpg){: srcset="/images/blog/lightweight-social-share-buttons/social-icon-grid.jpg 800w, /images/blog/lightweight-social-share-buttons/social-icon-grid@2x.jpg 1600w"} -->

---

## Markup and Styling

The social share buttons can be a simple anchor tag:

{% highlight html %}
<a href="https://www.facebook.com/sharer/sharer.php?u=http%3A%2F%2Fexample.com%2F"
  class="share-button facebook"
  target="_blank">Share on Facebook</a>
{% endhighlight %}

Adding an icon has a nice effect (from [materialdesignicons.com](https://materialdesignicons.com/)):

{% highlight html %}
<a href="https://www.facebook.com/sharer/sharer.php?u=http%3A%2F%2Fexample.com%2F"
  class="share-button facebook"
  target="_blank">
    <svg fill="#000000"
      height="24"
      viewBox="0 0 24 24"
      width="24"
      xmlns="http://www.w3.org/2000/svg"><path d="M19,4V7H17A1,1 0 0,0 16,8V10H19V13H16V20H13V13H11V10H13V7.5C13,5.56 14.57,4 16.5,4M20,2H4A2,2 0 0,0 2,4V20A2,2 0 0,0 4,22H20A2,2 0 0,0 22,20V4C22,2.89 21.1,2 20,2Z" /></svg> Share on Facebook</a>
{% endhighlight %}

Here are the base styles for the buttons shown above:

{% highlight css %}
.share-button {
  position: relative;
  display: inline-flex;
  align-items: center;
  padding: 3px 10px 2px 8px;
  margin: 10px;
  color: #fff;
  background-color: #333;
  border-radius: 2px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.35);
  text-decoration: none;
  font-family: "Open Sans", Helvetica, Arial, sans-serif;
  font-weight: 600;
  font-size: 15px;
}

.share-button:hover {
  color: #fff;
  background-color: #4f4f4f;
}

.share-button:active {
  top: 1px;
  box-shadow: 0 0 1px rgba(0, 0, 0, 0.25);
}

.share-button svg {
  fill: #ffffff;
  width: 19px;
  height: 19px;
  margin-right: 5px;
}

.share-button.facebook { background-color: #4A66B7; }
.share-button.facebook:hover { background-color: #556fbb; }

.share-button.twitter { background-color: #1B95E0; }
.share-button.twitter:hover { background-color: #269ce5; }

.share-button.pinterest { background-color: #c92228; }
.share-button.pinterest:hover { background-color: #cf4146; }

.share-button.linkedin { background-color: #0077B5; }
.share-button.linkedin:hover { background-color: #1e84b9; }

.share-button.reddit { background-color: #5f99cf; }
.share-button.reddit:hover { background-color: #75a6d4; }

.share-button.tumblr { background-color: #35465c; }
.share-button.tumblr:hover { background-color: #455166; }

.share-button.hacker-news { background-color: #ff6600; }
.share-button.hacker-news:hover { background-color: #ff7515; }

.share-button.designer-news { background-color: #2d72d9; }
.share-button.designer-news:hover { background-color: #3d82e9; }

.share-button.google-plus {
  background-color: #fefefe;
  color: #333;
}

.share-button.google-plus:hover {
  background-color: #f6f6f6;
  color: #333;
}

.share-button.google-plus svg {
  fill: #DB4437;
}
{% endhighlight %}

---

## URL Reference

Here's a collated list of provided share URLs and relevant options. It's better to only use relevant share buttons rather than bombard the visitor with every choice.

Options are provided for developers to customise the share experience. Previews are often generated automatically from the URL provided. The `url` query parameter is required on all share URLs except email:

* **url** is the encoded URL to share

#### Facebook

`https://www.facebook.com/sharer/sharer.php?u={url}`

#### [Google+](https://developers.google.com/+/web/share/#share-link)

`https://plus.google.com/share?url={url}`

#### [Twitter](https://dev.twitter.com/web/tweet-button/web-intent)

`https://twitter.com/intent/tweet?url={url}&text={text}&via={via}&hashtags={hashtags}`

* **text** is the pre-populated Tweet text
* **hashtags** is a comma separated list of pre-populated hash tags (without "#")
* **via** is the Twitter username of the author or site (without "@")

#### [Pinterest](https://developers.pinterest.com/docs/widgets/save/)

`https://pinterest.com/pin/create/button/?url={url}&description={description}&media={media}`

* **description** is the caption text for the pin
* **media** is an encoded URL to the image in the pin

#### [LinkedIn](https://developer.linkedin.com/docs/share-on-linkedin)

`https://www.linkedin.com/shareArticle?url={url}&title={title}&source={source}&summary={summary}&mini=true`

* **title** is the pre-populated title of the share
* **summary** is the description for the share
* **source** is your site name

#### [Tumblr](https://www.tumblr.com/docs/en/share_button#custom-button)

`https://tumblr.com/widgets/share/tool?canonicalUrl={url}&tags={tags}&caption={caption}`

* **tags** is a comma separated list of tags
* **caption** is the pre-populated share text

#### [Reddit](https://www.reddit.com/dev/api#POST_api_submit)

`https://reddit.com/submit?url={url}&title={title}&resubmit=true`

* **title** is the submission heading text

#### Hacker News

`https://news.ycombinator.com/submitlink?u={url}&t={title}`

* **title** is the submission heading text

#### Designer News

`https://www.designernews.co/submit?url={url}&title={title}`

* **title** is the submission heading text

#### Email

`mailto:?subject={subject}&body={body}`

* **subject** is the subject line of the pre-populated email
* **body** is the body text content of the pre-populated email

---

## Tips

The previews that are automatically generated rely heavily on the [Open Graph](http://ogp.me/) tags and well structured content. Facebook offers [this preview tool](https://developers.facebook.com/tools/debug/og/object/) to test and ensure these are set correctly on your sites.

Encode your query parameters on your Jekyll sites with the `cgi_escape` filter. The `uri_escape` filter is for entire URLs, so won't escape some characters (e.g. "?") and may break your buttons.

{% highlight html %}{% raw %}
<a class="share-button facebook" href="https://www.facebook.com/sharer/sharer.php?u={{ page.url | prepend: site.url | cgi_escape }}" target="_blank">Share on Facebook</a>
{% endraw %}{% endhighlight %}

Jekyll includes are a great way to reuse SVG icons in your buttons and the rest of a site.

{% highlight html %}{% raw %}
<a class="share-button facebook" href="https://www.facebook.com/sharer/sharer.php?u={{ page.url | prepend: site.url | cgi_escape }}" target="_blank">{% include facebook-icon.html %} Share on Facebook</a>
{% endraw %}{% endhighlight %}

Tracking your share buttons can help you discover how your visitors are interacting with the site and focus your resources more effectively. Track [custom events](https://developers.google.com/analytics/devguides/collection/analyticsjs/events) for clicks on your share buttons with Google Analytics using the following code:

{% highlight javascript %}
var buttons = document.getElementsByClassName("share-button");

for (var i = 0; i < buttons.length; i++) {
  buttons[i].addEventListener("click", function handleOutboundLinkClicks(event) {
    ga("send", "event", {
      eventCategory: "Outbound Link",
      eventAction: "click",
      eventLabel: event.target.href,
      transport: "beacon"
    });
  });
}
{% endhighlight %}

---

Be sure to let us know in the comments if you've got any tips or feedback on the topic!