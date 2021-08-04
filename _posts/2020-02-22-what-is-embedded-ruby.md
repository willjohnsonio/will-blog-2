---
layout: post
title: What Is Embedded Ruby?
date: 2020-02-22
---

Embedded Ruby aka ERb. is a templating system that Ruby on Rails uses to show dynamic content on your website.

ERb files end in a .html.erb or .erb file type. You can write HTML and plain Ruby inside of ERb files.

In order to use Ruby you have to place your Ruby code inside this tag.

```erb
<%= %>
```
Inside this tag you can place varible names, methods, etc. The equal sign means it will display on the web page what inside of the tag.

So this will take data from your controller and display it on your page. For example, if you have diffrent users they will see the same page but it will be addressed with their name.

You can also use ERb without the equal sign.

```erb
<%  %>
```

This way will run the code inside of the tags but not show it on the webpage. This could be useful for if statements or loops.

```erb
<% @avengers.each do |book| %>
  <%= avengers.name %>
  <%= avengers.power %>
<% end %>
```

This will show each Avengers name and power on the page.

Thank you for reading. 👍🏾


