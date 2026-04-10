## Error Handling

```ruby
# begin / rescue / ensure / else
def divide(a, b)
  begin
    result = a / b
  rescue ZeroDivisionError => e
    puts "Can't divide by zero: #{e.message}"
    0  # return default
  rescue TypeError => e
    puts "Wrong type: #{e.message}"
    raise  # re-raise the exception
  else
    puts "Success!"  # runs if NO exception
    result
  ensure
    puts "Always runs (like finally)"  # cleanup
  end
end

# Inline rescue (single line)
value = Integer(user_input) rescue 0  # default to 0 on any error

# Custom exception hierarchy
class AppError < StandardError; end
class DatabaseError < AppError
  def initialize(msg = "Database error occurred")
    super(msg)
  end
end
class ConnectionError < DatabaseError; end

begin
  raise ConnectionError, "Cannot connect to host"
rescue AppError => e  # catches AppError and all subclasses!
  puts "App error: #{e.class} - #{e.message}"
end

# retry — retry the block
attempts = 0
begin
  attempts += 1
  connect_to_database  # might fail
rescue ConnectionError
  retry if attempts < 3  # retry up to 3 times
  raise  # give up
end
```

---

## Ruby on Rails & Ecosystem

```
Ruby on Rails Architecture (MVC)
──────────────────────────────────────────────────────────────────
Browser Request
     │
     ▼
Router (config/routes.rb)
     │ routes to controller action
     ▼
Controller (app/controllers/)
  ├── receives request params
  ├── calls Model methods
  ├── assigns @instance_variables
  └── renders View
          │
          ▼
Model (app/models/)                    View (app/views/)
  ├── ActiveRecord (ORM)              ├── ERB templates
  ├── Validations                     ├── JSON responses
  ├── Associations                    └── HTML
  ├── Callbacks
  └── Scopes
          │
          ▼
Database (PostgreSQL, MySQL, SQLite)
──────────────────────────────────────────────────────────────────
```

```ruby
# Rails example — blog with posts and comments
# Model (app/models/post.rb)
class Post < ApplicationRecord
  belongs_to :user
  has_many   :comments, dependent: :destroy
  has_many   :tags, through: :post_tags

  validates :title,   presence: true, length: { minimum: 5, maximum: 100 }
  validates :content, presence: true

  scope :published,  -> { where(published: true) }
  scope :recent,     -> { order(created_at: :desc).limit(10) }
  scope :by_author,  ->(user) { where(user: user) }

  before_save :set_slug

  private

  def set_slug
    self.slug = title.parameterize
  end
end

# Controller (app/controllers/posts_controller.rb)
class PostsController < ApplicationController
  before_action :authenticate_user!, except: [:index, :show]
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def index
    @posts = Post.published.recent.includes(:user, :tags)
  end

  def create
    @post = current_user.posts.build(post_params)
    if @post.save
      redirect_to @post, notice: "Post created!"
    else
      render :new, status: :unprocessable_entity
    end
  end

  private

  def set_post = @post = Post.find(params[:id])
  def post_params = params.require(:post).permit(:title, :content, tag_ids: [])
end
```

### Ruby Gems Ecosystem

```
Essential Gems
──────────────────────────────────────────────────────────
Web Frameworks:
  rails       ──► Full-stack MVC framework (the king)
  sinatra     ──► Minimalist web framework
  hanami      ──► Clean architecture framework

ORMs & Databases:
  activerecord ──► Rails ORM
  sequel       ──► Flexible ORM
  rom-rb       ──► Functional ORM

Authentication:
  devise       ──► Full auth solution
  rodauth      ──► Comprehensive auth framework

Background Jobs:
  sidekiq      ──► Redis-backed job queue (fast!)
  good_job     ──► PostgreSQL-backed jobs

Testing:
  rspec        ──► BDD testing framework (most popular)
  minitest     ──► Ruby standard library testing
  factory_bot  ──► Test data factories
  capybara     ──► Integration testing

APIs:
  grape        ──► REST API framework
  graphql-ruby ──► GraphQL implementation

Utilities:
  nokogiri     ──► HTML/XML parsing
  httparty     ──► HTTP client
  dry-rb       ──► Functional programming toolkit
  zeitwerk     ──► Code autoloading
──────────────────────────────────────────────────────────
```

---

## Modern Ruby

```ruby
# Ruby 3.x features

# Pattern Matching (Ruby 3.0+) — powerful deconstruction
case { name: "Alice", age: 30, role: :admin }
in { name: String => name, role: :admin }
  puts "Admin: #{name}"
in { name: String => name }
  puts "Regular user: #{name}"
end

# One-liner pattern match
data in { name: String => name }  # "find pattern" (non-exhaustive)

# Endless method (Ruby 3.0+)
def double(x) = x * 2
def square(x) = x ** 2

# Hash shorthand (Ruby 3.1+)
name = "Alice"
age  = 30
{ name:, age: }   # { name: "Alice", age: 30 } — no repetition!

# find pattern (Ruby 3.0+)
case [1, 2, 3, 4, 5]
in [*, 3, *rest]
  puts "Found 3, rest: #{rest}"  # rest = [4, 5]
end

# Data class (Ruby 3.2+) — immutable value object
Point = Data.define(:x, :y)
p = Point.new(x: 1, y: 2)
p.x   # 1
p.with(x: 10)  # Point(x=10, y=2) — new object

# Fiber Scheduler (Ruby 3.0+) — non-blocking I/O
# Ractor (Ruby 3.0+) — parallel execution (experimental)
ractor = Ractor.new do
  Ractor.receive * 2
end
ractor.send(21)
puts ractor.take  # 42

# Frozen string literals — performance optimization
# frozen_string_literal: true
# (comment at top of file — all string literals become frozen)
```

---

## Quick Reference

```
Conventions
──────────────────────────────────────────────────────
local_variable    Lowercase snake_case
CONSTANT          All uppercase
@instance_var     Single @ prefix
@@class_var       Double @@ prefix
ClassName         CamelCase
method_name?      Returns boolean (predicate)
method_name!      Mutates or raises (dangerous)
──────────────────────────────────────────────────────

Common Idioms
──────────────────────────────────────────────────────
x || y            x if truthy, else y
x ||= default     assign if nil/false
array.any?(&:nil?)     check with symbol
array.map(&:to_s)      map with symbol
hash.transform_values(&:to_s)
result = condition ? a : b
puts object.tap { |o| o.validate! }  # debug mid-chain
──────────────────────────────────────────────────────

String Literals
──────────────────────────────────────────────────────
"double"          Interpolation + escape sequences
'single'          Literal (no interpolation)
%w[a b c]         Word array
%i[a b c]         Symbol array
%q(string)        Like single quotes
%Q(string)        Like double quotes
<<~HEREDOC        Squiggly heredoc (strips indent)
──────────────────────────────────────────────────────

Useful Methods to Know
──────────────────────────────────────────────────────
object.class      Class of object
object.is_a?(X)   Is it a kind of X?
object.respond_to?(:m)  Does it have method m?
object.freeze     Make immutable
object.frozen?    Check if frozen
object.dup        Shallow copy
object.clone      Shallow copy (preserves frozen state)
object.inspect    Debug string representation
object.tap { }    Yield self, return self (for debugging)
object.then { }   Yield self, return block result
──────────────────────────────────────────────────────
```

---

*Ruby: the language where code is poetry, everything is an object, and your programmer happiness is the primary feature, not an afterthought. 💎*
