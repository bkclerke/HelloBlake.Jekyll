---
layout: post
title: "Setting Up Umbraco Locally"
description: "How to setup Umbraco locally with Microsoft SQL Server Management Studio and Visual Studio 2015 in this step-by-step tutorial."
date: 2015-09-05
feature_image: 
tags: [umbraco7]
disqus_enabled: true
---

There are a lot of ways to setup Umbraco locally. This is a step-by-step on setting up a database in SQL Server Management Studio and configuring Umbraco to run locally. <!--more--> Here is what you will need to get started:

1. Umbraco Version Zip Files
2. Microsoft Visual Studio
3. SQL Server Management Studio

I am going to assume you have these programs installed and have downloaded the required files needed for Umbraco. These instructions will be using Visual Studio 2015 on Windows 10 with SQL Server 2014 Management Studio.

First let’s create our database that we will need for our site. Open SQL Server Management Studio and right click on Databases.

{% include image_caption.html imageurl="/images/posts/2015-09-05/new-db.jpg" title="New Database" %}

Name your database in the Database name field. In this case I’ll use *example*. Click Okay.

{% include image_caption.html imageurl="/images/posts/2015-09-05/db-name.jpg" title="Name Database" %}

You will notice that the new database will show up in your Databases folder in the Object Explorer.

{% include image_caption.html imageurl="/images/posts/2015-09-05/new-db-list.jpg" title="Database Listing" %}

In the Security > Logins section of Management Studio, create a new user by right clicking on Logins and clicking New Login.

{% include image_caption.html imageurl="/images/posts/2015-09-05/new-db-user.jpg" title="New Database User" %}

Type in your new login name in the Login name field, click SQL Server authentication, and uncheck Enforce password policy. Type in your password in the Password field and confirm it. For Default database, select your database that you just created.

{% include image_caption.html imageurl="/images/posts/2015-09-05/new-login-1.jpg" title="New Database Login" %}

Remember your credentials! You are going to need these when we setup Umbraco later. Click OK when you are finished.

Let’s go back to your database in Management Studio and go to Security > Users. Right click on Users and select New User…

{% include image_caption.html imageurl="/images/posts/2015-09-05/new-db-user.jpg" title="New Database User" %}

Enter your username in both User name and Login name fields. In my case, I’ll use *example_user*. Enter dbo in the Default Schema field.

{% include image_caption.html imageurl="/images/posts/2015-09-05/user-setup-1.jpg" title="Database User" %}

In Owned Schemas, select the *db_owner* schema.

{% include image_caption.html imageurl="/images/posts/2015-09-05/user-setup-2.jpg" title="Database Schema" %}

In Membership select *db_owner* as the role.

{% include image_caption.html imageurl="/images/posts/2015-09-05/user-setup-3.jpg" title="Database Owner" %}

Click OK once you have these settings configured.

We are done with Management studio for now, let’s extract our Umbraco CMS files where we would like to have our site. I use the default Projects folder that Visual Studio creates so I am going to extract my Umbraco.CMS.7.2.8 zip into my C:/Documents/Visual Studio 2015/Projects/example folder.

{% include image_caption.html imageurl="/images/posts/2015-09-05/projects-folder.jpg" title="Folder Structure" %}

With my files extracted open up Visual Studio and go to File > Open > Web Site.

{% include image_caption.html imageurl="/images/posts/2015-09-05/vs-open-site.jpg" title="Visual Studio Open Site" %}

Navigate to the folder where you extracted your Umbraco files and select the folder. In my case I am selecting example.

{% include image_caption.html imageurl="/images/posts/2015-09-05/file-system.jpg" title="File System" %}

Click Open and Visual Studio will load your website. Start debugging your website by either hitting F5, or the play icon in the toolbar.

{% include image_caption.html imageurl="/images/posts/2015-09-05/vs-debug.jpg" title="Visual Studio Debug" %}

Visual Studio will take a moment to build your website and it will open in the browser. You may be prompted to Modify your web config file to enable debugging, click Okay.

{% include image_caption.html imageurl="/images/posts/2015-09-05/vs-debug-prompt.jpg" title="Enable Debugging" %}

When it is finished you will be presented with the Umbraco install screen. Enter your name, email and password. Check or Uncheck the news checkbox, this is your preference, and click the Customize button.

{% include image_caption.html imageurl="/images/posts/2015-09-05/install-step-1.jpg" title="Install Step 1" %}

Next we are going to configure our database. Select Microsoft SQL Server from the Database type drop down list.

{% include image_caption.html imageurl="/images/posts/2015-09-05/install-step-2.jpg" title="Install Step 2" %}

We are now presented with our options to configure our database. Here we need to enter our Server name, Database name, database Login and Password. This is where you will need your credentials that you setup earlier.

For my example I am going to enter my server name, which you can find in Management Studio or by typing in the command prompt window SQLCMD –L to get your database name. This is something that you would have setup when you installed Management Studio.

Management Studio:

{% include image_caption.html imageurl="/images/posts/2015-09-05/sql-server-name.jpg" title="SQL Server Name" %}

CMD Prompt:

{% include image_caption.html imageurl="/images/posts/2015-09-05/cmd.jpg" title="Command Prompt" %}

My Database name was example so I am going to enter that in the Database name field. My user was example_user so I am putting that in the Login field and entering the password I created for this user into the Password field.

{% include image_caption.html imageurl="/images/posts/2015-09-05/install-step-2-b.jpg" title="Install Step 2 Configure your database" %}

Once you have filled out all the information, click Continue. If your information was correct Umbraco will display a Install a starter website prompt. You can chose to either install a starter website or click No thanks. Once Umbraco finishes setting up your website you will be taken straight into the back office. This is where the fun begins!

{% include image_caption.html imageurl="/images/posts/2015-09-05/backoffice.jpg" title="Umbraco Backoffice" %}