# README

  <strong>Rolify</strong>
  <br>
rolify Gem Version build status Code Climate Coverage Status
Very simple Roles library without any authorization enforcement supporting scope on resource object.

Let's see an example:

user.has_role? (:moderator, @forum)
=> false # if user is moderator of another Forum
This library can be easily integrated with any authentication gem (devise, Authlogic, Clearance) and authorization gem* (CanCanCan, authority, Pundit)

*: authorization gem that doesn't provide a role class

Requirements
Rails >= 4.2
ActiveRecord >= 4.2 or Mongoid >= 4.0
supports ruby 2.2+, JRuby 1.6.0+ (in 1.9 mode) and Rubinius 2.0.0dev (in 1.9 mode)
support of ruby 1.8 has been dropped due to Mongoid >=3.0 that only supports 1.9 new hash syntax
Installation
Add this to your Gemfile and run the bundle command.

gem "rolify"
Getting Started
1. Generate Role Model
First, use the generator to setup Rolify. Role and User class are the default names. However, you can specify any class name you want. For the User class name, you would probably use the one provided by your authentication solution.

If you want to use Mongoid instead of ActiveRecord, just add --orm=mongoid argument, and skip to step #3.

rails g rolify Role User
NB for versions of Rolify prior to 3.3, use:

rails g rolify:role Role User
The generator will create your Role model, add a migration file, and update your User class with new class methods.

2. Run the migration (only required when using ActiveRecord)
Let's migrate!

rake db:migrate
3.1 Configure your user model
This gem adds the rolify method to your User class. You can also specify optional callbacks on the User class for when roles are added or removed:

class User < ActiveRecord::Base
  rolify :before_add => :before_add_method

  def before_add_method(role)
    # do something before it gets added
  end
end
The rolify method accepts the following callback options:

before_add
after_add
before_remove
after_remove
Mongoid callbacks are also supported and works the same way.

The rolify method also accepts the inverse_of option if you need to disambiguate the relationship.

3.2 Configure your resource models
In the resource models you want to apply roles on, just add resourcify method. For example, on this ActiveRecord class:

class Forum < ActiveRecord::Base
  resourcify
end
# Rolifygem
