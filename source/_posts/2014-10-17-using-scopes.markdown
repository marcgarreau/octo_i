---
layout: post
title: "Using Scopes"
date: 2014-10-17 14:52:32 -0600
comments: true
categories:
---

Scopes allow you to make (what amounts to) helper methods for
commonly used Active Record queries.

For example, we can easily display a restaurant's appropriate
seasonal menu using scopes. Within the model, we'll build a scope
for items on the winter seasonal menu:

```ruby item.rb
class Item < ActiveRecord::Base
  scope :winter, -> { where(seasonal_menu: "winter") }
end
```

In our items controller now, we can call `Item.winter` to pass
only those items on the winter menu to our view. We also experience
the joy of pushing logic from our controller, down to the model.

Scopes can also be chained, to form more specific queries:

```ruby item.rb
class Item < ActiveRecord::Base
  scope :winter, -> { where(seasonal_menu: "winter") }
  scope :winter_high_carb, -> { winter.where("carbs > 30") }
end
```

Another common use case for scopes is to redefine how our query results
are ordered by default. The helper method `default_scope` exists:

```ruby item.rb
class Item < ActiveRecord::Base
  default_scope { order('name ASC') }
end
```

In this case, items will now be displayed in alphabetical order by default.
