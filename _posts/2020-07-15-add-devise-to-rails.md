---
layout: post
title: Add Devise To Rails for User Authentication
date:  2020-07-15
---

Devise is a Ruby Gem that is used for User Authentication in Rails Apps. It handles sign in, sign out, password security and more. You can create your own authentication system in Rails, but Devise is a trusted and powerful solution in the Rails community.

## Install and Set Up Devise

In your Gemfile add the Devise gem save it and run bundle install in the terminal

```Ruby
gem 'devise'
```

After the devise is finished installing run the generator

```bash
rails generate devise:install
```

This will create two files config/initializers/devise.rb and config/locales/devise.en.yml. The terminal will then tell you of some additional manual set up.

* Define a default url in the config/environments/developement.rb
* Set a root route in routes.rb (this can be any valid route)
* Add flash message to application.html.erb
* Generate Devise views

The last one important because the views that allow you to sign up and sign out using devise aren't available by default to use. I'll cover that later.


## Create a User model Using Devise

Run a generate command create a new User model with the devise authentication defaults.

```bash
rails generate devise User
```

This command creates a migration file, a User model file, test files, and adds devsie_for :user to routes.rb. Which includes all of the routes to use for Devise actions. To see all the new Devise routes in the terminal run `rails routes | grep users`

Let's take a look at the User model this command created. Inisde of app/models/user.rb you'll see a model with a devise method with and a lot of defaults.

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable
end
```

This is the list of modules devise provides out the box you can uncomment the others to use their features as well. By default you get 5 modules in your model they are:

* Database Authenticatable - Hashes and stores passwords in the database to validate the authenticity of a user thats signing in
* Registerable - Signs up users and lets them edit and delete their account
* Recoverable - Sends reset password instructions.
* Rememberable - Lets user stay signed in bu remembering a user from a saved cookie.
* Validatable: Validations if email and password are present.

Now that you checked the model run "rails db:migrate" to add the new User to that database table. 

Devise also provides some helper methods. One that you need is "before_action :authenticate_user!". Replace "user" with your model name if you need to. Add inside of your application contoller file.

```ruby
class ApplicationController < ActionController::Base
    before_action :authenticate_user!
end
```

## Create Devise Views

Devise puts it's views inside the gem itself not into your apps files. That means you can't customize trhe views. To pull them out of Devise and into your app run:

```bash
rails generate devise:views
```

This will create views for confirmations, passwords, regisrations, sessions and more. You can find these new views in app/views/devise.

Now if you visit localhost:3000/users/signup you'll get the app/views/registrations/bew.html.erb view and you can now customize it to fit your needs.

## Recap 

In this post we:

* Installed the Devise Gem
* Created a Devise User model
* Added the :authenticate_user! helper method 
* Pull Devise views out of the gem and into our app to be customized

Thank you for reading!