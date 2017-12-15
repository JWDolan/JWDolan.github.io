---
layout: post
title:      "Sinatra: Key Concepts"
date:       2017-12-15 18:44:46 +0000
permalink:  sinatra_key_concepts
---

= a Rack-based Domain-Specific Language of pre-written methods (i.e. get and post) used for writing Ruby web applications (respond to HTTP requests)

**Shotgun** is a Ruby gem for developing and testing Rack-based Ruby web applications
	With shotgun, all code is reloaded upon every request
	Install with gem install shotgun

**Routes** connect HTTP requests to responsive methods (controller actions)
1. The HTTP request verb GET matches the get controller method while the /medicines path matches the /medicines path in the controller method
2. The block  (between do / end keywords) that accompanies the route is run: queries the Medicine table for the collection of Medicine instances and stores them in the variable, @medicines
3. The @medicines collection is rendered via the index.html.erb file as a response to the user
4. If no match is found, the response is a 404 error informing the user’s browser that the application cannot find a match for the requested URL

**MVC File Structure**
**Gemfile** holds a list of gems needed to run the application. The bundler gem provides access to the command bundle install. The Bundler looks in the Gemfile and installs gems and gem dependencies

**App Directory**
**Models** are logical Ruby classes that connect to the database to persist data
--Represent the data and object logic
--Should have attributes that can be set upon initialization
--Keeps track of each instance created
--Contains a class method all to return an array of those instances
**Views** (.erb files) are displayed in the browser
--Filenames should match the action that renders them
i.e. a GET request to / renders a file called index.erb
       get ‘/‘ do
	        erb :index
       end
**Controllers** are written in Ruby and consist of routes 
--Take requests sent from the browser
--Run code based on those requests by using models
--Render views as a response for the user to see.
--configure block tells the controller where the views and public directory are.

**config.ru**
—Details to Rack the environment requirements of the application and starts the application server 
—Requires a valid Sinatra Controller (class that inherits from Sinatra::Base). This inheritance transforms the app into a web application by giving it a Rack-compatible HTTP interface
Example:
	require ‘sinatra’ 		                       	#loads the Sinatra library
	require_relative ‘./app.rb’        	#requires the application file
	run ApplicationController     	#creates an instance of the ApplicationController class that can respond to requests from a client

**config directory**
Holds the environment.rb file that connects all the files to gems and to each other and loads the app directory
**public directory**
Holds our front-end assets (i.e. css and JS directories, images, etc.)
**spec directory**
--Unit Tests test models and how they interaact with the database
--Controller Tests test the code responsible for delivering data to a user (i.e. ensuring an HTTP request returns the expected HTTP response)
--Integration Tests test how the user interacts with HTML and forms	

**ERB**
Is a templating engine that uses a combination of HTML and Ruby to reduce duplication of HTML and generate content that can change based on available data
Substitution tags <%= %> evaluate Ruby code and display the results in the view
Scripting tags <% %> evaluate Ruby code but don’t display the results

**Capybara**
Is a library of code (gem) that allows us to write code in our integration tests that simulate how a user interacts with the application
**visit** navigates the test’s browser to a specific URL (equivalent to a user typing a URL into the browser’s location bar
	Argument = string for the URL that is loaded within the test
**page** gives a Capybara::Session object that represents the browser page the user looks at when they navigate to the URL passed to visit
	Responds to methods that represent actions a user takes, such as click_link, 		fill_in, or body
	page.body gives all the HTML in the current page as a string

*Example*: spec/application_integration_spec.rb
Tells Capybara to visit the / URL and sets the expectation that the body of the page includes the text “Welcome” 
	require ‘spec_helper’
	describe “GET ‘/‘ - Greeting Form” do
		it ‘welcomes the user’ do
		  visit ‘/‘
		  expect(page.body).to include(“Welcome”)
		end
	end
have_selector, have_field, & have_text are matchers for ensuring the page contains matching HTML or text

**Dynamic Routes**
Created based on attributes within the URL of the request
The content on the page changes depending on the URL in the browser
Example
	get ‘/medicines/:id’ do
	     @medicine = all_the_medicines.select do |medicine|
		medicine.id == params[:id]
	      end.first
	      erb :’/medicines/show.html’
	end

**Layouts and Yield**
layout.erb contains all the code we want to exist on every single web page
Within the body of layout.erb, <%= yield %> inserts the unique page content

**Forms**
<form method=“POST” action=‘/food’>
	<p>Your Name: <input type=“text” name=“name”></p>
	<p>Your Favorite Food: <input type=“text” name=”favorite_food”</p>
	<input type=“submit”>
</form>

--method = request type, action = specific route the request is sent to
--name defines how the application will identify each <input> data

The data from the form is packaged in the params hash
	params = {:name => “Sam”, :favorite_food => “Onions”}

post ‘/food’ do
	“My name is #{params[:name]}, and I love #{params[:favorite_food]}”
end 

**Nested Forms**
Used to create more than one object (student and their courses)
<form action=“/student” method=“post”>
	Student Name: <input type=“text” name=“student[name]”>
	Student Grade: <input type=“text” name=“student[grade]”>
	Course Name: <input type=“text” name=“student[course][name]”>
	Course Topic: <input type=“text” name=“student[course][topic]”>
	<input type=“submit”>
</form>

To allow multiple courses,
<form action=“/student” method=“post”>
	Student Name: <input type=“text” name=“student[name]”>
	Student Grade: <input type=“text” name=“student[grade]”>
	Course Name: <input type=“text” name=“student[courses][][name]”>
	Course Topic: <input type=“text” name=“student[courses][][topic]”>
	Course Name: <input type=“text” name=“student[courses][][name]”>
	Course Topic: <input type=“text” name=“student[courses][][topic]”>
	<input type=“submit”>
</form>

	post ‘/student’ do
		@student = Student.new(params[:student])
		params[:student][:courses].each do |details|
			Course.new(details)
		end
		@courses = Course.all
		erb :student
	end

student.erb
	<h1>Student</h1>
	<div class=“student”>
		<h3>Name: <%= @student.name %></h3><br>
		<h4>Grade: <%= @student.grade %></h4>
	</div><br>
	<h1>Courses</h1>
	<% @courses.each do |course| %></h3><br>
	 <div class=“course”>
		<p>Name: <%= course.name %></p><br>
		<p>Topic: <%= course.topic %></p><br>
	</div><br>
         <% end %>

**Sessions and Cookies**
A *session* is an object, like a hash, that stores data describing a client’s interactions with a website at a given point in time
Lives on the server and can be accessed by any controller

A *cookie* is a hash that’s stored in the browser and sent back to the server with every request
Usually contains a URL to the generating website, the date on which it was 	created, and other info like remembered login info, user preferences, etc.

*Session cookies* allow movement from page to page for a specific session without having to reauthenticate the user.
Stored in the web browser
Expire every time you log out or navigate away from a website

*Persistent cookies* allow website to remember user info/preferences for future visits
Stored in the computer
Allow for faster page loading, automatic login, and user authentication, and access to other potential web application features
Created on the first visit to a page after account creation and persist unless the web app has a protocol in place for cookie expiration or the user clears their browser’s cache

**CRUD**
Create
get ‘/model/new’ —> new.erb —> post ‘/models’
Read
get ‘/models’ —> index.erb
get ‘/models/:id’ —> show.erb
Update
get ‘/model/:id/edit’ —> edit.erb —> post ‘/model/:id’
Delete
get ‘/models/:id’ —> show.erb —> post ‘/models/:id/delete’

**Join Tables**
Online Store: A user can have many items, and an item can belong to many users (more than object can own many objects of the same class)
	class UserItem < ActiveRecord::Base
		belongs_to :user
		belongs_to :item
	end

**Tux**
A Ruby gem that allows access to the database and CRUD operations through the terminal
Loads a full environment in the console that allows you to see all routes and views


