---
layout: post
title: Active Record Associations Basics
date:  2020-07-22
---

Associations are the way you tell Rails how models are related to each other in the database. Think of a blog site, an author would have blog posts, comments, categories etc. Ruby on Rails makes it simple to link all of these models together using what looks like plain english.

Ruby on Rails has six types of associations. Today Let's take a look at three common Ruby on Rails Associations and explain what they do.

## belongs_to
```ruby
# app/models/blog.rb
class Blog < ApplicationRecord
    belongs_to :author
end
```

The `belongs_to` association here creates a one to one connection to the Author model. Meaning each Blog model created will can be connected to just one Author model.

## has_many
```ruby
# app/models/author.rb
class Author < ApplicationRecord
    has_many :blogs
end
```

The `has_many` association makes a one to many associations to the Book model. This means that the Author model has zero or more blogs that can be connected to it. Usually `has_many` is the other end of `belongs to`. 

## has_many :through
```ruby
# app/models/author.rb
class Author < ApplicationRecord
    has_many :categories
    has_many :blogs, through :categories
end

# app/models/category.rb
class Category < ApplicationRecord
    belongs_to :author 
    belongs_to :blog
end

# app/models/blog.rb
class Blog < ApplicationRecord
    belongs_to :author
    has_many :categories
end
```
A `has_through` association builds a one to many connection with another model through a third model. In the example, when someone looks up a certain category they can find all the authors amd blogs in that category. 

In our app the category of the blog would be set in the UI when creating the blog not the author. So in order for an author to be associated with a category it would have to use and go through the blog model to get the category. 

The Category Model would ve connected the the author and the blog using the belongs_to association we discussed earlier.

```ruby
# app/models/Author.rb
class Author < ApplicationRecord
    has_many :categories
    has_many :blogs, through :categories
end
```

Those are the three out six associations I've worked with so far in my Rails apps. This is feature really shows how easy Rails makes it for you to accomplish things on the backend.

Thank you so much for reading!