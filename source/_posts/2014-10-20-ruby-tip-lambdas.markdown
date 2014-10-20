---
layout: post
title: "Ruby Tip: Lambdas"
date: 2014-10-20 09:31:37 -0600
comments: true
categories:
---

If you have a background in a language like Javascript, you're likely familiar
with the concept of lambdas, but for those who are just getting started with
Ruby, lambdas are a likely source of confusion.

In the most basic sense, a lambda is a method without a name. In Javascript, this
is called an "anonymous function." While they are methods, lambdas are also objects.
This allows you to save the method to a variable to be used later.

For example:

```ruby
name_giver = lambda { "Your name is John" }
name_giver.call  # => "Your name is John"
```

First, notice the lambda syntax: `lambda` followed by a block inside of curly braces.
On its own, this method could never be called in an application, because it is
unnamed. For that reason, we store the method in a variable ( `name_giver` ). When we
are ready to return the output of the lambda, we can do so with the `call` method
on the variable ( `name_giver.call` ).

Like other methods, lambdas can take parameters to be used in the block:

```ruby
name_giver = lambda { |name| "Your name is #{name}" }
name_giver.call("Wes")  # => "Your name is Wes"
```

For one example of how lambdas are used in rails, see the post on
[scopes](/blog/2014/10/17/using-scopes/). Note the usage of the alternative syntax
using `->`, also known as a lambda literal, or a dash rocket.
