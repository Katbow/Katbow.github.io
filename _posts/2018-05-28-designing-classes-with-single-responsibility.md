---
layout: post
title:  "Designing Classes with a Single Responsibility"
subtitle: "Notes from 'Practical Object Oriented Design in Ruby'"
date:   2018-05-28
background: '/img/posts/bicycle.jpg'
---

## Why Single Responsibility?

How do you define a well-written application?

One way is that it works as it is meant to _right now_ and that it can be
easily changed _in the future_.

Code that is easy to change should be <abbr title="Transparent Reasonable Usable Exemplary">TRUE</abbr>

<dl>
  <dt>Transparent</dt>
  <dd>&emsp;It should be obvious what changes will mean across the codebase</dd>

  <dt>Reasonable</dt>
  <dd>&emsp;The cost of change should correspond to the benefit of the change</dd>

  <dt>Usable</dt>
  <dd>&emsp;You should be able to use the code in new contexts</dd>

  <dt>Exemplary</dt>
  <dd>&emsp;The code should be a model for how you want future code to be written</dd>
</dl>

Classes & methods with [single responsibility](https://en.wikipedia.org/wiki/Single_responsibility_principle) do one thing and have one reason to change. That one thing is
isolated from the rest of the application. This isolation allows for easy
change, and avoids duplication.

## Determining Single Responsibility
There are a couple of methods you can employ to determine if your class has
single responsibility.

### 1. Ask the class questions

Consider asking questions about all the data and behaviour in a class. For each
question you ask, does it make sense for the class to be able to answer the
question?

### 2. Describe the purpose of the class

You should be able to describe a class in a single sentence. If you can't
describe your class without using _and_ or _or_, then it indicates
the class has multiple responsibilities.

Everything that the class does should relate to the purpose of the class. If
some methods don't make sense in what the class is trying to achieve, then it
might not belong in this class.

## Enforcing Single Responsibility
There are various ways you can change your class to achieve single responsibility.

### 1. Depend on Behaviour, Not Data
Depending on data can mean depending on complicated structures that are
referenced several times throughout a class. Whether the data structure is
simple or complicated, directly referencing data will mean when the data changes,
so does your code _in many places_.

#### 1a. Hide instance variables
Rather than directly calling `@instance_variable` every time you need to use it,
you can include an `attr_reader` for the variable. This wraps the data in a
method which looks like

```ruby
def instance_variable
  @instance_variable
end
```

This is important because if you access `@instance_variable` directly throughout
a class and it needs to change, that change will need to happen in many
places. If the data is modelled as behaviour and accessed with `instance_variable`,
then the change only needs to occur in a single place.

#### 1b. Hide data structure

If you are [directly referencing a complicated data structure](https://github.com/skmetz/poodr/blob/8d93843ff40f16eb158d20d2702a7e7b27961341/chapter_2.rb#L104-L117),
this can be confusing because it obscures what the data is representing.
Additionally, the code is dependent on the structure of the data. When the
structure changes, this means changes in many places in the code.

If instead you [reveal what the data is referencing](https://github.com/skmetz/poodr/blob/8d93843ff40f16eb158d20d2702a7e7b27961341/chapter_2.rb#L124-L141),
it makes it clear what the data represents. Since the data structure is
referenced in one area only, it only requires one change when the structure
changes.

### 2. Single Responsibility Everywhere
Single responsibility is not just about the class having a single purpose. The
methods should only be responsible for a single behaviour.

#### 2a. Extract Extra Responsibilities from Methods
The `gear_inches` defined [here](https://github.com/skmetz/poodr/blob/master/chapter_2.rb#L160)
has multiple responsibilities; it calculated the gear inches, and also the diameter.
Moving the calculation for diameter into [its own method](https://github.com/skmetz/poodr/blob/master/chapter_2.rb#L167) means
the `gear_inches` (and `diameter`) method has a single thing to calculate.

#### 2b. Isolate Extra Responsibilities in Classes
If after these refactors, it still feels like your class has extra behaviour
that doesn't fit with the purpose, you should isolate this behaviour. You can either
[create an entirely separate class](https://github.com/skmetz/poodr/blob/master/chapter_2.rb#L199),
or [move the behaviour to a Struct](https://github.com/skmetz/poodr/blob/master/chapter_2.rb#L175).
