The Java memory model specifies how the Java virtual machine works with the computer's memory (RAM). The Java virtual machine is a model of a whole computer so this model naturally includes a memory model - AKA the Java memory model.

It is very important to understand the Java memory model if you want to design correctly behaving concurrent programs. The Java memory model specifies how and when different threads can see values written to shared variables by other threads, and how to synchronize access to shared variables when necessary.

The original Java memory model was insufficient, so the Java memory model was revised in Java 1.5. This version of the Java memory model is still in use in Java 8.  
`Java内存模型是指Java虚拟机如何利用计算机内存（RAM）。Java虚拟机是整个计算机的一个模型，因此这个模型自然包含了一个内存模型——也就是Java内存模型`  
`如果要正确地设计并发程序理解Java内存模型是很重要的。Java内存模型指明了`

### The Internal Java Memory Model
The Java memory model used internally in the JVM divides memory between thread stacks and the heap. This diagram illustrates the Java memory model from a logic perspective:
### 内部Java内存模型  
在JVM内部使用的Java内存模型在先线程栈与堆之间划分了内存。这张图从逻辑上解析了Java内存模型：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-1.png)  
Each thread running in the Java virtual machine has its own thread stack. The thread stack contains information about what methods the thread has called to reach the current point of execution. I will refer to this as the "call stack". As the thread executes its code, the call stack changes.
每一个运行再Java虚拟机上的线程都拥有自己的线程栈。这个线程栈包含了线程当前执行点方法的信息。我称此为“调用栈”。当线程执行
它的代码时，调用栈就发生改变。  

The thread stack also contains all local variables for each method being executed (all methods on the call stack). A thread can only access it's own thread stack. Local variables created by a thread are invisible to all other threads than the thread who created it. Even if two threads are executing the exact same code, the two threads will still create the local variables of that code in each their own thread stack. Thus, each thread has its own version of each local variable.  
线程栈也包含了每个正在执行的每个所有局部变量（在调用栈中的所有方法）。一个线程只访问它自己的线程栈。由线程创建的局部变量对其他线程时不可见的。即使两个线程执行相同的代码，这两个线程仍然在它们自己的线程栈中创建代码的局部变量。因此，每个线程都拥有自己的每个本地便量版本。  

All local variables of primitive types ( boolean, byte, short, char, int, long, float, double) are fully stored on the thread stack and are thus not visible to other threads. One thread may pass a copy of a pritimive variable to another thread, but it cannot share the primitive local variable itself.  
所有的初始类型的局部变量（boolean,byte,short,char,int,long,float,double）是完全存储在线程栈中的，因此对其他线程是不可见的。一个线程也许传递给其他线程一个原始变量一个副本，但是它不能分享原始局部变量自身。


The heap contains all objects created in your Java application, regardless of what thread created the object. This includes the object versions of the primitive types (e.g. Byte, Integer, Long etc.). It does not matter if an object was created and assigned to a local variable, or created as a member variable of another object, the object is still stored on the heap.  
堆包含了在你应用中的所有的被创建的对象，不管是哪些线程创建的对象。堆包含了原始类型对象版本（例如，Byte,Integer,Long等等）。它不关心一个对象是被创建的还是分配给一个局部变量，或者作为其他对喜爱嗯的成员变量创建的，这个对象仍然是存储在堆中的。
Here is a diagram illustrating the call stack and local variables stored on the thread stacks, and objects stored on the heap:  
这里是阐述了调用栈和存储在线程栈的局部变量和存储在堆的对象图：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-2.png)  
A local variable may be of a primitive type, in which case it is totally kept on the thread stack.  
一个局部变量如果是原始类型，在这种情况下它完全存放在线程栈中。

A local variable may also be a reference to an object. In that case the reference (the local variable) is stored on the thread stack, but the object itself if stored on the heap.  
一个局部变量也许是一个对象的引用。这种情况下，引用（该局部变量）是存储在线程栈种的，但是对象本是是存储在堆种的。

An object may contain methods and these methods may contain local variables. These local variables are also stored on the thread stack, even if the object the method belongs to is stored on the heap.  
*一个对象也可能包含了方法，并且这些方法也许又包含了局部变量。这些本地便令同样存储在线程栈种，即使这些方法所属的对象是存储在堆种的。*

An object's member variables are stored on the heap along with the object itself. That is true both when the member variable is of a primitive type, and if it is a reference to an object.  
对象的成员变量与对象自己一起被存储在堆种。当成员变量是一个原始类型，如果它是一个对象的引用时，这是正确的。

Static class variables are also stored on the heap along with the class definition.
静态类变量也与类定义一起被存储在堆中。

Objects on the heap can be accessed by all threads that have a reference to the object. When a thread has access to an object, it can also get access to that object's member variables. If two threads call a method on the same object at the same time, they will both have access to the object's member variables, but each thread will have its own copy of the local variables.  
堆中的对象可以被分配给所有引用该对象的线程。当一个线程访问一个对象时，线程也就可以访问对象的成员变量。如故两个线程同时调用同一个对象的一个方法，它们头可以获取该对象的成员变量的访问，但是每个线程将得到该局部变量的一个副本。

Here is a diagram illustrating the points above:  
这里是堆上面内容的图解：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-3.png)  
Two threads have a set of local variables. One of the local variables (Local Variable 2) point to a shared object on the heap (Object 3). The two threads each have a different reference to the same object. Their references are local variables and are thus stored in each thread's thread stack (on each). The two different references point to the same object on the heap, though.  
这两个线程又一系列的局部变量。其中一个（局部变量2）指向了一个在堆中贡献的对象。这两个线程分别又对同一个对象的不同引用。它们的引用是局部变量，因此存储在各自的线程栈种，即使这两个不同的引用指向的是堆中相同的对象。


Notice how the shared object (Object 3) has a reference to Object 2 and Object 4 as member variables (illustrated by the arrows from Object 3 to Object 2 and Object 4). Via these member variable references in Object 3 the two threads can access Object 2 and Object 4.  
注意如何将共享对象(对象3)引用对象2和对象4作为成员变量。通过这些在对象2中的成员变量两个线程可以访问对象2与对象4.

The diagram also shows a local variable which point to two different objects on the heap. In this case the references point to two different objects (Object 1 and Object 5), not the same object. In theory both threads could access both Object 1 and Object 5, if both threads had references to both objects. But in the diagram above each thread only has a reference to one of the two objects.  
此图同时展示了一个指向在堆种不同的连个对象的局部变量。在这个例子指向两个不同对象（对象1和对象5）的引用，而非同一个对象。原理上，如果两个线程引用了两个对象，线程可以访问对象1和对象5。但是在这个图中，每个线程只引用了两个对象中的一个。  

So, what kind of Java code could lead to the above memory graph? Well, code as simple as the code below:  
因此，什么样的Java代码可以线程上面的内存图？代码如下：  

```
public class MyRunnable implements Runnable() {

    public void run() {
        methodOne();
    }

    public void methodOne() {
        int localVariable1 = 45;

        MySharedObject localVariable2 =
            MySharedObject.sharedInstance;

        //... do more with local variables.

        methodTwo();
    }

    public void methodTwo() {
        Integer localVariable1 = new Integer(99);

        //... do more with local variable.
    }
}

public class MySharedObject {

    //static variable pointing to instance of MySharedObject

    public static final MySharedObject sharedInstance =
        new MySharedObject();


    //member variables pointing to two objects on the heap

    public Integer object2 = new Integer(22);
    public Integer object4 = new Integer(44);

    public long member1 = 12345;
    public long member1 = 67890;
}  
```
If two threads were executing the run() method then the diagram shown earlier would be the outcome. The run() method calls methodOne() and methodOne() calls methodTwo().  
如果两个线程执行run()方法，那么像刚才的图那样输出。run()方法调用methodO()，methodOne()调用methodTwo()方法。  

methodOne() declares a primitive local variable (localVariable1 of type int) and an local variable which is an object reference (localVariable2).  
methodOne()声明了一个原始局部变量（整形的局部变量localVariable1）和一个对象引用的局部变量（localVariable2）  

Each thread executing methodOne() will create its own copy of localVariable1 and localVariable2 on their respective thread stacks. The localVariable1 variables will be completely separated from each other, only living on each thread's thread stack. One thread cannot see what changes another thread makes to its copy of localVariable1.  
每个线程在执行methodOne()方法将在它自己的线程栈创建localVariable1和localVariable2的副本。localVariable1变量副本之间完全是独立的，只存在每个线程的线程栈中。线程看不到其他线程堆localVariable1副本的修改。

Each thread executing methodOne() will also create their own copy of localVariable2. However, the two different copies of localVariable2 both end up pointing to the same object on the heap. The code sets localVariable2 to point to an object referenced by a static variable. There is only one copy of a static variable and this copy is stored on the heap. Thus, both of the two copies of localVariable2 end up pointing to the same instance of MySharedObject which the static variable points to. The MySharedObject instance is also stored on the heap. It corresponds to Object 3 in the diagram above.  
每个线程在执行methodOne()方法同样创建它们自己的localVariable2副本。然而，这两个不同的localVariable2副本都指向了堆中的同一个对象。这段代码通过静态变量让localVariable2指向一个被静态变量引用的对象。仅有一个静态变量副本，并且该副本存储在堆中。因此，localVariable2的两个副本指向同一个MySharedObject实例，该实例被静态变量所指向。该MySharedObject实例同样存储在堆中。它与上图中的对象3相对应。

Notice how the MySharedObject class contains two member variables too. The member variables themselves are stored on the heap along with the object. The two member variables point to two other Integer objects. These Integer objects correspond to Object 2 and Object 4 in the diagram above.  
注意MySharedObject类包含了两个成员变量。这些变量自身与该对象一块存储在堆中。这两个成员变量指向了两个Integer对象。这些Integer对象与上土的对象2与对象4相应。

Notice also how methodTwo() creates a local variable named localVariable1. This local variable is an object reference to an Integer object. The method sets the localVariable1 reference to point to a new Integer instance. The localVariable1 reference will be stored in one copy per thread executing methodTwo(). The two Integer objects instantiated will be stored on the heap, but since the method creates a new Integer object every time the method is executed, two threads executing this method will create separate Integer instances. The Integer objects created inside methodTwo() correspond to Object 1 and Object 5 in the diagram above.  
也要注意methodTwo()方法创建了一个名字为localVariable1的局部变量。该局部变量是一个Integer对象的对象引用。该方法让localVariable1引用值喜爱嗯了一个新的Integer实例。该localVariable1引用将存储每个线程执行methodTwo()方法副本中。这两个被初始化的整形对象被存储在堆中，但是因为该方法每次被执行时候都会创建一个整形对象，当两个线程执行该方法将船舰独立的整形实例。这些被methodTwo()方法创建的整形对象对应这上图中的对象1与对象5。  

Notice also the two member variables in the class MySharedObject of type long which is a primitive type. Since these variables are member variables, they are still stored on the heap along with the object. Only local variables are stored on the thread stack.  
同时要注意在MySharedObject类中的两个长整形的成员变量是原始类型。因为这些变量是成员变量，它们仍然与对象一起存储在堆中。只有局部变量被存储在线程栈中。

Hardware Memory Architecture
Modern hardware memory architecture is somewhat different from the internal Java memory model. It is important to understand the hardware memory architecture too, to understand how the Java memory model works with it. This section describes the common hardware memory architecture, and a later section will describe how the Java memory model works with it.  
硬件内存架构  
现代的硬件内存架构与内部Java内存模型是有些不同的。理解硬件内存架构，理解Java内存如何利用硬件架构同样是非常重要的，该部分描述了公共硬件内存架构，下一节将描述Java内存模型如何与硬件架构工作的。

Here is a simplified diagram of modern computer hardware architecture:   
这里是一个简单的硬件计算机硬件架构图： 
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-4.png)  
A modern computer often has 2 or more CPUs in it. Some of these CPUs may have multiple cores too. The point is, that on a modern computer with 2 or more CPUs it is possible to have more than one thread running simultaneously. Each CPU is capable of running one thread at any given time. That means that if your Java application is multithreaded, one thread per CPU may be running simultaneously (concurrently) inside your Java application.  
现代的计算机通常有两个甚至更多的CPU.一些CPU可能拥有多个核心。这意味着，在现在的拥有两个或者更过个CPU的计算机可能同时有多个线程在运行。每个CPU能够在给定的任何时间运行一个线程。这意味着，如果你的Java应用是多线程的，在java应用中每个CPU可以同时运行一个线程（并发）  

Each CPU contains a set of registers which are essentially in-CPU memory. The CPU can perform operations much faster on these registers than it can perform on variables in main memory. That is because the CPU can access these registers much faster than it can access main memory.  
每个CPU都有一些在寄存器，这些寄存器是CPU内部的内存。在寄存器中CPU可以比执行在主存中的变量更快。这是因为CPU访问寄存器比主存更快。 

Each CPU may also have a CPU cache memory layer. In fact, most modern CPUs have a cache memory layer of some size. The CPU can access its cache memory much faster than main memory, but typically not as fast as it can access its internal registers. So, the CPU cache memory is somewhere in between the speed of the internal registers and main memory. Some CPUs may have multiple cache layers (Level 1 and Level 2), but this is not so important to know to understand how the Java memory model interacts with memory. What matters is to know that CPUs can have a cache memory layer of some sort.
  
每个CPU也有一个CPU缓存层。实际上，大多数现代的CPU有一个某个大小的内存缓存层。CPU可以比主存更快地访问缓存层，但是通常不会像访问其寄存器那样快。因此，CPU缓存是介于内部寄存器与主存之间。一些CPU可以有多个缓存层（一级缓存和二级缓存），但是这对于理解Java内存模型与内存如何交互的并不重要。重要的是要知道CPU可以有某些内存缓存。


A computer also contains a main memory area (RAM). All CPUs can access the main memory. The main memory area is typically much bigger than the cache memories of the CPUs.
计算机也可以包含一个主存区（RAM）。所有的CPU都可以访问主存。通常主存比CPU的缓存要大。

Typically, when a CPU needs to access main memory it will read part of main memory into its CPU cache. It may even read part of the cache into its internal registers and then perform operations on it. When the CPU needs to write the result back to main memory it will flush the value from its internal register to the cache memory, and at some point flush the value back to main memory.  
通常当CPU需要访问主存时，CPU读取部分内存到CPU缓存中，甚至读取部分缓存到内部寄存器中然后在上面执行操作。当CPU需要写回结果到主存中，它将结果从寄存器冲洗到缓存中，并在某个时刻把值冲洗到主存中。 

The values stored in the cache memory is typically flushed back to main memory when the CPU needs to store something else in the cache memory. The CPU cache can have data written to part of its memory at a time, and flush part of its memory at a time. It does not have to read / write the full cache each time it is updated. Typically the cache is updated in smaller memory blocks called "cache lines". One or more cache lines may be read into the cache memory, and one or mor cache lines may be flushed back to main memory again.  

通常，缓存中的值在CPU需要存储其他值到缓存时被冲洗到主存中。CPU缓存可以一次写入其内存的一部分，并一次刷新部分内存，而不会每次更新时就读/写所有的缓存。通常，缓存会在称为“缓存l链”的较小内存块中进行更新。一个或多个缓存链被读取到缓存，并且一个或多个缓存链刷新到主存中。

Bridging The Gap Between The Java Memory Model And The Hardware Memory Architecture
As already mentioned, the Java memory model and the hardware memory architecture are different. The hardware memory architecture does not distinguish between thread stacks and heap. On the hardware, both the thread stack and the heap are located in main memory. Parts of the thread stacks and heap may sometimes be present in CPU caches and in internal CPU registers. This is illustrated in this diagram:   
Java内存模型与硬件架构的不同，正如上面所述，Java没存模型与硬件内存架构师不同的。硬件内村架构不会区别线程栈与堆。在硬件上，线程栈与堆都在主存中。线程堆栈和堆的部分有时可能出现在CPU缓存和内部CPU寄存器中。图中说明了这一点:  
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-5.png)  
When objects and variables can be stored in various different memory areas in the computer, certain problems may occur. The two main problems are:  
在计算机中，当对象和变量被存储在不同的内存区域中，一些问题就出现了。两个主要的问题是：

+ Visibility of thread updates (writes) to shared variables.
+ Race conditions when reading, checking and writing shared variables.  
* 线程对贡献变量更新（写）的可见性。  
* 对贡献变量读，检查与写操作的竞争条件  
Both of these problems will be explained in the following sections.  
这两个问题将在下文中得到解答。

### Visibility of Shared Objects  
### 共享变量的可见性  

If two or more threads are sharing an object, without the proper use of either volatile declarations or synchronization, updates to the shared object made by one thread may not be visible to other threads.  
如果两个或多个线程共享一个对象，如果不正确使用volatile声明或同步，则对另一个线程所做的共享对象的更新可能不会对其他线程可见。  

Imagine that the shared object is initially stored in main memory. A thread running on CPU one then reads the shared object into its CPU cache. There it makes a change to the shared object. As long as the CPU cache has not been flushed back to main memory, the changed version of the shared object is not visible to threads running on other CPUs. This way each thread may end up with its own copy of the shared object, each copy sitting in a different CPU cache.   
假设共享对象最初存储在主内存中。运行在CPU1上的线程读取共享对象到它的CPU缓存中。在那里，它对共享对象进行了更改。只要CPU缓存还没有刷新到主存中，那么贡献对象的修改对其他CPU上的线程就是不可见的。这样，每个线程最终都有自己的共享对象副本，每个副本都位于不同的CPU缓存中。


The following diagram illustrates the sketched situation. One thread running on the left CPU copies the shared object into its CPU cache, and changes its count variable to 2. This change is not visible to other threads running on the right CPU, because the update to count has not been flushed back to main memory yet.  
下图阐述了大概情况。在左边CPU运行的线程复制一份共享对象到它的CPU缓存中，并且修改它的count值为2.这个变化对右边CPU中的线程是不可见的，因为目前count的修改没有刷新大主存中。
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-6.png)  
To solve this problem you can use Java's volatile keyword. The volatile keyword can make sure that a given variable is read directly from main memory, and always written back to main memory when updated.  
为了解决这个问题，你可以使用Java中lolatile关键字。这个valatile关键字可以确保给定的变量直接从主存中读取，并且当修改时写回到主存。

### Race Conditions  
### 竞争条件

If two or more threads share an object, and more than one thread updates variables in that shared object, race conditions may occur.  
如果一个或多个线程共享一个对象，并且多个线程更新共享兑现过得变量，竞争条件也许就会发生。

Imagine if thread A reads the variable count of a shared object into its CPU cache. Imagine too, that thread B does the same, but into a different CPU cache. Now thread A adds one to count, and thread B does the same. Now var1 has been incremented two times, once in each CPU cache.  
假设线程A读取共享对象的count变量到它的CPU缓存中，并且线程B也是这么做的，只是放入了不同的CPU缓存。现在线程对coutn值加1，线程B也加1，现在var1已经增加了2次，分别在每个CPU缓存中。

If these increments had been carried out sequentially, the variable count would be been incremented twice and had the original value + 2 written back to main memory.  
如果这些增量是循序执行的，那么count变量将增加两次，并且原来的值加2写入到主存中。

However, the two increments have been carried out concurrently without proper synchronization. Regardless of which of thread A and B that writes its updated version of count back to main memory, the updated value will only be 1 higher than the original value, despite the two increments.  
但是，这两次增加缺乏同步而并发的执行。不管是线程A还是线程B写回它的count更新后的值到主存中，更新后的值仅仅在原来的值上加1，而不是增加2.

This diagram illustrates an occurrence of the problem with race conditions as described above:  
该图说明了上面描述的竞争条件问题的发生：
![image](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-7.png)  
To solve this problem you can use a Java synchronized block. A synchronized block guarantees that only one thread can enter a given critical section of the code at any given time. Synchronized blocks also guarantee that all variables accessed inside the synchronized block will be read in from main memory, and when the thread exits the synchronized block, all updated variables will be flushed back to main memory again, regardless of whether the variable is declared volatile or not.   

为了解决该问题可以使用Java同步块。一个同步块保证了在给定的时间中只有一个线程可以执行的代码块。同步块还保证在同步块中访问的所有变量都将从主内存中读取，当线程退出同步块时，所有更新的变量将再次刷新到主内存，不管变量是否被声明为volatile。
