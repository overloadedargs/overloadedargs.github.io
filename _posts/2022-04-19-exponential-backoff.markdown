---
layout: post
title:  "Exponential Backoff in Ruby"
date:   2022-04-19 12:04:08 +0100
categories: jekyll update
---

This is a really quick post about the importance of exponential backoff and retry loop. In Ruby sometimes we want to use exception handling to make our programs handle errors more gracefully. Usually if we start catching exceptions or creating specific exception classes then overall our strategy of programming tends to be more defensive. Whereas if we decide not to handle exceptions or cover edge cases then our programs tend to be more offensive; no-one actually calls their programs more offensive, they usually only say if a program is defensive but if it’s less defensive the it reduces the cyclomatic complexity and increases the ability for the code to be changed later on. There is a concept of code that is secure by design and code that handles exceptions lends itself well to producing error free code. The best Ruby book on exception handling is called Exceptional Ruby by Avdi Grimm.

I guess the point here is that if you happen to have the client on site, or the design documents seem complete then it’s better to write code that won’t have unforeseen circumstances, whereas if you have to iterate on your code quite a lot or know that you will have to add features later then you may not need to worry about handling errors; a metric to help figure out if your code should be more defensive is churn, if it’s being updated quite a lot then maybe you won’t need to add a lot of exception handling. What does this have to do with Exponential Backoff?

In web development we want an optimum amount of retries when a service goes down or is delayed because of congestion in the network. This can be achieved very easily in Ruby or any language which supports try and catch. It works really well if you throw an exception and use retry. The idea here is that you build your retry loop based on a timer that you increase exponentially before retrying, this allows you to write code that ignores problems with network latency. If it wasn’t retried exponentially you would have a large amount of extra redundant retries, this is especially bad if that part of the code is memory intensive. It just so happens that increasing the timer exponentially leads to the optimum amount of retries. Basically in your code that rescues the exception you call sleep. This is relevant to code that makes HTTP requests and not really for general error handling.

Let's give an example with a symbols are faster than strings and ternary if:

{% highlight ruby %}

begin 
  method_which_can { raise Exception }
rescue Exception
  timeout(:exponential) ? retry : timer_then_retry
end

{% endhighlight %}