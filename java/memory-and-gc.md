---
description: >-
  Java Memory and GC notes. Mostly taken from "Optimizing Java" from Benjamin J.
  Evans, Chris Newland and James Gough
---

# Memory and GC

### GC

Java does not use the operating system memory system to manage dynamic memory. Instead, the JVM allocates memory up front, when the JVM process starts, and manages a single, contiguous memory pool from user space.

### Stack

Memory bounded to a method context and is destroyed once the thread returns from the function, i.e., the stack objects exist within the scope of the function created in. Stack only contains references.

### Heap

Heap objects exist outside the method scope and is available till GC recollects the memory. Java stores all objects in Heap whether they are created from a method or a class. All class level variables and references are also stored in the heap so can be accessed from anywhere.

Heap holds class and arrays instances.

#### Eden

Pool from which memory is initially allocated for most objects.

#### Survivor 1/2

Pool containing objects that have been survived to GC.

#### Tenured \(Old Gen\)

#### Non Heap

It is created at the JVM startup and stores per-class structures such as runtime constant pool, field and method data, and the code for methods and constructors, as well as interned Strings. The default maximum size of non-heap memory is 64 MB. This can be changed using `–XX:MaxPermSize` VM option. This flag is legacy and applies only to Java 7 and before. In Java 8 and up the _PermGen_ has been replaced by _Metaspace_.

#### Basic Heap Sizing Flags

`-Xms<size>` Sets the minimum size reserved for the heap

`-Xmx<size>` Sets the maximum size reserved for the heap

`-XX:MaxPermSize=<size>` Sets the maximum size permitted for PermGen \(Java 7\)

`-XX:MaxMetaspaceSize=<size>` Sets the maximum size permitted for Metaspace \(Java 8\) 

### Garbage Collection

The essence of Java's garbage collection is that rather than requiring the programmer to understand the precise lifetime of every object in the system, the runtime should keep track of objects on the programmer's behalf and automatically get rid of objects that are no longer required. The automatically reclaimed memory can then by wiped and reused. This expresses the conception of Java as a blue-collar language for getting things done.

There are 2 fundamental rules of GC that all implementations are subject to:

* Algorithms must collect all garbage
* No live objects must ever be collected

And there are two primary drivers of the GC behavior of a Java application:

* Allocation Rate
* Object lifetime

#### Main concerns when choosing a Garbage Collector

* Pause time
* Throughput
* Pause frequency
* Reclamation efficiency
* Pause consistency

Examples:

* Big Data - choose throughput; pause length is not very important
* Batch Jobs - efficiency; pause length is not very important

#### Weak Generational Hypothesis

Garbage collected heaps should be structured in such a way as to allow short-lived objects to be easily and quickly collected, and ideally for a long-lived objects to be separated from short-lived objects. HotSpot uses several mechanisms to try to take advantage of the Weak Generational Hypothesis:

* Tracks the "generational count" of each object \(the number of GCs the object survived\)
* With the exception of large objects, new objects are created in the "Eden" space
* Maintains a separate area of memory called "Eden", to hold objects that are deemed to have survived and are likely to be long-lived.

#### Mark and Sweep

The introductory form of mark-and-sweep algorithm uses an allocated object list to hold a pointer to each object that has been allocated, but not yet reclaimed. The overall GC algorithm can be expressed as:

1. Loop through the allocated list, clearing the mark bit
2. Starting from the GC roots, find the live objects
3. Set a mark bit on each object reached
4. Loop though the allocated list, and for each object whose mark bit hasn't been set:
   1. Reclaim the memory in the heat and place it back on the free list
   2. Remove the object from the allocated list

The live objects are usually located depth-first, and the resulting graph of objects is called the _live object graph_. It is sometimes also called the _transitive closure of reachable objects_. This can be seen with `jmap`. Syntax: `jmap -histo <pid>`

#### Terminology: Stop-the-world \(STW\)

The GC cycle requires all application threads to be paused while garbage is collected. This prevents application code from invalidating the GC thread's view of the state of the heap. This is the usual case for more simple GC algorithms.

#### Terminology: Compacting

At the end of the collection cycle, allocated memory \(i.e., surviving objects\) is arranged in a single continuous region as single contiguous region \(usually at the start of the region\), and there is a pointer indicating the start of empty space that is available for objects to be written to. A compacting collector will avoid memory fragmentation.

#### Terminology: Evacuating

At the end of the collection cycle, the collected region is totally empty, and all live objects have been moved \(evacuated\) to another region of memory.

#### Runtime object representation

HotSpot represents Java objects at runtime via a structure called _oop_. This is a short for _ordinary object pointer,_ and s a genuine pointer in the C sense. These pointers can be placed in local variables of reference type where they point from the stack frame of the Java method into the memory that corresponds to the Java heap. There are several different data structures that comprise the family of _oops_, and the sort that represent instances of a Java class are called _instanceOops_. The memory layout of an instanceOop starts with two machine words of header present on every object. The mark word is the first of these, and is a pointer that points at instance-specific metadata. Next is the _klass_ word, which points at class-wide metadata.

From Java 8, the _klasses_ are held outside of the main part of the Java heap. _Klass_ words in these versions of Java do not require an object header, as they point outside the Java heap.

In Java, array are objects. This means that the JVM arrays are represented as _oops_ as well. This is why arrays have a third word of metadata as well as the usual mark and _klass_ words. This third word is the array's length - which also explains why array indices in Java are limited to 32 bits. 

#### GC Roots are Arenas

GC roots are anchors points for memory, essentially known pointers that originate from outside a memory pool of interest and point into it. They are external pointers as opposed to internal pointers, which originate inside the memory pool and point to another memory location within the memory pool.

The HotSpot garbage collector works in terms of areas of memory called _Arenas_. 

#### Thread Local Allocation Buffers \(Eden GC\)

Eden is a critical region to manage efficiently, as it is where most of the objects are created, and very short-lived objects will never be allocated somewhere else. For efficiency, the JVM partitions Eden into buffers and hands out individual regions of Eden for application threads to use as allocation region for new objects. The advantage of this approach is that each thread knows that it does not have to consider the possibility that other threads are allocating within that buffer. These regions are called _thread-local allocation buffers_ \(TLABs\).

The TLABs are sized dynamically on a per-thread basis, and regular objects are allocated in the TLAB, if there's space. If not, the thread requests a new TLAB from the VM and tries again. If the object will not fit an empty TLAB, the VM will next try to allocate the object directly in Eden, in an area outside of any TLAB. If this fails, the next step is to perform a young GC \(which might resize the heap\). Finally, if this fails and there's still not enough space, the last resort is to allocate the object directly in Tenured. Only large arrays are candidates to be directly allocated in Tenured. Hotspot has a couple of tuning flags that are relevant to the TLABs and the pretenuring of large objects.

`-XX:PretenureSizeThreshold=<n>`

`-XX:MinTLABSize=n`

#### Hemispheric Collection \(Survivor\)

This type of collector uses two equally sized spaces. The spaces are used as a temporary holding area for objects that are not actually long-lived. This prevents short-lived objects from cluttering up the Tenured generation and reduces the frequency of full GCs.

* When the collector is currently collecting the live hemisphere, objects are moved in a compacting fashion to the other hemisphere and the collected hemisphere is emptied for use.
* One half of the space is kept completely empty all the times.

HotSpot uses this hemispheric approach in combination with the Eden space to provide a collector for the young generation. The hemispheric part of HotSpot's young heap is referred to as the _survivor spaces_.

## Concurrent GC

Nondeterministic nature of STW pauses \(both is duration and frequency\) is the major annoyance of using GC techniques. One approach is to use a collector that is concurrent \(or partially concurrent\) in order to reduce pause time by doing some wok needed for collection while application threads are running. This reduces the power available for the actual work of the application, as well as complicating the code needed to perform collection.

### JVM Safepoints

The JVM is not actually a fully preemptive environment. The OS can still preempt \(remove a thread from the core\) at any time. This is done, for example when a thread has exhausted its time-slice or put itself into a _wait\(\)_. The runtime requires each application thread to have special execution points, called _safepoints_, where the thread's internal data-structures are in good state. At these times, the thread is able to suspend for coordinated actions.

In a STW doing GC for example, the runtime requires a stable object graph. This means that all applications threads must be paused. There is no way for a GC thread to demand that the OS enforces this demand on an application thread, so the application threads must cooperate to achieve this. There are two primary rules that govern the JVMs approach to safepointing:

1. The JVM cannot force a thread into the safepoint state
2. The JVM can prevent a thread from leaving the safepoint state

This means that JVM interpreter must contain code to yield at a barrier if safepoint is required. For JIT-compiled methods, equivalent barriers must be inserted into the generated machine code. The general case for reaching safepoints looks like:

1. The JVM sets a global "time to safepoint" flag
2. Individual application threads poll and see the flag has been set
3. They pause and wait to be woken up again

When this flag is set, all app threads must stop. In compiled code, the most common cases where the JIT compiler as inserted a poll of safepoints are exiting a compiled method and when a loop branches backward.

#### Tri-Color Marking

The tri-color marking algorithm was created by Dijkstra and Lamport's in 1978 and remains an important part of garbage collection theory. The algorithm works like this:

* GC roots are colored gray
* All other objects are colored white
* A marking thread moves to a random gray node
* If the node has any white children, the marking thread first colors them gray, then colors the node black.
* This process is repeated until there are no gray nodes left
* All the black objects have been proven to be reachable and must remain alive
* White nodes are eligible for collection and correspond to objects that are no longer reachable

#### Snapshots at the beginning \(SATB\) 

Concurrent collection frequently uses this technique. This means that the collector regards objects as live if they were reachable at the start of the collection cycle or have been allocated since. This adds some minor wrinkles to the algorithm, such as mutator threads needing to create new objects in the black state if a collection is running, and in the white state if no collection is in progress. 

The tri-color algorithm needs to be combined with a small amount of additional work to ensure that the changes introduced by the running application threads do not cause live objects to be collected. **This is because in a concurrent collector, application \(mutator\) threads are changing the object graph, while marking threads are executing the tri-color algorithm.**

### Parallel Garbage Collection

In Java 8 and earlier versions, the default collectors for the JVM are the parallel collectors. These are full STW for young and full collections, and they are optimized for throughput. After stopping all the application threads the parallel collectors use all available CPU to collect memory as quickly as possible. The available parallel collectors are:

* Parallel GC - The simplest collector for the young generation
* ParNew - A slight variation of Parallel GC that is used with the CMS collector
* ParallelOld - A parallel collector for the old \(aka Tenured\) generation

#### GC Algorithms evolution

| Type | Before | Then |
| :--- | :--- | :--- |
| Young | Parallel GC | ParNew |
| Old | ParallelOld | CMS |

#### Young Parallel Collections \(Parallel GC\)

Occurs when a thread tries to allocated an object into Eden but doesn't have enough space in it's TLAB, and the JVM can't allocate a fresh TLAB for the thread. When this happens all the application threads - because if one thread is unable to allocate, then very soon another thread will be unable.

1. Application threads are stopped
2. HotSpot looks at the young generation and identified all the objects that are not garbage. This will utilize GC roots \(and card table to identify GC roots from old generation\) as starting points for a parallel marking scan.
3. Starts evacuating all the surviving objects into the currently empty survivor space.
4. Eden and evacuated survivor are marked as empty, reusable space
5. Application threads are started

This approach takes full advantage of _Week Generational Hypothesis_ by touching only live objects. It uses all cores as much as possible to shorten the STW pause time.

#### Old Parallel Collections

In Java 8 is the default collector for the old generation. Similar to Parallel GC, but with the main difference that is a hemispheric evacuating collector, whereas ParallelOld is a compacting collector with only a single continuous memory space. Old generation has not other space to be evacuated to. Thus, the collector can potentially be very efficient in its use of memory, and will not suffer from memory fragmentation. This results in a very efficient memory layout at the cost of using a potentially large amount of CPU during full GC cycles. By default the size of old generation is 7 times the young generation. This fact alone will make the expected STW length a full collection much longer than for a young collection. Another fact is that the marking time is proportional to the number of live objects in a region. Old objects may be long-lived, so a potentially larger number of old objects may survive a full collection. The STW time will scale roughly linearly with the size of the heap. As heap sizes continue to increase ParallelOld starts to scale badly in terms of a pause time.

### CMS

The Concurrent Mark and Sweep collector is designed to be an extremely low-pause collector for the Tenured space only. CMS does as much work as possible while application threads are still running, so as to minimize pause time. It uses tri-color marking, and for this reason has a lot more set of phases, usually referred as follows:

1. Initial Mark - provides a stable set of starting points for GC that are within the region; these are known as internal pointers and provide a equivalent set to the GC roots for the purpose of collection cycle. The advantage of this approach is that it allows the marking phase to focus on a single GC pool without having to consider other memory regions.
2. Concurrent Mark - Runs the tri-color marking algorithm on the heap, keep track of any changes that might might later require fix.
3. Concurrent Preclean - Reduce the length of the STW Remark phase as much as possible.
4. Remark Phase - Uses card tables to fix up the marking that might have been affected by mutator threads during the Concurrent Mark phase.
5. Concurrent Sweep
6. Concurrent Reset

For the 2 initial phases application threads must be stopped. The overall effect should be to replace a single STW pause with two STW pauses, which are usually very short. As a result application threads don't stop for as long, but application throughput is reduced. Uses more CPU and memory and it doesn't compact heap, so Tenured can become fragmented.

#### CMS coordination with Young GC

What happens if Eden fills up while CMS is running? Because application threads cannot continue, they pause and a STW young runs wile GC is running. This young GC run will usually take longer than in the case of parallel collectors, because it only has half the cores available for young generation \(the other half of the cores are running CMS\). At the end of this young collection, these objects need to be moved into Tenured while CMS is still running, which requires some coordination between the two collectors. **This is why CMS requires a slightly different young collector.**

#### Fallback to ParallelOld

It might happen a Concurrent Mode Failure \(CMF\) - there is a very high premature promotion in the young collection. In this case the JVM has no choice  but to fallback to a collection using ParallelOld. Effectively, the allocation pressure was so high that CMS doesn't have time to finish processing the old generation before all the "headroom" space to accommodate newly promoted objects. In order to avoid this issue, CMS starts a collection \(by default\) at 75%.

Another common issue that leads to CMF is the heap fragmentation. CMS does not support compact Tenured and this means that after a collection, the free space n Tenured is not a single contiguous block, and objects that are promoted have to be filed into the gaps between existing threads. In this case, there's a fallback on ParallelGC.

#### Flags for CMS

`-XX:+UseConcMarkSeepGC`- On modern versions of Hotspot, this will activate `ParNewGC` for the young generation.

#### Avoiding Premature Promotion

Allocation rates also affect the number of object promoted to Tenured. Higher allocation rates will result in young GCs that are closer together. If the collections become too frequent, then sort-lived object may not have had the time to die and will be erroneously promoted to Tenured. To guard against this, the JVM will resize survivor spaces dynamically to accommodate greater amounts of surviving data without promoting it to Tenured. One way of tuning this is to control how many GCs an object must survive to be promoted to Tenured.

`-XX:MaxTenuringThreshold=n` - default is 5, but it can be changed up to 15

### G1

Is a very different style of collector than either the parallel collectors or CMS. It became stable and production-ready with the release of Java 8u40. Is the default collector in Java 9. The idea behind G1 is to be a replacement low-pause collector that was:

* Much easier to tune than CMS
* Less susceptible to premature promotion
* Better scaling behavior \(especially pause time\) on big heaps
* Able to eliminate \(or avoid the need to fall back to\) full STW collections

The concepts of TLAB allocation, evacuation to survivor space, and promoting to Tenured are broadly similar to the other HotSpot GCs, although unlike the parallel or CMS collectors, G1 does not have a dedicated, contiguous memory spaces per generation. In addition, it does not follow the hemispherical heap.

The G1 heap is based upon the concept of regions. These are areas which are by default 1 MB in size. The use of regions allows for noncontiguous generations and makes makes possible to have a collector that **does not need to collect all garbage on each run**.

G1 allows regions of 1, 2, 4, 6, 18, 16, 32 and 64 MB. By default it expects to have between 2048 and 4095 regions in the heap and it will adjust the region size to achieve this. 

`Region size = heap size / 2048`

`Number or regions = heap size / region size`

#### Algorithm design

The high-level picture of G1 is that:

* Uses a concurrent marking phase
* Is an evacuating collector
* Provides "statistical compaction"

While warming up, the collector keeps track of the statistics of how many typical regions can be collected per GC duty cycle. G1 still has a concept of a young generation made up of Eden and survivor regions, but of course, this regions are not contiguous. The size of the young generation is adaptive and is based on the overall pause time goal.

The G1 collector has a feature to help with region tracking. The _Remembered Sets_ \(usually called _RSets_\)   as per-region entries that track outside references that point into a heap region. This means that instead of tracing though the entire heap for references that point into a region, G1 just needs to examine the RSets an than scan those regions for references. Both RSets and card tables are approaches that can help with a GC problem called _floating garbage._

#### Floating Garbage

This is an issue that is caused when objects that are otherwise dead are kept alive by references from dead objects outside the current allocation set. That is, on a global mark they can be seen to be dead, but a more limited local marking may incorrectly report them as alive, depending on the root set used.

#### G1 Phases

G1 has these collection phases:

1. Initial Mark \(STW\)
2. Concurrent Root Scan - scans survivor regions of the Initial Mark for references to the old generation. This phase must complete before the next young GC.
3. Concurrent Mark
4. Remark \(STW\) - The marking cycle is completed. This phase also performs reference processing and handles cleanup relating to implement the SATB approach.
5. Cleanup \(STW\) - Comprises accounting and RSet "scrubbling". The accounting task identifies regions that are now completely free and ready to be reused.

#### Flags for G1

`+XX:UseG1GC` - enables G1 in Java 8 and before

`-XX:MaxGCPauseMillis` - soft goal to be achieved

-`XX:G1HeapRegionSize` - size of memory regions, always power of 2

### Shenandoah

Effort by Red Hat to implement a low pause GC algorithm shipped with OpenJDK. It's approach is to perform concurrent compaction, with the following steps:

1. Initial Mark \(STW\)
2. Concurrent Marking - Traces through the heap and marks any live objects. If an object reference points to an oop that has a forwarding pointer, the the reference is updated to point directly at the new oop location
3. Final Marking \(STW\) - Stops the world to re-scan the root set, then copy and update roots to point to the evacuated copies. 
4. Concurrent Compaction

Theses phases are similar to G1, although there is a fundamental difference. This collector uses a technique called _Brooks Pointer_. It uses an additional word of memory per object to indicate whether the object has be relocated during a previous phase of garbage collection and to give the location of the new version of the object's contents. This mechanism is sometimes called a forwarding pointer approach. If the object has not been relocated, then the Brooks pointer simply points at the next word of memory.

Application threads can load references to objects that might have been relocated, and the Brooks pointer is used to keep track of their new location.

Shenandoah can be enabled with the flag `-XX:+UseShenandoahGC`

#### Concurrent Compaction

The GC threads, perform evacuation as follows:

1. Copy the object into a TLAB
2. Use a CAS operation to update the Brooks pointer to a point at the speculative copy.
3. If it succeeds, the the compaction thread won the race, and all future accesses to this version of the object will be via the Brooks pointer.
4. If it fails, the compaction thread lost. It undoes the speculative copy, and follows the Brooks pointer left by the winning thread. 

### C4 \(Zulu Zing\)

The Zing VM contains novel software technologies including the C4 \(Continuously Concurrent Compacting Collector\) garbage collector and some novel JIT technology, including _ReadyNow_ and a compiler called _Falcon_. 

Zing doesn't use a Brooks pointer. Zing's object header is a single 64 bit word \(rather than the two-word header that Hotspot uses\). The single header work contains a kid, which is a numeric klass ID \(25 bits\) rather than a klass pointer. The bottom 32 bits of the header are used for lock information. This includes the lock state, in the case of a thin lock this will be the owning thread ID for thin locks.

#### Self Healing Barrier

To avoid load references to objects that might have been relocated, the Brooks pointer \(Shenandoah\) is used to keep track of this new location. LVB in Zing avoids this pattern by providing a solution where every loaded reference is pointing directly at the current location of the object as soon as the load has completed. This means that each reference is updated at most once, and if the reference is never used again, no work is done to keep it up to date. As well as the header, Zing's object references use some of the bits of the reference to indicate metadata about the GC state of the object. This saves some space by using bits of the reference itself, rather than bit of the single header word.

```text
struct Reference {
unsigned inPageVA;
unsigned PageNumber;
unsigned NMT; // Not Marked Through
unsigned SpaceID;
unsigned unused;
}
```

The NMT metadata bit is used to indicate whether the object has already been marked in the current collection cycle. At the end of the collection C4 flips the state bit, so now all surviving objects are ready for the next cycle.

The overall phases of the GC cycle in C4 are:

1. Mark
2. Relocate
3. Remap

#### Hand-over-hand compaction

C4 uses a technique called _hand-over-hand_ compaction to provide continuous compaction. This relies upon a feature of virtual memory system - the disconnect between physical and virtual addresses. In normal operation, the virtual memory sub-system maintains a mapping between a virtual page in the process address page and an underlying physical page. \(Unlike Hotspot Zing does make use of calls into the kernel as part of GC cycle\).

Objects are relocated by being copied to a different page, which naturally corresponds to a different physical address. Once all objects from a page have been copied, the physical page can be freed and returned to the operating system.

### Epsilon

Epsilon is not a legacy collector, however it is included here because it must not be used in production under any circumstances. It is an experimental collector designed for testing purposes only. It is a zero-effort collector. This means it makes no effort to actually collect any garbage. Every byte of memory allocated will be a memory leak. Memory cannot be reclaimed and will eventually cause the JVM to run out of memory. It should be used for the following purposes:

* Performance testing and microbenchmarks. 
* Regression testing.
* Testing low/zero-allocation Java application or library code

