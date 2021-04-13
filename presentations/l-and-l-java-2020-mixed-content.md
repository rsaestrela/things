# L&L - Java 2020 - Mixed Content

### Release Cadence

* Oracle announced in 2017 \(verify\) the new release cadence, with a new version appearing every 6 months, the intent was to increase the pace of innovation within the platform. The 6 month cycle is allowing the cutting edge of Java to move faster.
* Java Core Value: backward compatibility. New features must not cause problems in existing code and must interact cleanly with the established language semantics and syntax. The maturity and age of Java means that there are many touchpoints that any new feature may have to consider.
* Retargeting features - if a feature is not ready for a particular release can be shipped 6 months later.
* Incubating features - new alpha features are shipped in a preliminary state. They are delivered as a module in the namespace that makes clear that the API is not final and may change.
* Preview features - allow developers to try a proposed feature in detail and provide feedback before the feature is finalized.

### Upgrade Challenges

* Companies are reluctant migrating to the latest version of Java. Now, it's more difficult because Java is release every 6 months.
* Migrating from Java 8 to Java 12 can be challenging. 

#### Advantages of migration

* Local variable type inference - syntactic sugar that helps to reduce boilerplate code.
* Convenience factory methods for collections - makes it significantly easier to create collections like lists, maps, sets and unmodifiable collections also
* Collecting to unmodifiable is possible with new collector
* `Predicate::not` - provides easy way to negate predicate lambdas or method references
* New methods on `Optional` - gives more options for coding using an Optional instead of the traditional `if else` conditions
* JShell - The new REPL that allows to run individual lines of code, or even scripts in Java
* HttpClient - A new built-in client that supports HTTP 1.1 and 2.0 with both synchronous and asynchronous programming models.
* JLink - Tool made possible by the Java Module System which lets us package and deploy only the sections of the JDK we really need. Helps to reduce the size of the artifacts.
* Performance - Improvements in startup time, reduction in memory usage, and the use of CPU instructions resulting in fewer CPU cycles.
* Garbage Collection - G1 was made the default GC algorithm in Java 9. Three new collectors were introduced and are in experiemental mode:
  * Java 11 - Epsilon and ZGC
  * Java 12 - Shenandoah

#### Disadvantages of migration

* Oracle started releasing 2 different builds, each with a different license. A commercial build and an OpenJDK build and changed their update and support models. This gives more choice over the JDK used.
* Java 9 was a big deal - Some changes regarding encapsulation of API's that used to be available in the JDK are no longer accessible. This, along with the removal of `tools.jar` and `rt.jar` caused some trouble for library and framework developers.
  * Classes in packages started with `sun.misc.*` were hidden from use.

#### Which version

* Every 3 years there is a Long Term Support \(LTS\) release. Java 8 was an LTS, although Oracle stopped providing updates for Java 8 for free \(commercial\) use in January this year. However updates are still supported via OpenJDK, for free.
* Java 11 is the current LTS
* The choice now is: be prepared to upgrade every 6 months, or wait until the next LTS version.

#### Depracation

Since Java 8, there a lot more compilation warnings. Deprecation in the form of compilation warnings is way more common in recent versions \(Java 10 and 11\).

There is a tool called `jdeps`  that's part of JDK that we can run with a flag `-jdkinternals` to check if a set of classes is using something it shouldn't. This tool not only identifies the classes that use the internal APIs, but provides a suggestion for what to use instead.

APIs that are not core to the JDK were removed. This includes Java EE, Corba modules and JavaFX.

Make sure all the dependencies are up to date.

In Java 11, the JRE is no longer available as a stand-alone or a subdirectory of the JDK.

#### Build Tools

* Java 11 - Gradle 5.0
* Maven 3.5.0 and Maven compiler plugin is at least of version 3.8

#### Running the application in a new JDK

It should be possible to run an application using a modern JRE compiled with an older JDK without any code changes, i.e., it's not necessary to build again. That's the reason why developers work so hard in maintaining backward compatibility. 

Remember that using Maven compiler it's even possible to compile to a previous Java, using a more recent JDK.

### Local Variable Type Inference

### Scripting

### Graal VM

