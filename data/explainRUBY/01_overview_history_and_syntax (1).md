# Ruby 💎

> *"Ruby is designed to make programmers happy."*
> — Yukihiro "Matz" Matsumoto, creator of Ruby

---

## Table of Contents

1. [Overview](#overview)
2. [History & Philosophy](#history--philosophy)
3. [Core Characteristics](#core-characteristics)
4. [Syntax & Fundamentals](#syntax--fundamentals)
5. [Everything Is an Object](#everything-is-an-object)
6. [Data Types](#data-types)
7. [Collections](#collections)
8. [Blocks, Procs & Lambdas](#blocks-procs--lambdas)
9. [Object-Oriented Programming](#object-oriented-programming)
10. [Modules & Mixins](#modules--mixins)
11. [Metaprogramming](#metaprogramming)
12. [Error Handling](#error-handling)
13. [Ruby on Rails & Ecosystem](#ruby-on-rails--ecosystem)
14. [Modern Ruby](#modern-ruby)

---

## Overview

Ruby is a **dynamic, interpreted, fully object-oriented** programming language designed for **programmer happiness and productivity**. Its philosophy prioritizes natural, elegant code that reads like plain English.

```
Ruby's Design Goals
──────────────────────────────────────────────────────────────
          Human Efficiency > Machine Efficiency
                  Elegance > Verbosity
                Flexibility > Strictness
              Convention > Configuration
──────────────────────────────────────────────────────────────

The MINASWAN Principle:
"Matz Is Nice And So We Are Nice"
(Community philosophy of kindness and inclusion)
```

**Key Stats:**
- Created: 1993–1995 by Yukihiro "Matz" Matsumoto in Japan
- Released publicly: 1995
- Current version: Ruby 3.3+ (2024)
- Paradigms: Object-Oriented, Functional, Imperative, Reflective
- Famous for: Ruby on Rails (web), scripting, developer tools

---

## History & Philosophy

```
Ruby Timeline
────────────────────────────────────────────────────────────────
1993 ──► Matz begins Ruby development (inspired by Perl, Smalltalk, Lisp)
1995 ──► Ruby 0.95 publicly released in Japan
1999 ──► First English-language Ruby book
2004 ──► David Heinemeier Hansson (DHH) creates Ruby on Rails
2005 ──► Rails 1.0 — web development revolution begins
2007 ──► Ruby 1.9 — major performance improvements, YARV VM
2009 ──► Ruby 1.9.1 — first stable 1.9 release
2013 ──► Ruby 2.0 — keyword arguments, lazy enumerators
2016 ──► Ruby 2.3 — safe navigation operator (&.)
2020 ──► Ruby 3.0 — 3x faster than Ruby 2.0 (3x3 goal achieved!)
         Static type checking with RBS, concurrency with Ractor
2022 ──► Ruby 3.2 — WASM support, YJIT production-ready
2023 ──► Ruby 3.3 — YJIT major improvements, Prism parser
────────────────────────────────────────────────────────────────
```

### Matz's Influences

```
Language Influences on Ruby
─────────────────────────────────────────────────────
Perl      ──► Text processing, practical scripting
Smalltalk ──► Pure OOP (everything is an object)
Lisp      ──► Flexibility, macros → blocks/procs
Python    ──► Readability (then diverged intentionally)
Ada/Eiffel──► Exception handling concepts
CLU       ──► Iterators
─────────────────────────────────────────────────────
```

---

## Core Characteristics

| Feature | Ruby |
|---------|------|
| **Typing** | Dynamic, duck-typed |
| **Execution** | Interpreted (MRI/CRuby) or JIT (YJIT) |
| **Memory** | Garbage collected |
| **OOP** | Pure — everything is an object, even classes |
| **Open Classes** | Any class can be reopened and modified |
| **Mixins** | Modules as multiple inheritance alternative |
| **Blocks** | Anonymous code with special built-in syntax |
| **Convention** | snake_case, ?, ! method suffixes |
| **DSLs** | Excellent for building domain-specific languages |

---

## Syntax & Fundamentals

```ruby
# Comments start with #
# No semicolons! (optional, allowed but discouraged)
# No curly braces for blocks! Use do...end or { }

# Variables
name    = "Alice"        # local variable (lowercase)
@name   = "Bob"          # instance variable (@)
@@count = 0              # class variable (@@)
NAME    = "CONSTANT"     # constant (uppercase) — warning if reassigned
$global = "everywhere"   # global variable (avoid!)

# Multiple assignment
a, b, c = 1, 2, 3
x, y = y, x              # swap! (no temp variable needed)
first, *rest = [1, 2, 3, 4]   # splat operator: first=1, rest=[2,3,4]
*init, last = [1, 2, 3, 4]    # init=[1,2,3], last=4

# String interpolation
greeting = "Hello, #{name}! You are #{2024 - 1994} years old."

# Multi-line strings
long_text = <<~HEREDOC
  This is a heredoc.
  It can span multiple lines.
  Indentation is stripped by ~.
HEREDOC

# Symbols — immutable, interned strings (great for identifiers)
:hello          # symbol literal
status = :active
options = { name: "Alice", role: :admin }  # symbol keys (modern syntax)

# Ranges
(1..10)    # 1 to 10 inclusive
(1...10)   # 1 to 9 exclusive (three dots)
('a'..'z') # character range
(1..Float::INFINITY)  # infinite range!
```

---

