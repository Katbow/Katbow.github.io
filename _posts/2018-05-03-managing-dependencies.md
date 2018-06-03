---
layout: post
title:  "Managing Dependencies"
subtitle: "Notes from 'Practical Object Oriented Design in Ruby'"
date:   2018-06-03
background: '/img/posts/bicycle.jpg'
---


## What are Dependencies?

When an object knows something about other objects, this knowledge creates
a dependency.

An object will have a dependency if it knows

* The **name of another class**
* The **name of a message** it wants to send to another class
* The **arguments a message requires**
* The **order of arguments** in a message

Some of these dependencies will be necessary, to allow for collaboration
between classes. However, each dependency will mean another way the class will
need to change when it's dependencies change. Because of this, a class should
know enough to do what it needs to, but nothing more.

## Reduce Dependencies by Decoupling Code

When one class knows a lot about another, it's said to be _tightly coupled_.
This means that although each class may seem like separate entities, they know
so much about one another that they can act as and need to be changed as if
they were a single object. Unnecessary dependencies need to be recognised and
removed to reduce coupling.

* Inject dependencies
* Isolate dependencies
* Remove argument order dependencies
  * Use hashes for initialisation
  * Explicitly define defaults
  * Isolate multiparameter initialisation - Wrap creation of new objects in
  modules so you can control the signature. This is a _factory_, which has the
  sole purpose to create instances of another class.

## Managing Dependency Direction

Dependencies can be reversed. If objectA knows something about objectB, in some
cases it is better to reverse this so that objectB instead knows about objectA.

### Choosing dependency direction

<dl>
  <dt>Understanding likelihood of change</dt>
  <dd>
    &emsp;If a class is likely to change, you shouldn't depend on it
  </dd>

  <dt>Recognising concretions vs abstractions</dt>
  <dd>
    &emsp;<i>Concrete</i> means direct calls to other classes; <i>abstract</i> involve dependency injections etc
  </dd>

  <dt>Avoiding dependent-laden classes</dt>
  <dd>
    &emsp;If a class relies on a lot of other classes, there are many chances for it to need to change
  </dd>

  <dt>Finding the dependencies that matter</dt>
  <dd>
    &emsp;The number of dependents combined with the likelihood of change can
    help determine dependency direction, and when to make changes to reduce coupling. If there is a high likelihood of requirements change with many
    dependents, the class <i>will</i> need to be changed.
  </dd>
</dl>
