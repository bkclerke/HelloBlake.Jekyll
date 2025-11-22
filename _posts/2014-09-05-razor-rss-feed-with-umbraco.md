---
layout: post
title: "Razor RSS Feed With Umbraco"
description: "Here you will find the Razor markup to build your own RSS feed inside of Umbraco 7 and a few tips on some errors you may encounter when setting up your RSS feeds."
date: 2014-09-05
feature_image: 
tags: [umbraco]
disqus_enabled: true
---

First let’s make sure we have a node in the Content section that has the RSS template assigned to it.

<!--more-->

{% include image_caption.html imageurl="/images/posts/2014-09-05/rss-node.jpg" title="RSS Node" %}

In the Settings section make sure your new template has no master since this page should act as a standalone page.

{% include image_caption.html imageurl="/images/posts/2014-09-05/rss-template.png" title="RSS Template" %}

The markup used for generating an RSS feed in Umbraco 7 is as follows:

```cshtml
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{
    Layout = null;
}<?xml version="1.0" encoding="UTF-8"?>
@{
	umbraco.library.ChangeContentType("text/xml");		
	var siteURL = "http://" +  Request.Url.Host.ToString();
	var rssPage = CurrentPage.AncestorOrSelf(1).Rss.First();
	var articles = CurrentPage.AncestorOrSelf(1).Blog.First().Children.OrderBy("date desc");
}	
	<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
    <channel>
        <atom:link href="@siteURL/rss" rel="self" type="application/rss+xml" />
        <title>@rssPage.title</title>
        @Html.Raw("<link>")@siteURL@Html.Raw("</link>")
        <description>@rssPage.description</description>
        <pubDate>@String.Format("{0:ddd, dd MMM yyyy HH:mm:ss} EST", @rssPage.CreateDate)</pubDate>
        <lastBuildDate>@String.Format("{0:ddd, dd MMM yyyy HH:mm:ss} EST", DateTime.Now)</lastBuildDate>
        <language>en</language>
        <generator>Umbraco</generator>
 
        @foreach (var article in articles)
        {
            <item>
                <title>
                    @if (article.HasValue("title"))
                    {@article.title}
                    else
                    {@article.Name}
                </title>
                @Html.Raw("<link>")@siteURL@article.Url@Html.Raw("</link>")
                <description>@article.previewText</description>
                <pubDate>@String.Format("{0:ddd, dd MMM yyyy} {1:HH:mm:ss} EST", @article.date, @article.CreateDate)</pubDate>
                <guid>@siteURL/@article.Id</guid>
            </item>
        }
    </channel>
</rss>
```

## Common Issues

One of the issues I ran into when building this was an error message “XML declaration allowed only at the start of the document”. What this message means is that if you don’t have your XML rendering right at line 1 of your document then it won’t work. You get no breaks here. You can check the source code of your rendered page to see where the XML starts.

{% include image_caption.html imageurl="/images/posts/2014-09-05/rss-template-source.png" title="XML Declaration" %}

In the code you will see that the markup has the `<?xml version="1.0" encoding="UTF-8"?>` tag directly after the initial layout declared on the template.

{% include image_caption.html imageurl="/images/posts/2014-09-05/xml-markup-start.png" title="XML Markup Start" %}

I wrote this razor directly on the template itself because when I used a partial view, my XML version declaration would appear on line 2 of my source code and I was getting the error I mentioned above. I found the only way to get my XML version declared on the first line was to put my code directly on the template. Typically I don’t recommend writing your razor scripts directly on your template but in this case we know this page is a one off, I know I won’t be using this RSS code anywhere else in my site, and I get an error if I try to put it into a partial view (again, only because of line spacing).

## Break Down

Once we have declared our XML version we need to tell Umbraco that we want to change this content type to be text/xml. We can do this with `umbraco.library.ChangeContentType(“text/xml”)`. Now that we have this set up we need to define a couple variables that will make it easy to setup our RSS Feed. Here I’ve chosen to setup my siteURL, rssPage, and define my articles so I can loop through them. I know my Url Scheme is always going to be http so I’ve just gone ahead and ‘hard coded’ this into my siteURL variable. My rssPage variable returns my dynamic published content for my RSS page in my content tree and articles is returning my dynamic published content list of articles.

On line 11 is where my RSS feed starts. I’ve declared that I’m using RSS 2.0 and I’m ready to start declaring details of my channel. My rssPage has a title and description fields on it so I’m using those to manage the title and description for my RSS feed. In the code you will notice `@Html.Raw(“<link>”)` around the `<link>` tags. This is because link is a keyword in Razor and when I didn’t have this there I was getting an error in the application saying that my `<rss>` tag didn’t have a matching opening tag. Of course it did and my `<rss>` tag was not the issue. Keep that in mind if you run into that error too. My `<pubDate>` tags are using the article’s Created Date that is stored when the node is created in Umbraco. Since my date field is only a date picker and not a date picker with time I decided to use the time from the create date of the article to have some time stamp vs having all 00:00:00s as my time stamps.

There are a lot of channel elements that can be used in your RSS feed. For more details about the tags used here check out these links:

1. [http://cyber.law.harvard.edu/rss/index.html](http://cyber.law.harvard.edu/rss/index.html){:target="_blank"}
2. [http://www.w3schools.com/RSS/default.asp](http://www.w3schools.com/RSS/default.asp){:target="_blank"}

Good luck with your feed! If you run into any issues or setup your RSS feeds differently, let me know in the comments. I’d love to hear how you work in Umbraco.