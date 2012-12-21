---
layout: post
title: Optimizing Your Web App To The Max
tags: glyphs mvc4 css
excerpt: Another way to optimize your web app - kill all images and usin glyphs with clever css tricks.
---
One of my previous posts talked about how we dropped CMS and re-built one of our product sites using ASP.NET MVC4. The objective was performance and so we leveraged Yahoo! YSlow to guide us towards a 'Grade A' rating.

But what about 'UI' performance when you are building your application on top of a framework like ASP.NET MVC4? Sure, we can use the same tricks and leverage things like CDN - but there are other considerations when you are building products (a web app) beyond CDN usage.

### Kill All Images

You're joking right?

Images are needed if you want to put up a toolbar, print icon, or nice gradient backgrounds to make your web app more appealing.

But there is a way you can build a killer UI and yet use no images. No joke.

### The Toolbox

Tools required to get the job done:

* Open source [Inkscape](http://inkscape.org) tool for creating 'icon' fonts
* [Font Squirrel Generator](http://www.fontsquirrel.com/fontface/generator) to generate web embeddable fonts
* [ColorZilla](http://colorzilla.com/gradient-editor) CSS gradient editor

Let's see how we can use these tools.

### It All Starts With Fonts

All modern browsers (actually from IE6 onwards) have the ability to download extra fonts from your web server and use them as required. Most browsers (IE8+) support the CSS 2.1 pseudo selector :after which is required for the CSS only approach detailed below.

That's why people like Typekit exist - pick a nice font and use it for your web site knowing full well that on first-access, the browser will suck down the required fonts.

You can specify your custom fonts using such stylesheet code:

![Include font files](/images/glyphs-include.png "Include font files")

### Representing Icons With Font

Using our own font in a MVC4 web app means we can explore a method to represent icons using our own custom font.

A typical web app may use icons for things like "print", "save", "edit", pager controls, social tools and plenty more.

The objective is to really kill off those image files so that our MVC4 webpage does not even request any image assets.

So how on earth can you remove all those pesky "print", "save" icons from my web app? Use a font that contains your icons.

Here is an example of where we have built a toolbar using this approach:

![Common icons](/images/glyphs-commonicons.png "Common icons")

No images, all CSS with a custom font that contains representations of our required icons.

If you have no images then this could reduce the need to use CSS sprites, image squishers and potentially tools like Matt Wrock's [RequestReduce](http://requestreduce.com) (truly an amazing tool, BTW).

### Build Your Own "Icon" Font

We use the excellent open source Inkscape tool to generate the initial SVG font file.

You simply design your icon and assign it to alpha-numeric in your font layout. For example, lower case "t" is used to represent a star icon (glyph).

![Star icon](/images/glyphs-star.png "Star icon")

You can then go ahead and design all your icons and bind them to different glyphs in your font.

![All icons](/images/glyphs-icons.png "All icons")

Inkscape will simply save your work as a SVG file format.

### Make Your Font Ready For Web Embedding

Now we can use the [Font Squirrel @font-face Generator](http://www.fontsquirrel.com/fontface/generator) to simply crunch our SVG file and give us some web embeddable fonts for use in our MVC4 project.

Just select your .SVG file and then click the **"Download You Kit"** button.

![Download font kit](/images/glyphs-downloadkit.png "Download font kit")

*Tip: when you click "Add Fonts" you will notice the file upload will not select .SVG files - just type in the name of your SVG file and continue.*

### Preview Your Web Embeddable Font

The kit you download will contain everything you need.

![Kit files](/images/glyphs-files.png "Kit files")

Just fire up the .html file and see how your icons look as represented by your new font. All your icons will be mapped to characters such as "a".

![Glyphs chart](/images/glyphs-chart.png "Glyphs chart")

### Referencing Your Web Embeddable Font

Place your web embeddable font inside your MVC4 solution like any other asset:

![Glyphs assets](/images/glyphs-assets.png "Glyphs assets")

We simply consume the font through stylesheet code:

![Glyphs css](/images/glyphs-css.png "Glyphs css")

### Using Your Web Embeddable Font

And now the fun part - making use of our icon font to display images.

We need to generate some stylesheet code that can be used to display icons in our MVC4 web app.

This class is using our custom web embeddable font:

![Glyphs fonticon](/images/glyphs-fonticon.png "Glyphs fonticon")

Did you spot the "voodoo" syntax above? We are saying that this style matches any element that has a CSS class attached which starts with "fonticon-". This selector has been around since the days of IE6.

And now we can define a CSS class that extends this rule on a per icon type. In this case lower case "b" is our "help" icon.

![Glyphs fonticon](/images/glyphs-fonticon2.png "Glyphs fonticon")

Finally, lets put "icons" on the webpage:

![Using Glyphs](/images/glyphs-useit.png "Using Glyphs")

...and this will result in:

![Using Glyph result](/images/glyphs-result.png "Using Glyph result")

And because our icons are represented through a font, we can change the color and size as required using CSS attributes like **color** and **font-size**:

![Using Glyph result](/images/glyphs-result2.png "Using Glyph result")

There is simply no need to produce different images files for various image sizes or colors.

### Killing off Gradient Images Too

Background style gradient images are typically used to make web app's look pretty.

But we can also replace these images with CSS equivalents using [ColorZilla](http://colorzilla.com/gradient-editor). It's possible to generate via CSS most effects that you would normally produce in Photoshop.

![gradient](/images/glyphs-gradient.png "gradient")

### Summary

You can build a web app using MVC4 or Web Forms that uses no images but looks awesome.

Pure CSS. Zero images. Reduced web requests, Faster web app.

Hope this helps you at some point and you should also check out Scott Hanselman's [post on optimization.](http://www.hanselman.com/blog/TheImportanceAndEaseOfMinifyingYourCSSAndJavaScriptAndOptimizingPNGsForYourBlogOrWebsite.aspx)

### Download

[Download](http://www.geminiplatform.com/downloads/blog/countersoft-iconfont.zip) the Countersoft icon font sample complete with sample HTML and CSS.