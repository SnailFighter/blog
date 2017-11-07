### How Nested Monitor Lockout Occurs
Nested monitor lockout is a problem similar to deadlock. A nested monitor lockout occurs like this:  
### 嵌套的监视器如何发生锁定 
`嵌套的监视器锁定是与死锁类似的问题。嵌套的监视器锁会像这样发生：`
```
Thread 1 synchronizes on A
线程1  在A上同步
Thread 1 synchronizes on B (while synchronized on A)
线程1 在A上同步的同时在B上同步
Thread 1 decides to wait for a signal from another thread before continuing  
线程1  等待另一个线程的信号
Thread 1 calls B.wait() thereby releasing the lock on B, but not A.
线程1  调用B的wait()方法，释放B上的锁，但没有释放A的锁

Thread 2 needs to lock both A and B (in that sequence)
        to send Thread 1 the signal.  
线程2 需要同时锁定A和B（按照顺序），想线程1发送信号
Thread 2 cannot lock A, since Thread 1 still holds the lock on A. 
线程2  不能锁定A，因为线程1仍然持有A上的锁
Thread 2 remain blocked indefinately waiting for Thread1
        to release the lock on A
线程2  无限期地阻塞等待线程1释放A上的锁

Thread 1 remain blocked indefinately waiting for the signal from
        Thread 2, thereby
        never releasing the lock on A, that must be released to make
        it possible for Thread 2 to send the signal to Thread 1, etc.  
线程1 无限制地阻塞等待来自线程2的信号，因此，永远不释放A上的锁，这个锁必须释放，线程2才有可能向线程1发送信号

```
This may sound like a pretty theoretical situation, but look at the naive Lock implemenation below:  
`这听起来是理论上的情形，但看看下面的幼稚的锁的实现：`  
```
//lock implementation with nested monitor lockout problem

public class Lock{
  protected MonitorObject monitorObject = new MonitorObject();
  protected boolean isLocked = false;

  public void lock() throws InterruptedException{
    synchronized(this){
      while(isLocked){
        synchronized(this.monitorObject){
            this.monitorObject.wait();
        }
      }
      isLocked = true;
    }
  }

  public void unlock(){
    synchronized(this){
      this.isLocked = false;
      synchronized(this.monitorObject){
        this.monitorObject.notify();
      }
    }
  }
}
```  
Notice how the lock() method first synchronizes on "this", then synchronizes on the monitorObject member. If isLocked is false there is no problem. The thread does not call monitorObject.wait(). If isLocked is true however, the thread calling lock() is parked waiting in the monitorObject.wait() call.

The problem with this is, that the call to monitorObject.wait() only releases the synchronization monitor on the monitorObject member, and not the synchronization monitor associated with "this". In other words, the thread that was just parked waiting is still holding the synchronization lock on "this".

When the thread that locked the Lock in the first place tries to unlock it by calling unlock() it will be blocked trying to enter the synchronized(this) block in the unlock() method. It will remain blocked until the thread waiting in lock() leaves the synchronized(this) block. But the thread waiting in the lock() method will not leave that block until the isLocked is set to false, and a monitorObject.notify() is executed, as it happens in unlock().

Put shortly, the thread waiting in lock() needs an unlock() call to execute successfully for it to exit lock() and the synchronized blocks inside it. But, no thread can actually execute unlock() until the thread waiting in lock() leaves the outer synchronized block.

This result is that any thread calling either lock() or unlock() will become blocked indefinately. This is called a nested monitor lockout.


`看到lock()方法首先在“this”上同步，接着在monitorObject成员上同步。如果isLocked是false就没有问题。该线程不会调用monitorObject.wait()方法。但是如果isLock是true，调用lock()的线程暂停，等待monitorObject.wait()的调用。`  
`问题是，monitorObject.wait()的调用只释放了monitorObject成员上的监视器，与“this”关联的监视器并没有被释放。或者说，等待的线程仍然持有在“this”上的同步锁。`  
`当在上一个地方锁定线程试着通过调用unlock()解锁的将在试着进入unluck()方法的synchronized(this)时阻塞，直到在lock()中等待的线程离开synchronized(this)阻塞。但是在lock()中的等待线程不会离开该阻塞，直到isLocked被设置为false，并且monitorObject.notify()方法被执行，这些都在unlock()方法中。`  
`简而言之，在lock()方法中等待的线程需要unlock()方法调用，执行成功，它将退出lock()和方法中的同步块。但是没有线程可以执行unlock()方法在lock()方法中等待的线程离开外层的同步块。`  
`其结果是任何调用lock()或者unlock()的方法都将立刻被阻塞。这称为一个嵌套的监视锁。`

### A More Realistic Example
You may claim that you would never implement a lock like the one shown earlier. That you would not call wait() and notify() on an internal monitor object, but rather on the This is probably true. But there are situations in which designs like the one above may arise. For instance, if you were to implement fairness in a Lock. When doing so you want each thread to call wait() on each their own queue object, so that you can notify the threads one at a time.

Look at this naive implementation of a fair lock:  

### 更实际的例子
`你也许会说你永远也不会实现像上面那样的锁。你不调用内部监视对象的wait()和notify()方法，宁可调用this上的。但是有些情况象上面的设计可能会发生。例如，你打算在锁中实现公平。当你这么做了，你想让每个线程带调用它们自己的队列对象的wait()方法，这样你就可以同时唤醒了线程？？。`  
`看一下本地公平锁的实现：`  
```
//Fair Lock implementation with nested monitor lockout problem

public class FairLock {
  private boolean           isLocked       = false;
  private Thread            lockingThread  = null;
  private List<QueueObject> waitingThreads =
            new ArrayList<QueueObject>();

  public void lock() throws InterruptedException{
    QueueObject queueObject = new QueueObject();

    synchronized(this){
      waitingThreads.add(queueObject);

      while(isLocked || waitingThreads.get(0) != queueObject){

        synchronized(queueObject){
          try{
            queueObject.wait();
          }catch(InterruptedException e){
            waitingThreads.remove(queueObject);
            throw e;
          }
        }
      }
      waitingThreads.remove(queueObject);
      isLocked = true;
      lockingThread = Thread.currentThread();
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
      QueueObject queueObject = waitingThread.get(0);
      synchronized(queueObject){
        queueObject.notify();
      }
    }
  }
}
```
```
public class QueueObject {}
```  
At first glance this implementation may look fine, but notice how the lock() method calls queueObject.wait(); from inside two synchronized blocks. One synchronized on "this", and nested inside that, a block synchronized on the queueObject local variable. When a thread calls queueObject.wait()it releases the lock on the QueueObject instance, but not the lock associated with "this".

Notice too, that the unlock() method is declared synchronized which equals a synchronized(this) block. This means, that if a thread is waiting inside lock() the monitor object associated with "this" will be locked by the waiting thread. All threads calling unlock() will remain blocked indefinately, waiting for the waiting thread to release the lock on "this". But this will never happen, since this only happens if a thread succeeds in sending a signal to the waiting thread, and this can only be sent by executing the unlock() method.

And so, the FairLock implementation from above could lead to nested monitor lockout. A better implementation of a fair lock is described in the text Starvation and Fairness.  
`乍一看，这个实现好像是不错的，但是注意到lock()方法调用queueObject.wait()方法在两个同步块内。一个同步在this上，在此内部，一个同步块在queueObject本地变量上。当线程调用queueObject.wait()方法它将释放在QueueObject实例上的锁，但是在“this”上的锁不会被释放。`  
`同时要注意到，unlock()方法被声明为同步方法，这相当于synchronized(this)块。这意味着，如果一个线程等待lock()相关的监控对象"this"的等待线程将被锁定。所有调用unlock()方法的线程将无限期地被阻塞，等待等待线程释放在“this”上的锁。但是这永远不会发生，因为这只会发生在一个线程成功地向这个等待线程发送一个信号，并且只能通过执行unlock()方法发送信号。`  
`所以，上面的公平锁实现将导致嵌套监视锁。更好的公平锁实现是在“饥饿锁和公平锁”一文中。`

### Nested Monitor Lockout vs. Deadlock
The result of nested monitor lockout and deadlock are pretty much the same: The threads involved end up blocked forever waiting for each other.

The two situations are not equal though. As explained in the text on Deadlock a deadlock occurs when two threads obtain locks in different order. Thread 1 locks A, waits for B. Thread 2 has locked B, and now waits for A. As explained in the text on Deadlock Prevention deadlocks can be avoided by always locking the locks in the same order (Lock Ordering). However, a nested monitor lockout occurs exactly by two threads taking the locks in the same order. Thread 1 locks A and B, then releases B and waits for a signal from Thread 2. Thread 2 needs both A and B to send Thread 1 the signal. So, one thread is waiting for a signal, and another for a lock to be released.

The difference is summed up here:  
### 嵌套监视对象锁 VS 死锁
`嵌套监视锁和死锁的后果很相似：相关的线程永远被阻塞，相互等待。`  

`但是这两种情况并不是完全仙童的。在“死锁”一文中所说的，死锁发生在当两个线程按不同的顺序获取锁。线程1锁定A,等待B.线程2 已经锁定B，等待A.在“避免死锁”一文中，死锁可以通过按照相同的顺序锁定锁就可以避免（锁定顺序）。但是，嵌套监视锁恰恰发生在两个线程按照相同的顺序获取锁的情况下。线程1锁定A和B，接着释放B，等待来自线程2的信号。线程2需要A和B来向线程1发送信号。因此，一个线程等待信号，另一个等待锁的释放。`  
`它们的不同概括如下：` 
```
In deadlock, two threads are waiting for each other to release locks.  
死锁中，两个线程彼此等待对方释放锁。

In nested monitor lockout, Thread 1 is holding a lock A, and waits
for a signal from Thread 2. Thread 2 needs the lock A to send the
signal to Thread 1.  
在嵌套监视锁中，线程1持有锁A,等待来自线程2的锁，线程2需要锁A来向线程1发送信号。
```
