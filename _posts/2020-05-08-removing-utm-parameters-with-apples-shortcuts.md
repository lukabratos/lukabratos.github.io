---
layout: post
title: "Removing UTM parameters with Apple's Shortcuts"
date: 2020-05-08 14:40:00 +0000
---

Here's the thing. You're browsing on the internet and find an interesting website that you want to share with a friend.

You tap on a share button and copy the website's URL. Suddenly you realize that the URL contains pesky UTM parameters.

> Urchin Tracking Module (UTM) parameters are five variants of URL parameters used by marketers to track the effectiveness of online marketing campaigns across traffic sources and publishing media.[^1]

As a self-respecting friend you're -- you quickly remove those from the end of the URL before you share it with your friend.

## iOS Shortcuts

I knew about the app[^2] for a while, from the times when it was called Workflow[^3] but I never gave it a proper chance until today. While I was browsing through the gallery of all the available shortcuts I came up with an idea ...

## Automate all the things

There's a large set of actions supported in Shortcuts. The big deal however is the ability to [run](https://support.apple.com/en-gb/guide/shortcuts/apd218e2187d/ios) JavaScript on a web page.


Since the URL polluted with UTM parameters looks like this:

https://www.example.com/page?<mark>utm_content=buffercf3b2&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer</mark>

I came up with a naive solution in JavaScript that would drop all the query parameters from the URL:

```javascript
const url = window.location.href;
const result = url.split('?')[0];
completion(result);
```

Complete shortcut:

Part 1             |  Part 2
:-------------------------:|:-------------------------:
![Shortcuts1]({{ site.url }}{{ site.baseurl }}/images/Shortcuts1.jpg)  |  ![Shortcuts2]({{ site.url }}{{ site.baseurl }}/images/Shortcuts2.jpg)

As a last step, I've enabled the shortcut in the Share Sheet so it's easily accessible:

![Shortcuts1]({{ site.url }}{{ site.baseurl }}/images/Shortcuts3.jpg)  |  ![Shortcuts2]({{ site.url }}{{ site.baseurl }}/images/Shortcuts4.jpg)

## Sorry marketers

Usage workflow is following:

1. You're viewing web page in Safari
2. Tap on share button
3. Tap on **Remove UTM** button
4. Copy or share the sanitised URL

You can download the shortcut [here](https://www.icloud.com/shortcuts/97544ab172b54c9bb2619c5dd19c8b3f). As a prerequisite you need to enable setting for [Untrusted Shortcuts](https://support.apple.com/en-hk/HT210628).

Do you use Shortcuts app? Which shortcuts are your favourite ones? Do you know any other tricks? Let me know on [Twitter](https://twitter.com/lukabratos/status/1258819152095195141)!

Found a typo? Feel free to [edit](https://github.com/lukabratos/lukabratos.github.io/edit/master/_posts/2020-05-08-removing-utm-parameters-with-apples-shortcuts.md) this blog post.

[^1]: [Wikipedia](https://en.wikipedia.org/wiki/UTM_parameters)
[^2]: [Shortcuts](https://apps.apple.com/us/app/shortcuts/id915249334)
[^3]: [Workflow](http://workflow.is/)
