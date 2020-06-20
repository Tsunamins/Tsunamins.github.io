---
layout: post
title:      "Rails Project: Some reference points"
date:       2019-09-06 19:00:49 -0400
permalink:  rails_project_things_to_remember_and_forget
---


Rails added some excitement and dynamic features to the developer learning process.  It was also a long 4 weeks worth of work.  So for this blog about this post I’m going to make a sheet of references for frequently forgotten or frequently used code snippets, terminal commands and any other relevant items of interest; I will explain their importance where applicable.

#### Generating a new skeleton of a Rails app, still frequently forgotten for now:

> `rails new <filename-here>`
	

* Why it’s important: creates nearly all the file structure necessary within the rails app, includes most needed gems and dependencies, and allows easy inclusion for reference to images and css sheets, among others.

	

#### Git local directory vs. rails command to build a new application:
It was tricky to get my local environment file structure on the same page after git cloning my repository down but also initiating a new rails app, tried restarting the process several times to consistently end up with a project folder within a project folder.

**My solution, instead of re-starting a new directory and new rails app:**
1. Moved the files in the file explorer up one directory
2. git Commit command
> `git commit - m “message here”`
3. *Error message: untracked files*
4. Git track files/add files command
> `git add <filename>/`
(In this case I added all the files listed as untracked, individually)

5. All the files were in the same directory now, in both local and remote environments. One last push:
> `git push -u origin master`
* Push all the changes to the master branch that have been tracked/staged/commited/etc


#### Frequently used, and finally not too frequently forgotten
> `rake db:migrate`

* run migrations on all the migrate files recently created

> `rake routes`

* find all avaliable routes as are currently defined in config/routes.rb



#### Links and redirects within rails
> `redirect_to <some_named_path>(optional variable depending)`

* If the path has an :id, then a variable will need to be placed as a parameter

> `<%= link_to “What you want the link to say”, <some_named_path>(variable depending) %>`

Variation
If the link needs to say what’s stored in a variable:
> `<%= link_to post.title, <some_named_path>(variable depending) %>`

#### Various Migrations
**Basic table creations: with a variety of types**
Within -class CreateTableName < ActiveRecord::Migration[5.2]

```
def change
   create_table :table_name do |t|
   t.string :name
   t.string :password_digest
   t.date :today 
	 t.boolean :yes-or-no 
	 t.integer :quantity 
	 t.belongs_to :user, index: true, foreign_key: true
   t.references :favorited, polymorphic: true, index: true
 end
end
```





**Table additions, subractions and changes, within def change:**

Drop table:
> `drop_table :table_name`

Add column:
> `add_column :table_name, :column_name, :column_type`

Delete column:
> `remove_column :table_name, :column_name`

Change column type:
> `change_column :table_name, :column_name, :new_type`


	
#### Explanation of Associations, as it applies to my project, * The Amateur Wine Reviewer *
4 models were needed to ensure I was covering all the requirements, or so I thought, as I went along checking them off.  The models are `wine`, `review`, `user` and `liked_review`. `liked_review` was added last in case review to user to wine was not sufficient enough for 2 of the requirements for the project
*I adapted this example from the following resource on stackoverflow: 
[https://stackoverflow.com/questions/13240109/implement-add-to-favorites-in-rails-3-4 ](http://)*


```
in: class User
   has_many :reviews    
   has_many :wines, through: :reviews  
   has_many :liked_reviews
   has_many :likes, through: :liked_reviews, source: :review
```


```
in: class LikedReview
   belongs_to :user
   belongs_to :review
```


```
in: class Review
   belongs_to :user
   belongs_to :wine 
   has_many :liked_reviews
   has_many :liked_by, through: :liked_reviews, source: :user
```


```
in: class Wine
   has_many :reviews  
   has_many :users, through: :reviews
```

I knew I had enough nearly enough assoication requirements met between `User`, `Wine` and `Review`, however, I wasn’t sure about user submittable attributes so I added in the dynamic source based associations with `LikedReview`, this allowed to change the tense of the has_many through the liked_reviews to allow a differentiation in  chaining agains users vs. reviews themselves, all via the opposite source.

**This requried a couple other modifications**
Instead of a `LikedReviews controller`, `#like` was added to the `ReviewsController` as its own method:
```
def like
       @review = Review.find(params[:id])
       type = params[:type]
       if type == "like"
          
          current_user.likes << @review
          redirect_to like_review_path(current_user)
 
       elsif type == "unlike"
           current_user.likes.delete(@review)          
       end
   end
```


**Additionally a new nested path was built specifying additional routes: ** 

```
 resources :reviews do
    put :like, on: :member
    get :like, on: :member
  end
 ```

* This added 2 routes necessary:

```
like_review PUT    /reviews/:id/like(.:format)   reviews#like 

            GET    /reviews/:id/like(.:format)   reviews#like
```

* PUT was used in /reviews/show.html.erb:
	To add the “like” or “unlike” functions that were sent back to reviews#like
	And essntially become the user submittable attribute upon a review created
* GET was used in /reviews/like.html.erb
	To visit the current users liked reviews with the url /reviews/:id/like
	Can of course do much more with it, however I was ready to turn the project in 

#### Places OmniAuth must appear:
*And also noteworthy this is my quick summary, but I didn’t remember or discover all this on my own, I was referred to the following resource, describing most of these steps in greater detail at: [https://medium.com/@rachel.hawa/google-authentication-strategy-for-rails-5-application-cd37947d2b1b](http://)*
	
	
**In this case google based login:**
1. Setting this up with google at, approximately [https://console.developers.google.com](http://)

2. Generally speaking create a new app and work with the omniauth tabs, there’s more to it than that, but that’s the gist

3. Gemfile - needs three things added:
> `Gem ‘omniauth’`
> 
`gem 'dotenv-rails'`
>
`Gem 'omniauth-google-oauth2'`

4. Create a `.env` file in the `root` of the app to add the google client_id and client_secret, found while setting new app within google developers console


```
So within the new .env file:

GOOGLE_CLIENT_ID=<generated number from google here>`

GOOGLE_CLIENT_SECRET=<generated number from google here>`
```


5)  add .env to .gitignore file, just type in
> `.env `

* add to the list of items ignored by github

6) Create a omniauth.rb in /config/initializers, add all of this:

```
	Rails.application.config.middleware.use OmniAuth::Builder do
       provider :google_oauth2, ENV["GOOGLE_CLIENT_ID"],ENV["GOOGLE_CLIENT_SECRET"], skip_jwt: true
  end
```

7) In routes.rb add:

> `	get '/auth/:provider/callback' => 'sessions#omniauth'`

8) In user.rb
```
def self.from_omniauth(auth)
       where(email: auth.info.email).first_or_initialize do |user|
         user.username = auth.info.name
         user.email = auth.info.email
         user.password = SecureRandom.hex
       end
     end
```

9) In SessionsController
```
def omniauth
       @user = User.from_omniauth(auth)
       @user.save
       session[:user_id] = @user.id
       redirect_to user_path(@user)
     end
```
 
 as well as: 
```
private
 
     def auth
       request.env['omniauth.auth']
     end
```

9) Basic link to use this to make a login/signup with google, placed wherever necessary:
> `<%= link_to "Log In with Google", '/auth/google_oauth2' %>`

10) Optional: I found the branding icons at 

[ `https://developers.google.com/identity/branding-guidelines`](http://)


* With a set of downloads of differnt types of google images and instructions for use

#### Speaking of logging, in don’t forget about in user.rb
> `has_secure_password`












