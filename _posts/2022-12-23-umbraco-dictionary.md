---
layout: post
title: "Why You Need The Umbraco Dictionary"
description: "Here are a few good reasons to get started using the Umbraco Dictionary today and how to use it."
date: 2022-12-23
feature_image:
tags: [umbraco11]
disqus_enabled: true
og_type: article
og_image: /images/posts/2022-12-23/og-dictionary.jpg
---

When setting up a content management system, it is important to make content editable within the CMS. In some cases the content may be utilized in multiple places throughout the site in a macro, or doesn’t warrant having its own property on a content page. <!--more-->

## Make Content Editable

Take a search input for example. It's likely your content has a search page specifically setup with its own template to render search results, however, the search input label, placeholder and submit button are usually strings that are less often updated after the initial setup. While these could be properties on the search page, these types of properties are better suited to be managed somewhere else and not clutter the content page with properties that will be rarely updated.

## Translate Your Content

Enter the Umbraco Dictionary. The Umbraco Dictionary is perfectly suited to handle the needs of allowing content editors to update content that would typically be “hard-coded” into a template. Utilizing the Dictionary also allows for the ability to customize the content based on the localization information. 

If you have a multi-lingual website and are utilizing the Umbraco Dictionary items in your templates, when your website is loaded using a different language, your dictionary items will utilize the content specified for that language. This makes one less thing you need to worry about and future proofs your website for multi-lingual support.

## Deploy Less

Another benefit of utilizing the Umbraco Dictionary is that when you no longer have "hard-coded" text values in your macros or templates, you won't have to do a deployment for a simple text change. Looking at the deployment process as a whole and taking into account the multiple environments that changes get pushed through, the QA process, scheduling a deployment and verifying that deployment was sucessful. There is a lot of time and effort cross-department that goes into a deployment. You can save a lot of valuable time with allowing content editors the ability to utilize content from the Dictionary to make text changes instead of "hard-coding" text inside of your views.

## Using the Dictionary

Using our search example, in the Umbraco Dictionary, we could create a group called Search to house a few dictionary items to handle editing these values in the CMS including:

1. Search.Placeholder
2. Search.Label
3. Search.Button

The Umbraco Dictionary can be found in the Translation section of Umbraco and creating new dictionary items is as easy as creating the rest of the content within the content trees. To create a new dictionary item, simply right-click on the Dictionary, click Create, enter your dictionary item name, and click Create. Your new item will display in the Translation Tree.

{% include image_caption.html imageurl="/images/posts/2022-12-23/dictionary-items.png" title="Dictionary Items" %}

Once you have created your dictionary item, you will see a text area that displays next to your localization language for your site. The more languages you have configured, the more text areas you will see. Each text area allows the content editor the ability to configure the text that displays when this dictionary item is rendered in the code. Enter your text in the text area and click Save to save your changes.
{% include image_caption.html imageurl="/images/posts/2022-12-23/dictionary-item-value.png" title="Dictionary Item Value" %}

## Displaying Dictionary Items

As a developer setting up the view that displays your dictionary items, the way to reference this value is simply with the name of the dictionary item.

```cshtml
@Umbraco.GetDictionaryValue("Search.Button")
```

You also have the ability to enter some fallback text if your dictionary item is empty. Your code would then look something like this:

```cshtml
@Umbraco.GetDictionaryValue("Search.Button", "Submit Your Search")
```

I hope you can utilize the power of the Umbraco Dictionary on your own website. If you are looking for more information on using the dictionary, you can check out the official Umbraco documentation about [Dictionary Items](https://docs.umbraco.com/umbraco-cms/fundamentals/data/dictionary-items){:target="_blank"} here.

And to leave you with a parting word of wisdom when building your sites, remember not all content needs to be edited in the content section but your content should be editable from the CMS.
