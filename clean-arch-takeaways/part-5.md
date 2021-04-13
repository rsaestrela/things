---
description: Personal takeways from "Robert C. Martin - Clean Architecture" Part 5
---

# Part 5

p136 - _"The word "architecture" conjures visions of power and mystery."_

p136 - _"The architecture of a software system is the shape given to that system by those who build it. \(..\) The purpose of that shape is to facilitate the development, deployment, operation, and maintenance of the software system contained with it."_

p137 - _"\(..\) the role \(of software architecture\) is passive and cosmetic, not active or essential. There are few, if any, behavioural options that the architecture of a system can leave open."_

p137 - _"Good architecture makes the system easy to understand, easy to develop, easy to maintain, and easy to deploy. The ultimate goal is to minimize the lifetime cost of a system and to maximize programmer productivity."_

p139 - _"A good software architecture communicates the operational needs of a system."_

p139 - _"Architecture should reveal operation."_

p140 - _"All software systems can be decomposed into two major elements: policy and details. The policy element embodies all the business rules and procedures. The policy is where the true value of the system lives. The details are those things that are necessary to enable humans, other systems, and programmers to communicate with the policy, but that do not impact the behaviour of the policy at all."_

p140 - _"The goal of the architect is to create a shape of the system that recognises policy as the most essential element of the system while making the details irrelevant to that policy. This allows decisions about those details to be delayed and deferred."_

p142 - _"A good architect pretends that the decision has not been made, and shapes the system such that those decisions can still be deferred or changed for as long as possible."_

p143 - _"A good architect maximizes the number of decisions not made."_

p149 - _"Conway's law: Any organisation that designs a system will produce a design whose structure is a copy of the organisation's communication structure."_

p151 - _"A good architecture makes the system easy to change, in all the ways that it must change, by leaving options open."_

## Decoupling

p152 - _"A good architecture leaves options open. The decoupling mode is one of those options."_

p155 - _"A good architecture will allow a system to be born as a monolith, deployed in a single file, but then to grow into a set of independently deployable units, and then all the way to indenpendent server and/or micro-services. Later, as things change, it should allow for reversion that progression and sliding all the way back into a monolith."_

## Duplication

p155 - _"Be careful. Resist the temptation to commit the sin of skee-jerk the elimination of duplication. Make sure the duplication is real."_

## Boundaries: drawing lines

p160 - _"Software architecture is the art of drawing lines that I call boundaries."_

p166 - _"A database is a tool that the business rules can use indirectly."_

p173 - _"Boundaries are drawn where there is an axis of change. The components on one side of the boundary change at different rates, and for different reasons, than the components on the other side of the boundary. "_

