[原文](http://tutorials.jenkov.com/java-concurrency/index.html)


Back in the old days a computer had a single CPU, and was only capable of executing a single program at a time. Later came multitasking which meant that computers could execute multiple programs (AKA tasks or processes) at the same time. It wasn't really "at the same time" though. The single CPU was shared between the programs. The operating system would switch between the programs running, executing each of them for a little while before switching.  

回顾计算机单核时代，计算机载某一时刻只能执行一个程序。随后出现的多任务处理能在同一时刻执行多个程序（AKA 任务或者进程），尽管不是真正意义上的同一时刻。单个CPU可以在多个进程间共享。操作系统从这些运行的进程，执行每一个进程一小段时间在CPU转换任务前。

Along with multitasking came new challenges for software developers. Programs can no longer assume to have all the CPU time available, nor all memory or any other computer resources. A "good citizen" program should release all resources it is no longer using, so other programs can use them.  
随着多任务处理的到来，对于开发人员带来了新的挑战。进程不再占用所有的CPU、内存或者其他计算机资源。一个好的进程应该释放所有它不再使用的资源，这样其他进程就可以使用了。

Later yet came multithreading which mean that you could have multiple threads of execution inside the same program. A thread of execution can be thought of as a CPU executing the program. When you have multiple threads executing the same program, it is like having multiple CPUs execute within the same program.  
==随后到来的多线程可以在同一个进程中执行多个线程。一个执行的线程可理解为一个证在被CPU执行的进程，但你以后多个线程在同一个进程中执行的话，就像有个多个CPU处理同一个进程。==

Multithreading can be a great way to increase the performance of some types of programs. However, mulithreading is even more challenging than multitasking. The threads are executing within the same program and are hence reading and writing the same memory simultanously. This can result in errors not seen in a singlethreaded program. Some of these errors may not be seen on single CPU machines, because two threads never really execute "simultanously". Modern computers, though, come with multi core CPUs, and even with multiple CPUs too. This means that separate threads can be executed by separate cores or CPUs simultanously.    
==多线程是一个很棒的方式以提升一些进程的执行。但是，多线程相比多任务是更有挑战性的。因为在同一个进程中的多个线程是同时读写同一块内存，可能导致在单线程中不会出现的错误。这些错误可能不会出现在单核CPU的机器上，因为两个线程永远不会同时执行。现在的计算机，有多个CPU，甚至有多通道，这意味着不同的线程可以被不同的内核或者不同的cpu执行，在同一时刻。==

![image](http://tutorials.jenkov.com/images/java-concurrency/java-concurrency-tutorial-introduction-1.png)  

If a thread reads a memory location while another thread writes to it, what value will the first thread end up reading? The old value? The value written by the second thread? Or a value that is a mix between the two? Or, if two threads are writing to the same memory location simultanously, what value will be left when they are done? The value written by the first thread? The value written by the second thread? Or a mix of the two values written?  
==如果一个线程读取一块内寸同时另一个线程向该内存位置进行写入，第一条线程读取到的内容是什么呢？或者是两者的最小值？亦或，如果两个线程同时向同一内存位置写入，当这连个线程执行完成后这个值应该是什么？是第一条线程写入的值？第二条？后者是他们的最小值？==

Without proper precautions any of these outcomes are possible. The behaviour would not even be predictable. The outcome could change from time to time. Therefore it is important as a developer to know how to take the right precautions - meaning learning to control how threads access shared resources like memory, files, databases etc. That is one of the topics this Java concurrency tutorial addresses.  
==缺乏对这样的输出的预防是有可能的。甚至，这样的行为无法被预测。输出可能每次都有变化。因此，作为一个开发者了解采用怎样的预防措施是很重要的，这就意味着要学习多线程如何访问共享资源，例如内存、文件、数据库等等。这是java并发之旅的主题。==

### Multithreading and Concurrency in Java  
Java was one of the first languages to make multithreading easily available to developers. Java had multithreading capabilities from the very beginning. Therefore, Java developers often face the problems described above. That is the reason I am writing this trail on Java concurrency. As notes to myself, and any fellow Java developer whom may benefit from it.

==Java是最早使开发人员易于使用多线程的语言之一。java一开始就有多线程处理的能力。因此，java开发者经常面临着上面的问题。这是我写这一些列文章的原因。对我自己和后面的java开发者都可从中获益。==

The trail will primarily be concerned with multithreading in Java, but some of the problems occurring in multithreading are similar to problems occurring in multitasking and in distributed systems. References to multitasking and distributed systems may therefore occur in this trail too. Hence the word "concurrency" rather than "multithreading".   

==该系列文章主要关注java多线程，但多线程中出现的一些问题类似于多任务处理和分布式系统中的问题，因此在该系列中会涉及到一些多任务和分布式系统。所以使用“并发”而不是“多线程”==
### Java Concurrency in 2015 and Forwar 

A lot has happened in the world of concurrent architecture and design since the first Java concurrency books were written, and even since the Java 5 concurrency utilities were released.  

==自从第一个Java并发书编写以来，并发架构和设计领域发生了很多事情，甚至在Java 5并发实用程序发布之后。==


New, asynchronous "shared-nothing" platforms and APIs like Vert.x and Play / Akka and Qbit have emerged. These platforms use a different concurrency model than the standard Java / JEE concurrency model of threading, shared memory and locking. New non-blocking concurrency algorithms have been published, and new non-blocking tools like the LMax Disrupter have been added to our toolkits. New functional programming parallelism has been introduced with the Fork and Join framework in Java 7, and the collection streams API in Java 8.

==新的、异步的“无共享”平台和像Vert这样的api。x和Play / Akka和Qbit出现了。这些平台使用不同的并发模型，而不是线程、共享内存和锁定的标准Java / JEE并发模型。新的非阻塞并发算法已经发布，新的非阻塞工具，如LMax破坏者已经添加到我们的工具包中。在Java 7中使用Fork和Join框架引入了新的函数式编程并行，Java 8中的collection streams API。==


With all these new developments it is about time that I updated this Java Concurrency tutorial. Therefore, this tutorial is once again work in progress. New tutorials will be published whenever time is available to write them.

==有了所有这些新开发，我就该更新Java并发教程了。因此，本教程再次进行了研究。只要有时间，新的教程就会发表。==  

