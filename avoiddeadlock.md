In some situations it is possible to prevent deadlocks. I'll describe three techniques in this text:

1. Lock Ordering
2. Lock Timeout
3. Deadlock Detection
Lock Ordering
Deadlock occurs when multiple threads need the same locks but obtain them in different order.

If you make sure that all locks are always taken in the same order by any thread, deadlocks cannot occur. Look at this example:  
`在一些清下中是可能避免死锁的。在本文中我将描述三种技术。`  
1. 锁顺序
2. 锁超时
3. 死锁探测
`死锁发生在多个线程需要相同的锁，但是获取锁的顺序不同的情况下。`  
`如果你确定所有的锁在任何的线程中都是按照相同的顺序发生，死锁就不会发生，看这个例子：`

```
Thread 1:

  lock A 
  lock B


Thread 2:

   wait for A
   lock C (when A locked)


Thread 3:

   wait for A
   wait for B
   wait for C
```
If a thread, like Thread 3, needs several locks, it must take them in the decided order. It cannot take a lock later in the sequence until it has obtained the earlier locks.  


For instance, neither Thread 2 or Thread 3 can lock C until they have locked A first. Since Thread 1 holds lock A, Thread 2 and 3 must first wait until lock A is unlocked. Then they must succeed in locking A, before they can attempt to lock B or C.

Lock ordering is a simple yet effective deadlock prevention mechanism. However, it can only be used if you know about all locks needed ahead of taking any of the locks. This is not always the case.

### Lock Timeout
Another deadlock prevention mechanism is to put a timeout on lock attempts meaning a thread trying to obtain a lock will only try for so long before giving up. If a thread does not succeed in taking all necessary locks within the given timeout, it will backup, free all locks taken, wait for a random amount of time and then retry. The random amount of time waited serves to give other threads trying to take the same locks a chance to take all locks, and thus let the application continue running without locking.

Here is an example of two threads trying to take the same two locks in different order, where the threads back up and retry:  
`假如一个线程，例如线程3，需要多个锁，它必须获取它们按照指定的顺序。它不能获取序列中后面男的锁，直到它获取到前面的锁。`  
`例如，线程2和线程3都不能在获锁定A前锁定C。因为线程1持有锁A,线程2和线程3必须先等到锁A被释放。接下来，他们必须成功地锁定A,才能试着锁定B和C`  
1
`锁顺序是一种简单高效的避免死锁的机制。然而，它只能用于你提前知道你需要的所有锁。但总不是一直都这样`  
### 锁超时  
`另外一个锁避免机制在锁意图上设置超时时间，意思是线程只能在给定的时间内获取锁。如果一个线程在给内的超时时间内没有获取需要的所有的锁，线程将回退，释放所有的已经获取到的锁，等待一个不确定的时间再重视。随机的时间等待服务使其他线程尝试使用相同的锁，以获得所有锁的机会，从而让应用程序继续运行，而无需锁定。`  
`这里有一个例子，两个线程试图以不同的顺序使用相同的两个锁，其中的线程重新启动并重试:`
```
Thread 1 locks A
Thread 2 locks B

Thread 1 attempts to lock B but is blocked
Thread 2 attempts to lock A but is blocked

Thread 1's lock attempt on B times out
Thread 1 backs up and releases A as well
Thread 1 waits randomly (e.g. 257 millis) before retrying.

Thread 2's lock attempt on A times out
Thread 2 backs up and releases B as well
Thread 2 waits randomly (e.g. 43 millis) before retrying.
```
In the above example Thread 2 will retry taking the locks about 200 millis before Thread 1 and will therefore likely succeed at taking both locks. Thread 1 will then wait already trying to take lock A. When Thread 2 finishes, Thread 1 will be able to take both locks too (unless Thread 2 or another thread takes the locks in between).

An issue to keep in mind is, that just because a lock times out it does not necessarily mean that the threads had deadlocked. It could also just mean that the thread holding the lock (causing the other thread to time out) takes a long time to complete its task.

Additionally, if enough threads compete for the same resources they still risk trying to take the threads at the same time again and again, even if timing out and backing up. This may not occur with 2 threads each waiting between 0 and 500 millis before retrying, but with 10 or 20 threads the situation is different. Then the likeliness of two threads waiting the same time before retrying (or close enough to cause problems) is a lot higher.

A problem with the lock timeout mechanism is that it is not possible to set a timeout for entering a synchronized block in Java. You will have to create a custom lock class or use one of the Java 5 concurrency constructs in the java.util.concurrency package. Writing custom locks isn't difficult but it is outside the scope of this text. Later texts in the Java concurrency trails will cover custom locks.  
`在上面的例子中，线程2在线程1前重新获取锁大约200毫秒，很有可能成功获取两个锁。线程1就继续等待试着获取锁A.当线程2完成后，线程1就能够获取这两个锁（除非线程2或其他线程在中间获取了锁）`  
`要记住的问题是，仅仅因为锁超时并不意味着线程已经陷入僵局。它还可能意味着锁定锁的线程(导致另一个线程超时)需要很长时间才能完成它的任务。` 
`除此之外，如果足够多的线程竞争相同的资源，它们可能会使多个线程在同一时间一次一次地重试，即使是超时和回退。这在两个线程之间，等待0到500毫秒的情况下不可能发生，但是在10-20个线程的情况是不同的。两个线程在重试前等待相同的资源的可能性大大增加。`  
`锁超时机制的问题是在java中为重新进入同步快设置超时时间是不可能的。你不得不创建一个自定义的锁类或者使用Java 5 中的 java.util.concurrency package.下的并发工具，写自定义的锁并不困难，但是这超出了本文范围。随后的文章中在Java并发将包含自定义锁`


### Deadlock Detection
Deadlock detection is a heavier deadlock prevention mechanism aimed at cases in which lock ordering isn't possible, and lock timeout isn't feasible.

Every time a thread takes a lock it is noted in a data structure (map, graph etc.) of threads and locks. Additionally, whenever a thread requests a lock this is also noted in this data structure.

When a thread requests a lock but the request is denied, the thread can traverse the lock graph to check for deadlocks. For instance, if a Thread A requests lock 7, but lock 7 is held by Thread B, then Thread A can check if Thread B has requested any of the locks Thread A holds (if any). If Thread B has requested so, a deadlock has occurred (Thread A having taken lock 1, requesting lock 7, Thread B having taken lock 7, requesting lock 1).

Of course a deadlock scenario may be a lot more complicated than two threads holding each others locks. Thread A may wait for Thread B, Thread B waits for Thread C, Thread C waits for Thread D, and Thread D waits for Thread A. In order for Thread A to detect a deadlock it must transitively examine all requested locks by Thread B. From Thread B's requested locks Thread A will get to Thread C, and then to Thread D, from which it finds one of the locks Thread A itself is holding. Then it knows a deadlock has occurred.

Below is a graph of locks taken and requested by 4 threads (A, B, C and D). A data structure like this that can be used to detect deadlocks.  
### 死锁探测 
`死锁探测是一种更重量级的死锁避免机制，主要用于锁顺序和锁超时都不能用的情况下。`  
`每次一个线程获取一个锁后都备注在一个线程和锁的数据结构（映射，图等）。此外，无论何时线程请求一个锁都在这个数据结构中备注一下。`  
`当一个线程请求一个锁，但是该请求被拒绝，这个线程就可以跨过这个锁图来检查这个死锁。例如，如果下个线程A请求锁7，但是锁7证被线程B所持有，那么线程A可以检查线程B是否已经请求了线程A持有的任何锁（任何的一个）。如果线程B已经请求了，那么死锁就已经发生了（线程A持有锁1，请求锁7，线程B已经持有锁7，请求锁1）`  
`当然，死锁情形可能更加复杂比像个线程持有彼此的锁。线程A可能等待线程B，线程B等待线程C，线程C等待线程D,线程D等待线程A.为了使线程A检测死锁，它必须通过线程B对所有请求的锁进递归地检查。从线程B请求的锁开始，线程A检查线程C,检查D，从中找到一个线程A自己持有的锁。那么它就知道死锁发生了。`  
`下面是一张锁图，被四个线程请求（A,B,C,D）.这样的数据结构可以用来探测死锁。`

![image](http://tutorials.jenkov.com/images/java-concurrency/deadlock-detection-graph.png)  

So what do the threads do if a deadlock is detected?

One possible action is to release all locks, backup, wait a random amount of time and then retry. This is similar to the simpler lock timeout mechanism except threads only backup when a deadlock has actually occurred. Not just because their lock requests timed out. However, if a lot of threads are competing for the same locks they may repeatedly end up in a deadlock even if they back up and wait.

A better option is to determine or assign a priority of the threads so that only one (or a few) thread backs up. The rest of the threads continue taking the locks they need as if no deadlock had occurred. If the priority assigned to the threads is fixed, the same threads will always be given higher priority. To avoid this you may assign the priority randomly whenever a deadlock is detected.  
`那么如果死锁被探测到将做什么呢？`  
`一种可能的动作是释放所有的锁，回退，等待一个随机的时间再重试。这和简单一点锁超时有些相似，只除了死锁发生后回退。不仅是它们的锁请求超时。然而，如果有大量的线程竞争相同的锁，他们可能无尽地在死锁中重复，即使他们回退等待。`  
`更好的选择是指定或者设置线程的优先级，这样只有一个（或少量的）线程回退。剩下的线程继续获取它们需要的锁，如果没有死锁发生。如果分配给线程的优先级是固定的（还是被修改的意思？？？？），那么相同的线程将是更高的优先级。为了避免这样，你应该在探测到死锁是随机分配优先级。`
