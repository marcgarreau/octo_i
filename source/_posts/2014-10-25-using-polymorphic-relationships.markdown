---
layout: post
title: "Using Polymorphic Relationships"
date: 2014-10-25 20:43:24 -0600
comments: true
categories:
---

If your application has some type of activity feed, there's a reasonable chance that a polymorphic relationship is right or you.

In this example, we're going to allow a user to bookmark jobs and events, then view those in one bookmarks feed.

When it comes to building our data model, we have at least three options:

1.  __Users have many Jobs and many Events.__<br/>
Having separate relationships will require separate database queries. Huge performance red flag.
2.  __Users have many Jobs and many Events, through a join table.__<br/>
The disadvantage here is that the join table will have columns to account for both relationships: `job_id` and `event_id`.
Everytime a job is bookmarked, the value for the `event_id` will be left blank/unused. Similarly, for every row representing a bookmarked
event, there will be no value for the `job_id`. As our application grows, this inefficiency may result in a performance loss.
3.  __Users have many Bookmarks, which may be Jobs or Events, using a polymorphic relationship.__<br/>
As you suspected, we're going to explore this option.

First, create the migration:
```ruby
class CreateBookmarks < ActiveRecord::Migration
  def change
    create_table :bookmarks do |t|
      t.references :user, null: false
      t.references :bookmarkable, null: false, polymorphic: true
    end
    add_index :bookmarks, [:user_id, :bookmarkable_id, :bookmarkable_type], unique: true, name: ‘bookmarks_index’
  end
end
```

Our concern is here: `t.references :bookmarkable, polymorphic: true`.
The result of this reference is that our bookmarks table will have the columns `bookmarkable_type` and `bookmarkable_id`.
So, if a job is bookmarked, the row in the bookmarks table will have an `id`, the appropriate `user_id`, a `bookmarkable_type` of "Job",
and a `bookmarkable_id` of that job's `id`. Notice that we waste no fields on event-related properties. When an event is bookmarked,
the row will look similar, but have a `bookmarkable_type` of "Event".

To utilize this polymorphic association, we'll have to complete the related relationships:

```ruby user.rb
class User < ActiveRecord::Base
  has_many :bookmarks
end
```

```ruby bookmark.rb
class Bookmark < ActiveRecord::Base
  belongs_to :user
  belongs_to :bookmarkable, polymorphic: true
end
```

Now in our views, we're free to display a user's bookmarks however we choose to scope them.
