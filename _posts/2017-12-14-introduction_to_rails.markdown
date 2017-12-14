---
layout: post
title:      "Introduction to Rails"
date:       2017-12-14 22:08:38 +0000
permalink:  introduction_to_rails
---

In 2004, David Heinemeier noticed that many of his Ruby web applications contained code that was structurally similar to his older applications. In order to avoid repeatedly copy pasting code from previous work, David created Rails so that he and all Ruby developers could focus more on the unique aspects of future applications. After over a decade of open source contributions, Rails has become a useful web framework for building Ruby web applications quickly and effectively. Generally, a web framework provides developers with tools for tasks such as routing, asset management, and database connections. Like many web frameworks, Ruby on Rails follows the MVC (Model-View-Controller) architecture, which places logic in the model files, code flow management in the controllers, and content display in the views. Additionally, Ruby on Rails is a Ruby gem containing a set of Ruby code libraries. To install the gem, run ‘gem install rails’ in the terminal. Then, to create an application, run ‘rails new first-app’. This will generate a long list of directories and files described below:

# Directories
**app**: contains MVC and front-end assets like css and js files, images, videos, and fonts. This is the only directory that can accept changes without the developer needing to restart the Rails server.

**bin**: built-in Rails tasks that usually aren’t relevant to development

**config**: environment, language, application, and database settings, modules that are initialized when the application starts, application routes, and the secret key base

**db**: database schema listing tables, columns, and each column’s data type, as well as a seeds.rb file that allows creation of data to be used in the application

**lib**: custom rake tasks like database creation and migration

**log**: logs for debugging

**public**: custom error pages and the robots.txt files which lets developers control how search engines index the application

**spec**: specs, factories, test helpers, and test configuration

**tmp**: temporary items

**vendor**: integrates client-side MVC frameworks like AngularJS

# Files
**Gemfile**: gems (libraries) that extend functionality of the app. Any change to this file requires the developer to run ‘bundle install’ in their terminal

**Gemfile.lock**: displays the dependencies and versions of each gem

**README.rdoc**: application details, including instructions to other developers like how to get the app up and running locally

Before starting the rails server, create the database by running ‘rake db:create’. Then, run ‘rails s’ to startup the server. To start up the Rails console run ‘rails c’. The Rails console allows the developer to perform several tasks such as running database queries and application code, performing CRUD tasks with the database, and allowing the developer to switch between making permanent database changes and running in sandbox mode to test scripts. To close the session, run ‘control + d’ to return to the regular terminal session.

# RAILS MVC
**Model** — a Ruby class that inherits from ActiveRecord::Base, allowing it to access useful methods for working with the database. The model contains the application’s logic, including database queries, data relationships, callbacks, validations, and custom algorithms. Like most Ruby classes, the model also contains methods and attributes.

**Controller** — connects the models, views, and routes. The view connects to the controller and only has access to available instance variables that contain data sent from the database. The routes file also connects to the controller and ensures the controller methods match the items in the routes file. The controller contains the full set of CRUD features with standard methods: index, new, create, show, edit, update, and destroy.

**View** — renders what is sent from the database by the controller and is supplied with ActionView helper methods that allow us to dynamically set HTML tags without writing HTML. This functionality results in more Ruby, less HTML, and cleaner views.

# ROUTING
Routes are added to the draw block of the config/routes.rb file and tell the application what view to render to users

**Static routes** render views that don’t change, like about or contact pages
**Dynamic routes** accept parameters and render different content based on those parameters

**STEPS**
1. HTTP REQUEST: URL is entered in the browser
2. The request is processed through the routes.rb file
3. The route file maps the request through whichever controller method is called
4. Controller returns the HTTP RESPONSE containing the page viewed in the browser


**Route Example:** get ‘about’, to: ‘static#about’
**Route Components:**
*	HTTP verb*: get
*	Path*: ‘about’ represents the path in the URL bar that the route is mapped to
*	Controller Action (Method*): ‘static#about’ points to the static controller’s about action

**Two options for mapping views:**
**Explicit Rendering**: in the about method in the static controller, render “some_page”, which renders static/some_page in the views directory
**Implicit Rendering**: Rails follows standard convention that looks for the view file with the same name as the controller action, rendering static/about.html.erb. 
