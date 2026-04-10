## Blocks, Procs & Lambdas

The heart of Ruby's functional programming.

```
Ruby's Callable Objects
──────────────────────────────────────────────────────────────
Block   │ Anonymous code chunk, NOT an object, attached to method
Proc    │ Object version of a block (loose: returns from calling method)
Lambda  │ Like a proc but strict (like a function, returns from itself)
Method  │ Named method converted to callable object
──────────────────────────────────────────────────────────────
```

```ruby
# BLOCKS — passed to methods with {} or do...end
[1, 2, 3].each { |n| puts n }           # single line → {}
[1, 2, 3].each do |n|                   # multi-line → do...end
  puts "Number: #{n}"
end

# Methods that YIELD to a block
def repeat(n)
  n.times { yield }           # yield executes the block
end
repeat(3) { puts "Hello!" }  # prints Hello! 3 times

# block_given? — check if block was provided
def maybe_block
  if block_given?
    yield
  else
    puts "No block given"
  end
end

# Capture block as explicit Proc parameter (&)
def capture(&block)
  block.call(42)  # call the block like a proc
end
capture { |n| puts n * 2 }  # 84

# PROCS — blocks as objects
square = Proc.new { |n| n * n }
double = proc { |n| n * 2 }

square.call(5)   # 25
square.(5)       # 25 (shorthand)
square[5]        # 25 (shorthand)

# LAMBDAS — strict procs
cube  = lambda { |n| n ** 3 }
add   = ->(a, b) { a + b }   # stabby lambda (modern syntax)

cube.call(3)    # 27
add.call(2, 3)  # 5
add.(2, 3)      # 5

# Proc vs Lambda differences
# 1. Argument checking: lambda checks arity, proc doesn't
add.call(1)        # ArgumentError (lambda is strict)
double.call(1, 2)  # 2 (proc ignores extra, fills missing with nil)

# 2. Return behavior:
# Lambda return returns from lambda
# Proc return returns from ENCLOSING method!

# Converting methods to procs
[1, 2, 3].map(&method(:puts))  # prints each, returns [nil, nil, nil]
["1","2","3"].map(&method(:Integer))  # [1, 2, 3]
[1, -2, 3].select(&method(:positive?))  # — needs context
```

---

## Object-Oriented Programming

```ruby
class Animal
  # Class variable (shared by all instances)
  @@count = 0

  # Accessor macros (generate getter/setter methods)
  attr_reader   :name      # getter only
  attr_writer   :sound     # setter only
  attr_accessor :age       # getter AND setter

  # Class method
  def self.count = @@count
  def self.create(name, age) = new(name, age)  # factory

  def initialize(name, age, sound = "...")
    @name  = name       # instance variable
    @age   = age
    @sound = sound
    @@count += 1
  end

  def speak
    "#{@name} says #{@sound}!"
  end

  # Predicate methods end with ? by convention
  def young?
    @age < 3
  end

  # Dangerous methods end with ! by convention (mutate or raise)
  def rename!(new_name)
    raise ArgumentError, "Name cannot be empty" if new_name.empty?
    @name = new_name
    self  # return self for chaining
  end

  # to_s for string conversion
  def to_s
    "#{self.class.name}(#{@name}, #{@age})"
  end

  # == for equality
  def ==(other)
    other.is_a?(Animal) && name == other.name && age == other.age
  end

  protected

  def secret
    "shhh"
  end

  private

  def internal_method
    "only I can call this"
  end
end

class Dog < Animal  # inheritance with <
  def initialize(name, age, breed)
    super(name, age, "Woof")  # call parent initialize
    @breed = breed
    @tricks = []
  end

  # Override parent method
  def speak
    "#{super} (tail wagging)"  # call parent's speak
  end

  # Method chaining with self
  def learn(trick)
    @tricks << trick
    self
  end

  def show_off
    @tricks.each { |t| puts "#{@name} performs: #{t}" }
    self
  end
end

# Usage
rex = Dog.new("Rex", 2, "Labrador")
rex.learn("sit").learn("stay").learn("roll over").show_off
puts Animal.count   # 1
puts rex.young?     # true
puts rex            # Dog(Rex, 2)
```

---

