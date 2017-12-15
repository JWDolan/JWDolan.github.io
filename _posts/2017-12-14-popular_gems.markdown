---
layout: post
title:      "Popular Gems"
date:       2017-12-15 03:40:13 +0000
permalink:  popular_gems
---

Gems are external libraries of code that extend the functionality of an application. They are particularly useful when a developer is trying to solve a problem in their application that an available gem has already solved. In other words, gems aid developers by allowing them to avoid ‘reinventing the wheel’. One of the noteworthy aspects of the Ruby language is its plethora of gems. Before searching online for gems, try running ‘gem search gemname’ in the command line. If that fails, go to Google and search ‘rails gem problem-description’. Although this usually will produce the desired result, it may be better to utilize one of the many online resources for finding Ruby Gems. Ruby Toolbox, Awesome Ruby, and Awesome Rails Gems all facilitate browsing of the most popular and useful gems available. When choosing a gem, popularity is a reliable determinant of the gem’s quality. The more popular a gem is, the more likely it will be secure and maintained with new Rails versions. Additionally, a popular gem will often have questions and comments about it on StackOverflow, which can be a useful resource for troubleshooting the gem. One way to gauge the popularity of a gem is by looking at the number of downloads, commits, or versions of the gem on Ruby Toolbox. You can also look on Github at the gem’s documentation. A well-documented gem will have a README file that explains how to use it and many tests that indicate how the gem works. Three popular gems that Flatiron School emphasizes in their Full Stack Web Development course are Paperclip, Kaminari, and Active Admin.

**Paperclip** is a gem used for uploading photos as attachments to models, configuring storage options, and processing photos after upload. To set up Paperclip, install the ImageMagick dependency (on OS X: ‘brew install imagemagick’, on Linux: ‘sudo apt-get install imagemagick -y’, other: check the ImageMagick website). Once ImageMagick is installed, add ‘gem “paperclip”’ to your Gemfile and run ‘bundle install’. As an example, let’s say we are adding an avatar image to an Author model:
	class Author < ActiveRecord::Base
		has_attached_file :avatar, default_url: ‘style/default.png’, styles: { thumb: “100x100>” }
		validates_attachment_content_type :avatar, content_type: /\Aimage\/.*\z/
	end
Paperclip’s has_attached_file method indicates the attribute name for file access while the validates_attachment_content_type method ensures we get an image file. To add the avatar field to the Authors table, run ‘rails g paperclip author avatar’ (then ‘rake db:migrate’), which generates the following migration:
	class AddAttachmentAvatarToAuthors < ActiveRecord::Migration
		def self.up
			change_table :authors do |t|
			   t.attachment :avatar
			end
		end
		def self.down
			remove_attachment :authors, :avatar
		end
	end
In the form view, shown below, ‘html: {multipart: true}’ generates a form that submits both text and binary data while the file_field helper generates the image input and a “choose” button to attach a file.
	#views/authors/_form.html.erb
		<%= form_for @author, html: {multipart: true} do |f| %>
			<%= f.label :avatar %>
			<%= f.file_field :avatar %><br>
			<%= f.label :name %>
			<%= f.text_field :name %><br>
			<%= f.label :bio %>
			<%= f.text_area :bio %>
			<%= f.submit %>
		<% end %>
In the authors_controller, update the strong params to allow the new avatar field:
	def author_params
		params.require(:author).permit(:bio, :name, :avatar)
	end
To display the avatar, add it to the show and index views, as shown below:
	#views/authors/show.html.erb
		<h1><%= @author.name %></h1>
		<%= image_tag @author.avatar.url %>
		<p><%= @author.bio %></p>
	#views/authors/index.html.erb
		<% @authors.each do |author| %>
			<li>
			      <%= image_tag author.avatar.url(:thumb) %>
			      <%= link_to author.name, author_path(author) %></br>
			      <%= author.bio %>
			</li>
		<% end %>

**Kaminari** is a gem used for paginating (in this example) blog posts. Run ‘rake db:seed’ to create posts, run the server, then browse to /posts. Add ‘gem ‘kaminari’’ to the Gemfile, run ‘bundle install’, then ‘rails g kaminari:config’ to generate a configuration file in config/initializers that indicates how many posts will be viewed per page:
#config/initializers/kaminari_config.rb
	Kaminari.configure do |config|
		config.default_per_page = 10
	end
In the posts controller’s index method, order the posts by most recent and control which page is shown using the page method:
	def index
		@posts = Post.order(created_at: :desc).page(params[:page])
	end
Kaminari also comes with several helpers for adjusting display settings: paginate adds pagination controls, page_entries_info adds contextual information, and window adjusts the number of page numbers shown on either side of the current page
	<h1>Blog Posts!</h1>
	<%= page_entries_info @posts %>
	<%= paginate @posts, window: 2 %>
	<% @posts.each do |post| %>

**ActiveAdmin** is a gem used for adding administrative features to an application. Before running ‘bundle install’, add ‘gem ‘activeadmin’, github: ‘activeadmin’’ to your Gemfile. ‘github: ‘activeadmin’ allows you to use the latest under-development version. Then, generate the ActiveAdmin installation with ‘rails g active_admin:install’, which will create a migration for an AdminUser model, add a default user to seeds.rb, and generate the base ActiveAdmin files. To finish the setup, run ‘rake db:migrate’ and ‘rake db:seed’, then restart the Rails server and browse to http://localhost:3000/admin. Log in to the dashboard with admin@example.com and password = password, then run ‘rails generate active_admin:resource Author’, which creates app/admin/author.rb, shown below. Reload /admin and you will see an item in the top bar for Authors that will allow you to add new authors and edit and delete existing ones.

#app/admin/author.rb
	ActiveAdmin.register Author do
		permit_params :name, :genre
		actions :all, except: [:destroy]
	end

In the above example, permit_params allows only the name and genre to be set while ‘actions :all, except: [:destroy]’ allows all CRUD actions (besides “Delete”) to be available to the administrator. To remove the bio so that the form matches the permit_params, use the form do…end block, which overrides the default form:

	permit_params :name, :genre
	actions :all, except: [:destroy]

	form do |f|
	  inputs ‘Author’ do
		f.input :name
		f.input :genre
	  end
	  f.semantic_errors
	  f.actions	
	end
	
