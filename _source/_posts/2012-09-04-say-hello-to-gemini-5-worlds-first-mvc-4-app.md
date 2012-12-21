---
layout: post
title: World's First MVC4 App - Say Hello to Gemini 5
tags: aspnet mvc4 gemini issue-tracking
excerpt: I have been speaking at events, user groups and blogging about how we switched to MVC 4 while it was still in Developer Preview. See what we cooked up.
---
I have been speaking at events, user groups and blogging about how we switched to MVC4 while it was still in Developer Preview. Our new tech stack consists of ASP.NET MVC4, Web API, Sass and NHibernate. We re-built our Gemini product afresh and it sports a pretty unique approach to application navigation - App-Nav™. Some folks have commented this approach is Metro-esque. See for yourself.

### It's Here...

I am delighted to say we have finally shipped Gemini 5. In summary it's an all-in-one agile tracker that covers Agile, Scrum, Help Desk, Ticketing and good old Issue Tracking. Go [check it out](http://www.geminiplatform.com) and see why we got rave reviews at Microsoft TechEd!

![issue tracker](/images/first-look.png "issue tracker")

Gemini 4 was built over a number years on the ASP.NET WebForms 2.0 stack. We just shifted our entire tech stack to the latest from Microsoft. I will blog later why we "started again" even though [conventional wisdom](http://www.joelonsoftware.com/articles/fog0000000069.html) says otherwise.

Here is a quick breakdown of what we used to build a commercial product on top of MVC 4.

### User Interface

No JQuery UI, Twitter Bootstrap or third party widgets. Everything in the UI was either home-grown or carefully hand-picked to work with our MVC4 web app.
 
* [Chosen](http://harvesthq.github.com/chosen/) - probably the best dropdown, data selector control
* [jScrollPane](http://jscrollpane.kelvinluck.com/) - nice, neat custom scrollbar library
* [Colorbox](http://www.jacklmoore.com/colorbox) - safe and easy lightbox plugin
* [Dynatree](https://code.google.com/p/dynatree/) - which app doesn't need a tree control?!
* [Micro-charting](http://omnipotent.net/jquery.sparkline/#s-about) - nice little micro-charting library
* [File Uploader](https://github.com/valums/file-uploader) - cross-browser ajax file uploader
* [Spin.js](http://fgnass.github.com/spin.js/) - customizable Javascript based progress indicators
* [Full Calendar](http://arshaw.com/fullcalendar/) - Outlook style calendar control
* [Date Picker / Color Picker](http://www.eyecon.ro/) - basic common picker controls
* [Context Menu](http://labs.abeautifulsite.net/archived/jquery-contextMenu/demo/) - cross-browser right-click goodness
* [Sass](http://sass-lang.com/) - if your not using Sass or Less for CSS dev then you are doing it wrong

We must have tested out 100+ UI widget controls and libraries – most failed the cross-browser, usability and the "can I get it to easily integrate" test.

### Middleware

It's dead simple:

* Razor View Engine - it works well but the generated views still have too much whitespace for the time being
* Just regular Controllers but with a design rule that put Ajax actions into separate Controllers from actions that return Views. It's better from a maintenance and processing cycle view-point
* Application caching is just based on regular ASP.NET Cache with an option to leverage Redis

![file structure](/images/first-structure.png "file structure")

We simply used the framework as-is with the usual mix of Design Patterns and best practices (e.g. using [Unity](http://unity.codeplex.com) for Dependency Injection (because it works on Medium Trust as well)).

### Data Persistence

We work to support any edition of SQL thanks to our decision to stick with NHibernate. It just works with options for performance tuning and so don't discount using an ORM such as NHibernate or Entity Framework. Cut through the FUD and do what works best for you.

Simple mapping files are all we need to define persistence.

![Hibernate](/images/first-hbm.png "Hibernate")

Who says you cannot get ORM to scale and into commercial products?!

### Product API

We you build a commercial product the chances are people will want to integrate and extend in ways that you will not even dream up. Cue your product's API strategy.

We built our API in just two days using the awesome ASP.NET Web API framework and the equally useful RestSharp library.

### ...And Finding Another Way To Navigate

Tired of clunky, same-old navigation? I am.

I challenged the UI guys to come up with something more intuitive that actually put users in control.

How about you kill all top-line navigation and instead allow users to pin, share and navigate using Cards/Tiles? Bake in badge notifications to let you know when things change and you've killed off email at the same time.

![AppNav](/images/first-appnav.png "AppNav")

Hope something here helps someone, someday.

Want to see MVC4 in action? [Head over here.](http://www.geminiplatform.com)



