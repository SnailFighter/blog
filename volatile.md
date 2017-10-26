The Java volatile keyword is used to mark a Java variable as "being stored in main memory". More precisely that means, that every read of a volatile variable will be read from the computer's main memory, and not from the CPU cache, and that every write to a volatile variable will be written to main memory, and not just to the CPU cache.  
Java中volotile关键字被用于标识Java变量是存储在主存的变量。更精确地说，对volatile变量的每次读取都是从计算机主存中读取，而不是从CPU缓存中，每次对volatile变量的写操作是写入到主存中，而不是写入到CPU缓存中。

Actually, since Java 5 the volatile keyword guarantees more than just that volatile variables are written to and read from main memory. I will explain that in the following sections.  
通常，从Java5开始volatile关键字不仅仅是保证了volatile变量写入到主存和从主存中读取。我将在下文中说。

### The Java volatile Visibility Guarantee  
### Java volatile 可见性保证
The Java volatile keyword guarantees visibility of changes to variables across threads. This may sound a bit abstract, so let me elaborate.  
Java volatile 关键字保证了线程对变量修改的可见性。这听上去有些抽象，我将详细阐述。  

In a multithreaded application where the threads operate on non-volatile variables, each thread may copy variables from main memory into a CPU cache while working on them, for performance reasons. If your computer contains more than one CPU, each thread may run on a different CPU. That means, that each thread may copy the variables into the CPU cache of different CPUs. This is illustrated here:  
在一个线程操作一个不是volatile的变量的多线程应用中，线程为了执行上的原因，在线程运行的时候也许从主存中复制变量到CPU缓存中。如果你的电脑包含了多个cpu，那么每个线程可能运行在不同的CPU上。这意味着，每个线程都赋值变量到不同的CPU缓存中。该图是上面的概述图解：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-volatile-1.png )  
With non-volatile variables there are no guarantees about when the Java Virtual Machine (JVM) reads data from main memory into CPU caches, or writes data from CPU caches to main memory. This can cause several problems which I will explain in the following sections.  
使用非volatile变量，Java虚拟机从主存读取数据到CPU缓存，或者从CPU缓存写回到主存的数据就没有保证。这会导致一些下面我将说到的问题。  

Imagine a situation in which two or more threads have access to a shared object which contains a counter variable declared like this:  
假设一种情况，这种情况是两个或者多个线程访问一个共享对象，共享对象包含一个counter变量，像这样：
```
public class SharedObject {

    public int counter = 0;

}
```
Imagine too, that only Thread 1 increments the counter variable, but both Thread 1 and Thread 2 may read the counter variable from time to time.  
假设只有线程1增加counter变量值，但是线程1和线程2都一次次读取counter值。  

If the counter variable is not declared volatile there is no guarantee about when the value of the counter variable is written from the CPU cache back to main memory. This means, that the counter variable value in the CPU cache may not be the same as in main memory. This situation is illustrated here:   
如果counter变量没有声明为volatile，当counter值从CPU缓存写入到主存就没有保证。这意味着，在CPU中的counter变量的值与在主存中的不一致。这种情况是这样的：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-volatile-2.png)
The problem with threads not seeing the latest value of a variable because it has not yet been written back to main memory by another thread, is called a "visibility" problem. The updates of one thread are not visible to other threads.  
因为线程没有写回到主存导致的最后变量值不可见的多线程问题称为“可见性”问题。线程的更新操作对其他线程是不可见的。

By declaring the counter variable volatile all writes to the counter variable will be written back to main memory immediately. Also, all reads of the counter variable will be read directly from main memory. Here is how the volatile declaration of the counter variable looks:  
通过声明counter变量为volatile变量，所有对counter的写操作会立刻写入到主存中，同样，对counter的读操作会立刻从主存中读取。这里是如何声明counter变量volitale的例子：
```
public class SharedObject {

    public volatile int counter = 0;

}
```
Declaring a variable volatile thus guarantees the visibility for other threads of writes to that variable.  
声明一个变量为volatile保证了其他线程对变量写操作的可见性。

### The Java volatile Happens-Before Guarantee  
### Java 
Since Java 5 the volatile keyword guarantees more than just the reading from and writing to main memory of variables. Actually, the volatile keyword guarantees this:  
自从Java 5以来，volatile关键字保证的不只是从主存中读写变量。通常，volatile关键字保证了这些：  

* If Thread A writes to a volatile variable and Thread B subsequently reads the same volatile variable, then all variables visible to Thread A before writing the volatile variable, will also be visible to Thread B after it has read the volatile variable. 

* The reading and writing instructions of volatile variables cannot be reordered by the JVM (the JVM may reorder instructions for performance reasons as long as the JVM detects no change in program behaviour from the reordering). Instructions before and after can be reordered, but the volatile read or write cannot be mixed with these instructions. Whatever instructions follow a read or write of a volatile variable are guaranteed to happen after the read or write.  
* 如果线程A 对volatile变量进行写操作，那么线程B可以立刻读取相同的volatile变量，在对volatile变量写前，所有的变量对线程A都是可见的。在读取volatile变量后对线程B同样是可见的。  
* 
These statements require a deeper explanation.

When a thread writes to a volatile variable, then not just the volatile variable itself is written to main memory. Also all other variables changed by the thread before writing to the volatile variable are also flushed to main memory. When a thread reads a volatile variable it will also read all other variables from main memory which were flushed to main memory together with the volatile variable.  
当线程对valatile变量写操作，不仅仅是volatile变量自己写入到主存中，其他被该线程修改的所有变量也会刷新到主存中。当线程读取volatile变量时也会读取从主存中读取其他所有的与volatile变量一块写入主存的变量。
Look at this example:    
看这个例子：
```
Thread A:
    sharedObject.nonVolatile = 123;
    sharedObject.counter     = sharedObject.counter + 1;

Thread B:
    int counter     = sharedObject.counter;
    int nonVolatile = sharedObject.nonVolatile;
```
Since Thread A writes the non-volatile variable sharedObject.nonVolatile before writing to the volatile sharedObject.counter, then both sharedObject.nonVolatile and sharedObject.counter are written to main memory when Thread A writes to sharedObject.counter (the volatile variable).  
因为线程A在写入volatile共享变量前写了非volatile变量sharedObject.nonVolatile，那么当先线程A在写sharedObject.counter（这个volatile变量）是sharedObject.nonVolatile和sharedObect.counter读u写入到主存中。


Since Thread B starts by reading the volatile sharedObject.counter, then both the sharedObject.counter and sharedObject.nonVolatile are read from main memory into the CPU cache used by Thread B. By the time Thread B reads sharedObject.nonVolatile it will see the value written by Thread A.  
线程B系统后就读取volatile sharedObject.counter,那么sharedObject.counter 和sharedObect.nonVolatile变量都会从主存读取到CPU缓存。同时，线程B也会读取到sharedObject.nonVolatile，并看到线程A写入的值。  


Developers may use this extended visibility guarantee to optimize the visibility of variables between threads. Instead of declaring each and every variable volatile, only one or a few need be declared volatile. Here is an example of a simple Exchanger class written after that principle:  
开发者可以利用这样的扩展可见性保证在线程间变量的可见性。不用声明每一个变量都是volatile的，只需要一个或者少量的变成声明为volatile即可。这里是在此原则下的简单的Exchanger 示例。
```
public class Exchanger {

    private Object   object       = null;
    private volatile hasNewObject = false;

    public void put(Object newObject) {
        while(hasNewObject) {
            //wait - do not overwrite existing new object
        }
        object = newObject;
        hasNewObject = true; //volatile write
    }

    public Object take(){
        while(!hasNewObject){ //volatile read
            //wait - don't take old object (or null)
        }
        Object obj = object;
        hasNewObject = false; //volatile write
        return obj;
    }
}
```
Thread A may be putting objects from time to time by calling put(). Thread B may take objects from time to time by calling take(). This Exchanger can work just fine using a volatile variable (without the use of synchronized blocks), as long as only Thread A calls put() and only Thread B calls take().  
线程A每次都调用put()方法放入对象，线程B每次调用take()方法获取对象。这个Exchanger只使用一个volatile变量（不是用同步块）就可以很好地工作，只要线程A只调用put()方法，线程B只调用take()方法。  

However, the JVM may reorder Java instructions to optimize performance, if the JVM can do so without changing the semantics of the reordered instructions. What would happen if the JVM switched the order of the reads and writes inside put() and take()? What if put() was really executed like this:  

但是，如果JVM可以这样做，JVM可以重新排序Java指令以优化性能，而不会改变重新排序指令的语义。如故虚拟机在put（）和take()方法切换了读写顺序会发生什么呢？如果put()像下面这样执行会怎么样呢？

```
while(hasNewObject) {
    //wait - do not overwrite existing new object
}
hasNewObject = true; //volatile write
object = newObject;
```
Notice the write to the volatile variable hasNewObject is now executed before the new object is actually set. To the JVM this may look completely valid. The values of the two write instructions do not depend on each other.  
注意volatile变量hasNewObject现在在新的对象设置前都执行了。对于虚拟机这看起来是完全可用的。这两个写指令的值不是相互依赖的。  

However, reordering the instruction execution would harm the visibility of the object variable. First of all, Thread B might see hasNewObject set to true before Thread A has actually written a new value to the object variable. Second, there is now not even a guarantee about when the new value written to object will be flushed back to main memory (well - the next time Thread A writes to a volatile variable somewhere...).  
然而，指令重排将会损害object变量的可见性。首先，线程B在线程A对object变量写入新值前看到hasNewObject设置为true。第二，当新的值写入到object变量无法保证可以刷入到主存中（也许，下次线程A写入volatile变量新值的时候）。

To prevent situations like the one described above from occurring, the volatile keyword comes with a "happens before guarantee". The happens before guarantee guarantees that read and write instructions of volatile variables cannot be reordered. Instructions before and after can be reordered, but the volatile read/write instruction cannot be reordered with any instruction occurring before or after it.  
为了避免上面描述的情况发生，关键字volatile带有“happens before guaranntee”规则。这个规则保证了volatile变量的读写指令不能被重排序。前后的指令（volitale指令前后）都可一被重排，但是不能排到volatile 读写指令的前面或后面。  

Look at this example:  
看下面的例子  
```
sharedObject.nonVolatile1 = 123;
sharedObject.nonVolatile2 = 456;
sharedObject.nonVolatile3 = 789;

sharedObject.volatile     = true; //a volatile variable

int someValue1 = sharedObject.nonVolatile4;
int someValue2 = sharedObject.nonVolatile5;
int someValue3 = sharedObject.nonVolatile6;
```
The JVM may reorder the first 3 instructions, as long as all of them happens before the volatile write instruction (they must all be executed before the volatile write instruction).   
虚拟机可以重排前三条指令，只要它们发生在volatile写指令前（它们必须在volatile写指令写执行）。


Similarly, the JVM may reorder the last 3 instructions as long as the volatile write instruction happens before all of them. None of the last 3 instructions can be reordered to before the volatile write instruction.  
同理，虚拟机可以重排后三条指令，只要volatile写指令在它们前执行。后三条指令不能重排到volatile写指令前。  

That is basically the meaning of the Java volatile happens before guarantee.  
这是java volatile 的 ”happens before guarantee“的大概意思。  

### volatile is Not Always Enough
Even if the volatile keyword guarantees that all reads of a volatile variable are read directly from main memory, and all writes to a volatile variable are written directly to main memory, there are still situations where it is not enough to declare a variable volatile.  
### volatile并不总是充分的  
即使volatile关键字保证了所有的volatile变量直接从主存读取，所有对volatile变量直接写入到主存，但是仍然有声明volatile变量不充分的情况。  

In the situation explained earlier where only Thread 1 writes to the shared counter variable, declaring the counter variable volatile is enough to make sure that Thread 2 always sees the latest written value.    
在前文的解释下，只有线程1写入共享计数器变量，声明计数器变量volatile足以确保线程2始终看到最新的写入值。

In fact, multiple threads could even be writing to a shared volatile variable, and still have the correct value stored in main memory, if the new value written to the variable does not depend on its previous value. In other words, if a thread writing a value to the shared volatile variable does not first need to read its value to figure out its next value.  
事实上，多个线程可以对共享的volatile变量惊醒写操作，如果被写入的新的值不依赖上一个值，就仍然会把正确的值写入到主存中。换句话说，如果一个线程写入到共享volatile变量不需要读取它的值来算出它的下一个值

As soon as a thread needs to first read the value of a volatile variable, and based on that value generate a new value for the shared volatile variable, a volatile variable is no longer enough to guarantee correct visibility. The short time gap in between the reading of the volatile variable and the writing of its new value, creates an race condition where multiple threads might read the same value of the volatile variable, generate a new value for the variable, and when writing the value back to main memory - overwrite each other's values.  
只要一个线程需要首先读取volatile变量的值，再根据此值生成下一个新值，这个volatile变量就不再保证正确的可见性。对volatile变量的读和写一个新值之间的短时差创造了一个竞争条件，堆个线程读取了volatile变量相同的值，而且写会到主存中----覆盖彼此的值。  


The situation where multiple threads are incrementing the same counter is exactly such a situation where a volatile variable is not enough. The following sections explain this case in more detail.  


Imagine if Thread 1 reads a shared counter variable with the value 0 into its CPU cache, increment it to 1 and not write the changed value back into main memory. Thread 2 could then read the same counter variable from main memory where the value of the variable is still 0, into its own CPU cache. Thread 2 could then also increment the counter to 1, and also not write it back to main memory. This situation is illustrated in the diagram below:    
假设线程1读取共享的counter变量的值0到CPU缓存中，增加到1，并且不写会到主存。线程2从主存读取相同的值，这个值仍然是0，到它自己的CPU缓存中。线程2也增加值到1，也不写会到主存中。像下面这样：  
![image](http://tutorials.jenkov.com/images/java-concurrency/java-volatile-3.png)  
Thread 1 and Thread 2 are now practically out of sync. The real value of the shared counter variable should have been 2, but each of the threads has the value 1 for the variable in their CPU caches, and in main memory the value is still 0. It is a mess! Even if the threads eventually write their value for the shared counter variable back to main memory, the value will be wrong.  

线程1 与线程2 现在是不同步的。共享变量counter实际上的值应该是2，但是每个线程在各自的CPU缓存中这个值都是1，主存中该值仍是0.它是混乱的。即使线程最终把值写入到主存中，该值将是错误的。
### When is volatile Enough?
As I have mentioned earlier, if two threads are both reading and writing to a shared variable, then using the volatile keyword for that is not enough. You need to use a synchronized in that case to guarantee that the reading and writing of the variable is atomic. Reading or writing a volatile variable does not block threads reading or writing. For this to happen you must use the synchronized keyword around critical sections.  

As an alternative to a synchronized block you could also use one of the many atomic data types found in the java.util.concurrent package. For instance, the AtomicLong or AtomicReference or one of the others.

In case only one thread reads and writes the value of a volatile variable and other threads only read the variable, then the reading threads are guaranteed to see the latest value written to the volatile variable. Without making the variable volatile, this would not be guaranteed.

The volatile keyword is guaranteed to work on 32 bit and 64 variables.  

  
### 什么时候volatile是足够的呢？
我早已注意到，如果两个线程都读/写一个共享变量，使用volatile关键字就不是充分的。这种情况下你需要使用同步块保证变量的读写操作是原子的。volatile变量的读写不会阻塞线程的读写。为此，必须在关键部分使用synchronized关键字。  
同步块最为一种选择你也可以使用很多的在java.util.concurrent 包下的原子类型。例如AtomicLong，AtomicReference或者其他的。  
在只有一个线程读写volatile变量，其他线程只读取该变量，可以保证读线程可以看到写入volatile变量的最后的值。 如果不标记变量为volatile，那么将无法保证。  
volatile关键字可以在32位的64个变量上工作（不知道这句话是不是写错了！！！！！）



### Performance Considerations of volatile
Reading and writing of volatile variables causes the variable to be read or written to main memory. Reading from and writing to main memory is more expensive than accessing the CPU cache. Accessing volatile variables also prevent instruction reordering which is a normal performance enhancement technique. Thus, you should only use volatile variables when you really need to enforce visibility of variables.  
### volatile的性能考虑 
读写volatile变量导致变量从主存或者写入到主存。从主存读写比访问CPU缓存是更昂贵的。访问volatile变量还可以防止指令重排序，这是一种常用的性能增强技术。因此，当您确实需要强制执行变量的可视性时，您应该只使用volatile变量。
