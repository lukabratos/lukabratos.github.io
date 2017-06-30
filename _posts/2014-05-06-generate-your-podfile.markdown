---
layout: post
title: "Generate your Podfile"
date: 2014-05-06 12:54:40 +0100
---
On March, 29th I attended a [CocoaPods Bug Bash](http://blog.cocoapods.org/CocoaPods-Bug-Bash/) event. The main idea was to clean up any issues and remove duplicates in the [CocoaPods repository](https://github.com/CocoaPods/CocoaPods).

Creativity, curiosity, execution.
====================
After the Hackathon finished I said to myself, wouldn't it be great if you could generate a [Podfile](http://guides.cocoapods.org/syntax/podfile.html) with all your favourite third party libraries when you're starting a new project. It would be similar to services like [Ninite](https://ninite.com) for bulk downloading multiple applications.

Because there is the [CocoaPods search API](http://blog.cocoapods.org/Search-API-Version-1/) and it was about time to say hello again to Ruby I started working on it.

I [asked](https://github.com/CocoaPods/CocoaPods/issues/1971) CocoaPods contributors what they thought about the idea, which served as a brief validation for me.

From mockup to actual product
====================
I wanted a pretty simple workflow: search for libraries that you need to start working on your project. A dropdown with suggested libraries will appear. Select library. Repeat. The tags will appear bellow the search bar so that you know which libraries have been selected.
Final step is one click away. It will generate and download a `Podfile` with your pods so you just need to `pod install` them!

{:refdef: style="text-align: center;"}
![]({{ site.url }}{{ site.baseurl }}/images/2014-6-5-Mockup.png)
{: refdef}

Because I like nice and simple products, I asked a friend of mine if he could help me with design and logo.

**He did the magic.**

{:refdef: style="text-align: center;"}
![]({{ site.url }}{{ site.baseurl }}/images/2014-6-5-Logo1.png)
{: refdef}

Cocoa Brewer
====================
{:refdef: style="text-align: center;"}
![]({{ site.url }}{{ site.baseurl }}/images/2014-6-5-CocoaBrewer.gif)
{: refdef}

Before you go...
====================
There's a ton of things that can be improved. Feel free to [contribute](https://github.com/CocoaBrewer/cocoabrewer.org).