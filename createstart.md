Java threads are objects like any other Java objects. Threads are instances of class java.lang.Thread, or instances of subclasses of this class. In addition to being objects, java threads can also execute code.   
`java线程与其他java对象一样。线程是java.lang.Thread的实例或者是该类的子类的实例。除了可作为对象外，java线程也是可执行的代码。`
### Creating and Starting Threads
### 创建开启线程
Creating a thread in Java is done like this:  
`这么做来创建一个线程：`
```
Thread thread = new Thread();
```
To start the Java thread you will call its start() method, like this:  
`为了启动java线程，你可以调用线程的start()方法，像这样：`
```
  thread.start();
``` 
This example doesn't specify any code for the thread to execute. The thread will stop again right away after it is started.  
`这个例子没有指定线程执行的任何代码，在线程启动后就立刻停止。`

There are two ways to specify what code the thread should execute. The first is to create a subclass of Thread and override the run() method. The second method is to pass an object that implements Runnable (java.lang.Runnable to the Thread constructor. Both methods are covered below.  
`有两种方式指定线程执行的代码。第一种是创建Thread的子类，覆盖run()方法。第二种方法是传递一个实现了Runnable（java.lang.Runnable）接口的对象.这两种方法可在下文中看到。`

### Thread Subclass
The first way to specify what code a thread is to run, is to create a subclass of Thread and override the run() method. The run() method is what is executed by the thread after you call start(). Here is an example of creating a Java Thread subclass:  
### Thread子类
`第一种指定线程执行代码的方法是创建Thread的子类，并覆盖run()方法。run()方法是在调用start()方法后调用的方法。这里是创建java Thread子类的例子：`
```
 public class MyThread extends Thread {

    public void run(){
       System.out.println("MyThread running");
    }
  }
```  
To create and start the above thread you can do like this:  
`可以像下面这么做开启上面的线程：`
```
  MyThread myThread = new MyThread();
  myTread.start();
```
The start() call will return as soon as the thread is started. It will not wait until the run() method is done. The run() method will execute as if executed by a different CPU. When the run() method executes it will print out the text "MyThread running".  
`一旦thread调用，start()方法会立刻返回。该`

You can also create an anonymous subclass of Thread like this:  
`也可以像这样创建一个Thread匿名类：`
```
Thread thread = new Thread(){
    public void run(){
      System.out.println("Thread Running");
    }
  }

  thread.start();
```  
This example will print out the text "Thread running" once the run() method is executed by the new thread.  
`一旦run()被新线程执行,这个例子将打印出：“Thread running”。`  
### Runnable Interface Implementation
The second way to specify what code a thread should run is by creating a class that implements java.lang.Runnable. The Runnable object can be executed by a Thread.

Here is a Java Runnable example:  
### Runnable 接口实现
`第二种指定线程执行代码的方式是实现Runnable接口。这个实现Runnable的对象可以被线程对象执行。`  
``
```
  public class MyRunnable implements Runnable {

    public void run(){
       System.out.println("MyRunnable running");
    }
  }
```
To have the run() method executed by a thread, pass an instance of MyRunnable to a Thread in its constructor. Here is how that is done:  
`传递一个MyRunnable实例到Thread的构造器中，让一个线程执行run()方法。这里展示了怎么去做：`
```
Thread thread = new Thread(new MyRunnable());
   thread.start();
```  
When the thread is started it will call the run() method of the MyRunnable instance instead of executing it's own run() method. The above example would print out the text "MyRunnable running".

You can also create an anonymous implementation of Runnable, like this:  
`当线程开启，线程将调用MyRunnable实例的run()方法而不是它（线程）自己的run()方法。上面的例子打印“MyRunnable running”。`
`你也可以像下面这样创建实现Runnable的匿名实现类：`
```
   Runnable myRunnable = new Runnable(){

     public void run(){
        System.out.println("Runnable running");
     }
   }

   Thread thread = new Thread(myRunnable);
   thread.start();
```  
### Subclass or Runnable?
There are no rules about which of the two methods that is the best. Both methods works. Personally though, I prefer implementing Runnable, and handing an instance of the implementation to a Thread instance. When having the Runnable's executed by a thread pool it is easy to queue up the Runnable instances until a thread from the pool is idle. This is a little harder to do with Thread subclasses.

Sometimes you may have to implement Runnable as well as subclass Thread. For instance, if creating a subclass of Thread that can execute more than one Runnable. This is typically the case when implementing a thread pool.  
### 子类还是Runnable接口？
`没有规定说这两种方法中哪一种最好。两种方法都起作用。个人认为，我更喜欢实现Runnable接口，把实现的实例传递给线程实例。但使用线程子类是有些难以实现这一点。`  
`有时不得不实现Runnable接口和Thread子类。例如，如果一个线程子类要执行多个Runnable，通常会在实现线程池时发生。`

### Common  : Calling run() Instead of start()
When creating and starting a thread a common mistake is to call the run() method of the Thread instead of start(), like this:   
### 
`在创建开启线程时，一个普通的错误是调用run()方法而不是调用start()方法，像这样`
```
  Thread newThread = new Thread(MyRunnable());
  newThread.run();  //should be start();
```
At first you may not notice anything because the Runnable's run() method is executed like you expected. However, it is NOT executed by the new thread you just created. Instead the run() method is executed by the thread that created the thread. In other words, the thread that executed the above two lines of code. To have the run() method of the MyRunnable instance called by the new created thread, newThread, you MUST call the newThread.start() method.  
`起初你可能没有注意到一些事情，因为接口Runnable的run()方法按你预期执行。然而，你`

### Thread Names
When you create a Java thread you can give it a name. The name can help you distinguish different threads from each other. For instance, if multiple threads write to System.out it can be handy to see which thread wrote the text. Here is an example:  
`在创建线程时可以给它指定一个名称。名字可以帮助你区分不同的线程。例如，多个线程向System.out写内容，可以方便地看到哪条线程写了该文本。这里有个例子：`
```
   Thread thread = new Thread("New Thread") {
      public void run(){
        System.out.println("run by: " + getName());
      }
   };


   thread.start();
   System.out.println(thread.getName());
```
Notice the string "New Thread" passed as parameter to the Thread constructor. This string is the name of the thread. The name can be obtained via the Thread's getName() method. You can also pass a name to a Thread when using a Runnable implementation. Here is how that looks:    
`注意字符串“New Thread”作为参数传递给Thread构造器。该字符串是该线程的名称。名称可以通过线程的getName()方法获取。你也可以给线程传递一个名称，在使用Runnable实现类时。`
```
   MyRunnable runnable = new MyRunnable();
   Thread thread = new Thread(runnable, "New Thread");

   thread.start();
   System.out.println(thread.getName());
```
Notice however, that since the MyRunnable class is not a subclass of Thread, it does not have access to the getName() method of the thread executing it.  
`但是要注意，因为MyRunnable类不是Thread的子类，没有线程getName()方法的访问权限。`

### Thread.currentThread()
The Thread.currentThread() method returns a reference to the Thread instance executing currentThread() . This way you can get access to the Java Thread object representing the thread executing a given block of code. Here is an example of how to use Thread.currentThread() :    
### Thread.currentThread()
`Thread.currentThread()方法返回对执行currentThread()方法的线程的实例。通过这种方式，您可以访问代表执行给定代码块的线程的Java线程对象。这里是一个如何使用Thread.currentThread()的例子：`
```
Thread thread = Thread.currentThread();
```
Once you have a reference to the Thread object, you can call methods on it. For instance, you can get the name of the thread currently executing the code like this:   
`一旦你有了线程对象的引用，你就可以调用其中的方法。例如，你可以获取并发执行线程的名称，代码如下：`
```
String threadName = Thread.currentThread().getName();
```

### Java Thread Example
Here is a small example. First it prints out the name of the thread executing the main() method. This thread is assigned by the JVM. Then it starts up 10 threads and give them all a number as name ("" + i). Each thread then prints its name out, and then stops executing.   
### JAVA 线程例子
`这里是一个小例子。起初，它打印执行main()方法的线程的名称。这个线程被JVM分配。接着它开启10条线程并且指定他们的位子为（“”+i）.每个线程打印出它的名字，然后停止执行。`
```
public class ThreadExample {

  public static void main(String[] args){
    System.out.println(Thread.currentThread().getName());
    for(int i=0; i<10; i++){
      new Thread("" + i){
        public void run(){
          System.out.println("Thread: " + getName() + " running");
        }
      }.start();
    }
  }
}
```
Note that even if the threads are started in sequence (1, 2, 3 etc.) they may not execute sequentially, meaning thread 1 may not be the first thread to write its name to System.out. This is because the threads are in principle executing in parallel and not sequentially. The JVM and/or operating system determines the order in which the threads are executed. This order does not have to be the same order in which they were started.  
`注意即使线程顺序开启，它们也许不顺序执行，意思是线程1也许不是第一条向System.out的线程。这是因为原则上线程是并行而非顺序执行的。JVM和/或者操作系统决定了线程的执行顺序。这个熟悉不必是与他们开启的顺序相同。`
