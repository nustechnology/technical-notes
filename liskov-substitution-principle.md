# Liskov Principle

Sometimes in programming, you can asked yourself with crazy question like *"should class A inherit from class B? whether B can be parent of A or not"*. Today I will tell you guy **“WHEN a CLASS CAN INHERIT FROM another CLASS through Liskov Substitution Principle”**. Ruby programming language is chosen whole my post.

Let’s start with Math simple example, Rectangle and Square. Math defines that *“Rectangle is a quadrilateral with four right angles”*. So in programming world, Rectangle will be defined as

    class Rectangle
      attr_accessor :width, :height
      def area
        width * height
      end
    end

And Math defines that *“Square is a special kind of Rectangle, it is one where all the sides have the same length. Thus every square is a rectangle because it is a quadrilateral with all four angles right angles”*. Surely in your mind, you will think quickly that *“Square will inherit from Rectangle”* with overridden setters as below

    class Square < Rectangle
      def width=(_width)
        @width = _width
        @height = @width
      end
      def height=(_height)
        @height = _height
        @width = @height
      end
    end

Square is a special kind of Rectangle and Square inherits from Rectangle, it's all right so far. But you’re wrong, let me show you what’re problems.

Square doesn’t need two edges width and height as Rectangle, we need only one edge to define Square. Let's compare the result of below snippets

    rectangle = Rectangle.new
    rectangle.width = 7
    rectangle.height = 4
    p rectangle.area # --> 28

    square = Square.new
    square.width = 7
    square.height = 4
    p square.area # --> 16

`square.area` is `16`, it should be `28` because that's the parent behavior (rectangle area is calculated by width * height). Up to now, we can conclude that Square changes Rectangle behavior and a rectangle can not be replaced by a square and this thing violates Liskov Substitution Principle. Take a look what does Liskov say?

    > the parent should easily replace the child object

Now then you knew why Square inherits from Rectangle is a bad idea. What's the solution? When Square can not inherits from Rectangle (`IS-A`), we can derive into Square has a composition known as special Rectangle (`HAS-A`)

    class Square
      attr_accessor :special_rectangle

      def initialize
        @special_rectangle = Rectangle.new
      end

      def width=(_width)
        @special_rectangle.width = _width
        @special_rectangle.height = _width
      end

      def area
        @special_rectangle.area
      end
    end

Conclusion, only when sub-class doesn't change parent-class behavior, this means sub-class can replace parent class, sub-class can inherit from parent-class. when sub-class can not inherit from parent-class as IS-A relationship, we can consider to convert into HAS-A relationship.