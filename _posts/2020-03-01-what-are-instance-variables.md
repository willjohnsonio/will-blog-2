---
layout: post
title: What Are Instance Variables in Ruby on Rails?
date: 2020-03-01
---

In Ruby instance variables start with the @ sign

```ruby
@batman
```

If you create a variable inside of a class, it's not available to every method in that class.

```ruby
class Name

def my_name
    batman = "Bruce Wayne"
end
```

In order to access the contents of batman outside of my_name
you put the @ sign in front of it.

Ruby on Rails takes this a step futher and allows instance variables that you create inside of your controllers (controllers are classes), to be available inside of your views.

So, for example if you have this code in your users controller:

```ruby
 def create
    @user = User.new(:name, :email)
 end
```

You assign the new user with an instance variable called @user with a name and an email.

Now when you go your views and put this code:

```html
<%= @user.name %>
<%= @user.email %>
```

The name and email will displayed on the page.

Each new user you create is going to have a different name and email, by using instance varibles you can access them in the views with one command.

You can display the content of mutiple instance varibles using embedded Ruby in your views.

```html
<% @user.each do |user| &>
<%= user.name %>
<% end %>
```

This will loop through each user and display their name in the erb view.

That's all I have for today's blog post. Thanks you for reading and following along as I learn Ruby on Rails in public.
