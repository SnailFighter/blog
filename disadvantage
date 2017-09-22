Going from a singlethreaded to a multithreaded application doesn't just provide benefits. It also has some costs. Don't just multithread-enable an application just because you can. You should have a good idea that the benefits gained by doing so, are larger than the costs. When in doubt, try measuring the performance or responsiveness of the application, instead of just guessing.  

==从单线程应用转到多线程应用不仅仅带来了好处，同时也带来了一些代价。不要仅仅因为你可以启用多线程而启用多线程。你应该很清楚，相比付出的代价，它可以给你带来更多好处。当陷入疑问时，动手测试应用的执行与响应，而不是仅仅靠猜测。==
### More complex design  

Though some parts of a multithreaded applications is simpler than a singlethreaded application, other parts are more complex. Code executed by multiple threads accessing shared data need special attention. Thread interaction is far from always simple. Errors arising from incorrect thread synchronization can be very hard to detect, reproduce and fix.  
### 更复杂的设计
==尽管一部分多线程应用比单线程应用更加简单，但是其他多线程是更复杂的。对于被多个线程访问共享数据的代码需要特别注意。线程间的交互不总是简单的。错误的线程同步产生的错误可能很难检测、重现和修复。==

### Context Switching Overhead  

When a CPU switches from executing one thread to executing another, the CPU needs to save the local data, program pointer etc. of the current thread, and load the local data, program pointer etc. of the next thread to execute. This switch is called a "context switch". The CPU switches from executing in the context of one thread to executing in the context of another.  
Context switching isn't cheap. You don't want to switch between threads more than necessary.
You can read more about context switching on Wikipedia: 
### 切换上下文的开销
==当CPU从一个正在执行的线程切换到另一个正在执行的线程时，CPU需要保存本地（该线程）数据，当前线程程序指针等。这样切换成为“上下文切换”。CPU从一个正在执行的线程的上下文切换到另一个正在执行的程序的上下文。==  
 
==上下文切换不是廉价的。你不会想非必要时切换上下文。你可以读到更多上下文切换的信息在Wikipedia:==
[http://en.wikipedia.org/wiki/Context_switch](http://en.wikipedia.org/wiki/Context_switch)

### Increased Resource Consumption  
A thread needs some resources from the computer in order to run. Besides CPU time a thread needs some memory to keep its local stack. It may also take up some resources inside the operating system needed to manage the thread. Try creating a program that creates 100 threads that does nothing but wait, and see how much memory the application takes when running.  
### 增加资源消耗 
==为了能够运行，线程需要一些计算机上的资源。除了CPU时间，线程需要一些内存资源来保存它的本地栈。它还在操作系统内部占用一些资源为了管理它。尝试创建一个创建100个线程的程序，这些线程除了等待之外什么都不做，并查看应用程序在运行时需要多少内存。==
