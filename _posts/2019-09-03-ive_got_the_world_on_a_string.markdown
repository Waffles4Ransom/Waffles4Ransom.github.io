---
layout: post
title:      "'I've got the world on a String...'"
date:       2019-09-03 21:48:56 +0000
permalink:  ive_got_the_world_on_a_string
---

## Sinatra Project Time!!

![](https://media.giphy.com/media/B8NSo2tYq4SRi/giphy.gif)

This Sinatra web app should:
* be a simple content management system (CMS)
* utilize CRUD and MVC structure
* use ActiveRecord with Sinatra 

Now not to be a one-trick pony, but when I read that this application should track something important to you, I just knew I was going to make a TIKI DRINK TRACKER. 

You can take the tiki drink away from the girl, but can't get the girl away from tiki drinks. C'est la vie. 

I HAVE other interests. I promise. 

Waffles and cats.

But that's it. 

ANYWAYS

### The TIKIPHILES

![](https://i.imgur.com/QPWwtNem.png)

The Tikiphiles is a place for anyone to join so they can record and rate each tiki drink they imbibe. 

Knowing what to order (or never order again!) is half the battle. 

#### Basic Features
* once a user has signed up they can view - all drinks uploaded by every user on the community homepage or  a personal homepage to view all their own drinks
* the user can create, edit, and delete a drink 
* a drink has a name, type, bar & location, a rating out of 0-10, and an optional image 

#### Getting Started
So the steps I followed to bring this website into the world:
1. build out the basic file structure 
2. set up the git repo 
3. set up the mechanics to get the site wired up correctly a lá a Gemfile, config.ru, env.rb, .gitignore, etc.
4. build out the ApplicationController, which inherits from Sinatra::Base 
5. test by running `shotgun` and check in the browser at `localhost:9393`
6. make a Rakefile and console task for testing
7. add the models and set up their associations
8. create a database, migrate it, and seed it
9. build other controllers and make sure they are mounted in the config.ru
10. go to town coding the routes and views 

Now I would just like to take a small detour here to address #2 "set up the git repo"...

If you ever find yourself displeased with your beginnings of a project and
you are feeling a dangrous combination of dramatic and frustrated 
so you delete it ALL including the repo on your Github
and start over in a haste and run a  `git init` in the root directory of your computer...

VSCode will be very upset with you. It will get overwhelmed trying to stage EVERY FILE ON YOUR COMPUTER.

![](https://i.imgur.com/9NS8y1gm.png)

And you will be very upset trying to figure out what you've done and send an SOS to your cohort lead. 

IN SUMMARY - pay close attention to where you run a  `git init`  command.

Moving on...

The dreaded "undefined method for nil:NilClass" error. 

While cruising along in the last step,  #10 "go to town coding the routes and views", everything was going smoothly until the Update portion of my CRUD was not working. 

![](https://i.imgur.com/hNOIIvmt.png)

This line of code `@drink = Drink.find_by(id: params[:id])`  , my @drink variable was coming up nil. It wasn't finding the right drink in the database, but why?

So I threw in a binding.pry to find out what params[:id] I was receiving.  

![](https://i.imgur.com/x5VKNscl.png?1)

Turns out the id was coming through as "edit". Oh boy. So now I had to go track down the form in my Edit View to see why it was passing off "edit" as the id and low and behold I had set the form's action to 
```
<form action="/drinks/<% @drink.id %>/edit" method="POST">
```
when it should have been 
```
<form action="/drinks/<%= @drink.id %>" method="POST">
```

![](https://media.giphy.com/media/XAdbHJywVjF5K/giphy.gif)



Overall, this project has been fun and I can't wait to add more functionality to it in the future! Cheers!!

![](https://i.imgur.com/APdQKT6l.png?1)





























