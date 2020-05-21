# Ruby Metaprogramming

![](https://i.ibb.co/ZKTYMH2/RUBY-METAPROGRAMMING.png)

Metaprogramming is a technique which can help us to write code that writes code by itself dynamically at runtime to generate objects, methods, functions, etc. Using metaprogramming you can reopen and modify classes, catch methods that don’t exist and create them on the fly, etc. Metaprogramming at first may seem to be an advanced topic, suitable only for programming language gurus, but it's not really that difficult once you know how to use. Let's discover it in this post.

## Check an object whether it can call a method

We can ask any object whether it has the ability to provide a response to a specific method call, we make it using the `respond_to?` method.

    "XXX".respond_to? :downcase
    # => true
    "XXX".respond_to? :abc
    # => false
    

## What instance variables and methods have been defined?

We can use the `methods` method to give us a list of all methods available to a specific object and the `instance_variables` method to give us a list of the instance variables defined/used by this object.

    class User
      attr_accessor :name, :birthdate
    
      def initialize(name, birthdate)
        @name = name
        @birthdate = birthdate
      end
    end
    
    user = User.new('NhanNM', Date.new(1989, 12, 15))
    
    user.methods
    # => [:name, :name=, :birthdate, :spit, :birthdate=, :instance_of?, :public_send, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :private_methods, :kind_of?, :instance_variables, :tap, :is_a?, :extend, :define_singleton_method, :to_enum, :enum_for, :<=>, :===, :=~, :!~, :eql?, :respond_to?, :freeze, :inspect, :display, :send, :object_id, :to_s, :method, :public_method, :singleton_method, :nil?, :hash, :class, :singleton_class, :clone, :dup, :itself, :taint, :tainted?, :untaint, :untrust, :trust, :untrusted?, :methods, :protected_methods, :frozen?, :public_methods, :singleton_methods, :!, :==, :!=, :__send__, :equal?, :instance_eval, :instance_exec, :__id__]
    
    user.instance_variables
    # => [:@name, :@birthdate]
    

## Sending Messages

When we invoke or call this method using the dot-notation, what we are really saying is that we are passing a message to the String, and it decides how to respond to that message.

    "NhanNM".downcase 
    # => "nhannm"
    In Ruby, this can be done another way, by using the send method:
    "NhanNM".send :downcase
    # => "nhannm"
    

Generally you wouldn’t use this form in normal programming, but because Ruby allows us to send messages (or invoke methods) in this form, it gives the option of sending a dynamic message or calling methods dynamically.

    class User < ActiveRecord::Base
      # table users has columns: name, birthdate, etc...
      ACCESSIBLE_ATTRS = %i(name birthdate)
      def.get_all_information attr
        result = {}
        if attr.in? ACCESSIBLE_ATTRS
          find_each do |user|
            result[user.id] = user.send(attr)
          end
        end
    
        result
      end
    end
    
    User.get_all_information :name
    # => {1 => "NhanNM", 2 => "TamNLT"}
    User.get_all_information :birthdate
    # => {1 => December 15, 1989, 2 => October, 1, 2019}
    

## Generate New Methods

### define_method

`define_method` is a method defined in `Module` class which you can use to create methods dynamically. To use `define_method`, you call it with the name of the new method and a block where the parameters of the block become the parameters of the new method. Let’s take a look at a simple example:

    class User < ActiveRecord::Base
      ROLES = %w(admin manager staff)
    
      validate :role, inclusion: {in: ROLES}
    
      ROLES.each do |r|
        define_method "is_#{role}?"
          role == r
        end
      end
    end
    
    manager = User.new(role: 'manager')
    manager.is_admin?
    # => false
    manager.is_manager?
    # => true
    

### Defining Methods Using “class_eval” and “instance_eval”

Beside the traditional way, there’s one more way to create a class method, and that is by using `instance_eval`:

    class User < ActiveRecord::Base
    end
    
    User.instance_eval do
      def class_method_1
        p "inside class method 1"
      end
    end
    
    User.class_method_1
    # "inside class method 1"
    

On the other hand, `class_eval` evaluates the code in the context of a class instead of an instance. It practically reopens the class. Here is how `class_eval` can be used to create an instance method:

    class User < ActiveRecord::Base
    end
    
    User.class_eval do
      def instance_method_1
        p "inside instance method 1"
      end
    end
    
    User.new.instance_method_1
    # "inside instance method 1"
    

To summarize, when you call `class_eval` method, you change self to refer to the original class and when you call `instance_eval`, self changes to refer to original class’ metaclass.

## Conclusion

Metaprogramming is fantastic but only when it is used sparingly. It can help you write repetitive code more easily (such as the money fields example), but it also can add indirection and make it much more difficult to figure out what is actually happening in the code. Only use metaprogramming if it provides a clear advantage.