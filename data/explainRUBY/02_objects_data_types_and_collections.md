## Everything Is an Object

```ruby
# In Ruby, EVERYTHING is an object — even primitives!

42.class           # Integer
3.14.class         # Float
true.class         # TrueClass
false.class        # FalseClass
nil.class          # NilClass
"hello".class      # String
:symbol.class      # Symbol
[1,2,3].class      # Array
{a: 1}.class       # Hash

# Even numbers respond to methods!
42.to_s            # "42"
42.to_f            # 42.0
42.even?           # true
42.times { |i| print "#{i} " }  # 0 1 2 ... 41

# nil is an object — not a null pointer!
nil.nil?           # true
nil.to_i           # 0
nil.to_s           # ""
nil.to_a           # []
nil.inspect        # "nil"

# Calling a method on nil is safe (it's just an object)
# But accessing attributes of nil raises NoMethodError
# → Use safe navigation &.
user = nil
user&.name         # nil (no error)
user&.name.upcase  # nil (no error)
user.name          # NoMethodError!
```

---

## Data Types

```ruby
# Integers
42            # Integer (automatically Bignum when needed)
1_000_000     # underscores for readability
0xFF          # hexadecimal
0b1010        # binary
0o777         # octal

# Floats
3.14
1.5e10
Float::INFINITY
Float::NAN

# Strings (mutable by default)
str = "Hello"
str << " World"      # mutates str (shovel operator)
str.frozen?          # false
str.freeze           # make immutable
frozen_str = "literal".freeze

# String methods
"hello world".capitalize   # "Hello world"
"hello world".split        # ["hello", "world"]
"hello world".gsub(/o/, '0')  # "hell0 w0rld"
"  spaces  ".strip         # "spaces"
"abc" * 3                  # "abcabcabc"
"hello"[1..3]              # "ell"
"hello".chars              # ["h", "e", "l", "l", "o"]

# Symbols (immutable, compare by identity — very fast)
:hello == :hello   # true (same object always!)
:hello.to_s        # "hello"
"hello".to_sym     # :hello

# Booleans and Nil — FALSY values in Ruby
# ONLY false and nil are falsy — everything else is truthy!
# 0 is TRUTHY in Ruby (unlike C, JS, Python)!
!nil    # true
!false  # true
!0      # false ← 0 is truthy!
!""     # false ← empty string is truthy!
```

---

## Collections

### Arrays

```ruby
# Array creation
arr = [1, "two", :three, 4.0, nil]  # mixed types fine
words = %w[apple banana cherry]      # word array shorthand
syms  = %i[red green blue]           # symbol array shorthand

# Access
arr[0]      # 1
arr[-1]     # nil (last)
arr[1, 3]   # ["two", :three, 4.0] (start, length)
arr[1..3]   # ["two", :three, 4.0] (range)
arr.first   # 1
arr.last    # nil

# Modification
arr.push(5)         # append
arr << 6            # append (shovel)
arr.unshift(0)      # prepend
arr.pop             # remove last
arr.shift           # remove first
arr.delete(:three)  # remove by value
arr.compact         # remove nils
arr.uniq            # remove duplicates
arr.flatten         # flatten nested arrays
arr.sort            # sort (must be same/comparable types)
arr.sort_by { |x| x.to_s }  # sort by custom key

# Functional methods (return new arrays)
[1,2,3,4,5].map    { |n| n * 2 }       # [2,4,6,8,10]
[1,2,3,4,5].select { |n| n.odd? }      # [1,3,5]
[1,2,3,4,5].reject { |n| n.odd? }      # [2,4]
[1,2,3,4,5].reduce(0) { |sum, n| sum + n }  # 15
[1,2,3,4,5].each_with_object([]) { |n, acc| acc << n*2 }

# Chaining
(1..10)
  .select(&:odd?)
  .map { |n| n ** 2 }
  .first(3)
# => [1, 9, 25]
```

### Hashes

```ruby
# Hash creation
person = { name: "Alice", age: 30, role: :admin }  # symbol keys (modern)
legacy = { "name" => "Alice", 1 => "one" }           # hash rocket (any key)

# Access
person[:name]        # "Alice"
person[:missing]     # nil (no KeyError!)
person.fetch(:name)  # "Alice"
person.fetch(:missing, "default")  # "default"
person.fetch(:missing) { |k| "No #{k}" }  # "No missing"

# Modification
person[:email] = "alice@example.com"  # add
person.merge({ city: "NYC" })         # returns new hash
person.merge!({ city: "NYC" })        # mutates in place

# Iteration
person.each { |key, value| puts "#{key}: #{value}" }
person.map { |k, v| [k, v.to_s] }.to_h
person.select { |k, v| v.is_a?(String) }
person.any? { |k, v| v == :admin }

# Useful methods
person.keys          # [:name, :age, :role]
person.values        # ["Alice", 30, :admin]
person.to_a          # [[:name, "Alice"], [:age, 30], ...]
person.size          # 3
person.empty?        # false
person.key?(:name)   # true
person.value?("Alice") # true

# Default values
counter = Hash.new(0)
counter[:a] += 1  # 1 (default 0, no KeyError)
counter[:a] += 1  # 2
```

---

