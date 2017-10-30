### Thread Deadlock
A deadlock is when two or more threads are blocked waiting to obtain locks that some of the other threads in the deadlock are holding. Deadlock can occur when multiple threads need the same locks, at the same time, but obtain them in different order.

For instance, if thread 1 locks A, and tries to lock B, and thread 2 has already locked B, and tries to lock A, a deadlock arises. Thread 1 can never get B, and thread 2 can never get A. In addition, neither of them will ever know. They will remain blocked on each their object, A and B, forever. This situation is a deadlock.

The situation is illustrated below:  
### 线程死锁
`死锁两个或多个线程阻塞等待其他在死锁中的线程持有的锁的情形。死锁发生在多个线程在相同的时刻需要相同的锁，但是以不同的顺序获取锁 `  
`例如，线程1 锁定A，并试着说定B，线程2已经锁定了B，并且试着锁定A，就发生死锁了。线程1永远获取不了B，且线程2也无法获取A。此外，他们都不知道彼此。线程将永远能阻塞在它们每个对象上，A和B。这种情形就是死锁。`  
`这种情形是下面这样的：`


```
Thread 1  locks A, waits for B
Thread 2  locks B, waits for A
```
Here is an example of a TreeNode class that call synchronized methods in different instances:  
`这里是一个例子，ThreeNode类在不同的实例中调用同步方法：`

```
public class TreeNode {
 
  TreeNode parent   = null;  
  List     children = new ArrayList();

  public synchronized void addChild(TreeNode child){
    if(!this.children.contains(child)) {
      this.children.add(child);
      child.setParentOnly(this);
    }
  }
  
  public synchronized void addChildOnly(TreeNode child){
    if(!this.children.contains(child){
      this.children.add(child);
    }
  }
  
  public synchronized void setParent(TreeNode parent){
    this.parent = parent;
    parent.addChildOnly(this);
  }

  public synchronized void setParentOnly(TreeNode parent){
    this.parent = parent;
  }
}
```
If a thread (1) calls the parent.addChild(child) method at the same time as another thread (2) calls the child.setParent(parent) method, on the same parent and child instances, a deadlock can occur. Here is some pseudo code that illustrates this:  
`如果线程1调用paraent.addChild(child)方法的同时另外的线程2调用了child.setParent(parent)方法，并且在相同的parent,child实例上，就会发生死锁。这里是这种情况的伪代码：`

```
Thread 1: parent.addChild(child); //locks parent
          --> child.setParentOnly(parent);

Thread 2: child.setParent(parent); //locks child
          --> parent.addChildOnly()
```
First thread 1 calls parent.addChild(child). Since addChild() is synchronized thread 1 effectively locks the parent object for access from other threads.

Then thread 2 calls child.setParent(parent). Since setParent() is synchronized thread 2 effectively locks the child object for access from other threads.

Now both child and parent objects are locked by two different threads. Next thread 1 tries to call child.setParentOnly() method, but the child object is locked by thread 2, so the method call just blocks. Thread 2 also tries to call parent.addChildOnly() but the parent object is locked by thread 1, causing thread 2 to block on that method call. Now both threads are blocked waiting to obtain locks the other thread holds.

Note: The two threads must call parent.addChild(child) and child.setParent(parent) at the same time as described above, and on the same two parent and child instances for a deadlock to occur. The code above may execute fine for a long time until all of a sudden it deadlocks.

The threads really need to take the locks *at the same time*. For instance, if thread 1 is a bit ahead of thread2, and thus locks both A and B, then thread 2 will be blocked already when trying to lock B. Then no deadlock occurs. Since thread scheduling often is unpredictable there is no way to predict *when* a deadlock occurs. Only that it *can* occur.  
`首先线程1调用了parent.addChild(child)方法，因为addChild()方法被同步的，线程1高效地锁定了parent对象以阻止其他线程访问。`  
`接着，线程2调用child.setparent(parent)方法。因为setParent()方法是同步的，线程2高效地锁定了child对象以阻止其他对象的访问。`  
`现在child和parent两个对象被两个不同的线程锁定。接着，线程1试着调用child.setParentOnly()方法，但是child对象被线程2锁定，因此该方法调用将被阻塞。线程2同样试着调用parent.addChildOnly()方法，但是parent对象被线程1锁定，导致线程2在该方法调用上被阻塞。现在，两个下次你哼都被阻塞等待获取其他线程持有的锁。`  
`注意：在上面的描述中，两个线程必须同时掉哦那个parent.addChild()和child.setParent()方法，并且这两个线程中的parent和child实例是相同的，死锁才能发生。上面的代码在发生死锁前可能很长时间都能很好地执行。`  
`这些线程要真实地同时获取锁。例如，如果线程1比线程2提前了一点点，然后租住了A和B，接着线程2被锁定当它试着锁定B时。不会有死锁发生。因为线程执行计划是无法被预测的，没有办法什么时候会发生死锁。只有它能发生才发生`  
  

### More Complicated Deadlocks
Deadlock can also include more than two threads. This makes it harder to detect. Here is an example in which four threads have deadlocked:  
### 更加复杂的死锁 
`死锁也可能包含对于两个的线程。。这使得死锁更能被发现。这里是四条线程发生死锁的例子：`

```
Thread 1  locks A, waits for B
Thread 2  locks B, waits for C
Thread 3  locks C, waits for D
Thread 4  locks D, waits for A
```
Thread 1 waits for thread 2, thread 2 waits for thread 3, thread 3 waits for thread 4, and thread 4 waits for thread 1.

### Database Deadlocks
A more complicated situation in which deadlocks can occur, is a database transaction. A database transaction may consist of many SQL update requests. When a record is updated during a transaction, that record is locked for updates from other transactions, until the first transaction completes. Each update request within the same transaction may therefore lock some records in the database.

If multiple transactions are running at the same time that need to update the same records, there is a risk of them ending up in a deadlock.

For example  
`线程1等待线程2，线程2等待线程3，线程3等待线程4，线程4等待线程1.`  
#### 数据库死锁
`一个更复杂的发生死锁的情形是是数据库事务。一个数据库事务可能包含了狠毒sql更新请求。当一条记录在一个事务中被更新，这条记录就从从其他事务加了锁以便更新，直到第一个事务完成。每次更新请求都砸同一个事务也许会锁定数据库中的一些记录。`  
`假如多个事务同时刻运行，且需要更显相同的记录，那么他们就有发生死锁的风险。`  
`例如：`

```
Transaction 1, request 1, locks record 1 for update
Transaction 2, request 1, locks record 2 for update

Transaction 1, request 2, tries to lock record 2 for update.
Transaction 2, request 2, tries to lock record 1 for update.
```
Since the locks are taken in different requests, and not all locks needed for a given transaction are known ahead of time, it is hard to detect or prevent deadlocks in database transactions.  
`因为锁发生在不同的请求中，并且不是所有的锁都需要提前知道给定的事务，很难预测或避免事务中的死锁。`
