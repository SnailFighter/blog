A Java synchronized block marks a method or a block of code as synchronized. Java synchronized blocks can be used to avoid race conditions.  
Java同步块让一个方法或者代码块成为同步的。Java同步块被用于避免发生竞争条件。

### The Java synchronized Keyword  
### Java synchornized关键字
Synchronized blocks in Java are marked with the synchronized keyword. A synchronized block in Java is synchronized on some object. All synchronized blocks synchronized on the same object can only have one thread executing inside them at the same time. All other threads attempting to enter the synchronized block are blocked until the thread inside the synchronized block exits the block.   
Java中的同步快由synchronized关键字标注。在java中，同步块对一些对象同步。使用同步块的对象同时只能有一个线程执行。其他的所有的线程如果试图进入同步块将被阻塞直到同步块中的线程退出同步块。


The synchronized keyword can be used to mark four different types of blocks:  
synchronized 关键字可以用于标记不同类型的块：

Instance methods
Static methods
Code blocks inside instance methods
Code blocks inside static methods
These blocks are synchronized on different objects. Which type of synchronized block you need depends on the concrete situation.  
实例方法  
静态方法  
实例方法中的代码块  
静态方法中的代码块  
这些代码在不同的对象中同步，你需要哪种同步代码块取决于具体的情况。  


### Synchronized Instance Methods
Here is a synchronized instance method:  
### 同步的实例方法
这里是同步实例方法的例子 ：  

```
  public synchronized void add(int value){
      this.count += value;
  }
```
Notice the use of the synchronized keyword in the method declaration. This tells Java that the method is synchronized.  
注意synchronized关键字使用在方法声明时，告诉java这个方法是被同步的。  

A synchronized instance method in Java is synchronized on the instance (object) owning the method. Thus, each instance has its synchronized methods synchronized on a different object: the owning instance. Only one thread can execute inside a synchronized instance method. If more than one instance exist, then one thread at a time can execute inside a synchronized instance method per instance. One thread per instance.  
Java中的同步实例方法在拥有该方法的实例(对象)上是同步的。因此，每个实例都有它自己的同步不同对象（拥有实例的读对象）上的同步方法。只能有一个线程执行同步实例方法。如果存在多个实例方法，同时只能有一个在一个实例中执行同步实例方法。一个实例对应一个线程。

### Synchronized Static Methods
Static methods are marked as synchronized just like instance methods using the synchronized keyword. Here is a Java synchronized static method example:    
### 同步的静态方法  
静态方法标记为同步的想实例方法一项使用synchronized关键字。这里是一个java同步的静态方法例子：  

```
  public static synchronized void add(int value){
      count += value;
  }

```  
Also here the synchronized keyword tells Java that the method is synchronized.  
童谣这里的synchronized关键字告诉java该方法是同步的  

Synchronized static methods are synchronized on the class object of the class the synchronized static method belongs to. Since only one class object exists in the Java VM per class, only one thread can execute inside a static synchronized method in the same class.  
同步的静态方法被同步在拥有该同步静态方法的类对象上。因为每个类在java虚拟机中只存在一个类对象，所以只能有一个线程可以在同一个类中执行静态同步方法。  

If the static synchronized methods are located in different classes, then one thread can execute inside the static synchronized methods of each class. One thread per class regardless of which static synchronized method it calls.  
如果静态同步方法在不同的类中，那么一条线程可以执行每个类中的静态同步方法。一个类对应一个线程，不管线程调用哪个静态同步方法。  

### Synchronized Blocks in Instance Methods
You do not have to synchronize a whole method. Sometimes it is preferable to synchronize only part of a method. Java synchronized blocks inside methods makes this possible.  
### 实例方法中的同步块  
你不必非要同步整个方法。有时同步方法的一部分是更好的。方法中的java同步块使之成为可能。  

Here is a synchronized block of Java code inside an unsynchronized Java method:   
这里是一个在非同步方法中java同步块：  
```
  public void add(int value){

    synchronized(this){
       this.count += value;   
    }
  }
```  
This example uses the Java synchronized block construct to mark a block of code as synchronized. This code will now execute as if it was a synchronized method.  
这个例子使用了Java synchronized块构造来标记一个代码块，它是同步的。这段代码现在就像同步方法一样执行。  

Notice how the Java synchronized block construct takes an object in parentheses. In the example "this" is used, which is the instance the add method is called on. The object taken in the parentheses by the synchronized construct is called a monitor object. The code is said to be synchronized on the monitor object. A synchronized instance method uses the object it belongs to as monitor object.  
注意，Java synchronized块构造如何在括号中获取一个对象。在这个例子中用到了“this”，代表调用add()方法的实例。被同步构造的在圆括号中的对象陈为监视对象。这段代码表明在监视对象上是同步的。？

Only one thread can execute inside a Java code block synchronized on the same monitor object.  
在同一个监视对象的java代码块中只能有一个线程执行。

The following two examples are both synchronized on the instance they are called on. They are therefore equivalent with respect to synchronization:    
下面的两个示例在它们被调用的实例上是同步的。因此它们在同步方面是等价的:
```
  public class MyClass {
  
    public synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }

  
    public void log2(String msg1, String msg2){
       synchronized(this){
          log.writeln(msg1);
          log.writeln(msg2);
       }
    }
  }
```  
Thus only a single thread can execute inside either of the two synchronized blocks in this example.  
在例子中只能有单线程可以在其中一个同步块中执行。

Had the second synchronized block been synchronized on a different object than this, then one thread at a time had been able to execute inside each method.  
如果第二个同步块在一个不同的对象上被同步，那么一个线程就可以在每个方法中执行。


### Synchronized Blocks in Static Methods
Here are the same two examples as static methods. These methods are synchronized on the class object of the class the methods belong to:  
### 在静态方法中的同步块  
这里是静态方法相同的两个例子。这些方法在类的类对象上是同步的:
```
  public class MyClass {

    public static synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }

  
    public static void log2(String msg1, String msg2){
       synchronized(MyClass.class){
          log.writeln(msg1);
          log.writeln(msg2);  
       }
    }
  }
```  
Only one thread can execute inside any of these two methods at the same time.  
只能有一个线程可以同时在这两个方法中任何一个执行。

Had the second synchronized block been synchronized on a different object than MyClass.class, then one thread could execute inside each method at the same time.  
如果第二个同步块同步在非MyClass类的不同对象上，那么一个线程可以同时在不同的方法上执行。

### Java Synchronized Example
Here is an example that starts 2 threads and have both of them call the add method on the same instance of Counter. Only one thread at a time will be able to call the add method on the same instance, because the method is synchronized on the instance it belongs to.   

### Java 同步例子
这里是一个示例，开启两条线程，两条线程调用同一个Counter实例的add()方法。在某一时刻，只能有一个线程调用这个实例的add()方法，因为该方式在其所在的实例中是同步的。 
```
 public class Counter{
     
     long count = 0;
    
     public synchronized void add(long value){
       this.count += value;
     }
  }
  public class CounterThread extends Thread{

     protected Counter counter = null;

     public CounterThread(Counter counter){
        this.counter = counter;
     }

     public void run() {
	for(int i=0; i<10; i++){
           counter.add(i);
        }
     }
  }
  public class Example {

    public static void main(String[] args){
      Counter counter = new Counter();
      Thread  threadA = new CounterThread(counter);
      Thread  threadB = new CounterThread(counter);

      threadA.start();
      threadB.start(); 
    }
  }
```  
Two threads are created. The same Counter instance is passed to both of them in their constructor. The Counter.add() method is synchronized on the instance, because the add method is an instance method, and marked as synchronized. Therefore only one of the threads can call the add() method at a time. The other thread will wait until the first thread leaves the add() method, before it can execute the method itself.  
创建了两条线程。相同的Counter实例传递给两个线程的构造器。Counter.add（）方法在实例中是同步的，因为add()方法是一个实例方法，且被标记为同步的。因此，只有一个线程在某个时刻可以执行。其他线程将一直等待在它执行执行方法前，直到第一条线程离开add()方法。

If the two threads had referenced two separate Counter instances, there would have been no problems calling the add() methods simultaneously. The calls would have been to different objects, so the methods called would also be synchronized on different objects (the object owning the method). Therefore the calls would not block. Here is how that could look:  
如果两个线程引用了两个不同的Counter实例，那么同时调用add()方法就没有问题了。调用将有不同的对象，因此调用的方法也在不同的对象上同步（有该方法的对象）。因此调用将不会被阻塞。看下面的示例：
```
  public class Example {

    public static void main(String[] args){
      Counter counterA = new Counter();
      Counter counterB = new Counter();
      Thread  threadA = new CounterThread(counterA);
      Thread  threadB = new CounterThread(counterB);

      threadA.start();
      threadB.start(); 
    }
  }
```  
Notice how the two threads, threadA and threadB, no longer reference the same counter instance. The add method of counterA and counterB are synchronized on their two owning instances. Calling add() on counterA will thus not block a call to add() on counterB.  
注意线程A与线程B 是怎么不再引用同一个counter实例的。counterA和counterB的add()方法在不同的实例中同步。调用counterA中的add()方法将不再阻塞调用counterB的add()方法。  
### Java Concurrency Utilities  
The synchronized mechanism was Java's first mechanism for synchronizing access to objects shared by multiple threads. The synchronized mechanism isn't very advanced though. That is why Java 5 got a whole set of concurrency utility classes to help developers implement more fine grained concurrency control than what you get with synchronized.  
### java并发工具 
同步机制是java中对线程同步访问共享变量的第一种机制。因为同步机制尽管不是很先进，这就是为什么在java5 中设置了一整套的并发工具类来帮助开发者实现比同步更好的并发控制。
