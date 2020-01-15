---
layout: post
title:      "Understanding The Uber Model"
date:       2020-01-15 05:40:32 +0000
permalink:  understanding_the_uber_model
---


Travis Kalanick and Garrett Camp were the orginators of the popular ridsharing app Uber, which grosses about $5.8 billion and is avaiable worldwide. In a nutshell the company experianced rapid growth creating more than 20,000 jobs per month to the growing public. It's almost safe to say this company is doing something right . They targeted the ridesharing niche and created a robust but yet simple application that joins passengers with outsourced drivers. They even added gamification, reviews and other social constructs within this wonderful application to foster engagment. Diving further into the application we see there are multiple moving parts.  We have users, passengers,drivers,trips, comments, ratings, earnings, promotions ect...  This convolution is simplified in its presentation for a better user experiance.

Richard Feynman said, "What I cannot create, I do not understand".. So lets try to understand, and create an Uber application model. We'll call it our FUber model. In our emulation we must start simple. We must identfy the core concepts and explian there details on a high-level. Before continuing lets setup or enviroment. We'll need Sinatra to build a simple web applicalication, Sqlite3 for our database and, Active Record as our ORM (Object Relational Mapper).


## User Model
The user model will represent each person that interacts with our application whether they are a passenger or a driver.
With that said a user has many passengers and many drivers.. Here is an example of our users table schema..

```ruby
  create_table "users", force: :cascade do |t|
    t.string   "name"
    t.string   "email",           null: false
    t.string   "password_digest"
    t.datetime "created_at",      null: false
    t.datetime "updated_at",      null: false
  end
```


In our user model we have to setup a few more things... First we must secure our login state for each user.  We will use a gem called bcrypt to take care of that.. Bcrypt has a special hashing algorithim that salts and hashes each password. Since a user uses an email to login, we would perfer our database to only accept unqiue emails. To accomplish this we'll use a validator to monitor our emails... Now  we will focus on the relationship between a user and the different roles they can have.  When a user signs up,  for our FUber application, they can either choose driver or a passenger role. In that case our model should indicate a user can either be a passenger or a driver. This is where the has_one association comes to play.

```ruby
class User < ActiveRecord::Base
    has_secure_password
    validates :email, uniqueness: true
    has_one :passenger
    has_one :driver
```

In active record we can use has_one macro class method to create an association between these models with their foreign keys.  If the passenger and driver schemas contain the foreign key, then we will use the belongs_to  macro class method for their models.


## Passenger, Driver, and Trip Models
The next simple concept is A passenger is able to request a ride. and a driver is able to accept it. The question we should ask is what is a passenger,driver or a ride.  We can see that a passenger and a driver both derived from the user class model, However they have their own distinct roles. In the real world a passenger is able to create read update and delete there ride or trip. A driver can accept a trip thus changes the status of the trip. Driver is unable to edit anything else about a trip. And a Trip or Ride is defined by a origin and destination address for a passenger. Here is 
a schema representation of these real world models. 


```ruby
  create_table "drivers", force: :cascade do |t|
    t.integer  "user_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "passengers", force: :cascade do |t|
    t.integer  "user_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "trips", force: :cascade do |t|
    t.integer  "passenger_id"
    t.integer  "driver_id"
    t.string   "from"
    t.string   "to"
    t.string   "status",       default: "pending"
    t.datetime "created_at",                       null: false
    t.datetime "updated_at",                       null: false
  end
```

Notice these models all share foreign keys, they have a special relationship with these other models. Both passenger and driver share the foreign key user, meaning user model can have many passengers or drivers, and the reverse is a passenger and driver belongs to a user.  Trips schema is different since it posesses mutliple foreing keys making it our join table or the single source of truth. A trip belongs to both a user and a driver and consiqently a passenger and driver has many trips. The advantage of having trips as our join table is we can have both driver and passenger communicate through the trip.  So in our models we must demonstrate that asscoiation using the has_many through macro. 


```ruby
class Passenger < ActiveRecord::Base
    belongs_to :user
    has_many :trips
    has_many :drivers, :through => :trips
end

class Driver < ActiveRecord::Base
    belongs_to :user
    has_many :trips
    has_many :passengers, :through => :trips
end

class Trip < ActiveRecord::Base
    belongs_to :driver
    belongs_to :passenger
end
```


## Reviews
Lastly, in keeping things simple we have reviews. Both passenger and drivers are able to place a review at the end of a completed trip.. Knowing this we can build it out in a few different ways. We can add a review column for both driver and passenger models or create a reveiws table holding their foriegn keys . While these are options it's advantagous for us to create a polymorphic assciation,*Poly* meaning many *morphic*  meaning forms. This allows for further abstraction between our models making our lives simplier and adhearing to the DRY principle. The Review table will include a comment, stars, reviewable, and reviewable_id. In active record if your table is polymorphic the unspoken convention is to have *able*  at the end of your table name. 

**Example:** our table is called Review meaning one of our columns should be named reviewable and reviewable_id. These act as placeholders for the model that interacts with it. If a passenger creates a review the reviewbale will reveal  the instance of the passenger. The same can be said if a driver created a review; reveiwable will refelect the instance of the driver.

In order to integrate this last bit into our models we would need to update our models association. We'll first define our reviews model that is polymorphic. 

``` ruby
class Review < ActiveRecord::Base
    belongs_to :reviewable, polymorphic: true
    belongs_to :trip
end
```


Then will update or other models to reflect the idea that both drivers and passengers have many reviews and  reveiws belong to a trip. 


```ruby
class Passenger < ActiveRecord::Base
    belongs_to :user
    has_many :trips
    has_many :drivers, :through => :trips
    has_many :reviews, as: :reviewable
end

class Driver < ActiveRecord::Base
    belongs_to :user
    has_many :trips
    has_many :passengers, :through => :trips
    has_many :reviews, as: :reviewable
end

class Trip < ActiveRecord::Base
    belongs_to :driver
    belongs_to :passenger
    has_many :reviews 
end
```



## Conclusion 

Through our Fuber application we were able to recreate a simplified version of the infamous uber rideshare app. We gannered certian key concepts that are ubiquitous throughout many web applications. However there are still multiple ways to create a simple version of the Uber model.  
