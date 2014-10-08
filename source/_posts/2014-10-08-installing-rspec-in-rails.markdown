---
layout: post
title: "Installing Rspec in Rails"
date: 2014-10-08 13:34:35 -0600
comments: true
categories:
---

By default, you'll get a `test` folder with Minitest ready to rock when you generate a new rails project. Switching to the other team is a cinch though.

To generate a new rails project without a test suite, run:

{% terminal %}
$ rails new sample_app -T
{% endterminal %}

Or to get rid of your Minitest folder in an existing project:

{% terminal %}
$ rm -rf test/
{% endterminal %}

Next, add RSpec to your Gemfile:

``` ruby Gemfile
gem 'rspec-rails'
```

Install the new gem:

{% terminal %}
$ bundle install
{% endterminal %}

Then install RSpec:

{% terminal %}
$ rails generate rspec:install
{% endterminal %}

Look at you go, with your fresh new `spec` folder in the root directory. Get testing!
