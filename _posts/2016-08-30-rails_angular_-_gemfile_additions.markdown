---
layout: post
title:  "Rails/Angular - Gemfile additions"
date:   2016-08-30 20:44:26 +0000
---


For my final project, I've been buidling a Rails/Angular app that makes and stores recipes. We added a number of gems so I thought it would be worthwhile to take a moment out and in written format think through what the various gems added are about. These are the gems I added to my gemfile for this [project](http://https://github.com/PeteAJ/test_angular_project/tree/master/app): ![](http://imgur.com/a/Y5VbC)

gem 'jquery-rails'
A gem to automate using jQuery and Rails. It quite simply allows us to access jquery from rails. 

gem 'active_model_serializers' 
A gem necessary for creating active model serializers. AMS allow us to create serializers in a Rails-like way. We need to create a serializer file as opposited to adding to our controller to use active model serializers.  

gem 'jbuilder' 
This gem is actually included in Rails 4 and up.  It allows us to display our data in json format. I found this [article](https://richonrails.com/articles/getting-started-with-jbuilder) to be a useful explanation.  

gem 'rack-cors'
This gem allows web apps to make cross-domain AJAX calls without using JSONP. It makes rack apps CORS compatible. Provides good domain specific language (DSL) for use in the application.rb. There really isn't a ton of elaboration on how these gems really work. Perhaps part of it is in their use.  

gem 'angular-rails-templates'
Removes the need to make AJAX calls to retrieve templates so we can move our Rails views to Angular. Templates in Angular combine model and view information to render the dynamic view the user sees.  

gem 'bootstrap-sass' 
Bootstrap, as we learned in [HTML/CSS part](https://learn.co/tracks/full-stack-web-development/html-and-css/bootstrap-framework/introduction-to-bootstrap) of this course is an open source "HTML, CSS and Javascript framework for developing responsive, mobile first projects on the web." It has pre-written css code for working on different screen sizes and baked in jquery plugins. The sass version is helpful for inclusion in ruby/rails projects.    


