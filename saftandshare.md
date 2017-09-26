Code that is safe to call by multiple threads simultaneously is called thread safe. If a piece of code is thread safe, then it contains no race conditions. Race condition only occur when multiple threads update shared resources. Therefore it is important to know what resources Java threads share when executing.  
`被多个线程同时调用是安全的则成为线程安全。如果一块代码是线程安全的，那么它就没有了竞争状态。竞争状态只会在多个线程更新共享资源时才会发生。因此，了解在执行时，Java线程共享资源是很重要的。`

### Local Variables
Local variables are stored in each thread's own stack. That means that local variables are never shared between threads. That also means that all local primitive variables are thread safe. Here is an example of a thread safe local primitive variable:  
### 局部变量
`局部变量被存储在每个线程自己的栈中。局部变量绝不会被线程所共享。所有的局部私有变量都是线程安全的。这里是局部私有变量线程安全的实例：`
```
public void someMethod(){

  long threadSafeInt = 0;

  threadSafeInt++;
}
```
### Local Object References
Local references to objects are a bit different. The reference itself is not shared. The object referenced however, is not stored in each threads's local stack. All objects are stored in the shared heap.

If an object created locally never escapes the method it was created in, it is thread safe. In fact you can also pass it on to other methods and objects as long as none of these methods or objects make the passed object available to other threads.

Here is an example of a thread safe local object:  
### 局部对象引用
`局部对象引用是有些不同。引用本身是不被共享的，引用不会存储在各个线程的本地栈。所有的对象都存储在共享的堆中。`  
`如果本地创建的对象不超出方法范围，那么它是线程安全的。实际中，你也可能传递这个对象到其他方法或对象，只要这些方法会对象不传递该对象到其他线程中。`  
`这里局部对象线程安全的例子：`
```
public void someMethod(){

  LocalObject localObject = new LocalObject();

  localObject.callMethod();
  method2(localObject);
}

public void method2(LocalObject localObject){
  localObject.setValue("value");
}
```

The LocalObject instance in this example is not returned from the method, nor is it passed to any other objects that are accessible from outside the someMethod() method. Each thread executing the someMethod() method will create its own LocalObject instance and assign it to the localObject reference. Therefore the use of the LocalObject here is thread safe.

In fact, the whole method someMethod() is thread safe. Even if the LocalObject instance is passed as parameter to other methods in the same class, or in other classes, the use of it is thread safe.

The only exception is of course, if one of the methods called with the LocalObject as parameter, stores the LocalObject instance in a way that allows access to it from other threads.  
`这个示例中的LocalObject实例并没有从该方法返回，也没有传递到来自someMethod()方法之外可访问的任何其他对象。每个正在执行someMethod()方法的线程都创建自己的LocalObject实例，并指定它localObject引用。因此这里的LocalObject的应用是线程安全的。`  
`实际上，整个someMethod()是线程安全的。即使LocalObject实例作为参数被传递给同类中其他的方法，或者其他的类，它的使用是线程安全的。`  
`当然，惟一的例外是，如果使用LocalObject作为参数调用的方法之一，以允许从其他线程访问LocalObject实例的方式存储LocalObject实例。`  

### Object Member Variables
Object member variables (fields) are stored on the heap along with the object. Therefore, if two threads call a method on the same object instance and this method updates object member variables, the method is not thread safe. Here is an example of a method that is not thread safe:  
### 对象的成员变量
`对象的成员变量和对象一起被存储在堆中。因此，如果两个线程调用同一个对象实例的方法更新成员变量，则这个方法是线程不安全的。这里是线程不安全的例子：`
```

public class NotThreadSafe{
    StringBuilder builder = new StringBuilder();

    public add(String text){
        this.builder.append(text);
    }
}
```
If two threads call the add() method simultaneously on the same NotThreadSafe instance then it leads to race conditions. For instance:  
`如果两个线程同时调用同一个NotTreadSaft实例的add()方法会导致竞争状态。例如：`
```
NotThreadSafe sharedInstance = new NotThreadSafe();

new Thread(new MyRunnable(sharedInstance)).start();
new Thread(new MyRunnable(sharedInstance)).start();

public class MyRunnable implements Runnable{
  NotThreadSafe instance = null;

  public MyRunnable(NotThreadSafe instance){
    this.instance = instance;
  }

  public void run(){
    this.instance.add("some text");
  }
}
```
Notice how the two MyRunnable instances share the same NotThreadSafe instance. Therefore, when they call the add() method on the NotThreadSafe instance it leads to race condition.

However, if two threads call the add() method simultaneously on different instances then it does not lead to race condition. Here is the example from before, but slightly modified:  
`注意这两个MyRunnable实例是如何共享同一个NotThreadSaft实例的。一次，当它们调用NotThreadSaft实例的add()方法会导致竞争状态。`  
`然而，如果两个线程同时调用不同实例的add()方法并不会导致竞争状态。这里是上面例子稍微修后后的例子：`
```
new Thread(new MyRunnable(new NotThreadSafe())).start();
new Thread(new MyRunnable(new NotThreadSafe())).start();
```
Now the two threads have each their own instance of NotThreadSafe so their calls to the add method doesn't interfere with each other. The code does not have race condition anymore. So, even if an object is not thread safe it can still be used in a way that doesn't lead to race condition.  
`现在两个线程拥有它们自己的NotThreadSaft的实例，所以它们对add()方法的调用不会相互冲突。这段代码在不会有竞争状态了。所以，即使一个对象不是线程安全的仍然通过不引发竞争状态的方法使用。`


### The Thread Control Escape Rule
When trying to determine if your code's access of a certain resource is thread safe you can use the thread control escape rule:

If a resource is created, used and disposed within
the control of the same thread,
and never escapes the control of this thread,
the use of that resource is thread safe.
Resources can be any shared resource like an object, array, file, database connection, socket etc. In Java you do not always explicitly dispose objects, so "disposed" means losing or null'ing the reference to the object.

Even if the use of an object is thread safe, if that object points to a shared resource like a file or database, your application as a whole may not be thread safe. For instance, if thread 1 and thread 2 each create their own database connections, connection 1 and connection 2, the use of each connection itself is thread safe. But the use of the database the connections point to may not be thread safe. For example, if both threads execute code like this:  
### 线程控制转义规则
`当试着确定访问某个资源的代码是否是线程安全的你可以使用线程控制转义规则：`  
`如果一个资源在同一个线程控制下创建、使用和配置，永远不会逃出该线程的控制，那么该资源的使用时线程安全的。资源可以是任何共享资源，例如一个对象、数组、文件、数据库连接，socket套接字等等。在Java中，您并不总是显式地处理对象，因此“dispose”意味着对对象的引用丢失或null。`  
`即使对象的使用是线程安全的，如果指向共享资源例如一个文件或者数据库的对象，你的应用作为一个整体也许不是线程安全的。例如，线程1和线程2各自创建自己的数据库连接，连接1和连接2，各自的连接本身的使用是线程安全。但是连接指向的数据库使用可能不是线程安全的。例如，如果两个线程想这样执行：`
```
check if record X exists
if not, insert record X
```
If two threads execute this simultaneously, and the record X they are checking for happens to be the same record, there is a risk that both of the threads end up inserting it. This is how:  
`如果两个线程同时执行，它们校验的记录X恰巧是同个记录，则存在两个线程最终都插入的风险，如下：`
```
Thread 1 checks if record X exists. Result = no
Thread 2 checks if record X exists. Result = no
Thread 1 inserts record X
Thread 2 inserts record X
```
This could also happen with threads operating on files or other shared resources. Therefore it is important to distinguish between whether an object controlled by a thread is the resource, or if it merely references the resource (like a database connection does).  
`这也可能在线程操作文件或者其他共享资源。因此区分是否是被同一个控制的对象或者仅仅是资源的引用（像数据库连接）是很重要的。`
