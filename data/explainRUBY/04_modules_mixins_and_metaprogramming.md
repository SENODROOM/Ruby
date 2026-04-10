## Modules & Mixins

Ruby uses modules instead of multiple inheritance.

```ruby
# Module as namespace
module Geometry
  PI = 3.14159

  def self.circle_area(r)
    PI * r ** 2
  end

  class Point
    attr_accessor :x, :y
    def initialize(x, y) = @x, @y = x, y
  end
end

Geometry::PI                  # 3.14159
Geometry.circle_area(5)       # 78.53975
point = Geometry::Point.new(1, 2)

# Module as Mixin — share behavior between classes
module Greetable
  def greet(other)
    "Hello, #{other}! I am #{name}."
  end

  def farewell(other)
    "Goodbye, #{other}! From #{name}."
  end
end

module Serializable
  def to_json
    instance_variables.each_with_object({}) do |var, hash|
      hash[var.to_s.delete('@')] = instance_variable_get(var)
    end.to_json
  end
end

class Person
  include Greetable      # mixin — instance methods
  include Serializable
  extend  Comparable     # extend — class methods (rare)

  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age  = age
  end

  def <=>(other)  # required by Comparable
    age <=> other.age
  end
end

alice = Person.new("Alice", 30)
bob   = Person.new("Bob", 25)

alice.greet("World")    # "Hello, World! I am Alice."
alice > bob             # true (from Comparable)
alice.to_json           # JSON string

# Enumerable — most powerful mixin in Ruby
# Include it in any class with an #each method
class WordList
  include Enumerable    # adds: map, select, sort, min, max, count, etc.

  def initialize(words) = @words = words
  def each(&block)      = @words.each(&block)
end

wl = WordList.new(%w[banana apple cherry date])
wl.sort         # ["apple", "banana", "cherry", "date"]
wl.min          # "apple"
wl.max_by(&:length)  # "banana"
wl.select { |w| w.length > 5 }  # ["banana", "cherry"]
```

---

## Metaprogramming

Ruby's killer feature — code that writes code.

```ruby
# Open classes — reopen and modify ANY class
class Integer
  def factorial
    return 1 if self <= 1
    self * (self - 1).factorial
  end

  def seconds = self
  def minutes  = self * 60
  def hours    = self * 3600
  def days     = self * 86400
end

5.factorial      # 120
2.hours          # 7200
1.days + 3.hours # 97200

class String
  def palindrome?
    self == self.reverse
  end

  def word_count
    split.size
  end
end

"racecar".palindrome?           # true
"hello world foo".word_count    # 3

# define_method — define methods dynamically
class Reporter
  %w[debug info warn error].each do |level|
    define_method("log_#{level}") do |message|
      puts "[#{level.upcase}] #{message}"
    end
  end
end

r = Reporter.new
r.log_info("Server started")   # [INFO] Server started
r.log_error("Something broke") # [ERROR] Something broke

# method_missing — handle undefined methods
class FlexibleObject
  def method_missing(method_name, *args)
    if method_name.to_s.start_with?("find_by_")
      attribute = method_name.to_s.sub("find_by_", "")
      "Finding records where #{attribute} = #{args.first}"
    else
      super  # let it raise NoMethodError
    end
  end

  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?("find_by_") || super
  end
end

obj = FlexibleObject.new
obj.find_by_name("Alice")  # "Finding records where name = Alice"
obj.find_by_age(30)        # "Finding records where age = 30"

# send — call any method by name (including private)
"hello".send(:upcase)    # "HELLO"
"hello".send(:+, " world")  # "hello world"

# instance_variable_get / set
class MyClass
  def initialize = @secret = 42
end

obj = MyClass.new
obj.instance_variable_get(:@secret)  # 42
obj.instance_variable_set(:@secret, 99)

# class_eval / module_eval — add methods to a class at runtime
String.class_eval do
  def shout
    upcase + "!!!"
  end
end
"hello".shout  # "HELLO!!!"
```

---

