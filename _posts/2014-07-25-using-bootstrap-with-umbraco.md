---
layout: post
title: "Using Bootstrap With Umbraco"
description: "Learn how to use Umbraco with Bootstrap by setting up your templates, style sheets, and scripting files to help you increase page load times."
date: 2014-07-25
feature_image: 
tags: [umbraco4]
disqus_enabled: true
---

I’ve been working on a redesign of my wedding website using Bootstrap with Umbraco and in my research process I was curious to see how other people have handled setting up Bootstrap with Umbraco. I didn’t find too many articles so I decided that I would write down my process in the hopes that it helps others and maybe people can even help me improve.

<!--more-->

## My expectations of you

I’m going to assume that you have some experience with Umbraco and Bootstrap independently. We are going to keep this concentrated on how to use Umbraco and Bootstrap together which is mostly in our Settings section of Umbraco. We are going to be working with Templates and assuming that you already have a working knowledge of Umbraco Document Types and creating content within Umbraco. The instructions and razor/c# snippets you will see are for Umbraco 4.11.10. Please keep in mind that the process will be similar in newer versions of Umbraco, but your code will change.

## Setup your Bootstrap files

Once you have a good understanding of how your website will work and your document types are setup in Umbraco, it is time to start setting up your template and CSS files. GetBootstrap.com has an abundance of documentation on how to use and customize Bootstrap for your needs. When starting a build, I use Bootstrap’s CDN for my CSS and JavaScript needs until I’m done building my site. Once the site is completed I go back and customize Bootstrap and download only the bits and pieces of the framework that I need for my specific website. This saves time downloading resources on page load since my files typically tend to be smaller this way.

When we begin setting up our template files in Umbraco we want to keep in mind the design. What pieces of the design are changing on the pages and what pieces are static. Typically you will always have some code that stays the same for your entire website. Usually this static code is your basic HTML markup. From my experience, the best way to start templates for your Umbraco site is to set up a “Master” template.

Log into Umbraco and head over to your Settings section. Right click on Templates and select Create from your drop down menu.

{% include image_caption.html imageurl="/images/posts/2014-07-25/right-click-menu-01.png" title="Right Click Menu" %}

In the modal window that displays, enter the name of your template, Master, and click Create. Your new template will display under your Template folder. Now let’s create a ‘child’ template underneath this Master template. Right click on the Master template and click Create. Name your new template “Homepage” and click Save. Your new template will appear underneath the Master template.

{% include image_caption.html imageurl="/images/posts/2014-07-25/templates-list-01.png" title="Templates List" %}

Another way to confirm that your Homepage template is using the correct master template is by selecting the template and confirming the Master template drop down box has the correct template name listed. You can also see this reflected in the `MasterPageFile=”~/masterpages/Master.master”` or in newer versions of Umbraco the `@{ Layout = “Master.cshtml” }`.

{% include image_caption.html imageurl="/images/posts/2014-07-25/homepage-master-dropdown-01.png" title="Home Master Dropdown" %}

Before we go any further, let’s make sure that your Homepage document type is using the new Homepage template that we just created. In order to do this, stay in the Settings section in Umbraco and in the Document Types folder find your Homepage document type that you created. Click on your Homepage Document Type and you will see on the Info tab that there is an Allowed Templates checkbox area. Select your Homepage template that we just created, double check that your Default Template updated to say the Homepage template as well (it should do this automagically) and then click the Save icon.

{% include image_caption.html imageurl="/images/posts/2014-07-25/homepage-templates-01.png" title="Homepage Templates" %}

Now that your Homepage document type is using your Hompage template, lets make sure that you have a homepage in the Content section. Assuming this is a fresh Umbraco install, navigate to your Content section, right click on the Content folder in the top left and click Create in the drop down menu. Select your Homepage Document Type, give it a name, and click Create.

Lets go back to our templates and start writing our markup on our Master template. Our master is going to contain our basic HTML markup for our website. My preferred starting point is a mix up between Bootstrap’s basic template and HTML 5 Boilerplate’s out of the box markup. Since Bootstrap comes with normalize I don’t include normalize.css.

Let’s take a look at the markup I’ve used for my Master template. Templates have a toolbar that allow you to insert content placeholders, content areas, inline macros, macros, and Umbraco items. You will notice I’ve already inserted some content placeholders and an inline macro script. Some of these things may be a little above your skill level and that is okay. I did not know Razor when I started using Umbraco but as time goes on you get the hang of it with practice. They also include some helpful macro scripts out of the box.

```cshtml
<%@ Master Language="C#" MasterPageFile="~/umbraco/masterpages/default.master" AutoEventWireup="true" %>
 
<asp:Content ContentPlaceHolderID="ContentPlaceHolderDefault" runat="server">
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title><umbraco:Item field="pageTitleTag" useIfEmpty="pageName" runat="server" /></title>
 
		<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
		<link rel="stylesheet" href="/css/main.css" >
		<link href='http://fonts.googleapis.com/css?family=Open+Sans:700' rel='stylesheet' type='text/css'>		
		
		<asp:ContentPlaceHolder Id="cp_head" runat="server"></asp:ContentPlaceHolder>
 
		<!--[if lt IE 9]>
		<script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
		<script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
		<![endif]-->
	</head>
	<body>
		
		<asp:ContentPlaceHolder Id="cp_content" runat="server"></asp:ContentPlaceHolder>
		
		<footer>			
			<umbraco:Macro runat="server" language="cshtml">
			<div class="container">
				<div class="row">					
					<div class="col-xs-12 col-sm-6 col-sm-push-6">
						@{ 
							var pages = Model.AncestorOrSelf(1).Children.Where("Visible");
							if(pages.Count() > 0)
							{
								<nav>
									<ul>
										<li><a href="@Model.AncestorOrSelf(1).Url">Home</a></li>
										@foreach(var page in pages)
										{
										<li><a href="@page.Url">@page.Name</a></li>
										}
									</ul>
								</nav>
							}
						}
					</div>
					<div class="col-xs-12 col-sm-6 col-sm-pull-6">
						<a href="@Model.AncestorOrSelf(1).Url"><img src="/css/images/logo.png" alt="B Loves K" class="logo"/></a>
						<p>&copy; Copyright @DateTime.Now.Year Mr. & Mrs. Smith<br />Website by <a href="http://www.helloblake.com" taget="_blank">HelloBlake</a>.</p>
					</div>
				</div>
			</div>
			</umbraco:Macro>
		</footer>
 
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
		<script src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
		
		<asp:ContentPlaceHolder Id="cp_scripts" runat="server"></asp:ContentPlaceHolder>
 
	</body>
</html>
</asp:Content>
```

The `asp:ContentPlaceHolder` is used as a placeholder for your ‘child’ templates that have the corresponding `asp:Content`. For most of my templates I typically setup at least 3 content placeholders including:

1. `cp_head` - used for adding page specific stylesheets to only the pages that require them
2. `cp_content` - used for body copy and main content area of a website
3. `cp_scripts` - used for adding page specific scripting files to only the pages that require them

These 3 placeholders have worked pretty well in the past and usually are all I need for most of my Umbraco projects. You can change the names to whatever works for you, I’ve just stuck with using `cp_` for content placeholder and then specified the specific area it is for.

Since I have these 3 content placeholders in my master that means that my child templates, in this case our Homepage template will need to have 3 corresponding `asp:Content` tags that contain the markup that will be inserted into my placeholders.

The rest of the markup in the master is some standard HTML using the grid structure from Bootstrap. Their documentation can do wonders for explaining the specific classes in detail and how to setup the grids. It is a great resource and I recommend checking it out if you need more  help on how to set up grids or using Bootstrap in general.

My footer nav contains an inline macro that is very simple for just populating a dynamic navigation list for me in my footer section. For this particular instance, I know that this will be a very small site and I won’t need to populate this same markup for the navigation anywhere else in the site, so I went with writing it inline on the Master template instead of using a Macro. If your site has a header and footer navigation that uses the same markup, using a Macro would be ideal since you won’t have to write the markup in two places. You would only need to update your markup in your .cshtml file and then your Macro is referencing that .cshtml file in multiple places. Using Macros can really help cut down on how much you have to code out by hand and you should take advantage of these benefits with your Umbraco site.

Let’s take a look at the Homepage template now and insert our content areas. Here you will see a lot of the Bootstrap grid being used to structure our homepage. You will also notice the `umbraco:item field` being used. This references the corresponding property alias on the specified document type.

```cshtml
<%@ Master Language="C#" MasterPageFile="~/masterpages/SEOMaster.master" AutoEventWireup="true" %>
 
<asp:content ContentPlaceHolderId="cp_content" runat="server">
	
	<umbraco:Macro runat="server" language="cshtml">
		@{ var home = Model.AncestorOrSelf(1); }
		<header id="banner" class="welcome" data-bg="@(Model.MediaById(home.bannerImage).UmbracoFile)">
			<div class="container">
				<div class="row">
					<div class="col-md-6 col-md-offset-3 col-sm-12 text-center">
						<a href="@Model.AncestorOrSelf(1).Url"><img src="/css/images/logo.png" alt="B Loves K" class="logo"/></a>
						<img src="/css/images/mrmrstxt.png" alt="Mr and Mrs Smith's" class="smiths-txt"/>
						<img src="/css/images/blogtxt.png" alt="Wedding Blog" class="blog-txt"/>
					</div>
				</div>
			</div>
		</header>
	</umbraco:Macro>
 
	<section class="pink-bg section">
		<div class="container">
			<div class="row">
				<div class="col-md-10 col-md-offset-1 col-sm-12 text-center">
					<h2><umbraco:Item field="middleTitle" recursive="true" runat="server" /></h2>
				</div>
			</div>
		</div>
	</section>
	<section class="cream-bg section">
		<div class="container">
			<div class="row">
				<div class="col-md-10 col-md-offset-1 col-sm-12 text-center">
					<h2><umbraco:Item field="bottomTitle" recursive="true" runat="server" /></h2>
				</div>
			</div>
		</div>
	</section>
</asp:content>
 
<asp:content ContentPlaceHolderId="cp_scripts" runat="server">
	<script src="/scripts/main.js"></script>
	<script type="text/javascript">
		var imgSRC = $("#banner").attr("data-bg");
		$("#banner").backstretch(imgSRC);
	</script>
</asp:content>
```

In the markup, the `umbraco:item field=”yourAliasHere”` is what is taking the content that your end user has entered into content fields on the homepage and putting it on the Template. This is what connects your editable content to output into your markup. When you insert fields in Umbraco with the toolbar icon you will see that you are presented with a lot of options. The Insert Umbraco Page Field will have a drop down list that contains all of your custom property aliases (that you set up on your document types) as well as some built in aliases for referencing properties like the current page’s Name (pageName) or created date (createDate).

The **Alternative Field** is used if your first field was empty. So if we forgot to enter our blog title text we could have an alternative field that shows up on the page instead of a blank title.

The same goes for the **Alternative Text**. If your first choice is empty, this text will display instead.

**Recursive** is great for things that you only want to edit in one spot but need to appear throughout the site. An example would be our footer text. If our footer text was editable in Umbraco, I’d add a textbox multiple to my Homepage DocumentType called footerText and then put it on my Master template with recursive set to true. This way no matter where I am in the site, only my Homepage has this field but the page recursively looks up the Umbraco content tree to find this field and place the text there.

This recursive functionality actually stems from the fact that everything saved in Umbraco is in an XML format. If you are familiar with this it will also make more sense but this is a more advanced topic I won’t get into right now.

In my experience I haven’t used the other options so much but they do have some notes that explain what they do. You can also visit umbraco.TV’s templating section that exlains the Insert Umbraco page field dialog in detail.

My homepage happens to be using a plugin called backstrech so I’m taking advantage of my `cp_scripts` placeholder by adding my scripts to only my Homepage in this area. This cuts my load times for other pages of my site since they don’t have load unnecessary scripting files that only the homepage uses. By now you probably have noticed that I load all my scripting files towards the end of the `<body>` tag. This is also to help increase page load times since the heavy lifting is done at the bottom of the page, after the HTML and CSS have already been loaded. Some projects may require you to load your scripts in the `<head>` so this placement is something you will need to consider on a project-by-project basis.

## Wrap Up

This was a quick overview of how I initially set up my wedding website Master and Homepage Templates in Umbraco using Bootstrap. If you have any feedback, questions or suggestions on my Umbraco with Bootstrap process, let me know in the comments below. I’d love to hear how you organize your Umbraco website when you are working on a Bootstrap project.