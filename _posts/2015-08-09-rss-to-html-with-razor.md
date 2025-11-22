---
layout: post
title: "RSS To HTML With Razor"
description: "If you are trying to style an RSS feed on your Umbraco site, use this macro to get the HTML output you need."
date: 2015-08-09
feature_image: 
tags: [umbraco]
disqus_enabled: true
---

Here is a simple macro that you can use to pull in your RSS feed and output an HTML unordered list. The functionality is simple and setup is minimal.

<!--more-->

In Umbraco 7, navigate to your Developer section and right click on Partial View Macro Files and Click Create.

{% include image_caption.html imageurl="/images/posts/2015-08-09/create-macro.png" title="Umbraco Macros" %}

Name your new macro what works for you. Here I've named this RSS2List. Keep the Create Macro checkbox selected and Create your new macro.

{% include image_caption.html imageurl="/images/posts/2015-08-09/name-macro.png" title="Umbraco Macros Name" %}

Now that we have our new macro created, Let's add some parameters to it to make this even more customizable for your users. Expand the Macros area in the Developer section and click on the Parameters tab. We need to create 2 new parameters to allow us some extra flexibility and re-usability with this macro, just in case we want to use it multiple times in our site for different RSS feeds. Create the following two parameters on your macro and click the Save button when you are finished.

{% include image_caption.html imageurl="/images/posts/2015-08-09/parameters.png" title="Umbraco Macro Parameters" %}

Now that we have our macro setup, lets add the following code to your partial view macro file.

```cshtml
@inherits Umbraco.Web.Macros.PartialViewMacroPage
@using System.Xml;
 
@{
    var url = "";
    if (Model.MacroParameters["rssUrl"].ToString() != "")
    {
        url = Model.MacroParameters["rssUrl"].ToString();
    }
 
    int i = 1;
    var amt = 5;
    if(Model.MacroParameters["amount"].ToString() != "")
    {
        amt = Int32.Parse(Model.MacroParameters["amount"].ToString());
    }
 
    if (url != "")
    {
        XmlDocument xml = new XmlDocument();
        xml.Load(url);
        XmlNodeList nodes = xml.SelectNodes("//item");
 
        <ul class="rss-list">
            @foreach (XmlNode node in nodes)
            {
                if (i <= amt)
                {
                    var title = node.SelectSingleNode("title").InnerText;
                    var link = node.SelectSingleNode("link").InnerText;
                    <li>
                        <a href="@link" target="_blank">@title</a>
                    </li>
                }
 
                i++;
            }
        </ul>
    }
}
```

You can either put this macro directly into a template and fill out the parameters like so:

`@Umbraco.RenderMacro("RSS2List", new { rssUrl = "http://www.myrssfeed.com/", amount = "10" })`

Or you can choose to allow the Macro in the rich text editor by selecting the Use in richtext editor and the grid checkbox on the Macro properties tab of the macro itself.

{% include image_caption.html imageurl="/images/posts/2015-08-09/macro-properties.png" title="Umbraco Macro Properties" %}

Either way will allow you to setup templates that have built in feeds or allow users to add a feed into the rich text editor and display a feed that they choose. The outputted markup is just an un-ordered list with a class of `.rss-list`. The list items contain the name of the rss item as a link to the item. This list can be styled in the CSS of your site to give a consistent look for your users. You can use css targeting to figure out where they have put it if it is in either a main content area or a sidebar and style it accordingly. 

Hopefully this gets you what you need. Enjoy!