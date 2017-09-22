Concurrent systems can be implemented using different concurrency models. A concurrency model specifies how threads in the the system collaborate to complete the jobs they are are given. Different concurrency models split the jobs in different ways, and the threads may communicate and collaborate in different ways. This concurrency model tutorial will dive a bit deeper into the most popular concurrency models in use at the time of writing (2015).  
==并发系统可以实现不同的并发模型。一个线程模型指定了在系统中线程如何相互合作完成各自的工作。不同的并发模型以不同的方式分割工作任务，以不同的方式相互配合。该并发系列文章将深入讲解现在（2015）最流行的并发模型。==
### Concurrency Models and Distributed System Similarities
The concurrency models described in this text are similar to different architectures used in distributed systems. In a concurrent system different threads communicate with each other. In a distributed system different processes communicate with each other (possibly on different computers). Threads and processes are quite similar to each other in nature. That is why the different concurrency models often look similar to different distributed system architectures.  
==本文中描述的并发模型与分布式系统中的不同的架构相似。在并发系统中不同线程之间相互沟通。在分布式系统中不同的进程相互交流（可能在不同的计算机上）。线程与进程本质上是十分相似的。这就是为什么不同的并发模型与分布式系统看起来看相似。==

Of course distributed systems have the extra challenge that the network may fail, or a remote computer or process is down etc. But a concurrent system running on a big server may experience similar problems if a CPU fails, a network card fails, a disk fails etc. The probability of failure may be lower, but it can theoretically still happen.
==当人分布式系统有自己特定的挑战，如网络失败或者远程计算机或者进程已经关闭等等。但是运行在大型服务上的并发系统也有相似的问题，假如CPU坏了，网卡坏了或者磁盘毁掉了等等。尽管这些可能写诗很低的，但理论上仍然会发生。==  

Because concurrency models are similar to distributed system architectures, they can often borrow ideas from each other. For instance, models for distributing work among workers (threads) are often similar to models of load balancing in distributed systems. The same is true of error handling techniques like logging, fail-over, idempotency of jobs etc.   
==因为并发模型与分布式系统架构很相似，他们之间经常相互借鉴。例如，线程分发工作的模型与分布式的负载均衡模型很相似。同样，错误处理技术也同样适用，比如日志记录、故障转移、工作的幂等。== 

### Parallel Workers
The first concurrency model is what I call the parallel worker model. Incoming jobs are assigned to different workers. Here is a diagram illustrating the parallel worker concurrency model:    
### 并行工作者
==第一个并发模型是我自己称之为“并行工作者模型”。进来任务被分配给不同的工作者。这是一副阐述并发工作者模型的图：==
![image](http://tutorials.jenkov.com/images/java-concurrency/concurrency-models-1.png)

In the parallel worker concurrency model a delegator distributes the incoming jobs to different workers. Each worker completes the full job. The workers work in parallel, running in different threads, and possibly on different CPUs.  
==在并行工作者模型中，一个代理把进来的工作分配给不同的工作者。每一个工作者独自完成工作。这些工作者在不同的线程，也可能在不同的CPU上并行运行。==

If the parallel worker model was implemented in a car factory, each car would be produced by one worker. The worker would get the specification of the car to build, and would build everything from start to end.  
==如果并行工作者模式在汽车工程中实现，那么每辆车由工作者生产。这个工作者获取汽车制作说明书，从头到尾建造每个事物。==

The parallel worker concurrency model is the most commonly used concurrency model in Java applications (although that is changing). Many of the concurrency utilities in the java.util.concurrent Java package are designed for use with this model. You can also see traces of this model in the design of the Java Enterprise Edition application servers.    
==并行工作者模型是在java应用中最广泛使用的并发模型（尽管java应用一直发生变化）。很多在java.util.concurrent包中的并发工具都是用了这个模型。你也可以在java企业级服务设计中看到该模型的痕迹。==
### Parallel Workers Advantages
The advantage of the parallel worker concurrency model is that it is easy to understand. To increase the parallelization of the application you just add more workers.

For instance, if you were implementing a web crawler, you could crawl a certain amount of pages with different numbers of workers and see which number gives the shortest total crawl time (meaning the highest performance). Since web crawling is an IO intensive job you will probably end up with a few threads per CPU / core in your computer. One thread per CPU would be too little, since it would be idle a lot of the time while waiting for data to download.  
### 并发工作者的优点
==并行工作者模式优势在于它很容易被理解。为了提高应用的并发能力只需要增加工作者就可以了。==  

==例如，你正在实现了一个web爬虫，你可能使用不同数量的工作者爬取一定数量的页面，并且那个工作者使用的爬取时间最短（意味着效率最高）。因为web爬取是一个IO密集型工作，在每一个CPU/核心中都可能执行完一些线程。在CPU中的每一个线程都很短，一次可能有大量的空闲时间以等待数据的下载。==
### Parallel Workers Disadvantages
The parallel worker concurrency model has some disadvantages lurking under the simple surface, though. I will explain the most obvious disadvantages in the following sections.  

### Shared State Can Get Complex
In reality the parallel worker concurrency model is a bit more complex than illustrated above. The shared workers often need access to some kind of shared data, either in memory or in a shared database. The following diagram shows how this complicates the parallel worker concurrency model:  
![image](http://tutorials.jenkov.com/images/java-concurrency/concurrency-models-2.png)  
Some of this shared state is in communication mechanisms like job queues. But some of this shared state is business data, data caches, connection pools to the database etc.  

### 并发工作者缺点    
==并发工作者模式看似简单，但是在简单的表面下却潜伏着一些缺点。我将阐述在以下方面的最明显的缺点。==  
### 共享变量变得混乱
==在真是环境中，并发工作者模型比上面图示的会复杂一些。被共享的工作者经常要访问一些共享数据，也许在内存中，也许在共享数据库中。下图展示了如何使并发工作者模式变得复杂：==

As soon as shared state sneaks into the parallel worker concurrency model it starts getting complicated. The threads need to access the shared data in a way that makes sure that changes by one thread are visible to the others (pushed to main memory and not just stuck in the CPU cache of the CPU executing the thread). Threads need to avoid race conditions, deadlock and many other shared state concurrency problems.  
==只要并发工作者模型中存在共享变量，那么就会变得复杂。线程需要以某种方式访问共享数据，以确保每个线程的更改都可以被其他线程所看到(被推到主内存，而不只是卡在执行线程的CPU的CPU缓存中)。线程需要避免竞争条件、死锁和许多其他共享的状态并发问题。==

Additionally, part of the parallelization is lost when threads are waiting for each other when accessing the shared data structures. Many concurrent data structures are blocking, meaning one or a limited set of threads can access them at any given time. This may lead to contention on these shared data structures. High contention will essentially lead to a degree of serialization of execution of the part of the code that access the shared data structures.  
==许多并发数据结构都是阻塞的，这意味着在任何给定的时间内，一个或一组有限的线程都可以访问它们。这可能会导致对共享数据结构的竞争。高并发对访问共享数据结构代码的执行将发生一定程度的序列化。==

Modern non-blocking concurrency algorithms may decrease contention and increase performance, but non-blocking algorithms are hard to implement.  
==现代非阻塞并发算法也许可以降低竞争，提高执行效率，但是非阻塞算法是很难实现的。==

Persistent data structures are another alternative. A persistent data structure always preserves the previous version of itself when modified. Thus, if multiple threads point to the same persistent data structure and one thread modifies it, the modifying thread gets a reference to the new structure. All other threads keep a reference to the old structure which is still unchanged and thus consistent. The Scala programming contains several persistent data structures.  
==持久化数据结构是另外的一种选择。当在修改的时候，持久化数据结构总是维护着一个自己上一个版本的数据。因此，当多个线程同时指向了同一个数据结构，其中一个线程修改它，其他所有的线程都将保持对旧数据的引用。Scala 有几个这样的数据结构。==


While persistent data structures are an elegant solution to concurrent modification of shared data structures, persistent data structures tend not to perform that well.  
==持久化数据结构对于并发修改共享数据结构是一种简洁的方案，但是持久化数据结构往往不能很好的执行。==

For instance, a persistent list will add all new elements to the head of the list, and return a reference to the newly added element (which then point to the rest of the list). All other threads still keep a reference to the previously first element in the list, and to these threads the list appear unchanged. They cannot see the newly added element.  
==例如，一个持久化的列表将新元素添加到列表头部，并且返回添加的最新的数据的引用（该元素指向list中的其他元素。其他的所有线程仍然保持着对上一个第一个元素的引用，对于这些线程来讲，list仍然未改变。他们看不到最新的添加的元素。==

Such a persistent list is implemented as a linked list. Unfortunately linked lists don't perform very well on modern hardware. Each element in the list is a separate object, and these objects can be spread out all over the computer's memory. Modern CPUs are much faster at accessing data sequentially, so on modern hardware you will get a lot higher performance out of a list implemented on top of an array. An array stores data sequentially. The CPU caches can load bigger chunks of the array into the cache at a time, and have the CPU access the data directly in the CPU cache once loaded. This is not really possible with a linked list where elements are scattered all over the RAM.  
==这样的持久列表最为链表实现。不幸的是链表在现在的硬件上不能很好地执行。在链表上的每个元素都是一个独立的对象，这些对象++遍布++计算机内存。现在的CPU访问连续的数据是更快速的，因此在现在的硬件上，通过数组实现的list将得到更高的执行效率。CPU缓存可以同时载入更大的数组数据块到缓存中，数据一旦载入，CPU将快速直接地访问数据。这对于在元素散布在内存中的链表是不可能的。==
