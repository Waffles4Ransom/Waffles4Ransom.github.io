---
layout: post
title:      "ActiveRecord, a wizard's BFF..."
date:       2020-07-11 18:30:14 -0400
permalink:  activerecord_a_wizards_bff
---


![](https://media.giphy.com/media/Qs75BqLW44RrP0x6qL/giphy.gif)

It might look and feel like magic. Amazing results achieved with minimal effort tends to feel like magic. But it's not, and if you are asked 'How does ActiveRecord work?"...magic is not the answer. 

### What is ActiveRecord??

ActiveRecord is a Ruby gem, serving up a library of code that acts as the interface between your database and your application. It is based on Martin Fowler's active record architectural pattern, which functions as an ORM (Object Relational Mapping) framework. Using the ORM technique, our classes are related to database tables and an object instance of the class is related to single row in the table. 

So, AR deals with the data of your application and in terms of the MVC design pattern - it's focused on the M, the model. It works by way of class inheritance. When using AR, your models will all inherit from `ActiveRecord::Base` or in a Rails app from `ApplicationRecord`. All Rails applications use ActiveRecord, it's one of the seven gems that Rails is built with. It provides basic CRUD capabilites, data validation options, relationship/associations, easy database schema migration creation and much more. When data is so cruical to most applications, having a structure to simplify accessing and changing the data is essential. 

### The Meat & Potatoes...

![](https://media.giphy.com/media/105OwsN7a4UQ2Q/giphy.gif)

So let's talk about the basics of ActiveRecord. The most commonly used features are: 

1. schema migration generation
2. associations
3. data validations
4. the query interface


### ActiveRecord Migrations 

With AR, we have the ability to spin up new schema modifications, rather than write them manually. Migrations are written in Ruby so we aren't actually writing any SQL, and are used to add, modify or delete database tables.

An example of a migration:
```
class CreateRings < ActiveRecord::Migration[6.0]
  def change
    create_table :rings do |t|
      t.string :name
      t.string :owner
      t.string :make
      t.string :power
			
      t.timestamps
    end
  end
end
```

Rails has a migration generator so we can create the above table with a single command via the terminal:
```
$ rails generate migration CreateRings name:string owner:string make:string power:string
```

A few conventions to be remember of when using a migration generator:
1. it should start with an action (ex: "Create...", "Add...", "Remove...")
2. name is followed by a list of column names and its data type (there are [12 supported data types](https://guides.rubyonrails.org/v3.2/migrations.html))
3. name should be CamelCase or snake_case and plural

Using the rails generator for models will also create a migration file, as well as the model file:
```
$ rails g model Ring name:string owner:string make:string power:string
```

In a model generator, the model name should be CamelCased and singular. 

Another accepted genreator column type is references, for example `user:references` .  This will give us a user_id column and appropriate index and is useful for creating data relationships which leads us to the next topic...

### ActiveRecord Associations
In AR, associations are connections between two models to create relationships within your data. They are created by the use of macros which are called from the model file; there are six association types.

Using the association macros looks like:
```
class Fellowship < ApplicationRecord
  has_many :members
end 

class Member < ApplicationRecord
  belongs_to :fellowship
end
```

It is also necessary to make sure that the database table for a belongs_to relationship has a column for the "foreign key" of it's association, so following the above example, the Members table would need a column for "fellowship_id". The foreign key is what identifies an association's record in a different table. 

A belongs_to association gives us access to 4 methods, while a has_many association give us 13 methods. For example...

a belongs_to association:
```
frodo = Member.fist
frodo.fellowship
=> #<Fellowship id: 1, name: "Fellowship of the Ring", formed: "25 October, T.A. 3018", location: "Rivendell", purpose: "to take the One Ring to Mordor so that it would be destroyed and ultimately kill the Dark Lord Sauron">
```
This returns the associated object. If no record is found, nil is returned.

a has_many association:
```
fotr = Fellowship.find(1)
fotr.members 
=> #<ActiveRecord::Associations::CollectionProxy [#<Member id: 1, name: "Frodo Baggins", race: "Hobbits", hometown: "Bag End", fellowship_id: 1 >, #<Member id: 2, name: "Aragorn", race: "Men", hometown: "???", fellowship_id: 1 >,  #<Member id: 3, name: "Legolas", race: "Elves", hometown: "Mirkwood", fellowship_id: 1 >]> 
```
This returns an array of all associated objects. If no records are found, an empty array is returned. 

![](https://media.giphy.com/media/twchU3xTn3Vg4/giphy.gif)

### ActiveRecord Validations 

To ensure we aren't persisting any unwanted data into our database, we can use built-in AR validation helpers. They are method calls made in a model class definition that check to make sure the data of a new object is valid before it is saved. That way we can stop objects from exisiting that might be missing required data or are in an unexpected format. If a validation fails, the object will not be saved and error messages are created.

For example, we can validate for uniqueness or presence by:
```
class Realm < ApplicationRecord 
  validates :name, uniqueness: true
  validates :capital, presence: true
end 

realm = Realm.new
realm.valid? 
=> false
realm.errors.size
=> 2

lorien = Realm.new(name: "Lothlórien", captial: nil)
lorien.save
=> false 
lorien.errors.full_messages
=> ["Capital must exist"]
```

When an object is created via a web form, we can use these error messages to display back to a user why their attempt has failed, which creates a good user experience. 

### ActiveRecord Query Interface 

ActiveRecord provides us with methods and conditions that perform queries on the database. We don't have to write any SQL to retrieve the data we want. 

Some class methods available to us are:

`Hobbit.find(4)` returns the record that matches the primary key given as the argument

`Hobbit.find_by(name: "Samwise")` returns the first record matching the specific condition given as the argument

`Hobbit.all` returns a collection of all hobbits

`Hobbit.last` returns the last hobbit record 

We can chain these methods and conditions:

`Hobbit.where(hometown: "Bag End").order(created_at: :desc)` returns all hobbits from Bag End in descending order of creation

We can ask for calculations: 

`Hobbit.sum(:pints)` returns the sum of pints consumed by all hobbits

`Hobbit.where(loves_beer: true).count` returns the number of hobbits that love beer 

![](https://media.giphy.com/media/1BLN4cSZGvhOo/giphy.gif)

The [docs](https://guides.rubyonrails.org/active_record_querying.html) are a great place to study up on all your querying possibilites.

ActiveRecord is one of the most powerful parts of Rails. Being able to wield such power, like anything takes planning and practice. A huge part of a successful project or API is being able to properly structure your associations. It's also important to know how to avoid unnecessary queries that slow down your application. For example, being mindful of when to use `.count`, which performs a query every time, verus `.size`, which only performs a query if needed. 

Now, go forth, study up and use ActiveRecord like a real wizard... 

![](https://media.giphy.com/media/fUm0aXBSxOU6I/giphy.gif)

