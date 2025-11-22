---
layout: post
title: "Bundle and Minify in Umbraco"
description: "Use the built in Client Dependency Framework in Umbraco to bundle and minify your website scripts and stylesheets."
date: 2015-11-29
feature_image: 
tags: [umbraco]
permalink: /articles/using-the-umbraco-client-dependency-framework-to-bundle-and-minify/
disqus_enabled: true
---

When building websites there are a few general best practice items to help you get the best performance out of your website. When using Umbraco you have some handy features built in like the Client Dependency Framework (CDF) that can help optimize your site.

<!--more-->

The CDF manages your scripts and stylesheets for your web application, including minifying, compressing and caching. To learn more about the framework check out the documentation that Shannon Deminick has put together on Github.

## Step 1: Determine Your Setup

There are a couple ways you can go about setting up scripts on your website and you should take the time to evaluate what will work best for your site needs. Typically I load only the scripts that are needed on the pages or templates that require them. For example, you have a slider on your homepage, but not on any other page in your site. Well you won’t really need the scripts and styles for the slider on every single page of your site so you only want to have those files referenced on the homepage template.

For some, the scripts that you use may be so well compressed that it wouldn’t make a noticeable impact to bundle all your scripts into a single minified script on every page.  You will need to determine this impact based on your site and the files you are loading.

## Step 2: Add Your Using Statement

Using Umbraco 7, it is pretty straight forward in setting up your files to be bundled. Take a look at your Master template. Typically this is the template with your core markup on it, markup that is used throughout your entire website. The first thing you need to do is add the using statement for the client dependency to your template.

`@using ClientDependency.Core.Mvc`

## Step 3: Add Your Scripts and Stylesheets

Now that Umbraco knows that we are using the Client Dependency, we need to add our files. Underneath the Layout = null; markup, lets add the following code with the relative paths to our files we need. Your code should look similar to this. Your file paths may be different depending on your website setup.

```cshtml
@{
     Layout = null;
     Html.RequiresCss("~/css/bootstrap-3.3.5.min.css", 1);
     Html.RequiresCss("~/scripts/less/main.min.css", 2);
     Html.RequiresJs("~/scripts/jquery-1.10.1.min.js", 1);
     Html.RequiresJs("~/scripts/bootstrap-3.3.5.min.js", 2);
}
```

The numbers after the file path indicate what order the scripts and stylesheets should load in. If you didn’t have this, you could end up in a situation where jquery loads last and therefore none of your plugins that require this library work since it hasn’t been loaded yet.

There are multiple ways to add files, you could reference an entire folder with `@Html.RequiresJsFolder("~/Js/Controls/");` or reference an entire bundle with MVC. You can also check out the wiki for more information on how to do these things too.

## Step 4: Render Your CSS and Scripts

On your template, you will need to determine where you want your scripts and style sheets to load. To render your CSS on the template you will need to add `@Html.Raw(Html.RenderCssHere())` in the head tag of your markup.

Typically I load the scripts last on the page body so towards the footer you would place `@Html.Raw(Html.RenderJsHere())` to render your scripts.

The client dependency is going to know what to put here based on the RequresCss and RequiresJs files that you added in step 2.

You can repeat these steps in order to add more files that are needed on some of the inner pages of your website. This depends on how you determined to setup your files in step 1. Don’t forget to order your main styles and scripts if you go this route as the innerpages that are added appear to load before the master page files that are added.

## Debugging Your Scripts and Styles

In your web config file, when debug is set to true, the CDF knows not to bundle all your scripts together just yet. This helps for testing your site and making sure you have all your scripts and stylesheets referenced. Once you set debug to false, typically in production environments, the CDF will bundle and minify your scripts and stylesheets on the page and cache them until you make changes to the files or update the version number for the CDF in the client dependency config file.

As you can see, it is pretty easy to use the client dependency framework built into Umbraco in order to bundle and minify your scripts and stylesheets. If you are looking for specifics on the CDF itself, [check out the documentation](https://github.com/Shazwazza/ClientDependency){:target="_blank"} and feel free to leave a comment if you have any questions!