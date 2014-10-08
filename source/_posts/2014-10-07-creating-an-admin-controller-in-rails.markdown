---
layout: post
title: "Creating an Admin Controller"
date: 2014-10-07 19:21:52 -0600
comments: true
categories:
---

Why create an Admin Controller?
-------------------------------

It is easy enough to use something like [CanCanCan](https://github.com/CanCanCommunity/cancancan)
to implement authorization, so why then would you need a separate Admin Controller for your application?

While CanCanCan provides a good base, adding an Admin Controller provides another layer of protection,
as the routes you are trying to protect simply do not exist unless you are properly authenticated as an Admin.

It is easy to leave a route unprotected or make a mistake if your edit/create/update/destroy routes are in
your public controllers, and having an Admin Controller helps prevent mistakes.

Implementation
--------------

1.  Within app/controllers, create another directory called 'admin' and create an `admin_controller.rb` in that new directory.
2.  Create each individual controller that you want in the admin namespace. For example, if you would like to only allow an admin to edit or delete an item, create a new Items Controller with those actions inside of the controllers/admin folder.
3.  Remove the newly protection actions from the old Items Controller, leaving only those actions that you'd like users to access (e.g. show and index).
4.  Add the admin namespace to your routes file:
``` ruby routes.rb
namespace :admin do
  resources :items
end

resources :items, only: [:index, :show]
```
Notice that our old Item Controller still maintains it's own routes, which we specify with `:index` and `:show`.

Now when we `rake routes`, you'll see the newly created namespaced routes:
{% terminal %}
new_admin_item  GET   admin/items/new         admin/items#new
   admin_items  POST  admin/items(.:format)   admin/items#create

  # etc...
{% endterminal %}

Sticking Point
--------------

Note that you will have to customize the POST from your admin forms:
`<%= form_for(@item, :url => admin_item_path(@item) ) do |f| %>`


_Authored by Ian Andersen ([@ianderse27](http://twitter.com/ianderse27)). The original post is available [here](http://mindfulprogramming.com/creating-an-admin-controller-in-rails/)._
