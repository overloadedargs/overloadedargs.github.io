---
layout: post
title:  "Attribute Accessor and Assignment"
date:   2021-08-15 11:08:08 +0100
categories: jekyll update
---
This post is a super simple post for complete Ruby newbies with some interesting thoughts about OOP. It should be suitable to anyone who is interested in OOP.

The most important thing with OOP in my opinion is encapsulation. It gives each object access to only the methods which should be acted on. When we think about the L in SOLID as Liskov Substitution Principle, we can see that encapsulation and pure OOP and substitution do not necessarily go hand in hand. I will give an example below with reference to the Ruby newbie attr_accessor etc.

In Ruby we have three main tools for defining ways to set and get data on a class. These are attr_accessor which produces two methods on the class at runtime. e.g. by writing attr_accessor :shape, we will get the methods:

{% highlight ruby %}
def shape
  @shape
end

and 

def shape=(shape)
  @shape = shape
end
{% endhighlight %}

similarly if we write attr_reader :shape, we will only get the getter method def shape, and attr_writer we will only get the method def shape=. 

All of these methods are interesting because they are getter and setter methods. Pure OOP encapsulation means that we don't actually use any getter or setter methods so we wouldn't use any attr assignment methods in Ruby, instead we would only initialise the data and then later on provide methods to manipulate the data.

Let's imagine that a top level class actually needed these getter and setter methods, but the sub class wanted to enforce complete encapsulation, with no getter or setter methods, for documentation, maintainability, readily, tersity etc and used private to encapsulate these methods. Well if we wanted to enforce Solid we would have to include these getter and setter methods on the subclass, so that each subclass could be substituted with the top superclass. It can be very hard to be strict with your OOP programming, and yes using parts of Solid can make your code more agile and less resistant to code smells and the need to refactor but they should be used flexibly. Thinking more in terms of not violating single responsibility can give you a better feeling for designing or refactoring code.

When there are other things to consider such as reducing method chaining, it could be found that trying to consider interface segregation and dependency inversion will be hard. Open-closed principle in Ruby is usually very easy to implement, along with substitution if your subclass defines the same methods as the superclass then the original class should not need to be modified in any way.

Finally just an extra mention that in inheritance classes should be closed for modification but open for extension.