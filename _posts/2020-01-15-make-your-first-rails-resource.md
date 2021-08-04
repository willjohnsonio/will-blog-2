---
layout: post
title: Make Your First Rails Resource
date:  2020-01-15
---

Ruby on Rails is very powerful framework even more powerful than I thought. When I created my first resource using a feature called scaffolding it was so easy I wasn't actually sure what I did. So I'm going to demystify what happens when you create a resource. 

A resource is something that users can interact with. 

To create a User resource using scaffolding in the terminal type:

```bash
rails generate scaffold User name:string email:string
```

Let's break that down.

"rails" run the Rails command

"generate" is a Rails subcommand that will create a bunch of files for you

"scaffold" is the powerful command that creates major parts of the application on it own. 

"User" is the model (a Ruby object that represents an element of the site) we want to create

"name" & "email" are the models attributes that we want User to have 

"string" is the data type we want the attributes to have

After we run that command, Rails will create a lot of files and pages for you. Next run `rails db:migrate` command to update the database with the new user that was created for us.

Then run `rails server` in the terminal and in the browser type in "/user" to view the page you just created.

This is what blew me away that command created the ability to add new users, see all the users we created, edit any user, and delete a user we longer need! aka a resource 🤯

This is what people mean when they say CRUD operations(Create Read Update Delete).

All of that from just typing `rails generate scaffold User name:string email:string` that is amazing! This command will create each page and the functionality needed to make all those changes.

Each page gets a URL 

"/users" shows all the users 

"/users/1" shows user with the id of 1 (which Rails creates automatically)

"/users/new" this is the page to make a new user

"/users/1/edit" this is the pages to edit the user with the id of 1

That's how you create a Ruby on Rails resource using scaffolding. Also, usually scaffolding is not used by developers in production apps. They usually make custom resources on their own. Scaffolding is good for learning and get something running fast. 

