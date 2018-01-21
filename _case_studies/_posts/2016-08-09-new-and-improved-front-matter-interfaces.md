---
title: New and Improved Front Matter Interfaces
category: Features
author: ross
image: /images/blog/new-and-improved-front-matter-interfaces/front-matter-example@2x.png
image_featured: false
---

CloudCannon is committed to bringing the best editing experience to static and Jekyll sites. We provide world-class interfaces for your clients to update their front matter, a core offering of Jekyll.

![Example front matter in CloudCannon](/images/blog/new-and-improved-front-matter-interfaces/front-matter-example.png){: .screenshot srcset="/images/blog/new-and-improved-front-matter-interfaces/front-matter-example.png 800w, /images/blog/new-and-improved-front-matter-interfaces/front-matter-example@2x.png 1600w"}

There are more front matter interfaces available in CloudCannon than ever before. We've recently added more interfaces and ways to customise them.

---

### Rich Text

Front matter is often useful for sections of text outside the main content of a page. Give clients control of formatting with this new interface. The `_html` and `_markdown` conventions set what format the text is saved in. Markdown values need to be run through the `markdownify` filter to render with formatting.

<i class="material-icons">format_size</i> [Full documentation for rich text](https://docs.cloudcannon.com/editing/front-matter/#rich-text)
{: .list-item-with-icon}

### Colour

Giving your clients access to colour doesn't always bode well for a design's longevity, but there are some good use cases. CloudCannon supports [colour in front matter](https://docs.cloudcannon.com/editing/front-matter/#colour) by providing a unified selector that outputs to a number of formats. You control the format, the client controls the colour.

<i class="material-icons">color_lens</i> [Full documentation for colour](https://docs.cloudcannon.com/editing/front-matter/#colour)
{: .list-item-with-icon}

### Multiselect

If you've ever needed an interface for clients to select a subset of items from a predefined set of values, this is for you. CloudCannon populates values from arrays or objects defined in `_config.yml`, and collection items. Clients add collection items to create additional values in the set.

<i class="material-icons">view_list</i> [Full documentation for multiselect](https://docs.cloudcannon.com/editing/front-matter/#multiselect)
{: .list-item-with-icon}

### Social

There's a undeniable link between social media and websites, and your editing interfaces are now aware of it. Social interfaces are text fields for URLs and IDs. Profile pictures are displayed alongside, validating for clients they have the correct URL and adding detail to an otherwise plain text field.

<i class="material-icons">person</i> [Full documentation for social](https://docs.cloudcannon.com/editing/front-matter/#social)
{: .list-item-with-icon}

### Documents and Images

The file selector based on the `_path` convention has been incredibly popular in sites on CloudCannon, especially for images. The new `_image_path` and `_document_path` conventions now allow you to restrict the files to images or documents only. Using a more specific type provides a better editing experience and reduces potential mistakes from clients.

<i class="material-icons">description</i> [Full documentation for files](https://docs.cloudcannon.com/editing/front-matter/#file)
{: .list-item-with-icon}

### Number

Take advantage of the HTML5 number input and store client-entered values numerically.

<i class="material-icons">casino</i> [Full documentation for numbers](https://docs.cloudcannon.com/editing/front-matter/#number)
{: .list-item-with-icon}

---

### Comments

Comments provide instructional or guide text for any interface. Use this heavily requested feature to remind clients of specific details for each interface when a title or key just isn't enough.

<i class="material-icons">comment</i> [Full documentation for comments](https://docs.cloudcannon.com/editing/front-matter/#comment)
{: .list-item-with-icon}

### Array Defaults

When clients create new items in an array, the keys and interface types are created from the previous item to ensure a consistent interface. You can now [provide the starting values for the new item](https://docs.cloudcannon.com/editing/front-matter/#array-defaults) as well.

Set starting values for variables to the most likely value, and set starting images to placeholders (retaining the path when the client selects another)

<i class="material-icons">library_add</i> [Full documentation for array defaults](https://docs.cloudcannon.com/editing/front-matter/#array-defaults)
{: .list-item-with-icon}

### Full Width Editor

Collection items are often files containing only front matter. Dedicating most of the editor to the content isn't the best experience in this scenario. Hide the content area by setting `_hide_content: true` in your front matter.

<i class="material-icons">edit</i> [Full documentation for full width editor](https://docs.cloudcannon.com/editing/content-editor/#hiding-the-content-area)
{: .list-item-with-icon}

---

The [full documentation for front matter](https://docs.cloudcannon.com/editing/front-matter/) has details and examples to get you set up with any of these interfaces and customise them further.

We have more interfaces planned, keep track of all the changes in CloudCannon in [the change log](https://docs.cloudcannon.com/changelog/). As always, we are keen to hear your feedback on the new features and look forward to seeing what you build with them.
