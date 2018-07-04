---
layout: post
title:  "Managing Dependencies"
subtitle: "Notes from 'Practical Object Oriented Design in Ruby'"
date:   2018-07-04
background: '/img/posts/interconnected.jpg'
---


## What are Dependencies?

When an object knows something about other objects, this knowledge creates
a dependency which is a type of coupling. Too many dependencies makes code more
difficult to change, since it has widespread effects.

An object will have a dependency if it knows

* The **name of another class**
* The **name of a method** from another class
* The **arguments a method requires**
* The **order of arguments** in a method

Some of these dependencies will be necessary to allow for collaboration
between classes. However, each dependency will mean another way the class will
need to change when its dependencies change. Because of this, a class should
know enough to do what it needs to, but nothing more.

## Reduce Dependencies by Decoupling Code

When one class knows a lot about another, it's said to be _tightly coupled_.
This means that although each class may seem like separate entities, they know
so much about one another that they can act as and need to be changed as if
they were a single entity.

Unnecessary dependencies need to be recognised and removed to reduce coupling.
You can decouple code by

* Injecting dependencies
* Isolating dependencies
* Removing argument order dependencies
  * Use named parameters over positional parameters, eg. hashes in Ruby
  * Explicitly define defaults
  * Isolate multiparameter initialisation - Wrap creation of new objects in
  modules so you can control the signature. This is a _factory_, which has the
  sole purpose of creating instances of another class.

## Manage Dependency Direction

Dependencies can be reversed; if objectA depends on objectB, the code can be changed
so that objectB depends on objectA. Reversing the dependency direction can make the
codebase more stable by relying on classes that change less often.

Determining the direction can be done by evaluating

<dl>
  <dt>Likelihood of change</dt>
  <dd>
    &emsp;If a class is likely to change, you should avoid depending on it
  </dd>

  <dt>Concrete vs abstract classes</dt>
  <dd>
    &emsp;Concrete classes (those with direct calls to other classes) are more
    likely to change than abstract classes (those with looser coupling)
  </dd>

  <dt>Dependent-laden classes</dt>
  <dd>
    &emsp;If a class has many dependents, there are many chances for it to need
    to change
  </dd>
</dl>

The number of dependents combined with the likelihood of change can
help determine dependency direction, and when to make changes to reduce
coupling. If there is a high likelihood of a change in requirements and many
dependents, the class _will_ require change. If many dependents are required,
then the class should be abstract to reduce the likelihood of change.
