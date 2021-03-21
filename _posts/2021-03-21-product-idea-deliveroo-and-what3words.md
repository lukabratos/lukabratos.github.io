---
layout: post
title: "Product idea: Deliveroo and what3words"
date: 2021-03-21 13:09:00 +0000
excerpt: "Using what3words in the Deliveroo driver app to easily locate customers"
---

_The aim of this blog post is to propose a potential solution which would make it easier for Deliveroo drivers to easily locate customers in large and crowded open spaces._

Today I went for my usual Sunday run and while I was running through Victoria Park I came up with the following idea.

## Ordering the food

Since the days in London are getting longer, the temperature is rising and the lockdown will hopefuly ease down soon, people will start meeting again more often.
However, the pandemic is not over yet so, in order to be compliant with the rules of social distancing, people will prefer to meet in parks.

Imagine now that you're with your friends in the park but to make the day perfect a nice, warm pizza is missing 🍕.

You open up the Deliveroo app and order the food.

Driver gets the request and attempts the delivery. Here's the problem. There's a lot of people in the park, so how will the driver know where I am without calling me?

## Delivering the food

I have mentioned [what3words](https://what3words.com) but how does this service fit in?

> We divided the world into 3 metre squares and gave each square a unique combination of three words. It’s the easiest way to find and share exact locations. [^1]

The beauty of what3words is that they're able to identify any location with a resolution of about 3 metres, and that's enough for the driver to know where in the park I'm sitting with my friends.

As an example [here's](https://w3w.co/shark.comical.cakes) a random location in Victoria Park.

## Implementation

Potential implementation could be like this:

1. When the customer makes the order, application already requests user's location
2. When application receives the location, API call to what3words is made, using one of their SDKs, which maps user's location to 3 unique words
3. User can then approve the location or edit it
4. Driver's app will show 3 words that are describing user's location
5. Now the driver has an option to either use the standard maps (Apple/Google) or what3words
6. If driver taps on 3 words, Deliveroo app can open what3words application, showing user's exact location

## Benefits

- The driver will be able to locate the customer easily
- If there's a problem, it's easier to communicate the location
- Less chances that food will arrive cold

[^1]: [What is what3words?](https://what3words.com/about)
