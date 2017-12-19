---
layout: post
title:      "Object Architecture"
date:       2017-12-19 19:59:16 +0000
permalink:  object_architecture
---


**Inheritance**
* When a child class (subclass) inherits from its parent (super class), it adopts all of its attributes and behaviors
* Use < to inherit the child class from the parent class (implies that a class is a type of something)
```
Class Vehicle
	attr_accessor :wheel_size, :wheel_number
	def initialize(wheel_size, wheel_number)
		@wheel_size = wheel_size
		@wheel_number = wheel_number
	end

	def go
		“vrrrrrrooom!”
	end
end

class Car < Vehicle
	def go
		“VRRRRRROOOOOOOOOOM!!!”
	end
end
```
When #go is called on a Car instance, it looks in the child class first, then moves on to the Vehicle class. 

**Super**
* If class Student inherits from User, we can allow the Student class to inherit a certain method from User or overwrite that method with another implementation specific to Student
* If we want Student to share some of the functionality or augment it in some way, we use the super keyword:

* Example: Users are either students or teachers
```
class User
	def log_in
		@logged_in = true
	end
end
```
* When a student logs into the app, we need to set their logged_in attribute to true AND their in_class attribute to true, but we don’t need to indicate that the teacher is in class
```
class Student < User
	def log_in
		super	
		@in_class = true
	end
end
```
* The super keyword calls on the log_in method defined in the super class, then runs the additional code. 

**Modules**
* Allow us to collect and bundle a group of methods and make them available to any number of classes

* **include** a module if you want its methods to be used as instance methods
* **extend** a module if you want its methods to be used as class methods

*Nested Modules*
BMW::Car gives the BMW class access to all constants, instance methods, etc. *without* stating that a BMW is a type of car

**EXAMPLE**

```
module FancyDance
	module InstanceMethods
		def twirl
			“I’m twirling!”
		end

		def jump
			“Look how high I’m jumping!”
		end
	end

	module ClassMethods
		def metadata
			“This class produces objects that love to dance.”
		end
	end
end

class Dancer
	extend FancyDance::ClassMethods
	include FancyDance::InstanceMethods
end

class Kid
	extend FancyDance::ClassMethods
	include FancyDance::InstanceMethods
end
```
Now, we can call FancyDance::InstanceMethods (twirl and jump) on Dancer and Kid instances and FancyDance::ClassMethods (metadata) on the Dancer and Kid classes.
