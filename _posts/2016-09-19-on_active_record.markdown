---
layout: post
title:  "On Active Record "
date:   2016-09-19 16:23:11 -0400
---

I thought it might be useful to write out some ideas about what Active Record is and what it does in light of my Sinatra assessment coming up. At it's some simple, it's a ruby library for working with databases that allows us to both read and write information. We can add, remove and change items in the database directly. Active Record uses Object Relational Mapping (ORM) to map elements of a table (columns) to the relevant object attributes. When we fire up a new database migration, Active Record adds the specified attributes automatically saving time/effort. Active Record objects have both persistent data and behavior that operates on that data. We have a number of methods to work with in Active Record including a CRUD system (create, read, update, destroy) and validation and callbacks. Without Active Record we have to do more work! We can't return a collection of all users using .all (read) without creating an @@all array and defining a class method to access its contents first.   

I'm looking at this [page](http://api.rubyonrails.org/classes/ActiveRecord/Base.html) here but I will help me to think through some of the Active Record basics. So this is mostly a copy of the page referenced that I felt was most relevant to my current project.   

Creating records with either a hash or a block. A user has a name and an occupation. 

by hash 

```
user = User.new(name: "Peter", occupation: "Designer")
user.name #=> "Peter"
```

by block 

```
user = User.new do |u|
u.name = "Peter"
u.occupation = "Designer"
end 
```

and we can also create a bare object and add information later

```
user = User.new 
user.name = "Peter"
user.name #=> "Peter"
```


Querying methods. 
We can ask within a method whether or not a object value is present using true/false.  

```
user = User.new(name: "Peter")
user.name? #=> true
```


Dynamic Attribute-based finders. We can use find_by to find an object by attribute or find_by to find by multiple attributes.  

```
User.find_by(name: "Peter", occupation: "Designer")
User.find_by_name_and_occupation("Peter, "Designer")
```

Pretty sure that's how that works.   
Active Record can serialize any object in text columns. This is something that came up in the Angular JS work I was working on. We call the class method serialize so we can store additional arrays, hashes and other non-mappable objects without extra work.  

```
class User < ActiveRecord::Base
serialize :preferences 
end 

user = User.create(preferences: {"color" => "blue", "second_color" => "green"}
User.find(user.id).preferences #=> {"color" => "blue", "second_color" => "green"}
```


Basic CRUD actions in Active Record are:

Create: User.create
  create and save a new instance. .new instantiates an object without saving. .save commits to the database 
	
Read: User.all ot User.find_by(id_number)
  accessing data. methods include all - return a collection of all users, first - return first user, .find_by - return by name,etc.   .where - users = User.where(name: 'David', occupation: 'Code Artist').order(created_at: :desc) - return all David/Code   Artists and sort by create_at reverse chronological order
	
Update: User.update
   User.find_by, .name, .save becomes User.find_by, user.update so we write:
	
```
	user = User.find_by(name: 'David')
  user.update(name: 'Dave')
```

  We can also use .update_all to update multiple records.

Delete: User.destroy  



create: 
get '/user/new' router renders the view page with form 
new.erb is the view page with the form for creating a new user
the form sends a POST request to another controller action, post '/users'
here we extract form data from the params, create a new instance of the usrer class

read: 
get '/users' handles requests for all instances of a class. Loads instances and sets them tequal to an instance variable @users = User.all then it renders the index.erb view 
index.erb uses erb to render all the instances stored in the @users instance variable
get '/users/:id' controller action handles request for a given instance 
show.erb view page uses erb to render the @user object

update:
get 'users/:id/edit' renders the edit.erb page 
edit.erb view contains the form for editing a given instance of a user
form sends a request to patch '/users/:id'. this action finds the instance ofthe model to update and updates and saves it 

delete:
a button on the show page of a given instance. it's a delete request to delete '/users/:id/delete' and has only

Our App or Application Controller class might look like: 

```
 get '/posts/new' do #loads new form
    erb :new
  end

  post '/posts' do  #creates a post
    @post = Post.create(params)
    redirect to '/posts'
  end 

  get '/posts' do #loads index page
    @posts = Post.all
    erb :index
  end

  get '/posts/:id' do  #loads show page
    @post = Post.find_by_id(params[:id])
    erb :show
  end

  get '/posts/:id/edit' do #loads edit form
    @post = Post.find_by_id(params[:id])
    erb :edit
  end

  post '/posts/:id' do  #updates a post
    @post = Post.find_by_id(params[:id])
    @post.name = params[:name]
    @post.content = params[:content]
    @post.save
    erb :show
  end


  post '/posts/:id/delete' do #delete action
    @post = Post.find_by_id(params[:id])
    @post.delete
    erb :deleted
  end
```

This is copied from [this](https://learn.co/tracks/full-stack-web-development/sinatra/activerecord/sinatra-activerecord-crud) lesson. Again, it just helps me to see it again and type things out in this way. Just to back up a second because I didn't move on to MVC file structure in my project. Yet? Get vs Post. 

Get - requests data from a specific resource 
Post - submits data to be processed to a specific resource 

In my current project:

```
  get '/' do
    erb :index
  end
```
	
display the index.erb page on access. If we click on the link here to sign up as a new user we send a get request to /signup. My signup.erb is in views>registrations>signup.  

```
 get '/signup' do
 erb :'/registrations/signup'
  end
```
	
Then I decided to redirect to the new book entry page because I think I do need a full CRUD views system to add the new member name and welcome them. But redirecting to the new book page works like this. I'm posting the information from the signup.erb to this new page. Not quite, but working on it.  

  ```
post '/registrations' do
    
    redirect :'/new'
  end
```

We can also validate the state of a model before in gets put into the db.  In the relevant class: 

`validates :name, presence: true `
	
And do callbacks which run whenever an Active Record object is created, saved, updated, deleted, validated, or loaded from the db. Most simply an ordinary method and a macro-style class method to register them as callbacks:

```
before_validation :ensure_login_has_a_value
 
  protected
    def ensure_login_has_a_value
      if login.nil?
        self.login = email unless email.blank?
      end
    end
```
		
I plan to keep adding to this page as I work through this. Again, noting that some of its content is definitely derived from information and exampled on learn.co and elsewhere online.  

	

	
	







