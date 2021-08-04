---
layout: post
title: Add Real Time Chat To Rails with ActionCable
date:  2020-07-08
---

ActionCable is feature of Ruby on Rails that brings in the usage of websockets. With it you can build cool things like real-time chat similar to discord and slack. You can also add "Who's online" notifications to show other users who is online at the same time as them.

This post is using Rails 5.2 and assumes you have already created a Rails app with a User and Message models. This is how to add ActionCable into an existing application.


## Create and connect to a channel

To get started create a channel using the rails generate command

```bash
rails generate channel chatroom
```

This will create chatroom_channel.rb and chatroom.coffee files. Inside of the chatroom channel file there two methods already created subscribed and unsubcribed

You want to uncomment and change the subcribed method to stream from "chatroom channel" instead of "some_channel".

```ruby
class ChatroomChannel < ApplicationCable::Channel

  def subscribed
    stream_from "chatroom_channel"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end
end
```

This creates a connection with the channel you just created.

## Add a route to connect the channel

Next you'll want add a route so the the data from the channel is being communicated. You do this by mounting the ActionCable server to the cable route in your config/routes.rb file:

```ruby
Rails.application.routes.draw do
root 'chatroom#index'
.
.
.

mount ActionCable.server, at: '/cable'
end
```

## Update your messages controller 

In your controller that handles the messages we'll call it messages_controller.rb. Add the ActionCable broadcast method and the name of the channel you want to broadcast to the create action.

```ruby
class MessagesController < ApplicationController

    def create
        message = current_user.messages.build(message_params)
        if message.save
            ActionCable.server.broadcast "chatroom_channel",  mod_message: message.body
    end
end

private

def message_params
    params.require(:message).permit(:body)
end

end
```

**NOTE: current_user is a helper method created in the application controller that find the user by its session id.**

The message object that was assigned recieves the content of the message from the broswer. Then message gets saved to the database.

ActionCable.server.broadcast takes in a ruby hash, that hash is received by the chatroom.coffee file as data. 

## Display messages in browser

In order to have the the content of mod_message display in the browser we can append it to the DOM. In your views where you want the chat to be displayed, add a div with id of "message-container" around your chat display. In this example I'll use app/views/chatroom/index.html.erb


```html
<div class="ui feed" id="message-container">
    <div class="event">
        <div class="content">
            <div class="summary">
            <em><%= message.user.username %></em>: <%= message.body %>
            </div>
        </div>
    </div>
</div>
```

This also grabs the messages username and the body of the message using embedded Ruby. It grabs this from the message object we assigned and saved to the database in the messages controller. 

Now we can add the message to the id we just created to display on the client side. Head to the chatroom.coffee file and add the following under received: (data) ->:

```javascript
  received: (data) ->
        $('#message-container').append data.mod_message
```

The id form the view gets grabbed using $('#message-container') then we add .append to attach the content of the message to the DOM.

The mod_message hash you added in the messages_controller is taken as a JavaScript object called data by the chatroom.coffee file. So you can access it using dot notation with the name of the key you gave the hash from messages controller in this example we used mod_message as they key and get it's value using data.mod_message.


## Submit messages remotely

By default Rails will send a HTTP POST request for the messages. That's not what we want for real-time chat we want to use AJAX and send the request remotely. To change this, the input form where we typing in the message needs to be updated.

In the views/chatroom.index.html.erb add remote: true to the form:

```erb
<%= form_for(@message, url: message_path, remote: true) do |f| %>
<%= f.text_field :body %>
```

## Associate Users with Messages

Now we have to know that Rails can connect the logged user to their message. We do this by using ActiveRecord associations.

In the message.rb file add:
```ruby
class Message < ApplicationRecord
    belongs_to :user
end
```


In the User.rb file add: 
```ruby
class User < ApplicationRecord
    has_many :messages
end
```

## Conclusion

In this post you learned how to:

* Generate a channel
* Connect/subcribe to the channel
* Mount the ActionCable route
* Broadcast the channel 
* Display messages in the browser using AJAX instead of HTTP POST
* Use Active Record to connect the User and Message models





