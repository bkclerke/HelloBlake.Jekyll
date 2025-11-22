---
layout: post
title: "How To Setup Custom Error Pages in Umbraco"
description: "How to setup custom 404 error pages in Umbraco 7, what does a 500 internal server error mean and some tips for what to put on your error pages to help your visitors."
date: 2014-08-30
feature_image: 
tags: [umbraco]
disqus_enabled: true
---

There are a lot of different error codes that can be returned when you hit something on the internet that no longer exists or you don’t have access to. Typical error messages that I’ve run across (chances are you have too) include the dreaded 500 internal server error (I hate when I see these), 403 forbidden, and probably the most common 404 not found errors.

<!--more-->

## 500 Errors Explained

500 errors usually require some more work to get fixed and involve making sure that you have your detailed error reporting turned on in IIS and/or ASP.NET. Personally if I can’t figure out if this happened from a change I just made or if I can’t interpret the detailed error message myself I ask a back-end developer for help! It’s okay to ask for help sometimes if you need it. Don’t be afraid! Ask someone who knows, and then ask them to explain it to you in terms that you can understand so that if you run across this problem in the future you can try to fix it yourself. Unfortunately since this error is on the server before Umbraco gets started, usually a pretty fatal error, you can’t use Umbraco’s error handling to display a page for this error.

## 403 Error Details

403 errors happen when you have come across a page or file online that you don’t have access to. More details about the different types of 403 substatus errors you can come across are here.

## Umbraco Custom 404 Error Pages

The most common error I plan for when I’m setting up an Umbraco site is a 404 error. Sometimes mistakes happen when typing in a URL or if a redirect isn’t working properly after a restructure of a website.

Setting up error pages in Umbraco is actually pretty easy. First you need to decide what errors you want to set up pages for. Here I have a 404 textpage setup with some basic information stating that the page can’t be found and I give the user some alternatives like searching for something or going back home to try to find what they are looking for.

{% include image_caption.html imageurl="/images/posts/2014-08-30/404-page.png" title="404 Page" %}

Once you have your error page setup in Umbraco, navigate to your properties tab and get the ID for your new page. We want to keep this handy because we are going to need it for the next step.

{% include image_caption.html imageurl="/images/posts/2014-08-30/404-page-properties.png" title="404 Page Properties" %}

Let’s go in our file system and navigate to the umbracosettings.config file in the config folder. Umbraco has done a great job at commenting the file so you can try to figure out what some things do in this file. The `<errors>` section will appear at the top of the config file. The `<error404>` tag is where we are going to define the ID for our custom error page that we have setup for 404 errors. The culture option defines which language you are setting this error page for. When you have a multilingual site you can setup different pages in each language and have those multilingual versions of your errors redirect to the specified page’s ID.

```
<errors>
      <error404>1113</error404>
      
        <!--<error404>
            <errorPage culture="default">1</errorPage>
            <errorPage culture="en-US">200</errorPage>
        </error404>-->
       
    </errors>
```

In our code we have setup our `<error404>` to point to our custom 404 page. Since this site isn’t multilingual I’ve left off the culture. In the commented version you can see the option `culture=”default”` which tells Umbraco to use this page for the default language.

Now if you have your files saved on your site you may notice that your new error page isn’t showing up just yet. With IIS 7.5+ you need to add a line to your web.config in order to let your custom errors show up. Add `<httpErrors existingResponse="PassThrough"/>` to your web.config file at the end of the `system.webServer` section.

```
<system.webServer>
     {...}
     <httpErrors existingResponse="PassThrough"/>
</system.webServer>
```

Once you have done this, try navigating to a page that you know doesn’t exist. You should see Umbraco displays your custom error page title tag and content, but the URL will remain what the user has typed in that doesn’t actually exist.

{% include image_caption.html imageurl="/images/posts/2014-08-30/not-found-page.png" title="Not Found Page" %}

Congratulations! That wasn’t too bad was it? What errors do you typically setup custom error pages for? Sound off in the comments below! Thanks for visiting.