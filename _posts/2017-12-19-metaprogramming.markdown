---
layout: post
title:      "Metaprogramming"
date:       2017-12-19 19:17:02 +0000
permalink:  metaprogramming
---


= a technique in which programs use programs as their data (instead of datatypes, like integers or strings). I first learned about metaprogramming while studying macros in Flatiron School’s Full-Stack Web Development Course. In Ruby, one of the many benefits of macros is that they allow you to abstract away getter and setter methods that define attributes of an instance. Instead of explicitly writing these methods:

	class Person
		def name=(name)
			@name = name
		end

		def name
			@name
		end
	end

, we can simply write:

	class Person
		attr_accessor :name
	end

, and achieve the same result. Consequently, this type of metaprogramming (macros) helps developers consolidate code so that it is more concise, efficient, and readable.

The “Mass Assignment Lab” provided another example in which I learned the value of metaprogramming. In mass assignment, objects of a class are initialized with a method that accepts keyword arguments (a hash) as its parameter. Since hashes are not sequentially ordered like arrays are, we do not have to be concerned about the order of the hash when it is passed to the method. Additionally, using metaprogramming, we can adapt the initialize method to accomodate changes in the hash that would otherwise cause the method to malfunction. In this lab, I was given a number of possible attributes of an instance of a Person class: 

:name, :birthday, :hair_color, :eye_color, :height, :weight, :handed, :complexion, :t_shirt_size, :wrist_size, :glove_size, :pant_length, :pant_width

Additionally, I was given attributes of two different instances of the Person class, each stored in a variable as a hash:

	bob_attributes = {name: “Bob”, hair_color: “Brown”}
	susan_attributes = {name: “Susan”, height: “5’11\”, eye_color: “Green”}

As you can see, the keys were different for Bob and Susan. To adapt the code to these differences, I used metaprogamming in the initialize method of the Person class, shown below:

	def initialize(attributes)
		attributes.each {|key, value| self.send((“#{key}=“), value)}
	end

Instead of explicitly setting the values of each attribute like I had done in the past, I iterated over each key/value pair in the attributes hash and set each available attribute (key) equal to its associated value using the .send method. This not only abstracted away the explicit setter method (like the macro shown previously) but also made the code more accomodating to the changes within the class.
