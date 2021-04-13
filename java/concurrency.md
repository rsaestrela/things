# Concurrency

Java supports multi-threading programming since the first version. The Java environment is inherently multi-threaded, as is the JVM. This produces additional, irreducible complexity in the behavior of Java programs. In most JVM implementations, each Java application thread corresponds precisely to a dedicated OS thread. The alternative, using a shared pool of threads to execute all Java application threads \(known as _green threads_\) proved not to provide acceptable performance profile and added needless complexity. Java current approach to multi-threading has these design principles:

* All threads in a Java process share a single, common garbage-collected heap
* Any object created by one thread can be accessed by another thread that has a reference to the object
*   Objects are mutable by default: that is, values held in object fields can be changed unless the programmer explicitly uses the `final` keyword to mark them as immutable

### Thread States

* NEW - Thread that hasn't started yet.
* RUNNABLE - Thread  being executed.
* BLOCKED - Thread blocked waiting for a lock release.
* WAITING - Thread waiting using `Object.wait` , `Thread.join` or any other mechanism that makes the thread stay in a waiting state. 
* TIMED\_WAITING - Thread is sleeping for example with `Thread.sleep`.
* TERMINATED - Thread ended execution.

