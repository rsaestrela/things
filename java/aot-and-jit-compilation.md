---
description: >-
  Personal notes on AOT and JIT Compilation. Mostly taken from "Optimizing Java"
  from Benjamin J. Evans, Chris Newland and James Gough
---

# AOT and JIT Compilation

### JIT Compilation

Just in time compilation is a general technique whereby programs are converted into highly optimized machine code at runtime. Hotspot and most other mainstream production-grade JVMs rely heavily on this approach. The technique gathers information about the program at runtime and builds a profile that can be used to determine witch parts of the program are used frequently and would benefit most from optimization.

The JIT subsystem shares VM resources with the running program, so the cost of this profiling and any optimizations performed needs to be balanced against the expected performance gains. It consumes CPU cycles and memory that could otherwise be dedicated to execution. For this reason JIT compilation is performed sparingly and the VM collects statistics about the programs to known where best to optimize.

The design of modern Java compilers is intended to produce dumb bytecode. It performs only very limited optimizations, and instead provides a representation of the program that is very easy for the JIT compiler to understand.

Hotspot does not attempt to save any profiling information and instead discards it when the VM shuts down, so the profile must be built again from scratch each time.

#### Pointer Swizzling

One of the most important groups of threads are the ones that comprise the JIT compilation system. This includes profiling threads that detect when a method is eligible for compilation and the compiler threads themselves that generate the actual machine code. When compilation is indicated, the method is placed on a compiler thread, which compiles in the background. When the optimized code is available, the entry in the _vtable_ of the relevant klass is updated to point at the new compiled code. All the new invocations will get the JIT compiled form.

#### Logging JIT compilation

`java -XX:+PrintCompilation`

`java -XX:+LogCompilation`

