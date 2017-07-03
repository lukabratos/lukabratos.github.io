---
layout: post
title: "A/B testing iOS apps like a pro"
date: 2014-01-14 14:48:54 +0000
---
[A/B testing](http://en.wikipedia.org/wiki/A/B_testing) is commonly used in several forms of advertising predominantly in online marketing and web development.
It is often called split-testing because sample of users is usually split in two halves or variants **A** and **B**.

Variants **A** (baseline) and **B** are compared which differ one from another in small aspect that might impact user's behaviour.
Point of A/B testing is finding the predominant version that will result in higher [conversion rates](http://en.wikipedia.org/wiki/Conversion_marketing).

Say hello to Bestly
====================
[Bestly](http://best.ly) offers an easy way to add A/B testing and staged rollouts to native mobile applications.

The easiest way to add Bestly into your iOS project is using [CocoaPods](http://cocoapods.org).
Simply create new **Podfile** or add to existing one ```pod 'Bestly'``` and run ```pod install``` in your terminal.

Then you need to add the following import to your ```.pch``` or ```AppDelegate.m```
{% highlight objc %}
#import <Bestly/Bestly.h>
{% endhighlight %}
Sign into [Bestly](http://best.ly) and create a new app. Paste the following line to your ```application:didFinishLaunchingWithOptions:``` and replace ```APP_KEY``` with key that was generated on the website.
{% highlight objc %}
[Bestly setupWithKey:@"APP_KEY"];
{% endhighlight %}

Creating your first controlled experiment
====================
You can run experiment with **two** *A, B* or **three** *A, B, C* variants.

In this example I will show you the basic usage with two variants.
{% highlight objc %}
/**
 * Call this whenever you'd like to run an experiment.
 * @param experimentID The experiment ID corresposding to the
 * experiment that you'd like to run.
 * @param a Experiment variation A. This is the baseline
 * variation. Please note that in any sort of failure
 * (e.g. lack of internet connectivity),
 * this will be the variation that is run.
 * @param b Experiment variation B.
 */
+ (void)runExperimentWithID:(NSString *)experimentID
                          A:(void(^)(void))a
                          B:(void(^)(void))b;
{% endhighlight %}

Let's say that we want to A/B test a button with different title. We've got two variants:

- **A**: Hi!
- **B**: Hello!

Paste this code and replace ```EXPERIMENT_ID``` with *Experiment ID* that you can find on the website.
{% highlight objc %}
[Bestly runExperimentWithID:@"EXPERIMENT_ID" A:^{
    // Insert your testing logic here...
    [self.testButton setTitle:@"Hi!" forState:UIControlStateNormal];
} B:^{
    [self.testButton setTitle:@"Hello!" forState:UIControlStateNormal];
}];
{% endhighlight %}

Place the following code to the ```UIButton``` action method so you can see if variation's goal was reached.
{% highlight objc %}
- (IBAction)testButtonTapped:(id)sender {
    [Bestly goalReachedForExperimentID:@"EXPERIMENT_ID"];
}
{% endhighlight %}

After playing around a little bit on the iPhone simulator we can see that the conversion rate is higher for the button with the title **Hello**. However to get accurate results, the number of users needs to be bigger than 100 per variation.

{:refdef: style="text-align: center;"}
![Experiment]({{ site.url }}{{ site.baseurl }}/images/2014-01-14-ExperimentResult.jpg)
{: refdef}

Conversion rate and change over baseline
====================

For each variation conversion rate is calculated as:

{:refdef: style="text-align: center;"}
![Conversion Rate](https://latex.codecogs.com/gif.latex?p&space;=&space;\frac{x}{n})
{: refdef}

Where **x** is number of conversions and **n** is number of people that have viewed variation.

Formula for calculating change over baseline is basically substraction of conversion **B** and **A** divided by conversion value of **A**.

{:refdef: style="text-align: center;"}
![Change Over Baseline](https://latex.codecogs.com/gif.latex?c&space;=&space;\frac{p_{b}-p_{a}}{p_{a}})
{: refdef}

Update
====================
The response for this blog post was immense. I was lucky enough to get on the the front page of [Hacker News](https://news.ycombinator.com/item?id=7063595) resulting in almost 3000+ visitors from 70+ countries in one day—not too bad.

I also received a very special email. It was from James, the founder of Bestly. He thanked me for writing the post and informed me that a new version of Bestly is coming out.

What changed?
---------------------
1. There's a new version of Bestly SDK on [GitHub](https://github.com/bestly/bestly-ios)
2. You don't need to use ```[Bestly goalReachedForExperimentID:@""];``` anymore[^1].
3. Design changes on the dashboard
4. Other things such as aliases

You can download the project [here](https://github.com/lukabratos/Bestly).

[^1]: You can send any event you want to track and it wil be tracked for every experiment.