---
layout: post
title: "Ruby-Study"
subtitle: ""
date: 2015-10-29 17:37:00
author: "rightpeter"
header-img: ""
---

# Ruby Names

[reference to rubylearning](http://rubylearning.com/satishtalim/ruby_names.html)

**Ruby Names** are used to refer to constants, variables, methods, classes, and
modules. *The first character of a name helps Ruby to distinguish its intended
use*. Certain names, are reserved words and should not be used as variable,
method, class, or module name. Lowercase letter means the characters "a"
through "z". Uppercase letter means "A" through "Z", and digit means "0"
through "9". A name is an uppercase letter, lowercase letter, or an
underscore("_"), followed by **_Name characters_** *this is any combination of
upper- and lowercase- letters, underscore and digits).

## Variables

Variables in Ruby can contain data of any type. You can use variables in your
Ruby programs _without any declarations_. Variable name itself denotes its
scope (local, global, instance, etc.).

 * A **local** variable (declared within an object) name consists of a lower
   case letter (or an underscore) followed by name characters (sunil, _z,
   hit_and_run).

 * An **instance** variable (declared within an object always "belongs to"
   whatever object **self** refers to) name starts with an "at" sign ("@")
   followed by a name (@sign, @_, @Counter).

 * A **class** variable (declared within a class) name starts with two "at"
   signs ("@@") followed by a name (@@sign, @@_, @@Counter). A class variable
   is shared among all objects of a class. Only one copy of  a particular class
   variable exists for a given class. Class variables used at the top level are
   defined in **Object** and behave like global variables. _Class variables are
   rarely used in Ruby programs_.

 * **Global** variables start with a dollar sign ("$") followed by name
   characters. A global variable name can be formed using "$-" followed by any
   single character ($counter, $COUNTER, $-x). Ruby defines a number of global
   variables that include other punctuation characters, such as $_ and $-K.

## Constants

A **constant** name starts with an uppercase letter followed by name
characters. Class names and module name are contants, and follow the constant
naming conventions. Examples: module MyMath, PI=3.1416, class MyPune.

## Method Names

**Method** names should begin with a lowercase letter (or an uderscore). "?",
"!" and "=" are the only weird characters allowed as method name suffixes (! or
bang labels a method as dangerous-specifically, as the dangerous equivalent of
a method with the same name but without the bang. More on **Bang methods** later.).

    The Ruby convention is to use underscores to separate words in a multiword
    method or variable name. By convention, most constants are written in all
    uppercase with underscores to separate words, LIKE_THIS. Ruby class and
    module names are also constants, but they are conventionally written using
    initial capital letters and camel case, LikeThis.
    It's to be noted that any given variable can at different times hold
    references to objects of many different types. A Ruby constant is also a
    reference to an object. Constants are created when they are first assigned
    to (normally in a class or module definition; they should not be defined in
    a method - more on constants later). Ruby lets you alter the value of a
    constant, although this will generate a warning message. Also, variables in
    Ruby act as "references" to objects, which undergo automatic garbage
    collection.

An example to show Ruby is **dynamically typed** - p007dt.rb

    # p007dt.rb
    # Ruby is dynamic
    x = 7       # integer
    x = "house" # string
    x = 7.5     # real

    # In Ruby, everything you manipulate is an object
    'I love Ruby'.length

The basic types in Ruby are **Numeric** (subtypes include **Fixnum**,
**Integer**, and **Float**), **String**, **Array**, **Hash**, **Object**,
**Symbol**, **Range**, and **RagExp**.

    Though we have not learned classes yet, nevertheless, here are some more
    details about a class called Float.

    Float is a sub class of Numeric.

    Float objects represent real numbers using the native architecture's
    double-precision floating point representation.

    DIG is a class constant that gives precision of Float in decimal digits.
    MAX is another class constant that gives the largest Float.

On my PC, the code:

    puts Fload::DIG

outputs 15. And.

    puts Float::MAX

outputs 1.79769313486232e+308

Let us look at Peter Cooper's example in his _Beginning Ruby_ book (never mind
if you do not follow the code yet):

    rice_on_square = 1
    64.times do |square|
        puts "On square #{square + 1} are #{rice_on_square} grain(s)"
        rice_on_square *= 2
    end

By square 64, you're up to placing many trillions of grains of rice on each
square!

It proves that Ruby is able to deal with extremely large numbers, and unlike
many other programming languages, there are no inconvenient limits. Ruby does
this with different classes, one called **Fixnum** (default) that represents
easily managed smaller numbers, and another, aptly called **Bignum**, that
represents "big" numbers Ruby needs to manage internally. Ruby will handle
Bignums and Fixnums for you, and you can perform arithmetic and other
operations without any problems. Results might vary depending on your system's
architecture, but as these changes are handled entirely by Ruby, there's no
need to worry.

Ruby doesn't require you to use **primitives**(**data types**) when
manipulating data of these types - if it looks like an integer, it's probably
an integer; if it looks like a string, it's probably a string. The class
**Object** has a method called **class** that returns the class of an object,
for example:

    s = 'hello'
    s.class     # String

Another example: (Don't worry if you do not understand the code now).

    puts 'I am in class = ' + self.class.to_s
    puts 'I am an object = ' + self.to_s
    print 'The object methods are = '
    puts self.private_methods.sort

We shall talk about **self** later on. **private_methods** is a method of the
**Object** class and **sort** is a method of the **Array** class.

In Ruby, everything you manipulate is an object, and the results of those
manipulations are themselves objects. There are no primitives or data-types.

    5.times { puts "Mice!\n" } # more on blocks later
    "Elephants Like Peanuts".length
