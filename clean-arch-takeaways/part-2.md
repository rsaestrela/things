---
description: Takeways from "Robert C. Martin - Clean Architecture" Part 2
---

# Part 2

## Paradigm Overview

p22 - _"Structured programming imposes discipline on direct transfer of control."_

p22 - _"Object-oriented programming imposes discipline on indirect transfer of control._

p23 - _"Functional programming imposes discipline upon assignment._

p23 - _"...Each of the paradigms removes capabilities from the programmer. None of them adds new capabilities. Each imposes some kind of discipline that is negative in its intent. The paradigms tell us what not to do, more than they tell us what to do."_

p24 - _"What does this history on paradigms have to do with architecture? Everything. We use polymorphism as the mechanism to impose discipline on the location of and access to data; and we use structured programming as the algoritmic foundation of our modules."_

p24 - _"Notice how those three \[paradigms\] align with the three big concerns of architecture: function, separation of components, and data management."_

## Structured Programming

p29 - _"Structured programming allows modules to be recursively decomposed into provable units, which in turn means that modules can be functionally decomposed."_

### Tests

Dijkstra - _"Testing shows the presence, not the absence, of bugs."_

p29 - _"All that tests can do, after sufficient testing effort, is allow us to deem a program to be correct enough for our purposes."_

p29 - _"Structured programming allows modules to be recursively decomposed into provable units, which in turn means that modules can be functionally decomposed."_

p29 - _"Structured programming forces us to recursibely decompose a program into a set of small provable functions. We can then use tests to try to prove those small provable functions incorrect. If such tests fail to prove incorrectness, then we deem the functions to be correct enough for our purposes."_

## Object-oriented Programming

### Encapsulation

p34 - _"The reason encapsulation is cited as part of the definition of OO is that OO languages provide easy and effective encapsulation of data and function. As a result, a line can be drawn around a cohesive set of data and functions. Outside of that line, the data is hidden and only some of the functions are known. We see this concept in action as the private data members and the public member functions of a class."_

### Inheritance

p37 - _"Inheritance is simply the redeclaration of a group of variables and functions within an enclosing scope. This is something C³ programmers were able to do manually long before there was an OO language."_

### Polymorphism

p42 - _"The bottom line is that polymorphism is an application of pointers to functions \(...\) In other words, OO has provided nothing new."_

p42 - _"Ah, but that's not quite correct. OO languages may not have given us polymorphism, but they have made it safer and much more convenient."_

### The Power of Polymorphism

p44 - _"OO allows the plugin architecture to be used anywhere, for anything."_

### Dependency Inversion

p45 - _"The fact that OO languages provide safe and convenient polymorphism means that any source dependency, no matter where it is, can be inverted."_

p47 - _"What is OO? There are many different answers to this question. To the software architect, however, the answer is clear: OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system."_

## Functional Programming

p51 - _"\(...\) This leads us to a surprising statement: Variables in functional languages do not vary."_

p56 - _"Software - the stuff of computer programs - is composed of sequence, selection, iteration, and inderection. Nothing more. Nothing less."_

