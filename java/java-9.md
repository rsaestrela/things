# Java 9

Release type: non-LTS - September 2017 - March 2018

## Features

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

