---
layout: post
title: How Does Strong Params Work In Ruby on Rails
date:  2020-08-27
---

If you're following a Rails tutorial you've probably seen some code like this in a controller:

```ruby
# app/controllers/users_controllers.rb
private
  def user_params
        params.require(:user).permit(:username, :email, :password)
    end
```
How exactly does this work?

What's the purpose of using this?

This is a feature of Rails called **strong parameters**. Let's break it down, `params` is the information that comes from the web, through user input or a form. The `private` keyword tells Ruby all the methods after it can only be called within the object. 

The `params.require` looks for the `user` params hash that we get from the browser. Then the `permit` method lets you list out what attributes received that you want to allow to be saved to the database. The rest will be ignored.

This is a security feature of Rails that protects against people injecting their own params in the form submission and gaining access to areas they shouldn't

After setting up strong params you can now assign an instance variable inside the create method of your users controller with only the attributes you allowed in the `user_params` method and save them to the database.

```ruby
# app/controllers/users_controllers.rb
    def create
        @user = User.new(user_params)
        @user.save
    end
```

As of Rails 4 you are forced to use a strong params if you try to save an object to the database without you will get a `ActiveModel::ForbiddenAttributesError`

That's how strong parameters work and how they can protect your site from some malicious activity.

Thank you for reading.