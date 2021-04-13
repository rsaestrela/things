---
description: >-
  Personal notes on JVM code execution. Mostly taken from "Optimizing Java" from
  Benjamin J. Evans, Chris Newland and James Gough
---

# Code Execution JVM

JVM executes interpreted bytecode, instead of machine code directly which can be considered slow. In order to be increase performance, Java environments  solve this problem by providing dynamic compilation, or JIT \(just-in-time\) compilation.

#### JIT

JIT is a mechanism but which the JVM monitors witch methods are being executed in order to determine whether individual methods are eligible to directly executable code. Just in time compilation is a technique whereby programs are converted into highly optimized machine code at runtime. Hotspot and most other production-grade JVMs rely heavily on this approach.

The technique gathers information about the runtime and builds a profile that can be used to determine which parts of the program are used frequently and would benefit most from optimization.

#### Bytecode Interpretation

The JVM interpreter operates as a stack machine. Unlike physical CPU's, there are no registers that are used as immediate holding areas for computation. Instead, all values are put in a evaluation stack, and the stack machine instructions work by transforming the values\(s\) at the top of the stack.

Areas to hold data:

* Evaluation stack, which is local to a particular method
* Local variables to temporarily store results
* Heap - is shared between methods and between threads

### JVM Bytecode

Each machine operation \(opcode\) is represented by 1 byte. Opcodes run from 0 to 255 \(Java 10 only uses around 200\). 

#### Load and Store

| Family name | Arguments | Description |
| :--- | :--- | :--- |
| load | \(i1\) | Loads value from local variable i1 onto the stack |
| store | \(i1\) | Stores top of stack into local variable i1 |
| ldc | c1 | Loads value from CP\#c1 |
| const |  | Loads simple constant value onto the stack |
| pop |  | Discards value on top of stack |
| dup |  | Duplicates value on top of stack |
| getfield | c1 | Loads value from field CP\#c1 in object on top of stack onto the stack |
| putfield | c1 |  |
| getstatic | c1 |  |
| putstatic | c1 |  |

#### Arithmetic

| Family name | Description |
| :--- | :--- |
| add | Adds two values from top of stack |
| sub | Subtracts two values from top of stack |
| div | Divides two values from top of stack |
| mul | Multiplies two values from top of stack |
| \(cast\) | Casts value at top of stack to a different primitive type |
| neg | Negates value at top of stack |
| rem | Computes remainder \(integer division\) of top two values on stack |

#### Flow control

| Family name | Arguments | Description |
| :--- | :--- | :--- |
| if | \(i1\) | Branch to the location indicated by the argument |
| goto | i1 | Unconditional branch to the supplied offset |
| tableswitch |  |  |
| lookupswitch |  |  |

#### Method invocation

| Opcode | Arguments | Description |
| :--- | :--- | :--- |
| invokevirtual | c1 | Invokes the method found at CP\#c1 via virtual dispatch |
| invokespecial | c1 | Invokes the method found at CP\#c1 via special dispatch |
| invokeinterface | c1, count, 0 | Invokes the interface method found at CP\#c1 using interface lookup |
| invokestatic | c1 | Invokes the static method found at CP\#c1 |
| invokedynamic | c1, 0, 0 | Dynamically looks up which method to invoke and executes it |

Platform Opcodes

| Opcode | Arguments | Description |
| :--- | :--- | :--- |
| new | c1 | Allocates space for an object of type found at CP\#c1 |
| newarray | prim | Allocates space for a primitive array of type prim |
| anewarray | c1 | Allocates space for an object array of type found at CP\#c1 |
| arraylength |  | Replaces array on top of stack with its length |
| monitorenter |  | Locks monitor of object on top of stack |
| monitorexit |  | Unlocks monitor of object on top of stack |

