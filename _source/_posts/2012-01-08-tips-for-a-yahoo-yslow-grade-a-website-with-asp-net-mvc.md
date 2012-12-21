---
layout: post
title: Tips for a YSlow 'Grade A' website with ASP.NET MVC 4
tags: aspnet-mvc mvc4 yslow
excerpt: Some straight-forward advice on how you can start to ensure your website can get the Yahoo! YSlow 'Grade A' and leverage a content delivery network such as Amazon CloudFront.
---
The first in a mini-series about how my team builds software with ASP.NET MVC4 and our perspective as a .NET shop.

We re-launched one of our [websites](http://www.geminiplatform.com) on Christmas Eve to freshen things up a little and this time around we dropped the CMS platform and went straight onto ASP.NET MVC 4. Shock. Horror. No CMS. Using beta status framework on a revenue generating dotcom. What next?!

One of the main goals of the website refresh was performance. We used [Yahoo! YSlow](http://developer.yahoo.com/yslow) as our guide to getting towards the 'Grade A' rating. We learnt some lessons along the way as well as using some great free tools that deserve a mention.

But why jump to MVC 4 when MVC 3 is current and stable release? Simple. We are Developers and are hooked on playing with the latest gadgets - that's what we do and live for. No other compelling reason.

The main focus was on putting in place a CDN to serve static content given that our traffic sources are worldwide and first impressions count for new visitors.

### Core Development Toolbox

A simple yet indispensable list of tools to help ensure we got good dotcom performance.

* Style sheets with [Sass](http://sass-lang.com) to leverage things like variables for common color codes, etc.
* Mindscape's [Web Workbench](http://visualstudiogallery.msdn.microsoft.com/2b96d16a-c986-4501-8f97-8008f9db141a) - works well within Visual Studio for Sass stylesheets.
* The [Microsoft Ajax Minifier](http://ajaxmin.codeplex.com) is a no-brainer for minifying your assets
* Nice web fonts sourced from either [Google Fonts](http://www.google.com/webfonts) (free) or [Typekit](https://typekit.com) (paid)
* Mad Kristensen's Web Essentials extension for Visual Studio - read [Scott Hanselman's overview] (http://www.hanselman.com/blog/UsefulVisualStudioExtensionWebEssentialsFromMadsKristensen.aspx)

### Choose Your CDN

There's no reason not to use a CDN service such as [Amazon CloudFront](http://aws.amazon.com/cloudfront) or [Windows Azure](<https://www.windowsazure.com/en-us/home/tour/cdn/). In fact if you want that YSlow 'Grade A' rating you will need a CDN to serve up your static content.

If 80% of end-user response time is spent downloading things like images, stylesheets and scripts, then doesn't it make sense to spread the load? Read the [advice](http://developer.yahoo.com/performance/rules.html#cdn) of the Yahoo! Exceptional Performance team on CDN and make up your own mind.

### Mirror Your CDN
Regardless of your CDN choice you will still need to create those static files inside your Visual Studio project and work with them locally.

![Assets folder](/images/yslow-assets-folder.png "Assets folder")

Once you have your assets in place you should mirror the same folder structure on your CDN. This will allow you to switch between locally deployed assets and those on the CDN. Here's a screenshot showing the mirrored folder structure on Amazon CloudFront.

![Amazon Assets folder](/images/yslow-assets-folder-amazon.png "Amazon Assets folder")

We use [CloudBerry Lab's](http://www.cloudberrylab.com/) cloud storage tools as they provide freeware tools to manage Amazon CloudFront, Windows Azure and other platforms.

### Integrate Microsoft Ajax Minifier in your Build

Minifing your static assets such as stylesheets and scripts is expected and the right thing to do if you want to squeeze out performance.

Install the Microsoft Ajax Minifier and then do the magic in your projects Post-build event.

![Microsoft Ajax Minifier](/images/yslow-minifier.png "Microsoft Ajax Minifier")

You should minify your stylesheets into a single file and your script into another file. Example:

	$(SolutionDir)\Build\AjaxMin.exe
	$(ProjectDir)assets\scripts\jquery.min.js.
	$(ProjectDir)assets\scripts\jquery.nivo.slider.js
	$(ProjectDir)assets\scripts\jquery.validate.js
	$(ProjectDir)assets\scripts\site.js
	-out $(ProjectDir)assets\scripts\all.min.js -clobber

You will need to reference individual files as per above - didn't find a wildcard switch to accept multiple files.

### Reference CDN Assets (or local assets)

Even with the resilience that CDN's bring, we still insisted that switching between locally available static assets or those on the CDN should be pretty straightforward.

![Reference CDN Assets](/images/yslow-localpth.png "Reference CDN Assets")

Did you notice the 'AssetsPath' reference above? This will either contain a local path or one that points towards the CDN. You can expose this variable to all your ASP.NET MVC views by using a custom [WebViewPage](http://msdn.microsoft.com/en-us/library/system.web.mvc.webviewpage(v=vs.98).aspx). You simply register your own View Page by editing the "web.config" file that resides **inside your "Views" folder** and reference your own "Web View Page" base class.

![Using own Web View Page](/images/yslow-config.png "Using own Web View Page")

And finally here is our implementation that exposes the correct assets path (yes, you could make this a configuration option).

![Assets path switch](/images/yslow-path.png "Assets path switch")

### Manage CDN Contents - watch out!
At this point:

* You have assets both locally and on your CDN
* Your assets are automatically minified as part of your build process
* You views can reference either local or CDN assets

The CloudBerry tools are a great way to upload your assets to your CDN. But there are a couple of gotcha's that you need to be aware - as we found out along the way.

### Gotcha 1 - Filename Case Sensitivity

For consistencies sake, lower case all your asset filenames such as images, scripts and stylesheets. If you don't Amazon, will chuck something like this back at you.

![Assets filename casing](/images/yslow-casing.png "Assets filename casing")

Bottom line is that casing matters on Amazon CloudFront.

### Gotcha 2 - Expires Header

Each time you upload a file to Amazon CloudFront you will have to set the Expires header or Yahoo! YSlow will [complain](http://developer.yahoo.com/performance/rules.html#expires) that you have not set them. Setting Expires will ensure that after the initial page load your website visitors will not need to download static content again.

In CloudBerry you can specify Expires Header for any content on your CDN.

![Assets file expires](/images/yslow-expires.png "Assets file expiry")

Thankfully you can set these headers on folders for recursive goodness.

### Gotcha 3 - Public Access

By default when assets are uploaded to Amazon CloudFront they are not publically accessible – that's not good when you consider your assets need to be accessible by everyone that visits your website.

Again, CloudBerry provides a way for you to ensure all assets are public.

![Access control list](/images/yslow-acl.png "Access control list")

These gotchas can initially result in Yahoo! YSlow not showing what you expect in terms of website performance best practice.

### Yahoo! YSlow (and that 'Grade A')

Finally, go and install the Yahoo! YSlow extension for your web browser and test website performance.

![Install YSlow](/images/yslow-install.png "Install YSlow")

When you first run the test you may have to do a couple of things to tell this tool what's what in order to get an accurate grading. This usually means telling YSlow about your CDN.

![Set CDN](/images/yslow-setcdn.png "Set CDN")

Once YSlow know about your CDN, all your previous work setting up CDN, Expires headers, minification should pay off with a nice 'Grade A'.

![YSlow Grade A](/images/yslow-result.png "YSlow Grade A")

There's nothing more pleasing than a job well done!

### Summary

Building a website using the ASP.NET MVC framework that **performs** according to best practice can be accomplished with the right tools.

Some essential tools to help you:

* CloudBerry Labs [cloud storage tools](http://www.cloudberrylab.com)
* [Web Workbench](http://visualstudiogallery.msdn.microsoft.com/2b96d16a-c986-4501-8f97-8008f9db141a) + [Sass](http://sass-lang.com/)
* [Microsoft Ajax Minifier](http://ajaxmin.codeplex.com)
* [Google Fonts](http://www.google.com/webfonts)
* [Mad Kristensen's Web Essentials](http://visualstudiogallery.msdn.microsoft.com/6ed4c78f-a23e-49ad-b5fd-369af0c2107f)

Hope this helps you at some point and here is our end result: [http://www.geminiplatform.com](http://www.geminiplatform.com)

For more easy to implement performance tips check out Troy Hunt's [Beyond YSlow - Squeeeezing out website network performance](http://www.troyhunt.com/2011/12/beyond-yslow-squeeeezing-out-website.html) blog post.
