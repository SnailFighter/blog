The ThreadLocal class in Java enables you to create variables that can only be read and written by the same thread. Thus, even if two threads are executing the same code, and the code has a reference to a ThreadLocal variable, then the two threads cannot see each other's ThreadLocal variables.

### Creating a ThreadLocal
Here is a code example that shows how to create a ThreadLocal variable:  
`Java中的Threadlocal类可以让你创建只能同一个线程读写变量。因此，即使有两个线程执行相同的代码，这段代码引用了一个ThreadLocal变量，那么这两个线程也看不到其他线程的ThreadLocal变量。`
### 创建ThreadLocal
`这里是创建ThreadLocal变量的示例：`

```
private ThreadLocal myThreadLocal = new ThreadLocal();
```
As you can see, you instantiate a new ThreadLocal object. This only needs to be done once per thread. Even if different threads execute the same code which accesses a ThreadLococal, each thread will see only its own ThreadLocal instance. Even if two different threads set different values on the same ThreadLocal object, they cannot see each other's values.

### Accessing a ThreadLocal
Once a ThreadLocal has been created you can set the value to be stored in it like this:
`正如你看到的这样，你实例化了一个新的ThreadLocal对象。每个线程只需要实例化一次。每个线程只看到自己的ThreadLocal实例，即使不同的线程执行了相同的访问ThreadLocal的代码。即使两个不同的线程在同一个ThreadLocal中设置了不同的值，他们也不会看到其他的值。`   

### 访问ThreadLocal 
`一旦ThreadLocal被创建，你就可以像这样存储一个值：`
```
myThreadLocal.set("A thread local value");
```
You read the value stored in a ThreadLocal like this:  
`你可以像下面这样读取存储在ThreadLocal中的值：`

```
String threadLocalValue = (String) myThreadLocal.get();
```
The get() method returns an Object and the set() method takes an Object as parameter.  
`get()方法返回一个对象，set()方法接收一个对象作为参数`

### Generic ThreadLocal
You can create a generic ThreadLocal so that you do not have to typecast the value returned by get(). Here is a generic ThreadLocal example:

### 泛型ThreadLocal
`可以创建泛型ThreadLocal，这样就不用每次对get()方法返回值就行类型转换。这里是一个泛型ThreadLocal的例子：`
```
private ThreadLocal<String> myThreadLocal = new ThreadLocal<String>();
```
Now you can only store strings in the ThreadLocal instance. Additionally, you do not need to typecast the value obtained from the ThreadLocal:

`现在你只可以在ThreadLocal实例中存储字符串。并且，不需要类型转换从ThreadLocal获取的值：`

```
myThreadLocal.set("Hello ThreadLocal");

String threadLocalValue = myThreadLocal.get();
```
### Initial ThreadLocal Value

Since values set on a ThreadLocal object only are visible to the thread who set the value, no thread can set an initial value on a ThreadLocal using set() which is visible to all threads.  


Instead you can specify an initial value for a ThreadLocal object by subclassing ThreadLocal and overriding the initialValue() method. Here is how that looks:  
### 初始化Threadlocal值 
`因为被设置在ThreadLocal对象的值只对设值得线程可见，任何线程都不可设置一个对所有线程可见的值。`  
`你可以实现ThreadLocal的子类，覆盖initialValue()方法，来设置ThreadLocal对象的初始值。像这样：`

```
private ThreadLocal myThreadLocal = new ThreadLocal<String>() {
    @Override protected String initialValue() {
        return "This is the initial value";
    }
}; 
```
Now all threads will see the same initial value when calling get() before having called set() .  
`现在所偶的线程都可以通过在调用set()方法前通过调用get()方法看到初始值。`

### Full ThreadLocal Example
Here is a fully runnable Java ThreadLocal example:  
### 完整的ThreadLocal示例
`这是一个完成的可运行的ThreadLcoal示例：`

```
public class ThreadLocalExample {


    public static class MyRunnable implements Runnable {

        private ThreadLocal<Integer> threadLocal =
               new ThreadLocal<Integer>();

        @Override
        public void run() {
            threadLocal.set( (int) (Math.random() * 100D) );
    
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
            }
    
            System.out.println(threadLocal.get());
        }
    }


    public static void main(String[] args) {
        MyRunnable sharedRunnableInstance = new MyRunnable();

        Thread thread1 = new Thread(sharedRunnableInstance);
        Thread thread2 = new Thread(sharedRunnableInstance);

        thread1.start();
        thread2.start();

        thread1.join(); //wait for thread 1 to terminate
        thread2.join(); //wait for thread 2 to terminate
    }

}
```
This example creates a single MyRunnable instance which is passed to two different threads. Both threads execute the run() method, and thus sets different values on the ThreadLocal instance. If the access to the set() call had been synchronized, and it had not been a ThreadLocal object, the second thread would have overridden the value set by the first thread.

However, since it is a ThreadLocal object then the two threads cannot see each other's values. Thus, they set and get different values.  
`这个示例创建了一个MyRunnable实例，被传递到两个不同的线程中。两个线程都执行run()方法，因此在ThreadLocal实例中设置了不同的值。如果对set()调用的访问是同步的，并且它不是ThreadLocal对象，那么第二个线程将会覆盖第一个线程所设置的值。`

### InheritableThreadLocal
The InheritableThreadLocal class is a subclass of ThreadLocal. Instead of each thread having its own value inside a ThreadLocal, the InheritableThreadLocal grants access to values to a thread and all child threads created by that thread.  
`InheritableThreadLocal是ThreadLocal的子类。不同于于ThreadLocal中每个线程都有自己的值，InheritableThreadLocal授权该线程和该线程创建的所有子线程（不是该线程的子类，不要弄错了！！！）对值访问权。`
