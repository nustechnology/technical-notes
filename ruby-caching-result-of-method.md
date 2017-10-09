# Ruby - Caching the results of the methods

![](https://preview.ibb.co/geM5G5/memory_1024x1024.jpg)

Caching the results of the methods, some people call it as "memorization", is a common technique we can use to speed up our accessor methods. It is not a technique for only Ruby, we can use it in many languages. The idea of this technique is we cache the results of methods that do time-consuming work that only needs to be done once. In this post, I'll introduce you some approaches and a gem to do memorization in Ruby.

## Basic memorizer
    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method
        # assuming expensive_call makes a network call, time-consuming system command, etc...
        @result ||= expensive_call
      end
    end

The `||=` an be translated as `@result || @result = expensive_call`. That means that we only invoke `expensive_call` the first time we call `expensive_method`, and future calls will return the value of the instance variable `@result` instead of re-do `expensive_call`.


## Multi-line memorizer
A method usually won't have only one line. There is a way to extend the basic memorizer to work with multiple lines of code:

    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method
        @result ||= begin
          data = expensive_call_part_1
          data ||= expensive_call_part_2
          data ||= expensive_call_part_3
        end
      end
    end

The `begin...end` block is the way of defining a bunch of code that needs to run on it’s own, in its own context. Like with normal methods, The last expression that is evaluated in the `begin...end` block is the result that will be returned. That’s why `||=` works just as well here as it did before.


## Memorizer for parameterizable method

The above memorizers work well for simple accessors. But what if you want to memorize a method that takes parameters, like this one?

    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method type
        @result ||= case(type)
          when 'aaa'
            expensive_call_1
          when 'zzz'
            expensive_call_2
          else
            expensive_call_3
        end
      end
    end

Fortunately, Ruby’s Hash has an initalizer that works perfectly for this case. If call Hash.new with a block:

    Hash.new { |h, key| h[key] = an_expression }

Then, every time we try to access a key in the hash which hasn’t been set yet, it’ll execute the block. And it’ll pass the hash itself along with the key we're trying to access into the block.

So, if we want to cache this method, we can do:

    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method type
        @result ||= Hash.new do |hash, key|
          hash[key] = case(key)
            when 'aaa'
              expensive_call_1
            when 'zzz'
              expensive_call_2
            else
              expensive_call_3
          end
        end

        @result[type]
      end
    end

Now, no matter what we pass into `expensive_method`, the correct result will be cached!

## How about the risk of the memorizer?

The first 2 memorizers have a hidden problem. In the first memorizer, what if the `expensive_call` returns `nil`? In the second, what if the `expensive_call_part_3` return nil?

**Every single time we call `expensive_method`, the `@result` will be `nil`, and we will perform the expensive fetches again.**

Therefore, `||=` is probably not the right way to go. We have to differentiate between `nil` and `undefined`:

    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method
        # assuming expensive_call makes a network call, time-consuming system command, etc...
        return @result if defined? @result
        @result ||= expensive_call
      end
    end

</br>

    # app/models/user.rb
    class User < ActiveRecord::Base
      def expensive_method
        return @result if defined? @result

        @result ||= begin
          data = expensive_call_part_1
          data ||= expensive_call_part_2
          data ||= expensive_call_part_3
        end
      end
    end

It's little bit ugly, isn't it? But it works with `nil`, `false`, and everything else!


## Another easy + simple way
If you create memorizer for a lot of methods, our code will become hard to read and ugly quickly. So if we work on an app that needs a lot of memorizers, the best way is build a module which help us to create a memorizer quickly and of course, it must be nice. Fortunately, you don't need to build it yourself. There is a gem which does most of works that we need to do: [Memoist](https://github.com/matthewrudy/memoist). [Memoist](https://github.com/matthewrudy/memoist) has a nice, friendly API. If you need a lots of memorizers, Memoist is worth to give a try ;)
