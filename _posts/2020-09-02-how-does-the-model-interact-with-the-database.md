---
layout: post
title: How Does The Model Interact With The Database In Ruby on Rails
date:  2020-09-02
---

You may understand that you can use a generator to create a Model. You also understand that the generator also creates a migration file with data attributes defined. 

You might ask **"Why aren't these attributes also defined in the Model?"** or **"How do I interact with these items in the Model?"**

Let's dive in and see what's happening!


## What is Active Record?

**Active Record is the M in the Rails MVC pattern(Model, View, Controller)**. It's the "magic" Rails uses to connect the Model objects we create to the database.

When you run `rails g model User email:string password:text` Rails does some the following:

* Creates a new model object that you can find at `app/models/user.rb`

* Creates a migration file with attributes defined at `db/migrate/{timestamp}_createusers.rb`

Active Record matches database column names to the attributes(email, password) that are defined in the migration file. That means you don't have to declare attributes inside Rails models.

Migrations are Ruby classes that make it simple to create and edit database tables. 

Here's an example migration file:

```ruby
# db/migrate/{timestamp}_createmodelname.rb
class CreateUser < ActiveRecord::Migration[6.0]
  def change
    create_user :users do |t|
      t.string :email
      t.text :password
 
      t.timestamps
    end
  end
end
```

## How To Access The Attributes?

You can access the attributes by assign the Model objects to variables then using dot notation(.). In your terminal open the Rails console by typing `rails c`. 

To access the email of User model we created we need to: 

Assign the User object to a variable

```bash
> user = User.first
```

Use dot(.) notation to access the attribute you want.

```bash
> user.email
=> "bruce@wayne.com"
```

You can use this same pattern to access attributes inside of your controllers, models, and views!

```ruby
# app/controllers/user.rb
class UsersController < ApplicationController

def create
  @user = User.first
end
```

So while you don't have to define your attributes inside of your Model. Rails makes it easy to for you access them and use them in your application.




