---
layout: post
title: "debugDescription"
date: 2014-03-25 20:03:38 +0000
---
When I was preparing my talk about remote debugging toolset [PonyDebugger](https://github.com/square/PonyDebugger) at [NSLondon](http://www.meetup.com/NSLondon/) I had a pre-presentation in front of my lovely co-workers. I was talking a lot about debugging and how to be efficient when doing this. After finished the talk I was asked many questions but this stood out...

> Luka, why don't you write code without bugs?
>
> @Isabel_Glover https://twitter.com/Isabel_Glover

Good question, but that's just how it is. We are all humans after all and we make mistakes sometimes. Or maybe the third party libraries we use are not polished enough and specific methods do not function properly so you need to debug.

Just a random model
====================
So with an abundance of models in your project imagine that you have a model for `Person`
{% highlight objc %}
@interface LBPerson : NSObject

@property NSString *name;
@property NSString *surname;
@property NSUInteger age;
@property NSString *city;

- (id)initWithName:(NSString *)name Surname:(NSString *)surname Age:(NSUInteger)age City:(NSString *)city;

@end
{% endhighlight %}

You are fiddling around, debugging and inspecting the values that are held inside a newly created object.
{% highlight objc %}
LBPerson *luka = [[LBPerson alloc] initWithName:@"Luka" Surname:@"Bratos" Age:25 City:@"London"];
{% endhighlight %}

Let's debug, yay!
====================
Let's do things like this and waste our time:
{% highlight objc %}
NSLog(@"%@", luka);
{% endhighlight %}
Output: `2014-03-25 21:53:28.488 Debug[6752:60b] <LBPerson: 0x8c6a110>`

{% highlight objc %}
NSLog(@"%@", luka.name);
{% endhighlight %}
Output: `2014-03-25 21:55:06.740 Debug[6794:60b] Luka`

This is not very useful. And it takes too much time to write these annoying `NSLog`'s.

The right way
====================
If you take a look and dive into `NSObject` header file you will see these two methods:
{% highlight objc %}
- (NSString *)description;
@optional
- (NSString *)debugDescription;
{% endhighlight %}

In the iOS Developer Library, more specifically in [Mac OS X Debugging Magic](https://developer.apple.com/library/mac/technotes/tn2124/_index.html#//apple_ref/doc/uid/DTS10003391-CH1-SECCOCOA) you can see the definition for `description` is:

> All Cocoa objects (everything derived from NSObject) support a description method that returns an NSString describing the object.

But in most cases the default description is not enough. You can still get some information out but it can be done better.

debugDescription
====================
You can take advantage of `GBD` or `lldb` debugger and use `print-object` or `po`.
By default `po` will basically call the `debugDescription` method which by default invokes `description` method of the specified object.

You can decouple these by implementing `debugDescription` on your own.
{% highlight objc %}
- (NSString *)debugDescription
{
	return [NSString stringWithFormat:@"Name: %@\nSurname: %@\nAge: %d\nCity: %@\n", self.name, self.surname, self.age, self.city];
}
{% endhighlight %}

End result:
--------------------
{:refdef: style="text-align: center;"}
![]({{ site.url }}{{ site.baseurl }}/images/2014-03-25-Po.png)
{: refdef}
