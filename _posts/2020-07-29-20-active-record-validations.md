---
layout: post
title: Active Record Validations Basics
date:  2020-07-29
---

You can add validations to our Rails model files to make sure that only correct data gets saved to the database. For example every user that signs up would require an email address before the user could be saved to your database. One cool thing about validations is that they don't care about what database you're using. You can use SQLite, Postgres, MySQL, etc. 

Let's take a look at a User model to see a validation in action.

```ruby
# app/models/User.rb
class User < ApplicationRecord
  validates :email, presence: true
end
```

In our User model we use the validates helper provided by Rails to make sure that an email address is present on the User model. Now go to the Rails console to create a new User object and check its validity. 

```bash
User.create(email: "bruce@wayneent.com").valid? # => true
User.create(email: nil).valid? # => false
```

As you can see the first User we tried to create was valid and the second one was not. Active Record will not save any object to the database that fails validation. To make sure validations are checked you have to use the correct methods. Some methods do not run validation checks.

The methods that run validation checks before saving to the database are:

```ruby
User.create
User.create!

User.save
User.save!

User.update
User.update!
```

## Validation Helpers

Rails active Records comes with validation helpers to use inside of your models. After you validate the data you can use helpers to add special tweaks to the model. For example, the length of the password or if the data is unique so two users with the same email won't get saved.

```ruby
# app/models/User.rb
class User < ApplicationRecord
  validates :email, presence: true, uniqueness: true
  validates :password, presence: true, length: { minimum: 3}
end
```

You can separate the helpers with commas(,) in order to add more than one to each piece of data in your model. There lost of helpers here are some of the most common

* Presence - Makes sure the fields aren't empty
* Confirmation - Check when two fields needs to have matching data like a password confirmation
* Length - Set a minimum and maximum character length
* Uniqueness - Makes sure no user have the same data like email address or usernames

There are more these are the ones I've used you can check the Rails docs for the rest

## Validation Errors

If you run into some errors when saving your data Rails provides error methods you can use to check and see what's wrong. Let's look at this in action.

```ruby
# app/models/User.rb
class User < ApplicationRecord
  validates :email, presence: true
  validates :username, presence: true

end
```

This User model requires an email and a username to be considered valid and get saved to the database. Let's create a new User in the Rails console that's missing a username and see what the errors are.

```bash
user = User.new(email: "bruce@wayneent.com")
user.valid? # => false
user.errors.messages
# => {username => ["can't be blank"]"}
```

This tells you exactly what is wrong with your data. You can Rails Docs for other errors handling methods.

You can also have these errors messages show up inside  of your Views. Inside of your user eRB file you can check if there are any validation errors(with @user.errors.any?) and have the validation errors shown to your users(@user.errors.full_messages). This allows you to share the same error messages within your Rails app.

```html
<% if @user.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize(@user.errors.count, "error") %> prohibited this user from being saved:</h2>
    <ul>
    <% @user.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

## Conclusion 


Active Record Validations allow you to make sure only valid data gets saved to your database without having to run SQL commands and works with various databases. It gives you error messages and several helpers for you to customize the way you want your data to be saved. Thank you for taking your time to check out my blog post!
