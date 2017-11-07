If a thread is not granted CPU time because other threads grab it all, it is called "starvation". The thread is "starved to death" because other threads are allowed the CPU time instead of it. The solution to starvation is called "fairness" - that all threads are fairly granted a chance to execute.

### Causes of Starvation in Java
The following three common causes can lead to starvation of threads in Java:

1. Threads with high priority swallow all CPU time from threads with lower priority.

2. Threads are blocked indefinately waiting to enter a synchronized block, because other threads are constantly allowed access before it.

3. Threads waiting on an object (called wait() on it) remain waiting indefinitely because other threads are constantly awakened instead of it.
### Threads with high priority swallow all CPU time from threads with lower priority

You can set the thread priority of each thread individually. The higher the priority the more CPU time the thread is granted. You can set the priority of threads between 1 and 10. Exactly how this is interpreted depends on the operating system your application is running on. For most applications you are better off leaving the priority unchanged.  

### Threads are blocked indefinitely waiting to enter a synchronized block

Java's synchronized code blocks can be another cause of starvation. Java's synchronized code block makes no guarantee about the sequence in which threads waiting to enter the synchronized block are allowed to enter. This means that there is a theoretical risk that a thread remains blocked forever trying to enter the block, because other threads are constantly granted access before it. This problem is called "starvation", that a thread is "starved to death" by because other threads are allowed the CPU time instead of it.

### Threads waiting on an object (called wait() on it) remain waiting indefinitely

The notify() method makes no guarantee about what thread is awakened if multiple thread have called wait() on the object notify() is called on. It could be any of the threads waiting. Therefore there is a risk that a thread waiting on a certain object is never awakened because other waiting threads are always awakened instead of it.

### Implementing Fairness in Java
While it is not possible to implement 100% fairness in Java we can still implement our synchronization constructs to increase fairness between threads.

First lets study a simple synchronized code block:  
`如果线程因为其他线程占用所有CPU时而得不到执行就称为“饥饿”线程。该线程是“饥饿至死”的线程，因为其他线程而不是该线程抢占了CPU。解决饥饿的方法称为“公平”，让所有的线程都公平地得到执行。`
### Java中饥饿发生的原因
`下面三种常见的原因可以导致java中的线程饥饿：`  
 1. 高优先级的线程抢占了低优先级所有的CPU时间片。
 2. 线程无限期地被阻塞以等待进入一个同步快，因为在此之前其他线程频繁地访问该同步快。
 3. 等待某个对象（调用该对象的wait()方法）的线程因为其他线程而非本线程被频繁唤醒而无限期地等待。
 ###  高优先级的线程抢占了低优先级所有的CPU时间片。
`可以独立地设置每个线程的线程优先级。优先级越高，线程获取的CPU时间片就越多。你可以在从1到10的范围内设置设置线程优先级。具体如何解读依赖于你应用运行的操作系统。对于大部分的应用，你最好保持优先级不变。`  
###  线程无限期地被阻塞等待进入一个同步。
`notify()方法无法保证线程被唤醒，如果多个线程调用了notify()方法的对象上调用了wait()方法。因此有这样的风险，等待某个对象的线程永远也不会被唤醒，因为总是其他线程而非此线程被唤醒。`  
### 在java中实现公平 
`尽管我们无法实现百分百的公平，但是我们可以实现自己的同步工具来提高线程间的公平性。`   
`首先，让我们学习一个简单的同步代码块：`
```

public class Synchronizer{

  public synchronized void doSynchronized(){
    //do a lot of work which takes a long time
  }

}
```
If more than one thread call the doSynchronized() method, some of them will be blocked until the first thread granted access has left the method. If more than one thread are blocked waiting for access there is no guarantee about which thread is granted access next.

### Using Locks Instead of Synchronized Blocks

To increase the fairness of waiting threads first we will change the code block to be guarded by a lock rather than a synchronized block:  
`如果对于一条的线程调用doSynchronized()方法，一些将被阻塞，直到第一条被授权访问的线程离开此方法。如果多于一条的线程被阻塞等待访问同步块，无法保证下一个被授权访问的是哪个线程。`  
### 使用锁代替同步块  
`为了提高等待线程的公平性，我们首先要改变代码块，同步锁来授权而不是同步块。`
```
public class Synchronizer{
  Lock lock = new Lock();

  public void doSynchronized() throws InterruptedException{
    this.lock.lock();
      //critical section, do a lot of work which takes a long time
    this.lock.unlock();
  }

}
```
Notice how the doSynchronized() method is no longer declared synchronized. Instead the critical section is guarded by the lock.lock() and lock.unlock() calls.

A simple implementation of the Lock class could look like this:  
`注意，doSynchronized()方法不再被声明为synchronized。相反，关键部分由lock. lock()和lock. unlock()调用保护。`  
`一个简单的锁类看起来像这样：`
```
public class Lock{
  private boolean isLocked      = false;
  private Thread  lockingThread = null;

  public synchronized void lock() throws InterruptedException{
    while(isLocked){
      wait();
    }
    isLocked      = true;
    lockingThread = Thread.currentThread();
  }

  public synchronized void unlock(){
    if(this.lockingThread != Thread.currentThread()){
      throw new IllegalMonitorStateException(
        "Calling thread has not locked this lock");
    }
    isLocked      = false;
    lockingThread = null;
    notify();
  }
}
```
If you look at the Synchronizer class above and look into this Lock implementation you will notice that threads are now blocked trying to access the lock() method, if more than one thread calls lock() simultanously. Second, if the lock is locked, the threads are blocked in the wait() call inside the while(isLocked) loop in the lock() method. Remember that a thread calling wait() releases the synchronization lock on the Lock instance, so threads waiting to enter lock() can now do so. The result is that multiple threads can end up having called wait() inside lock().

If you look back at the doSynchronized() method you will notice that the comment between lock() and unlock() states, that the code in between these two calls take a "long" time to execute. Let us further assume that this code takes long time to execute compared to entering the lock() method and calling wait() because the lock is locked. This means that the majority of the time waited to be able to lock the lock and enter the critical section is spent waiting in the wait() call inside the lock() method, not being blocked trying to enter the lock() method.

As stated earlier synchronized blocks makes no guarantees about what thread is being granted access if more than one thread is waiting to enter. Nor does wait() make any guarantees about what thread is awakened when notify() is called. So, the current version of the Lock class makes no different guarantees with respect to fairness than synchronized version of doSynchronized(). But we can change that.

The current version of the Lock class calls its own wait() method. If instead each thread calls wait() on a separate object, so that only one thread has called wait() on each object, the Lock class can decide which of these objects to call notify() on, thereby effectively selecting exactly what thread to awaken.  
`如果注意到你上面的Synchronizer类和Lock的实现，你将注意到线程现在在尝试调用lock()方法时被阻塞，如果多个线程同时调用了wait()方法。第二，如果锁被锁定，这些线程将在lock()方法的while循环中的wait()方法的调用中。请记住，线程调用wait()会释放锁实例上的同步锁，因此等待进入lock()的线程现在可以这样做。结果是多个线程最终会在lock()中调用wait()。`  
`如果你回顾doSynchronized()方法，你将注意到在lock()和unlock()方法之间的注释，在这两个方法调用之间执行了很长时间。让我们进一步假设，与调用lock()和wait()方法相比，这两个方法之间的代码执行时间更长，因为锁已经被锁定。就是说，等待锁定锁和进入关键区域的大部分等待时间是耗费咋了wait()调用，而不是lock()方法，在试着进入lock()方法时没有被阻塞。`  
`如上所书，同步块无法保证在多个线程的情况下一个线程获取访问权。wait()方法无法保证当notify()方法被调用是线程被唤醒。因此，当前这个Lock类版本与doSynchronized()的同步版本相比，对线程公平也没有任何作用。但是我们可以改变这种情况。`  
`当前的Lock类调用了它自己的wait()方法。如果每个线程都调用独立的对象的wait()方法，就每个线程就只有一个线程调用wait()方法，那么Lock类就可以知道调用notify()方法是在哪个对象上了，从而可以快速精确地选择哪个线程可以被唤醒。`

### A Fair Lock

Below is shown the previous Lock class turned into a fair lock called FairLock. You will notice that the implementation has changed a bit with respect to synchronization and wait() / notify() compared to the Lock class shown earlier.

Exactly how I arrived at this design beginning from the previous Lock class is a longer story involving several incremental design steps, each fixing the problem of the previous step: Nested Monitor Lockout, Slipped Conditions, and Missed Signals. That discussion is left out of this text to keep the text short, but each of the steps are discussed in the appropriate texts on the topic ( see the links above). What is important is, that every thread calling lock() is now queued, and only the first thread in the queue is allowed to lock the FairLock instance, if it is unlocked. All other threads are parked waiting until they reach the top of the queue.  
### 公平锁
`下面展示的是将上面的Lock转化为以公平锁。你将看到与之前Lock类相比，咋wait()/notify()方法和同步方面做了一些改变。`  
`我从上一个锁类开始的这个设计，就是一个包含了几个渐进设计步骤的更长的故事，每一步都解决了前一个步骤的问题:嵌套监视锁定、滑动条件和丢失信号。这个讨论被排除在文本之外，以保持文本的简短，但每个步骤都在主题的适当文本中讨论(参见上面的链接)。重要的是，每个调用lock()的线程现在都在排队，只有队列中的第一个线程被允许锁定FairLock实例，如果它没有解锁的话。所有其他线程都将等待直到到达队列的顶端`
``
```
public class FairLock {
    private boolean           isLocked       = false;
    private Thread            lockingThread  = null;
    private List<QueueObject> waitingThreads =
            new ArrayList<QueueObject>();

  public void lock() throws InterruptedException{
    QueueObject queueObject           = new QueueObject();
    boolean     isLockedForThisThread = true;
    synchronized(this){
        waitingThreads.add(queueObject);
    }

    while(isLockedForThisThread){
      synchronized(this){
        isLockedForThisThread =
            isLocked || waitingThreads.get(0) != queueObject;
        if(!isLockedForThisThread){
          isLocked = true;
           waitingThreads.remove(queueObject);
           lockingThread = Thread.currentThread();
           return;
         }
      }
      try{
        queueObject.doWait();
      }catch(InterruptedException e){
        synchronized(this) { waitingThreads.remove(queueObject); }
        throw e;
      }
    }
  }

  public synchronized void unlock(){
    if(this.lockingThread != Thread.currentThread()){
      throw new IllegalMonitorStateException(
        "Calling thread has not locked this lock");
    }
    isLocked      = false;
    lockingThread = null;
    if(waitingThreads.size() > 0){
      waitingThreads.get(0).doNotify();
    }
  }
}
public class QueueObject {

  private boolean isNotified = false;

  public synchronized void doWait() throws InterruptedException {
    while(!isNotified){
        this.wait();
    }
    this.isNotified = false;
  }

  public synchronized void doNotify() {
    this.isNotified = true;
    this.notify();
  }

  public boolean equals(Object o) {
    return this == o;
  }
}
```
First you might notice that the lock() method is no longer declared synchronized. Instead only the blocks necessary to synchronize are nested inside synchronized blocks.

FairLock creates a new instance of QueueObject and enqueue it for each thread calling lock(). The thread calling unlock() will take the top QueueObject in the queue and call doNotify() on it, to awaken the thread waiting on that object. This way only one waiting thread is awakened at a time, rather than all waiting threads. This part is what governs the fairness of the FairLock.

Notice how the state of the lock is still tested and set within the same synchronized block to avoid slipped conditions.

Also notice that the QueueObject is really a semaphore. The doWait() and doNotify() methods store the signal internally in the QueueObject. This is done to avoid missed signals caused by a thread being preempted just before calling queueObject.doWait(), by another thread which calls unlock() and thereby queueObject.doNotify(). The queueObject.doWait() call is placed outside the synchronized(this) block to avoid nested monitor lockout, so another thread can actually call unlock() when no thread is executing inside the synchronized(this) block in lock() method.

Finally, notice how the queueObject.doWait() is called inside a try - catch block. In case an InterruptedException is thrown the thread leaves the lock() method, and we need to dequeue it.

### A Note on Performance

If you compare the Lock and FairLock classes you will notice that there is somewhat more going on inside the lock() and unlock() in the FairLock class. This extra code will cause the FairLock to be a sligtly slower synchronization mechanism than Lock. How much impact this will have on your application depends on how long time the code in the critical section guarded by the FairLock takes to execute. The longer this takes to execute, the less significant the added overhead of the synchronizer is. It does of course also depend on how often this code is called.  
`你也许已经注意到了lock()方法不在声明为同步的。只是在需要的同步的阻塞内嵌在内部的同步块中。`  
`FairLock创建了QueueObject实例，并且每个调用lock()方法的线程都进入该队列。调用unlock()方法的线程获取对列的最上面的QueueObject，并调用它的doNotify()方法来唤醒在该对象等待的线程。这种方式只会唤醒一个等待的线程而不是所有的等待线程。这部分就是对FairLock的公平管理。`  
`同时要主要到QueueObject是真正的信号量。doWait()和doNotify()都在QueueObject中存储了信号。这么做是为了避免因为。queueObject.doWait()方法调用被放在sychronized(this)块外面避免嵌套的监视锁定，因此另外一个线程可以调用unlock()方法当没有下次呢很难过在执行在lock()方法的synchronized(this)块中。`  
`最后，注意到queueObject.doWait()在try - catch块中被调用。以防在线程离开lock()方法时抛出InterruptedException异常，我们需要重入让它入队。`  
### 性能报告
`如果你比较Lock和FairLock两个类，你将注意到在ock()和unlock()方法中有更多的操作。这些额外的代码将导致FairLock是有些慢的同步机制，与Lock相比。有多大的影响取决于在关键区域中代码的执行时间。执行的时间越长，同步器的额外开销就越小。当然，它也取决于此代码的调用频率。`
