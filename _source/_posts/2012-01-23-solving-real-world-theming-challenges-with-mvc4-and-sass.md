---
layout: post
title: Solving Real-world Theming Challenges with MVC4 and Sass
tags: aspnet-mvc mvc4 sass theming themes
excerpt: A theming approach with ASP.NET MVC4 and Sass stylesheets that supports multiple user-selectable themes.
---
Theming or skinning support within your application is great - users love to make an app their own.

ASP.NET Web Forms has theming baked in and it's a snap to adopt. We did this for years with one of our products and you would be surprised at how many enterprise customers leveraged ASP.NET Themes to skin the app to match their corporate colors. Dead simple.

Then along came ASP.NET MVC and no more out-of-the-box theming support because the focus is on CSS. This is not a big deal if you Stop-Think-Structure your web application in the right way. And let's not forget about the HTML5/CSS3 wave and the whole notion of responsive web design to support mobile devices.

Let's see how we can try to build structured theming support working with MVC4 and Sass.

### Core Development Toolbox

What you need to get the Theming job done.

* [Sass](http://sass-lang.com) for creating your stylesheets
* Mindscape [Web Workbench](http://visualstudiogallery.msdn.microsoft.com/2b96d16a-c986-4501-8f97-8008f9db141a) for automatic Sass stylesheet compilation
* Andy Webber's [Css3Gen](http://css3gen.com) tool for quickly generating box\text shadows and border radius effects
* Mad Kristensen's [Web Essentials](http://visualstudiogallery.msdn.microsoft.com/6ed4c78f-a23e-49ad-b5fd-369af0c2107f) Visual Studio extension for handy CSS editing
* Mad Kristensen's [Web Standards Update](http://visualstudiogallery.msdn.microsoft.com/a15c3ce9-f58f-42b7-8668-53f6cdc2cd83) for HTML5 and CSS3 intellisense support
* Mad Kristensen's [Image Optimizer](http://visualstudiogallery.msdn.microsoft.com/a56eddd3-d79b-48ac-8c8f-2db06ade77c3) extension for squashing images used by your themes

Themes evolve around CSS and images, so now let's see how we should structure our "assets" folder to store our stuff.

### Break-out your Foundation CSS

If you are serious about building your website or application then you will surely have to leverage some sort of widely-accepted base/grid system. For example:

* [HTML5 Boilerplate](http://html5boilerplate.com) for cross-browser HTML5 ready HTML/CSS template
* Any CSS Grid framework such as [960](http://960.gs) to bring uniformity to your layouts

Essentially, you have to keep your "foundation" stylesheets separate from "presentation" stylesheets (which are theme specific).

![Separate CSS](/images/theme-split.png "Separate CSS")

Note how the core stylesheets are outside of the "default" theme folder.

### Organize Images Folder

Images should be organized to ensure theme specific images are kept away from "meta" level images.

![Theme images](/images/theme-images.png "Theme images")

Notice how the "default" theme images folder lives inside a "design" folder whilst "meta" images are completely ring-fenced. This can be hard to refactor afterwards so get it done right from day one.

### Folder Structures By Theme, By Platform

How can you structure theme stylesheets that cater for both desktops and mobile devices?

What about controlling common CSS elements that are used by desktops and mobile devices?

What about common page specifics that do not change between themes?

We can address all these issues by carefully structuring our style assets:

![Theme structure](/images/theme-structure.png "Theme structure")

So now we can have different themes and within each theme we can support platform specific styling. As a bonus we also have a "global" stylesheet that contains common CSS elements applicable to all platforms and themes.

That's what we mean by **Stop-Think-Structure**.

### Sass and Nested CSS Files

Next step is to arrange "nested" stylesheets in a manageable manner. Enter Sass:

> Sass is a meta-language on top of CSS that's used to describe the style of a document cleanly and structurally, with more power than flat CSS allows. Sass both provides a simpler, more elegant syntax for CSS and implements various features that are useful for creating manageable stylesheets.

There are three levels of nesting that we can employ:

![Theme levels](/images/theme-levels.png "Theme levels")

### Level 0 - global.scss

This stylesheet contains theme-neutral variables and styles including re-usable Sass "mixins" (that can be thought of as CSS functions). The contents of this "global" stylesheet will be available to all themes.

![Theme level 0](/images/theme-level0.png "Theme level 0")

### Level 1 - theme.scss

Next up is the theme specific stylesheet that includes all the reusable stuff from our "global" stylesheet above.

![Theme level 1](/images/theme-level1.png "Theme level 1")

We specify the theme colors and other theme specific stuff.

### Level 2 - desktop.scss and mobile.scss

These platform specific stylesheets are BOTH theme and platform specific and extend the 'theme' stylesheet above.

We can now set up the 'desktop.scss' stylesheet with styling that is relevant for serving content to desktop browsers.

![Theme level 2](/images/theme-level2a.png "Theme level 2")

Similarly, we can do different styling for mobile devices using the 'mobile.scss' file.

![Theme level 2](/images/theme-level2b.png "Theme level 2")

We could use a SINGLE stylesheet for both desktop and mobile devices and leverage CSS Media Queries.

We have chosen to split out CSS for desktop and mobile. The next blog post in this mini-series will show you how we build upon these by using different Master Pages with the new MVC4 mobile features.

### Reference Theme In Master Pages

Now all your master pages can simply reference the required theme for the user:

![Theme reference](/images/theme-reference.png "Theme reference")

You can simply expose the theme name from your [BaseViewPage](http://haacked.com/archive/2011/02/21/changing-base-type-of-a-razor-view.aspx) implementation:

![Base View Page](/images/theme-baseviewpage.png "Base View Page")

By using our own BaseViewPage implementation we can centralize and inject non-view-model stuff into all view pages.

### Phew!

Wow. Nested stylesheets. Split by desktop and mobile devices.

That's the price that we have to pay as an ISV building products - complexity comes with the territory as we are not building yet-another-intranet-app. The CSS is layered for a reason and no doubt there could be ways to optimize this setup - there are a couple areas of improvement that I will cover off in the during the course of this mini-series.

### Finally, a Sass Tip For Developers

I love my Designers.

But sometimes I wonder when I ask for a color code if they are just making up hex numbers. EAEAEA. F4D3AF. 4C4C4C. F5F5F5?!

As a developer it drives me crazy to see two dozen hex color codes in my CSS. So try to work with a core half dozen colors and then leverage some Sass "darken" and "lighten" magic to create additional color shades!

![Sass Magic](/images/theme-sass.png "Sass Magic")

That works for me.

### Further Reading

An interesting blog post by Jason Grigsby entitled [CSS Media Query for Mobile is Fool's Gold](http://www.cloudfour.com/css-media-query-for-mobile-is-fools-gold).
