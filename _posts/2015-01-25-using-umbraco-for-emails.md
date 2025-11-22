---
layout: post
title: "Using Umbraco for Emails"
description: "You can use Umbraco to build out your recurring email campaigns quickly. Here you will find the statistics and learn how to set it up in Umbraco."
date: 2015-01-25
feature_image: 
tags: [umbraco]
disqus_enabled: true
---

Umbraco is a powerful tool that can be used for more than just managing your website. You can also utilize Umbraco for your email marketing needs. Customizing Umbraco for emails can save you a lot of time when it comes to building out those recurring emails for newsletters and other events.

<!--more-->

Here you will find general statistics of this campaign in particular as well as how it was setup in Umbraco.

## The Design

{% include image_caption.html imageurl="/images/posts/2015-01-25/newsletter-january-2015.png" title="Email Newsletter" %}

The initial design of this newsletter layout was chosen based on previous analytical data provided from prior email campaigns. One of the strongest determining factors was the amount of opens the email had on mobile devices. The iPhone took the lead in the email client list with 32.6%, following up with the iPad. It is apparent that people are consuming this data on the go so there isn’t much time to capture their attention.

With this in mind, this email was designed so that it would scroll easily, get to the point with strong titles, have quick previews, and large buttons.

## The Setup

Now that you have seen the design, let’s start with the document types. Always organize your document types! This email design has been divided into 4 Document Types.

{% include image_caption.html imageurl="/images/posts/2015-01-25/doc-types.png" title="Document Types" %}

No 1. **Email Folder** – This document type is allowed at the root and acts as our man email folder. All the emails will be children of this node in the content tree.

{% include image_caption.html imageurl="/images/posts/2015-01-25/email-folder-doctype-properties.png" title="Email Folder Document Type Properties" %}

No 2. **Email** – This is where our email will live. We’ve given the editor the ability to add a main article here which has different styling than just the boxes.

{% include image_caption.html imageurl="/images/posts/2015-01-25/email-doctype-properties.png" title="Email Document Type Properties" %}

No 3. **Email Section** – This is where we’ve broken out the different section boxes for content items in the email. This setup allows the user to create as many different sections as they want. The template for this will generate the HTML for the box and apply the color styles to the headers and buttons based on the user’s selection on the content node.

{% include image_caption.html imageurl="/images/posts/2015-01-25/email-section-doctype-properties.png" title="Email Section Document Type Properties" %}

No 4. **Email Section Testimonial** – This will be used for our testimonial section. The layout of this area is different than a regular section and had different properties which required a document type of its own.

{% include image_caption.html imageurl="/images/posts/2015-01-25/email-testimonial-doctype-properties.png" title="Email Testimonials Document Type Properties" %}

Now that we have our document types setup we need the HTML markup itself. Since this is just a one page email, it’s best to just have one template assigned to the Email document type. Below is the code used for the Email template.

*Note: Please note that this was written in Umbraco version 7.*

```cshtml
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{
    Layout = null;
}
 
@{
	var home = CurrentPage.AncestorOrSelf(1); 
 	@* Default Red *@
	var color = "#c51432";
	var border = "#990033";
	if(CurrentPage.HasValue("color"))
	{
		color = "#" + CurrentPage.color;
	}
	if(color == "#6abc45")
	{
		@* Light Green *@
		border = "#4C9E28";
	}
	else if(color == "#0069cb")
	{
		@* Blue *@
		border = "#0033CC";
	}
	else if(color == "#ffae00")
	{
		@* Yellow *@
		border = "#B9802B";
	}
	else if(color == "#7336")
	{
		@* Dark Green *@
		color = "#007336";
		border = "#004C24";
	}
 }
 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
	<title>@Umbraco.Field("pageTitleTag", altFieldAlias: "pageName")</title>
	<style type="text/css">
		/* Based on The MailChimp Reset INLINE: Yes. */  
		/* Client-specific Styles */
		#outlook a {padding:0;} /* Force Outlook to provide a "view in browser" menu link. */
		body{width:100% !important; -webkit-text-size-adjust:100%; -ms-text-size-adjust:100%; margin:0; padding:0; font-family:Arial, sans-serif; font-size:14px; line-height:18px; color:#898989; } 
		/* Prevent Webkit and Windows Mobile platforms from changing default font sizes.*/ 
		.ExternalClass {width:100%;} /* Force Hotmail to display emails at full width */  
		.ExternalClass, .ExternalClass p, .ExternalClass span, .ExternalClass font, .ExternalClass td, .ExternalClass div {line-height: 100%;}
		/* Forces Hotmail to display normal line spacing.  More on that: http://www.emailonacid.com/forum/viewthread/43/ */ 
		#backgroundTable {margin:0; padding:0; width:100% !important; line-height: 100% !important;}
		/* End reset */
 
		/* Some sensible defaults for images
		Bring inline: Yes. */
		img {outline:none; text-decoration:none; -ms-interpolation-mode: bicubic;} 
		a img {border:none;} 
		.image_fix {display:block;}
 
		/* Yahoo paragraph fix
		Bring inline: Yes. */
		p {margin: 1em 0;}
        
		/* Outlook 07, 10 Padding issue fix
		Bring inline: No.*/
		table td {border-collapse: collapse;}
 
		/* Remove spacing around Outlook 07, 10 tables
		Bring inline: Yes */
		table { border-collapse:collapse; mso-table-lspace:0pt; mso-table-rspace:0pt; }
 
		/* Styling your links has become much simpler with the new Yahoo.  In fact, it falls in line with the main credo of styling in email and make sure to bring your styles inline.  Your link colors will be uniform across clients when brought inline.
		Bring inline: Yes. */
		a {color:#464646; text-decoration:underline; }
	    a:hover {text-decoration:none; }
	    /*a.button {color:#FFFFFF; font-weight:bold; font-family:Arial, sans-serif; -webkit-text-shadow:0 1px 0 #000; text-shadow:0 1px 0 #000; text-decoration:none; border-radius:3px; -moz-border-radius:3px; -webkit-border-radius:3px; border:2px solid #990033; background-color:#C51432; padding:5px 10px; }*/
 
		/* DISCOVERTEC */
        h1, h2, h3, h4, h5, h6 {font-size:24px; font-family:Arial, sans-serif; font-weight:bold; margin:0; }
			
	</style>
 
	<!--[if gte mso 9]>
		<style>
		/* Target Outlook 2007 and 2010 */
		</style>
	<![endif]-->
</head>
<body>
    <table cellpadding="0" cellspacing="0" border="0" id="backgroundTable" style="background-color:#1E1E1E;">
        <tr>
            <td align="center">
                <p style="color:#ccc;">Trouble viewing this email? <a href="@Request.Url" style="color:#ccc;">View it in your browser</a>.</p>
            </td>
        </tr>
	    <tr>
		    <td valign="top">
		        <table cellpadding="0" cellspacing="0" border="0" align="center" style="width:700px; font-family:Arial, sans-serif; font-size:12px; line-height:18px; color:#898989;">
 
                    <!-- HEADER -->
			        <tr style="background-color:#EBECE6; border-bottom:1px solid #e2e3dd;">
				        <td width="75" valign="top" style="padding:10px 0;"></td>
                        <td width="550" valign="top" style="padding:10px 0;">
                            <table>
                                <tr style="color:#c51432;">
                                    <td width="275" style="text-align:left;">
                                        <a href="#" target="_blank"><img src="@Umbraco.Media(home.logo).Url" alt="" /></a>
                                    </td>
                                    <td width="275" style="text-align:right;">
                                        <span style="font-size:24px; line-height:28px;">@home.headerText</span><br /><span style="font-size:36px; line-height:38px; font-weight:bold;">@home.phoneNumber</span>
                                    </td>
                                </tr>
                            </table>
                        </td>
                        <td width="75" valign="top" style="padding:10px 0;"></td>
			        </tr>
 
                    <!-- BODY -->
                    <tr style="background-color:#FFFFFF;">
				        <td width="75" valign="top" style="padding:20px 0;"></td>
                        <td width="550" valign="top" style="padding:20px 0;">
		
							@if(CurrentPage.HasValue("mainArticleImage"))
							{
								<img src="@Umbraco.Media(CurrentPage.mainArticleImage).Url" alt="" />
							}
 
							@if(CurrentPage.HasValue("mainArticleSubTitle")){<h1 style="color:@color !important; margin:.5em 0; font-size:24px;">@CurrentPage.mainArticleSubTitle</h1>}
 
                            @Umbraco.Field("mainArticleBodyText")
							
							@if(CurrentPage.HasValue("mainArticleButtonLink") && CurrentPage.HasValue("mainArticleButtonText"))
							{
								<table width="550" cellspacing="0" cellpadding="0">
									<tr>										
										<td align="center" width="250" height="40" bgcolor="@color" style="border:2px solid @border; -webkit-border-radius: 5px; -moz-border-radius: 5px; border-radius: 5px; color: #FFFFFF; display: block; text-shadow:0 1px 1px #000;">
											<a href="@Umbraco.Field("mainArticleButtonLink")" target="_blank" style="font-size:16px; font-weight: bold; font-family: Helvetica, Arial, sans-serif; text-decoration: none; line-height:40px; width:100%; display:inline-block"><span style="color: #FFFFFF">@Umbraco.Field("mainArticleButtonText")</span></a>
										</td>
										<td width="300"></td>
									</tr>
								</table>
							}
 
                            @Html.Partial("EmailSections")
 
                            <table width="100%"><tr><td height="25" width="100%"></td></tr></table>
                        </td>
                        <td width="75" valign="top" style="padding:20px 0;"></td>
			        </tr>
 
                    <!-- FOOTER -->
                    <tr style="background-color:#EBECE6; border-top:1px solid #e2e3dd;">
				        <td width="75" valign="top"></td>
                        <td width="550" valign="top" style="font-size:11px; text-align:center;">
                            <p>If you no longer wish to receive these emails, <unsubscribe>unsubscribe here</unsubscribe>.</p>
                            <p>&copy; @DateTime.Now.Year Company Name</p>
                        </td>
                        <td width="75" valign="top"></td>
			        </tr>
 
					<tr>
						<td height="25"></td>
					</tr>
                </table>
            </td>
        </tr>
    </table>
</body>
</html>
```

Now that we have our basic markup written, we need to tell Umbraco how to handle displaying all the sections. I’ve broke this out into a partial view called “EmailSections.cshtml” which you will see rendered in the template.

*Note: The following code was written for version 7 for internal use. The intended functionality was that every section would have a button and hyperlink so there are no checks for these, just output. This could end up showing empty if the fields are not marked required on the document type.*

```cshtml
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
 
@{
    var sections = CurrentPage.Children;
 
    foreach (var item in sections)
    {
		@* Default Red *@
        var color = "#c51432";
        var border = "#990033";
        if(item.HasValue("color"))
        {
            color = "#" + item.color;
        }
		if(color == "#6abc45")
		{
			@* Light Green *@
			border = "#4C9E28";
		}
		else if(color == "#0069cb")
		{
			@* Blue *@
			border = "#0033CC";
		}
		else if(color == "#ffae00")
		{
			@* Yellow *@
			border = "#B9802B";
		}
		else if(color == "#7336")
		{
			@* Dark Green *@
			color = "#007336";
			border = "#004C24";
		}
        
		if(item.DocumentTypeAlias == "EmailSection")
		{
			<table width="100%"><tr><td height="25" width="100%"></td></tr></table>
			<table width="100%">
				<tr style="background-color:@color; color:#FFFFFF;">
					<th colspan="5"><h1 style="color:#FFFFFF !important; margin:.5em 0; font-size:24px;"><strong>@if(item.HasValue("sectionTitle")){@item.sectionTitle}else{@item.Name}</strong></h1></th>
				</tr>				
				<tr style="background-color:#EBEBEB;">
					<td width="15"></td>
					<td width="200" valign="top" align="center" style="padding:25px 0;">
						@if(item.HasValue("image"))
						{
							<a href="@item.buttonLink" target="_blank">
								<img src="@Umbraco.Media(item.image).Url" alt="@item.sectionTitle" />
							</a>
						}
					</td>
					<td width="15"></td>
					<td width="290" valign="top" style="padding:25px 0; color:#464646; font-size:14px; line-height:1.5em;">
						@if(item.HasValue("subTitle")){<h2 style="color:@color !important; margin:0; font-size:24px;"><strong>@item.subTitle</strong></h2>}
						@item.bodyText
						<table width="290" cellspacing="0" cellpadding="0">
							<tr>
								<td align="center" width="290" height="40" bgcolor="@color" style="border:2px solid @border; -webkit-border-radius: 5px; -moz-border-radius: 5px; border-radius: 5px; color: #FFFFFF; display: block; text-shadow:0 1px 1px #000;">
									<a href="@item.buttonLink" target="_blank" style="font-size:16px; font-weight: bold; font-family: Helvetica, Arial, sans-serif; text-decoration: none; line-height:40px; width:100%; display:inline-block"><span style="color: #FFFFFF">@item.buttonText</span></a>
								</td>
							</tr>
						</table>
					</td>
					<td width="30"></td>
				</tr>
			</table>
		}
		else if(item.DocumentTypeAlias == "EmailSectionTestimonial")
		{
			<table width="100%"><tr><td height="25" width="100%"></td></tr></table>
			<table width="100%">
				<tr style="background-color:@color; color:#FFFFFF;">
					<th colspan="5"><h1 style="color:#FFFFFF !important; margin:.5em 0; font-size:24px;"><strong>@if(item.HasValue("sectionTitle")){@item.sectionTitle}else{@item.Name}</strong></h1></th>
				</tr>
				<tr style="background-color:#EBEBEB;">
					<td width="30"></td>
					<td width="75" valign="top" align="center" style="padding:25px 0;">
						<a href="@item.buttonLink" target="_blank">
							<img src="@Umbraco.Media(1062).Url" alt="Testimonial" />
						</a>
					</td>
					<td width="20"></td>
					<td width="395" valign="top" style="padding:25px 0; color:#464646; font-size:14px; line-height:1.5em;">
						<img src="@Umbraco.Media(1063).Url" alt="5 Stars" />
						@item.bodyText
						<table width="395" cellspacing="0" cellpadding="0">
							<tr>
								<td width="195"></td>
								<td align="center" width="200" height="40" bgcolor="@color" style="border:2px solid @border; -webkit-border-radius: 5px; -moz-border-radius: 5px; border-radius: 5px; color: #FFFFFF; display: block; text-shadow:0 1px 1px #000;">
									<a href="@item.buttonLink" target="_blank" style="font-size:16px; font-weight: bold; font-family: Helvetica, Arial, sans-serif; text-decoration: none; line-height:40px; width:100%; display:inline-block"><span style="color: #FFFFFF">@item.buttonText</span></a>
								</td>
							</tr>
						</table>
					</td>
					<td width="30"></td>
				</tr>
			</table>
		}
    }
}
```

## The Content

So now that we are setup in Umbraco, let’s create our content nodes and see how our tree looks for our new email.

{% include image_caption.html imageurl="/images/posts/2015-01-25/content-tree.png" title="Content Tree" %}

Our Email Folder is at our root, acting as a container for our marketing site. This has our general information on it. We have our email as a child, and the email sections as children to the email.

{% include image_caption.html imageurl="/images/posts/2015-01-25/testimonial-content-node.png" title="Testimonial Content" %}
{% include image_caption.html imageurl="/images/posts/2015-01-25/emails-content-node.png" title="Emails Content" %}
{% include image_caption.html imageurl="/images/posts/2015-01-25/section-content-node.png" title="Section Content" %}
{% include image_caption.html imageurl="/images/posts/2015-01-25/email-content-node.png" title="Email Content" %}

## The Benefits

Once you have everything setup in Umbraco you will be on your way to building out emails much quicker than working straight in your markup. The initial setup of Umbraco will take you a bit longer to get the emails built but the time you will make up in the recurring tasks will greatly outweigh this extra effort put into the start. Building your emails into Umbraco can also alleviate some of the work load for more technical team members. For example, you have some not-as-technical people who aren’t familiar with HTML that could go into Umbraco and build emails quickly to send out versus having a developer go into the markup and create new emails every time. You are looking at less headaches, less errors in updating specific content (i.e. title tags) and easier task handling with this route. I highly recommend that you create your recurring web marketing material in Umbraco.

## The Stats

This email campaign has performed pretty well with an open rate of 30.99%, representing a little over 3,811 opens and 5.51% clicking on a link. The amount of time it takes to create an email in Umbraco and then gain 210 more people visiting a client’s website is worth the effort to get your email marketing campaigns into Umbraco.

Have any questions? Do you use Umbraco for some of your marketing materials? I’d love to hear your thoughts in the comments below.