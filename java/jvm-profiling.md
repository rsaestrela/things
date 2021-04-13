---
description: >-
  Personal notes on JVM Profiling. Mostly taken from "Optimizing Java" from
  Benjamin J. Evans, Chris Newland and James Gough
---

# JVM Profiling

### GC Logging Flags

`-Xloggc:gc.log` Controls which file to log GC events to

`-XX:+PrintGCDetails` Logs GC event details

`-XX:+PrintTenuringDistribution` Adds extra GC event detail that is vital for tooling

`-XX:+PrintGCTimeStamps` Prints the time at which GC events occurred

`-XX:+PrintGCDateStamps` Prints the wallclock at which GC events occurred

#### Log Rotation

`-XX:+UserGCLogFileRotation` Switches on log file rotation

`-XX:+NumberOfGCLogFiles=n` Sets the maximum number of log files to keep

`-XX:+GCLogFileSize=n` Sets the maximum size of each file before rotation

#### Garbage Collection Analysis Tools

Censum - Developed by jClarity this tool provides best-available GC log parsing, information extraction and automated analysis. Supports the following automatic analytics.

* Accurate allocation rates
* Premature promotion
* Aggressive or "spiky" allocation
* Idle churn
* Memory leak detection
* Heap sizing and capacity planning
* OS interference with the VM
* Poorly sized memory pools

GCViewer - Desktop tool that provides some basic GC parsing and graphing capabilities.

#### Steps before GC tuning

Using some tool like vmstat, check the following things:

* CPU utilization is close to 100%
* The vast majority of this time \(90%\) is being spent in user space
* The GC log is showing activity; indicating that GC is currently running 

### Important factors for GC Tuning

* Allocation
* Pause Sensitivity
* Throughput behavior
* Object life time

#### Allocation

Data from the young generational collection events can be used to calculate the amount the amount of data allocated and the time between the two collections. This information can then be used to calculate the average allocation rate during that time interval.

Tip: Allocation rates above 1GB/s are almost always indicative of performance problems that cannot be corrected by tuning the garbage collector. The only way of improving performance in this cases is to improve the memory efficiency of the application \(refactor\).

The first allocation strategy is to focus on the following areas:

* Avoidable object allocation and Boxing costs - auto generated code for serialization and ORM code. The most common types that cause memory problems are: `char[]`, `byte[]`, `double[]`, Map entries, `Object[]`, internal data structures \(_methodOops_ and _klassOops_\).
* Domains objects
* Large numbers of non-JDK framework objects

#### Pause Time

Analysis of pause-time tuning is to divide applications into three bread bands. These bands are based on the application needs in terms of responsiveness, expressed as the pause that the application can tolerate.

1. Can tolerate over 1s of pause
2. 1s-100ms - Can tolerate more than 200ms but less than 1s of pause
3. &lt;  100ms: Cannot tolerate 100ms of pause

| &gt;1s | 1s - 100ms | &lt; 100ms | &lt; 2GB |
| :--- | :--- | :--- | :--- |
| Parallel | Parallel | CMS | &lt; 4GB |
| Parallel | Parallel/G1 | CMS | &lt; 4GB |
| Parallel | Parallel/G1 | CMS | &lt; 10GB |
| Parallel/G1 | Parallel/G1 | CMS | &lt; 20GB |
| Parallel/G1 | G1 | CMS | &gt; 20GB |

### Tuning Parallel GC

The goals and the tradeoffs of Parallel GC are clear:

* Fully STW
* High GC throughput/computationally cheap
* No possibility of a partial collection
* Linearly growing pause time in the size of the heap

#### Flags

`-XX:NewRatio=n` - Sets ration of young gen to H

`-XX:SurvivorRatio=n` - Sets ration of survivor spaces to young gen

`-XX:NewSize=n` - Sets min size of young gen

`-XX:MaxNewSize=n` - Sets max size of young gen

`-XX:MinHeapFreeRatio` - Sets min % of heap free after GC to avoid expanding

`-XX:MaxHeapFreeRatio` - Sets max % of heap free after GC to avoid shrinking

### Tuning CMS

A low-pause collector collector like CMS should really be seen as a last resort, only to be used if the use case genuinely requires low STW pause times. Because it's a very difficult GC to tune, the engineer should resist to tune it as much as possible.

While CMS is running half cores are going GC. One very important situation to consider is the situation of the collector just before a concurrent mode failure occurs. This happens when a GC cycle just finished and another started - if the application allocates any faster, then reclamation will be unable to keep up and the result will be CMF. Throughput is reduced by 50% \(the GC is always running\), so one of the solutions would be to run on a host with more cores available. Or, reduce the number of cores assigned to GC but this is dangerous because the application will be less resilient to allocation spikes.

#### Flags

-`XX:CMSInitialOccupancyFraction=n`

`-XX:+UseCMSInitiatingOccupancyOnly`

The first flag is used to determine at what point CMS should begin collecting. Some heap headroom is required so that there's space space for objects to be promoted into from young collections that may occur while CMS is running. The default value in 75%.

The second flag disables dynamic sizing for the initiating occupancy. For some applications with bursty allocation rates, one strategy would be to increase the headroom while turning off adaptive sizing. It might  reduce concurrent mode failures, at the expense of CMS concurrent GCs occurring more often. 

#### CMF Memory Fragmentation

When CMF happens more likely memory will be fragmented. The following flag helps to see additional output:

`-XX:PrintFLSStatistics=1`



