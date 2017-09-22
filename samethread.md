Same-threading is a concurrency model where a single-threaded systems are scaled out to N single-threaded systems. The result is N single-threaded systems running in parallel.

A same-threaded system is not a pure single-threaded system, because it contains of multiple threads. But - each of the threads run like a single-threaded system.  
==同线程模型是一个并发模型，由一个单线程系统扩展为多个单线程系统。最终是N个单线程系统并行运行。==  
==同线程系统不是纯粹的单线程系统，因为它包含了多个线程。但是每一个线程都像单线程系统一样运行。==

### Why Single-threaded Systems?
You might be wondering why anyone would design single-threaded system today. Single-threaded systems have gained popularity because their concurrency models are much simpler than multi-threaded systems. Single-threaded systems do not share any data with other threads. This enables single thread to use non-concurrent data structures, and utilize the CPU and CPU caches better.  
### 为什么选择但线程系统
==你也许回想，在今天为什么还有人设计单线程系统。单线程系统过得流行时因为它的并发模型比多线程系统的更加简单。单线程系统之间不会共享任何数据。这能够让单线程使用非并发数据结构，更好地利用CPU和CPU缓存。==

Unfortunately, single-threaded systems do not fully utilize modern CPUs. A modern CPU often comes with 2, 4 or more cores. Each core functions as an individual CPU. A single-threaded system can only utilize one of the cores, as illustrated here:  
==不幸的是，单线程系统没有真正地利用现代的CPU。现在的CPU通常有2,4个甚至更过的核心。每个核心都像独立的CPU一样的功能。单线程系统仅仅利用了一个核心，如下图：==

![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-0.png)  

### Same-threading, Single-threading Scaled Out
In order to utilize all the cores in the CPU, a single-threaded system can be scaled out to utilize the whole computer.
### 同线程，单线程分发
==为了能够利用所有的CPU内核，单线程系统可以被分发以使用整个计算机。==

##### One Thread Per CPU
#### 每个CPU一个线程

Same-threaded systems usually has 1 thread running per CPU in the computer. If a computer contains 4 CPUs, or a CPU with 4 cores, then it would be normal to run 4 instances of the same-threaded system (4 single-threaded systems). The illustration below shows this principle:  
==同线程系统中通常每个CPU运行着一个线程，在计算机中。如果一台计算机拥有4个CPU，或者一个4核心的CPU，可以运行4个单线程系统实例。如下图所示：==
![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-0-1.png)  

### No Shared State
A same-threaded system looks similar to a multi-threaded system, since a same-threaded system has multiple threads running inside it. But there is a subtle difference.

The difference between a same-threaded and a multi-threaded system is that the threads in a same-threaded system do not share state. There is no shared memory which the threads access concurrently. No concurrent data structures etc. via which the threads share data. This difference is illustrated here:  
### 无贡献状态
==同线程系统与多线程系统看着很相似，因为单线程系统中有多个线程在运行。但是它们之又存在了细微的差别。==  
==同线程系统与多线程系统之间的不同是在同线程西系统中的线程之间不贡献状态。没有线程并发访问共享内存，没有并发数据结构等在线程之间共享数据。这个不同之处可用下图表示：==
![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-4.png)  

The lack of shared state is what makes each thread behave as it if was a single-threaded system. However, since a same-threaded system can contain more than a single thread, so it is not really a "single-threaded system". In lack of a better name, I found it more precise to call such a system a same-threaded system, rather than a "multi-threaded system with a single-threaded design". Same-threaded is easier to say, and easier to understand.

Same-threaded basically means that data processing stays within the same thread, and that no threads in a same-threaded system share data concurrently.  
==共享数据的缺失让每条线程表现地像单线程系统。然而，同线程系统中可以包含不止一条线程。所以它并不是一个真正的单线程系统。因为我找不到一个更好的名字，所以我就称之为同线程系统而不是“单线程的多线程系统设计“。同线程更容易表达，更容易理解。==

### Load Distribution
Obviously, a same-threaded system needs to share the work load between the single-threaded instances running. If not, only a single instance will get any work, and the system would in effect be single-threaded.

Exactly how you distribute the load over the different instances depend on the design of your system. I will cover a few in the following sections.  
### 负载分布
==很显然，同线程系统需要在单线程实例中共享工作负荷。如果不贡献的话，单线程就可以完成任何个工作了，系统也就成了单线程的了。==  
==确切地讲在不同线程实例如何分布符合取决于你系统的设计。我将在下部一部分介绍一些内容。==

##### Single-threaded Microservices

If your system consists of multiple microservices, each microservice can run in single-threaded mode. When you deploy multiple single-threaded microservices to the same machine, each microservice can run a single thread on a sigle CPU.

Microservices do not share any data by nature, so microservices is a good use case for a same-threaded system.  
##### 单线程微服务
==如果你的系统由多个微服务构成，每个为符合以单线程模式运行。当你部署多个单线程微服务到一台机器上时，每个微服务运行一单线程在单个CPU上。==  
==微服务不贡献任何数据，所以微服务是很好的同线程的使用案例。==

##### Services With Sharded Data

If your system does actually need to share data, or at least a database, you may be able to shard the database. Sharding means that the data is divided among multiple databases. The data is typically divided so that all data related to each other is located together in the same database. For instance, all data belonging to some "owner" entity will be inserted into the same database. Sharding is out of the scope of this tutorial, though, so you will have to search for tutorials about that topic.  
##### 共享数据的服务
==如果你的系统却是需要贡献数据，或者至少一个数据库，那么你也许可以贡献数据库。分片意味着数据被分发在多个数据库中。数据被分割以便互有关联的数据放置在同一个数据库中。例如，所有属于“wner”实体的数据将被插入到同一个数据库中。分片不在本系列的范围，因此，您不得不搜索该文的主题。==


### Thread Communication
If the threads in a same-threaded need to communicate, they do so by message passing. A thread that wants to send a message to thread B can do so by generating a message (a byte sequence). Thread B can then copy that message (byte sequence) and read it. By copying the message thread B makes sure that thread A cannot modify the message while thread B reads it. Once it is copied it is immutable for thread A.

Thread communication via messaging is illustrated here:   
### 线程通信
==如果同线程系统中的线程需相互通信，它们可以通过消息发送来实现该目的。A线程想发送一条消息给线程B，那么A线可以生成一条消息（一个字节序列）。线程B可以复制词条消息（字节序列）并读取。通过复制消息线程B要确保在线程B复制时，线程A不能再修改消息。一消息被复制，对于线程A来讲消息就不可变了。==  
==通过消息实现消息通信的示意图如下：==

![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-5.png)  

The thread communication can take place via queues, pipes, unix sockets, TCP sockets etc. Whatever fits your system.  
==线程通信可以使用队列，通道，unix套接字，TCP连接等，任何适合你系统嗯的方式。==

### Simpler Concurrency Model
Each system running in its own thread in same-threaded system can be implemented as if it was single-threaded. This means that the internal concurrency model becomes much simpler than if the threads shared state. You do not have to worry about concurrent data structures and all the concurrency problems such data structures can result in.
### 简单并发模型
==在同线程系统中运行的每个系统都可以实现，好像它是单线程的。这意味着内部并发模型比线程共享状态要简单得多。您不必担心并发数据结构和所有这些数据结构可能导致的并发问题。==

### Illustrations
Here are illustrations of a single-threaded, multi-threaded and same-threaded system, so you can easier get an overview of the difference between them.  
==这里展示了单线程，多线程和同线程系。你可以很容易地看出它们之间的不同==

The first illustration shows a single-threaded system.  
==第一个图示展示了单线程系统==

![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-1.png)  

The second illustration shows a multi-threaded system where the threads share data.  
==第二个图示展示了线程间贡献数据的多线程系统==
![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-2.png)

The third illustration shows a same-threaded system with 2 threads with separate data, communicating by passing messages to each other.  
==第三个图示展示了两个带有不同数据的线程的同线程系统，通过消息彼此通信==

![image](http://tutorials.jenkov.com/images/java-concurrency/same-threading-3.png)
