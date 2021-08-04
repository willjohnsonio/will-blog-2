---
layout: post
title: What are helpers in Ruby on Rails
date: 2020-02-20
---

In Ruby on Rails helpers are methods that let you have reuseable code that can be used in different the views of your app.

Rails has some built-in helpers like stylesheet_link_tag to link to the CSS file you want to use. You can place the helper in your view by placing it's name in between the embedded ruby tags in your .erb file.

```ruby
<%= stylesheet_link_tag 'amazingcss' %>
```

You can also create your own helpers. In your project files at app/helpers/application_helper.rb. That's the default file or you can create a new file in the same folder for example name_helper.rb. Then you can declare the method and what you want it to do in the file.


**For example:**

```ruby
module NameHelper
    def say_name(user)
        if user.name == "Batman"
            "I'm #{user.name}!"
            else
            "I'm not #{user.name}."
            end
        end
    end
```

The cool thing about helpers is that they are "just" available in all your views because of Rails magic. You don't have to do anything extra to access them.

In your .erb file in your views folder all you have to type is

```ruby
<%= say_name(@user) %>
```

That's it!