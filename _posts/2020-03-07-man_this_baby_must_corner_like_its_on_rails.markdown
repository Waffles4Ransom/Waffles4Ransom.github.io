---
layout: post
title:      "'Man, this baby must corner like it's on rails!'"
date:       2020-03-07 01:30:10 -0500
permalink:  man_this_baby_must_corner_like_its_on_rails
---

![](https://i.imgur.com/oCqCsii.gif)

Moving on from Sinatra to Rails.

The Rails application should:
* use the Ruby on Rails framework 
* be a CMS
* manage related data through complex forms and RESTful routes

So good news, I'm leaving my tiki drink appreciation behind this time and focusing on another love...MOVIES.

I work at the Denver Art Museum and my team has an affinity for movie going so we created an unofficial, official DAM Movie Club. 

We coordinate movies to go see together. We have member titles and profiles. We have a private group email. 

**We have member cards.** 

![](https://i.imgur.com/BxPXj43m.jpg) ![](https://i.imgur.com/4oJHdb9m.jpg)

All very official.  

So I thought...wouldn't it be even more unofficially, official if we had our very own website?? Yes. Yes, it would. 

Obviously this project isn't hosted yet, but it's my goal to one day do so for me and my friends. 

## DAM Movie Club 

A place for all members of Movie Club to rate &  review the movies we see together. 

### Basic Features

* once a member has signed up they are prompted to create their profile 
* afterwards they can start reviewing the movies they attended
* members can leave comments on each other's reviews to promote discourse and community

### Getting Started

The steps to make this dream a reality:
1. plan out the models and their relationships
2. start new rails app 
3. generate resources for models 
4. migrate database
5. test out the relationships in Rails console 
6. wire up the routes
7. set up user creation and sessions (aka signup, login, and logout) 
8. move on to the content creation and the forms ( CRUD )
9.  make some index and show pages to display the content
10. add CSS to make it all bearable to look at

The intial planning of your models before you even start coding is so cruical. It did take me quite a while to figure out exactly how I wanted my models to be associated with each other within the bounds of has_many and has_many through. 

Testing out those relationships in the console before anything else was so crucial and helpful. 

While checking on them I had a new error crop up..."ActiveRecord::AmbiguousSourceReflectionForThroughAssociation " 

#### ActiveRecord says:

![](https://i.imgur.com/nNbRcMB.gif)

It needs a source!

`has_many :users, through: :reviews` needed to be `has_many :users, through: :reviews, source: :user`  Problem solved, now I can ask my Movie about it's users. 

Even after all my planning, once I started on the user creation I quickly realized that all the "fun" user details, like member title and profile photo, were not something I wanted to be required during the sign up process.  I had to change my User migration and had a new Profile migration. A Profile belongs_to User and a User has_one Profile. This way I can keep my User sign up nice and concise (especially with an Omniauth login).

 I encountered another issue while creating an authorization method to insure a trouble-making member couldn't go around editing other people's profile information. 
 
 I started with defining the method in the application controller. 

```
def authorized
    unless current_user.id == params[:user_id]
      flash[:nope] = "Sneaky little member. Wicked, tricksy, false."
      redirect_to root_url
    end 
 end 
```

Seem good at first glance, but alas it was returning false when it shouldn't so I dropped a pry in the edit action of the Profiles Controller to take a look. Suprise surprise, `params[:user_id]` returns "7", not 7. 

![](https://i.imgur.com/O5k3yLb.png?1)

```
 def authorized
    unless current_user.id == params[:user_id].to_i
      flash[:nope] = "Sneaky little member. Wicked, tricksy, false."
      redirect_to root_url
    end 
  end 
```

Throw a little `.to_i` on the end and you've got yourself a working method. 

Now to be able to protect member reviews and comments,  I need to make some adjustments. Because of the way I rigged up my nested routes, the user id is not present (ex: '/movies/2/reviews/4/edit') so I needed to be able to pass the object in and retireve the user ID that way. 

```
def authorize(object)
    unless current_user.id == object.user.id 
      flash[:nope] = "Sneaky little member. Wicked, tricksy, false."
      redirect_to root_url
    end 
  end
```

We're cooking now.  

![](https://i.imgur.com/a5bekcP.gif)
 
 The last issue I can recall with clarity came from this method here: 
 
```
def self.upcoming_movie 
   Movie.where("date_attended > ?", DateTime.now).first
end
```

Let's talk about lazy evaluation. Most ActiveRecord queries will return a Relation, instead of the actual object. It will only query the database until it absolutely has to, which in the long run is more efficient and flexible. By flexible, I mean we can chain more complex queries all together to really pinpoint a result and ActiveRecord will choose the best time to query the database. You need to use a single_value_method to be able to retrieve a single object from the query.

So ` Movie.where("date_attended > ?", DateTime.now) ` will only return the ActiveRecord::Relation. It will look and smell like the object in an array but it is not, it is an instance of the Active Record Relation! If I want access to the actual object the query found I need to chain on #first or #last to retrieve the data I want.

On the flipside, ActiveRecord does have Finder Methods, such as #find and #find_by, that will fire the SQL query immediately and return a single instance. 

To summarize ActiveRecord Query Methods and Finder Methods have different behaviors! Know your returns!

See you at the movies! 

![](https://media.giphy.com/media/g1nOYYV0AHY3K/giphy.gif)
 




