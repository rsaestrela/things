---
description: Personal takeways from "Robert C. Martin - Clean Architecture" Part 3
---

# Part 3

p58 - _"The SOLID principles tell us how to arrange our functions and data structures into classes, and how those classes should be interconnected. \(...\) The use of word "class" does not imply that these principles are applicable only to object-oriented software."_

p59 - _"\(SRP: The Single Responsability Principle\) - The best structure for a software syste, is heavily influenced by the social structure of the organization that uses it so that each software module has one, and only one, reason to change."_

p59 - _"\(OCP: The Open-Closed Principle\) - The gist is that for software systems to be easy to change, they must be designed to allow the behaviour of those systems to be changed by adding new code, rather than changing existing code."_

p59 - _"\(LSP: Liskov Substituition Principle\) - \(...\) this principle says thast to build software systems from interchageable parts, those parts must adhere to a contract that allows those parts to be substituted one for another."_

p59 - _"\(ISP: The Interface Segregation Principle\) - This principle advises software designers to avoid depending on things that they don't use."_

p59 - _"\(DIP: The Dependency Inversion Principle\) - The code that implements high-level policy should not depend on the code that implements low-level details. Rather, details should depend on policies."_

## SRP: The Single Responsability Principle

p62 - _"A module should have one, and only one, reason to change."_

p62 - _"A module should be responsible to one, and only one, user or stakeholder."_

p62 - _"A module should be responsible to one, and only one, actor."_

p65 - _"The SRP says to separate the code that different actors depend on."_

p65 - _"The SRP says to separate code that supports different actors."_

## OCP: The Open-Closed Principle

p70 - _"A software artifact should be open for extension but closed for modification."_

p74 - _"Achitects separate functionality based on how, why, and when it changes, and then orgnize that separated functionality into a hierarchy of components. Higher-level components is that hierarchy are protected from the changes made to lower-level components."_

p75 - _"The OCP is one of the driving forces behind the architecture of systems. The goal is to make the system easy to extend without incurring a high impact of change. This goal is accomplished by partitioning the system into components, and arranging those components into a dependency hierarchy that protects higher-level components from changes in lower-level components."_

## DIP: The Dependency Inversion Principle

p87 - _"The Dependency Inversion Principle \(DIP\) tells us that the most flexible systems are those in which source code dependencies refer only to abstractions, not to concretions."_

p88 - _"It is the volatile concrete elements of our systems that we want to avoid depending on. Those are modules that we are actively developing, and that are undergoing frequent change."_

