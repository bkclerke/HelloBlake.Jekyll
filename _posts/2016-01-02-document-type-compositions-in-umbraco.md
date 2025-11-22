---
layout: post
title: "Document Type Compositions in Umbraco"
description: "What you know, what you need to know and what is to come for Umbraco Document Type Compositions. Learn about it here and plan for the changes to come!"
date: 2016-01-02
feature_image: 
tags: [umbraco]
disqus_enabled: true
---

Umbraco 7.2 introduced us to a new concept of Document Type Compositions. You may have seen this while you were in the back office building new sites, but what does it truly mean for you and how you build sites? To answer this, it helps to understand where this concept is coming from.

<!--more-->

## Previous “Nested Inheritance” Setup

Typically, in the past, when you build Umbraco websites you have the ability to “nest” Document Types under a “Master” Document Type. This nested structure allows you to inherit properties from the parent Document Type. This parent/child relationship is clearly visible in the back office (tree-like structure). For example, a simple site structure might look similar to this:

{% include image_caption.html imageurl="/images/posts/2016-01-02/doc-type-nested-structure.png" title="Umbraco Document Types" %}

## Compositions Explained

With the addition of compositions, you have the ability to not only inherit properties from nested Document Types (that tree-like structure) but you can also Compose from Document Types. This is technically almost the same thing as nesting with a few differences, which we will get to. With Compositions you select which Document Types you would like to “inherit” properties from. So while you can’t clearly see this structure in the back office, it is viewable when checking out the Structure tab to see what Document Type Compositions are checked.

If you are using a mixed setup taking advantage of nesting and using compositions you will see a checklist similar to this:

{% include image_caption.html imageurl="/images/posts/2016-01-02/ex-comp-list-checked-72.png" title="Document Type Compositions" %}

The grayed out Document Type Composition Master is a parent to the particular Document Type we are looking at. By default, this means that this Document Type will inherit the properties from the Master Document Type and unless we move it (which is really easy to do in v 7.4) this is what you get. The other Document Type, Banner, is checked and just using the compositions as intended which also says we want the properties setup on the Banner Document Type to be on this Document Type too.

With the new Document Type Editor in version 7.4 (currently in beta) you have a different editor all together, but the compositions are still the checkboxes allowing you to select which Document Type’s properties you would like to compose from.

{% include image_caption.html imageurl="/images/posts/2016-01-02/ex-comp-list-74.png" title="Document Type Compositions Applied" %}

## Umbraco 7.4 (Beta) Changes

In Umbraco 7.4 changes were proposed. The beta version completely got rid of the nested inheritance setup that a lot of us have been accustomed to using for a very long time and went the route of only compositions. There are some good reasons some have their reservations about moving fully to compositions just yet and Dave started up a thread on our asking the community their opinions on this improvement. [Long thread short](https://our.umbraco.org/forum/core/general/73809-no-more-master-doctypes-in-74-beta-is-this-really-an-improvement#comment-236822){:target="_blank"}, the core team listened to the community and is implementing a solution to keep both styles of back office setup, for now. (YAY!)

### How do these changes affect your workflow?

You are probably wondering what this changes, and does it really affect you. It is a big change for some, going from a very rigid structure of inheritance with parent/child relationships to a structure that is actually quite liberating! With an inheritance structure, you see the relationships but with compositions not so much… well at least until Offroad Code gave us Umbraco Backoffice Visualizations (Thank you!).

Really this is a minor complaint considering the vast amount of configuration options that compositions give us. Those who have worked with larger Umbraco site setups know the struggle is very real when it comes time to expand on a site, or when a client wants a different setup and it requires reworking the Document Types. Before compositions, this was a major pain because moving Document Types just wasn’t that easy. It was do-able, but with workarounds and a lot of time invested in copying, switching, republishing, etc. The nested process requires a lot of thinking into the whole setup as you breakdown what can be reused and therefore on Master (aka Parent) Document Types and what should be on its own child Document Type. With Compositions, you still have to think about these things, but making changes and restructuring your Document Types is as easy as checking boxes. That is a huge improvement! I think the only ‘con’ I can see with this, is making sure that you check all the compositions that are necessary for a Document Type, and really in the grand scheme of things, I think we can manage this.

### I’ll admit it, these changes made me nervous.

I’ll be first to admit, seeing that nested inheritance was going away in the beta was really off-putting. I’ve been using Umbraco for years and one of the things I really love about their UI is seeing relationships and locations of Document Types, Templates, Content Items. It actually really helped me to understand learning razor a bit better too, but I gave the beta a shot and I’ve been playing with it, reworking test sites, upgrading older sites, messing around with my blog and I think going full on Compositions will just be a step forward for the flexibility in building sites in Umbraco. We do technically keep the same ‘inheriting’ functionality in a sense, just with the ability to inherit or “compose” from more Document Types quicker.

## In Summary, go try out 7.4!

While 7.4 is still in beta, this has been an overview of just one aspect of the changes being made. There are a few UI updates to the grid and some excellent improvements to the Media section that I think you are really going to like, so go download it! Play with it! Embrace the compositions! Try out Folders! Umbraco is only getting better.