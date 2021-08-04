---
layout: post
title: Create a model in Ruby on Rails 
date: 2020-01-30
---

To store data in Ruby on Rails you use a model (The M in MVC). Models communicate with the database of the site.

To make a model with the attributes of name and email in the command line type 


```bash
rails generate model User name:string email:string
```
Notice that the User model is singular. There are two optional parameters, name:string and email:string. This tells Rails what attributes you want and the type you want them to to be.

The generate command you typed makes a mirgration file inside of the db/migrate folder so we can change the data gradually. In this example it made a user table in the database with the columns name and email.

### db/migrate/[timestamp]_create_users.rb

```ruby
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```

The file that created above inclues a change method that decides the changes to the database. The change method uses a Rails method called create_table to create a table in the database used for storing users. The create_table method accepts a block and the block variable |t| which represents table.

Inside of the create_table method t actually creates the name and email columns inside the database with the type of string. The last line in the block is t.timestamps, it creates two columns in the database named created_at and updated-at. The values for these get updated when a user is created and changed.

The `rails generate model User name:string email:string` command also creates the model file as well in `app/models/user.rb` You can see what was created

```ruby
class User < ApplicationRecord
end
```
There isn't much here yet but you don't have any users yet. 

It does show you `class User < ApplicationRecord` which means in Ruby that the User model has all the operations as `ApplicationRecord`. That topic is outside of the scope of this blog post.

I'll end it here at knowing what happens when a model in created in Rails. Thank you for reading. 