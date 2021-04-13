# Changelog

### Java 8

Release type: LTS - March 2014 - July 2019/July 2022

Features

* Language-level support for lambda expressions \(closures\) and Default methods on interfaces \(JSR 335, JEP 126\)
* Project Nashorn, a JavaScript runtime which allows developers to embed JavaScript code within applications

  \(JSR 223 and JEP 174\)

* Annotation on Java types \(JSR 308 and JEP 104\)
* Unsigned integer arithmetic
* Repeating annotations \(JSR 337, JEP 120\)
* Date and time API \(JSR 310 and JEP 150\)
* Statically-linked JNI libraries \(JEP 178\)
* Launch JavaFX applications \(direct launching of JavaFX application JARs\) \(JEP 153\)
* Remove the permanent generation \(JEP 122\)

### Java 9

Release type: non-LTS - September 2017 - March 2018

* Modularization of the JDK under Project Jigsaw \(Java Platform Module System\) \(JSR 376\)
* JShell: The Java Shell \(a Java REPL\) \(JEP 222\)
* Ahead-of-time compilation \(JEP 295\)
* XML catalogs \(JEP 268\)
* Java implementation of Reactive Streams, including a new Flow class that included the interfaces previously provided by Reactive Streams \(JEP 266\)
* Variable handles: define a standard means to invoke the equivalents of various `java.util.concurrent.atomic` and `sun.misc.Unsafe` operations \(JEP 193\)
* jlink: The Java Linker: create a tool that can assemble and optimize a set of modules and their dependencies into a custom run-time image. It effectively allows to produce a fully usable executable including the JVM to run it \(JEP 282\)
* JavaDB was removed from JDK
* HiDPI graphics: automatic scaling and sizing \(JEP 263\)
* Compact Strings \(JEP 254\)
* Milling Project Coin \(JEP 213\)
  * Allow `@SafeVarargs` on private instance methods
  * Allow effectively-final variables to be used as resources in the try-with-resources statement
  * Allow diamond with anonymous classes if the argument type of the inferred type is denotable
  * Complete the removal, begun in Java SE 8, of underscore from the set of legal identifier names
  * Support for private methods in interfaces \(proposal\)

### Java 10

Release type: non-LTS - March 2018 - September 2018

* Local-variable type inference \(JEP 286\)
* Experimental Java-based JIT compiler. This is the integration of the Graal dynamic compiler for the Linux x64 platform \(JEP 317\)
* Application class-data sharing. This allows application classes to be placed in the shared archive to reduce startup and footprint for Java applications \(JEP 310\)
* Time-based release versioning \(JEP 322\)
* Parallel full GC for G1 \(JEP 307\)
* Garbage-collector interface \(JEP 304\)
* Additional Unicode language-tag extensions \(JEP 314\)
* Root certificates \(JEP 319\)
* Thread-local handshakes \(JEP 312\)
* Heap allocation on alternative memory devices \(JEP 316\)
* Remove the native-header generation tool \(JEP 313\)
* Consolidate the JDK forest into a single repository \(JEP 296\)

### Java 11

Release type: LTS - September 2018 - September 2023

* Dynamic class-file constants \(JEP 309\)
* Epsilon: a no-op garbage collector \(experimental\) \(JEP 318\)
* Local-variable syntax for lambda parameters \(JEP 323\)
* Low-overhead heap profiling \(JEP 331\)
* HTTP client \(standard\) \(JEP 321\)
* Transport Layer Security \(TLS\) 1.3 \(JEP 332\)
* Flight recorder \(JEP 328\)
* ZGC: a scalable low-latency garbage collector \(Experimental\) \(JEP 333\)
* JavaFX, Java EE and CORBA modules have been removed from JDK
* Deprecated the Nashorn JavaScript engine \(JEP 335\)
* Unicode 10.0.0 support \(while current version is Unicode 11.0.0, it's only in Java 12\)
* A number of features from previous releases were dropped; in particular, Java applets and Java Web Start are no longer available.

### Java 12

Release type: non-LTS - March 2019 - September 2019

* Shenandoah: A Low-Pause-Time Garbage Collector \(Experimental\) \(JEP 189\)
* Microbenchmark Suite \(JEP 230\)
* Switch Expressions \(Preview\) \(JEP 325\)
* JVM Constants API \(JEP 334\)
* One AArch64 Port, Not Two \(JEP 340\)
* Default CDS Archives \(JEP 341\)
* Abortable Mixed Collections for G1 \(JEP 344\)
* Promptly Return Unused Committed Memory from G1 \(JEP 346\)

### Java 13

Release type: non-LTS - September 2019 - March 2020

* Dynamic CDS Archives \(JEP-350\)
* ZGC: Uncommit Unused Memory \(JEP-351\)
* Reimplement the Legacy Socket API \(JEP-353\)
* Switch Expressions \(Preview\) \(JEP-354\)
* Text Blocks \(Preview\) \(JEP-355\)

### Java 14

Release type: non-LTS - March 2020 - September 2020

* Pattern Matching for instanceof \(Preview\) \(JEP-305\)
* Packaging Tool \(Incubator\) \(JEP-343\)
* NUMA-Aware Memory Allocation for G1 \(JEP-345\)
* JFR Event Streaming \(JEP-349\)
* Non-Volatile Mapped Byte Buffers \(JEP-352\)
* Helpful NullPointerExceptions \(JEP-358\)
* Records \(Preview\) \(JEP-359\)
* Switch Expressions \(Standard\) \(JEP-361\)
* Deprecate the Solaris and SPARC Ports \(JEP-362\)
* Remove the Concurrent Mark Sweep \(CMS\) Garbage Collector \(JEP-363\)
* ZGC on macOS \(JEP-364\)
* ZGC on Windows \(JEP-365\)
* Deprecate the ParallelScavenge + SerialOld GC Combination \(JEP-366:\)
* Remove the Pack200 Tools and API \(JEP-367\)
* Text Blocks \(Second Preview\) \(JEP-368\)
* Foreign-Memory Access API \(Incubator\) \(JEP-370\)

### Java 15

Release type: non-LTS - September 2020 - March 2021

### Java 16

### Java 17

