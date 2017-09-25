A race condition is a special condition that may occur inside a critical section. A critical section is a section of code that is executed by multiple threads and where the sequence of execution for the threads makes a difference in the result of the concurrent execution of the critical section.  
`竞争状态是可能发生在临界状态内的特殊状态。临界状态是被多个线程执行的一段代码，在这个代码段中，线程的执行顺序影响临界状态的并发执行结果`

When the result of multiple threads executing a critical section may differ depending on the sequence in which the threads execute, the critical section is said to contain a race condition. The term race condition stems from the metaphor that the threads are racing through the critical section, and that the result of that race impacts the result of executing the critical section.  
`当多个线程执行临界状态的结果因执行循序不同而不同是，那么临界状态被称为包含竞争状态。术语“竞争状态”是多个线程执行到临界状态的比喻，竞争的结果影响到执行临界状态的结果。`

This may all sound a bit complicated, so I will elaborate more on race conditions and critical sections in the following sections.   
`这听起来有些复杂，我将在下文中更加详细地阐述竞争状态和临界状态。` 

### Critical Sections
Running more than one thread inside the same application does not by itself cause problems. The problems arise when multiple threads access the same resources. For instance the same memory (variables, arrays, or objects), systems (databases, web services etc.) or files.
### 临界状态
`在一个应用运行多个线程不会导致这个问题。当多个线程同时访问相同资源时增加该问题的风险，例如相同的内容（变量，数组或者对象），系统（数据库，web服务等）或者文件。`

In fact, problems only arise if one or more of the threads write to these resources. It is safe to let multiple threads read the same resources, as long as the resources do not change.  
`实际上，只有在一个或者多个线程对这些资源经行写操作时才会增加问题风险。只要是资源不变，多个线程读操作是安全的。`

Here is a critical section Java code example that may fail if executed by multiple threads simultaneously:    
`这里是多个线程同时执行会失败的临界区Java代码例子：`
```
  public class Counter {

     protected long count = 0;

     public void add(long value){
         this.count = this.count + value;
     }
  }
```
Imagine if two threads, A and B, are executing the add method on the same instance of the Counter class. There is no way to know when the operating system switches between the two threads. The code in the add() method is not executed as a single atomic instruction by the Java virtual machine. Rather it is executed as a set of smaller instructions, similar to this:  
`想象一下如果两个线程A和B正在执行同一个Conter类的实例的add方法。没有办法知道操作系统什么时候在这两个线程间切换。在add()方法中的代码不会被java虚拟机当作单原子操作。它被当为一些列更小的指令执行，类似于这样：`

+ Read this.count from memory into register.
+ Add value to register.
+ Write register to memory.     

+ 从内从中读取count到寄存器
+ 值添加到寄存器  
+ 从寄存器写入到内存

Observe what happens with the following mixed execution of threads A and B:  
`很明显，线程A、B如下混合执行：`  
```
       this.count = 0;

   A:  Reads this.count into a register (0)
   B:  Reads this.count into a register (0)
   B:  Adds value 2 to register
   B:  Writes register value (2) back to memory. this.count now equals 2
   A:  Adds value 3 to register
   A:  Writes register value (3) back to memory. this.count now equals 3
```  
The two threads wanted to add the values 2 and 3 to the counter. Thus the value should have been 5 after the two threads complete execution. However, since the execution of the two threads is interleaved, the result ends up being different.  
`这两个线程对counter增加2和3。在线程执行完成后，这个值应该是5，但是因为这两个线程的执行时交错的，结果是不同的。`

In the execution sequence example listed above, both threads read the value 0 from memory. Then they add their i ndividual values, 2 and 3, to the value, and write the result back to memory. Instead of 5, the value left in this.count will be the value written by the last thread to write its value. In the above case it is thread A, but it could as well have been thread B.   
`在上面的执行顺序例子中，两个线程从内从中读取0，接着它们增加了各自的值，2和3，到这个值。并且写回到内存。最终的结果是最后的线程写入的值，而不是值5.在上面的例子中，是线程A的值，但也可是是线程B的。`
### Race Conditions in Critical Sections
The code in the add() method in the example earlier contains a critical section. When multiple threads execute this critical section, race conditions occur.  
### 在临界区的竞争状态
`在例子中的add()方法的代码很容包含临界区。当多个线程执行这个临界区时，竞争发生。`

More formally, the situation where two threads compete for the same resource, where the sequence in which the resource is accessed is significant, is called race conditions. A code section that leads to race conditions is called a critical section.  
`更规范地说，在两个线程竞争相同资源的情况下，访问资源顺序很重要，则该资源称之为竞争状态。导致竞争状态的代码块成为临界区。`
### Preventing Race Conditions
To prevent race conditions from occurring you must make sure that the critical section is executed as an atomic instruction. That means that once a single thread is executing it, no other threads can execute it until the first thread has left the critical section.  
### 避免竞争状态
`为了避免发生竞争状态必须保证临界区作为原子操作执行。意思是说一旦一条线程在执行它，没有其他线程可以执行它，知道第一条线程释放了临界区。`

Race conditions can be avoided by proper thread synchronization in critical sections. Thread synchronization can be achieved using a synchronized block of Java code. Thread synchronization can also be achieved using other synchronization constructs like locks or atomic variables like java.util.concurrent.atomic.AtomicInteger.   
`在临界区中可以通过和时的线程同步避免竞争状态。线程同步可以使用java代码中的同步锁来实现。线程同步也可以使用其他的同步构造器，例如锁或者原子变量，像java.util.concurrent.atomic.AtomicInteger` 
### Critical Section Throughput
For smaller critical sections making the whole critical section a synchronized block may work. But, for larger critical sections it may be beneficial to break the critical section into smaller critical sections, to allow multiple threads to execute each a smaller critical section. This may decrease contention on the shared resource, and thus increase throughput of the total critical section.

Here is a very simplified Java code example to show what I mean:
### 临界区的吞吐量
对于更小的临界区，作为整个临界区是肯能的。但是，对于较大的临界区，分割为较小的临界区，允许多个线程去执行更小的临界区是可能是有好处的。这降低了对于共享资源的竞争，以提高整个临界区的吞吐量。  
这里是一个非常简单的java代码例子，来表达我的意思：
```
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;
    
    public void add(int val1, int val2){
        synchronized(this){
            this.sum1 += val1;   
            this.sum2 += val2;
        }
    }
}
```
Notice how the add() method adds values to two different sum member variables. To prevent race conditions the summing is executed inside a Java synchronized block. With this implementation only a single thread can ever execute the summing at the same time.

However, since the two sum variables are independent of each other, you could split their summing up into two separate synchronized blocks, like this:  
`注意add()方法是怎么向不同的sum成员变量增加值的。为了避免竞争状态，求和在java同步块中执行。通过这种实现，只有一个线程可以求和操作，在同一时刻。`  
`然而，因为这两个成员变量相互是独立的，你可以把求和分为两个同步块，像这样：`
```
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;

    private Integer sum1Lock = new Integer(1);
    private Integer sum2Lock = new Integer(2);

    public void add(int val1, int val2){
        synchronized(this.sum1Lock){
            this.sum1 += val1;   
        }
        synchronized(this.sum2Lock){
            this.sum2 += val2;
        }
    }
}
``` 
Now two threads can execute the add() method at the same time. One thread inside the first synchronized block, and another thread inside the second synchronized block. The two synchronized blocks are synchronized on different objects, so two different threads can execute the two blocks independently. This way threads will have to wait less for each other to execute the add() method.

This example is very simple, of course. In a real life shared resource the breaking down of critical sections may be a whole lot more complicated, and require more analysis of execution order possibilities.  
`现在两个线程可以同时执行add()方法。第一个同步块的线程，第二个同步块的线程，两个同步块在不同的对象上同步，因此，不同的线程可以独立地执行两个代码块。这种方式，线程可以更少等待执行add()方法。`   
`这个例子自然是很简单的。在一个真实的共享资源中，关键部分的分解可能要复杂得多，需要对执行顺序的可能性进行更多的分析。`
