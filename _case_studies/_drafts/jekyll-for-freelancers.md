---
title: Jekyll for Freelancers
author: mike
category: Tips
image: /images/blog/jekyll-for-freelancers/freelancer.jpg
image_featured: true
---


Jekyll is a fantastic tool for freelancers building websites for clients. Compared to a CMS like WordPress, there's a number of advantages:

* **Simplicity** - There's no complex templating or plugins to understand. Building a Jekyll site is similar to building a purely static website.
* **Maintenance** - You don't need to worry about updating CMS software or plugins. Jekyll outputs a static website which are significantly faster, secure and stable than a typical WordPress setup.
* **Backups**- Jekyll sites are typically connected to a Git repository which is used to undo unwanted changes by a client and as a full backup of all the files on the site.

With a Jekyll CMS like [CloudCannon](https://cloudcannon.com), you can have all these advantages&nbsp;**and** have an easy-to-use interface for clients to update content. In this post we're going over tips for freelancers to get the most out of CloudCannon.

## Client Sharing

[Client Sharing](https://docs.cloudcannon.com/sharing/client-sharing/)&nbsp;is a CloudCannon feature specifically for freelancers/agencies. Instead of each client needing their own CloudCannon account, you can set up a Client Sharing password in *Site Settings / Client Sharing.* Clients can then go to https://theirwebsite.com/update/ and enter the password. This gives the client full access to update content on their site. Client Sharing is available on paid plans at no extra charge.

![](/uploads/versions/client-login---x----1679-1049x---.png)

## Start with a template

CloudCannon has [templates available](https://learn.cloudcannon.com/jekyll-templates/)&nbsp;to help kickstart your next project. These templates are designed to get the most out of CloudCannon so they're a great starting point or reference of what's possible on CloudCannon.

![](/uploads/versions/template---x----1900-1188x---.png)

## Staging site

Having your client update the production site directly can be a dangerous game. Setting up a staging site which the client can promote to production gives them a chance to fix mistakes before the website is pushed live. To set up a staging site:

* Create another branch in your repository
* Create a new site in CloudCannon and sync it with your new branch
* Add a [publishing branch](https://docs.cloudcannon.com/syncing/publishing/) to your production branch

## Form submissions

Client sites often have a contact form. CloudCannon has a [Contact Forms](https://docs.cloudcannon.com/hosting/contact-forms/) feature you can use to set up an HTML form. When the form is submitted, the content is emailed to the client.

![](/uploads/versions/contact---x----1900-1188x---.png)

## Documentation site

We do our best to make CloudCannon as easy to use as possible. However, it can be valuable give the client instructions of how to perform common tasks to help them in a pinch. This is a great use case for the CloudCannon edition template. Simply add instructions and screenshots and your client has their own documentation website.

![](/uploads/versions/edition---x----1900-1188x---.png)

## Customise the Client interface

Add links to your support email and documentation to the client interface so your client can get help when they need it. These options are found in *Site Settings / Client Interface*.

## ![](/uploads/versions/client-interface---x----1900-1188x---.png)

## White Labelling

On the CloudCannon Professional plan you can completely white label the application. In *Organisation Settings / Details* you can set a colour for the sidebar, and logos for the sidebar, client editor and sites list.

![](/uploads/versions/branding---x----1900-1188x---.png)

## Summary

That's all the tips we have for today. Let us know if you have any other tricks when working with clients on CloudCannon.