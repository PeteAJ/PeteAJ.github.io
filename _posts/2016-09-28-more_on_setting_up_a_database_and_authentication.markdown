---
layout: post
title:  "More on Setting Up a Database & Authentication "
date:   2016-09-28 21:04:10 +0000
---


Experimenting with Active Record today so I thought it might be good to write out some issues or ideas coming up as I continue to work in Sinatra. I'm looking at [this page](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html) and made a discovery or two that it can't hurt making note of and recording. It is good practice to work in the command line and try things out. And good as well to slow down and test things to make sure the concepts are sinking in.  

This is all review but I'm trying to look at it again fresh so it's all clear! I created a new app using rails new APP_NAME and the basic instructions [here](http://guides.rubyonrails.org/getting_started.html) which give you a clear idea of first working with the app file structure and pushing things to the browser. In Sinatra we've been discussing making server requests for the client so really I should say I'm rendering pages for the client from server requests. In Rails we can go on to generate controllers with by name and include actions or methods as well like so: 

> $ bin/rails generate controller Welcome index

Rails generates a number of files for us this way including a route in config>routes get 'welcome#index' and helper and test files and a welcome directory in views and inside it a index.html.erb file so we could use the welcome controller if we wanted to do so. We could write a method in the index method of the welcome controller that writes a message defined by an instance variable @message = "Hello" and rendered by the index.html.erb view to the client by <%= @message  =>.

With our rails app, instead of irb we use rails console or rails c to test code in the terminal. In order to create a user with a name and password, I played around with a couple of options and learned something that I might have already but became clear on trail and error. I am thinking here of generate migration vs generate model. I am sure with more experience I would see more instances where generating just a migration is useful the model migrate quite literally does more from the start. 

For instance. A couple of things I tried. 
A. Creates a migration that has an empty change method  
> rails generate migration User name:string 

B. Creates a migration that has a create_table block with our string in our change method 
> rails generate migration CreateUser name:string 

C. Creates a migration (create_user) that has a create_table block with our string in our change method -and- a user.rb model and a test_user.rb test model 
> rails generate model User name:string 

Ideally it seems like we would want to know ahead of time at least one column in advance when creating a migration so we would be most happy with B. Option A. needs a CreateUser title and in fact drops our request to create a name:string. C. feels the most useful at the moment. Even if I hadn't been planning on using the model, I have it pre-made. Rails generate controller would create a controller and a view as discussed above for us so I could see using generate controller and generate model in consort. A broader way to begin building an app would be to create a scaffold which does the job of both of these generate operations but with the possible disadvantage of a more hand crafted approach if we only need a small number of views but it could be a great way to begin a project as well as it can create a large number of MVC files and "a helper, some JSON builders, a stylesheet, and a CoffeeScript file" as discussed [here](https://learn.co/tracks/full-stack-web-development/rails/crud-with-rails/rails-blog-scaffold).    

# has_secure_password 
To do simple password security in Sinatra we utilitize the open source gem b-crypt.  To the gemfile, we add: 
> gem 'bcrypt'

We can now access has_secure_password which allows us to add methods to set and authenticate against a b-crypt password per [here](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html/). These validations are added automatically: the password should be present, less than or equal to 72 characters and we can confirm it using a password_confirmation attribute.  Bcrypt stores a salted, hased version of our user's password in our database in a column called password_digest. We are required to have a password_digest in our database. It is our bcrypted hashed passwords. The macro has_secure_password is called just like a ruby method and allows us in conjunction with bcrypt to not store plain text paswords in our database. The attribute of password is given to us by has_secure_password.  

If we've set up a database with username and password_digest properly our signup post action could look like 

> post '/signup' do 
> user = User.new(:username => params[:username], :password => params[:password])
> if user.save 
> redirect '/login'
> else
> redirect '/failure'
> end

We create a new variable user with username and password. It's not an instance variable because we're using it just once here. Pretty sure. If we have successfully saved the user, redirect to a login view, otherwise redirect to a failure view. A login post request would look like: 

> post '/login' do 
> user = User.find_by(:username => params[:username])
> if user && user.authenticate(params[:password])
> session[:user_id] = user.id 
> redirect '/success'
> else 
> redirect '/failure'
> end 

In order to log in, we're looking for the username from params[:username]. The second two lines are new. Authenticate is a method proved by bcrypt and it takes a string. We check if we have a user and if the string provided matches the password_digest (in other words whether the user is authenticated. If they are, we set the session id and redirect on in the app or just back to a failure view if not.   









