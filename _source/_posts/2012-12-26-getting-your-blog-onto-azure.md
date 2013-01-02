---
layout: post
title: Getting Your Jekyll Powered Blog onto Azure
tags: blog azure jekyl github kudu
excerpt: How to get your Jekyll powered blog running on Azure with continuous deployment from GitHub.
---
Azure is simply a no-brainer for running your Microsoft stack in the cloud. But what about those scenarios where little or no Microsoft tech is involved? Like this blog. The great thing about Azure is you can run just about anything you like and with virtually zero deployment friction.

### Starting With Jekyll

If like me you want to try out something other than the usual WordPress + MySQL combo, then you need to check out Jekyll as an approach for managing a blog.

Jekyll is a blog-aware static site generator that will take a template directory and spit out a complete blog site. You write your blog posts in Markdown and watch Jekyll generate the HTML for your blog. It's the same technology behind GitHub Pages.

#### The Basics

Follow the Jekyll [installation](http://jekyllrb.com) process to get going. It's a Ruby gem but don't let that put you off if you're running Windows - not bad install process.

Once installed you can use my blog's [source code](https://github.com/HarveyKandola/collectivematters) as a template for your own blog.

Your blog structure will look like the following:

![Jekyll Structure](/images/azureblog-dir1.jpg "Jekyll Structure")

In summary:

* The "\_source" folder holds your blog site structure and post content
* The "website" folder is the output from Jekyll - your generated blog site
* The "\_config.yml" is the Jekyll configuration file

The blog template uses [Disqus](http://disqus.com) for comments and the usual social sharing icons are present.

#### The Input

A closer look at the "\_source" folder:

![Jekyll Source Folder](/images/azureblog-dir2.jpg "Jekyll Source Folder")

You can get the complete low-down on the Jekyll structure [from here](https://github.com/mojombo/jekyll/wiki/Usage).

#### The Output

Assuming you have Ruby with Jekyll installed and have used my blog as the basis of your own, then simply run Jekyll in your blog directory (via command prompt/terminal window):

    jekyll --server

Jekyll will now monitor the "\_source" folder for changes, re-generate the blog and place it in the "website" folder. Have a look at this folder to see the generated blog HTML site:

![Your Jekyll Blog](/images/azureblog-dir3.jpg "Your Jekyll Blog")

Now navigate over to http://localhost:4000 and see you blog running.

![Your Jekyll Blog](/images/azureblog-preview.jpg "Your Jekyll Blog")

#### Writing Blog Posts

You will find all posts residing in the "\_posts" folder. Use [Markdown](http://daringfireball.net/projects/markdown/syntax) to write your blog posts in any text editor (Markdown powers a lot of GitHub and Stackoverflow so you're in good company).

![Writing Blog Posts](/images/azureblog-dir4.jpg "Writing Blog Posts")

As soon as you have your blog post the Jekyll engine will spit out the re-generated site. Here is what the blog post you are reading now looks like in Markdown:

![Markdown](/images/azureblog-markdown.jpg "Markdown")

### GitHub

One of the great things about GitHub is you get free public repositories which are perfect for your blog. Even better is that Azure can continuously build and deploy from GitHub.

Once you have a your blog ready just push the entire folder structure onto GitHub ("\_source", "website", etc.).

This is my blog on GitHub:

![GitHub](/images/azureblog-github.jpg "GitHub")

Take special note of the ".deployment file".

### Deploy to Azure

If you haven't signed up for an Azure account then I recommend you do so and get 10 websites hosted for free.

I'll assume you are able to create a website on Azure and you can follow the [instructions](http://www.windowsazure.com/en-us/develop/net/common-tasks/publishing-with-git/#header-3) to set up Azure and GitHub for continuous deployment.

At this point take a look at the Azure Deployment tab to see if Azure has pulled from GitHub and deployed your blog:

![Azure Deployment](/images/azureblog-azure1.jpg "Azure Deployment")

Simply navigate to your Azure website URL (e.g. yourblog.azurewebsites.net) and you should see your blog up and running.

The great thing with this apporach is that we can roll back the blog site to any previous deployment state. Not entirely sure how you would do that with a WordPress blog but SQL restore springs to mind!

#### The .deployment File

The eagle eyed reader would have spotted a potential configuration issue with the Azure + GitHub continuous deployment approach. How do you tell Azure to deploy just the contents of a particular directory (in this case the contents of the Jekyll generated "website" directory)?

Remember that ".deployment" file I mentioned above?

Azure allows you to "control" the deployment process for pushing live your code. Adding the following line to the ".deployment" file will ensure the desired folder is deployed by Azure:

    [config]
    project = website

Side note: if you wanted to deploy a specific project that is part of a Visual Studio solution then the above project setting should be used to tell Azure which project to deploy.

The ".deployment" file is just an INI file and is part of the [Kudu engine](https://github.com/projectkudu/kudu) that handles Azure deployments. It's open-sourced over at GitHub.

All of a sudden cutting code or writing a blog post feels the same. Job done.

### Further Reading

* [Jekyll](https://github.com/mojombo/jekyll)
* [Kudu](https://github.com/projectkudu/kudu)
