Race conditions occur only if multiple threads are accessing the same resource, and one or more of the threads write to the resource. If multiple threads read the same resource race conditions do not occur.

We can make sure that objects shared between threads are never updated by any of the threads by making the shared objects immutable, and thereby thread safe. Here is an example:  
`竞争状态只发生在多个线程访问相同的资源和一个或多个线程写入到资源时。如果多个线程读取相同的资源竞争状态是不会发生的。`  
`我们可以确保在线程之间共享的对象不会被任何线程更新，从而使共享对象不可变，从而使线程安全。如下：`
```
public class ImmutableValue{

  private int value = 0;

  public ImmutableValue(int value){
    this.value = value;
  }

  public int getValue(){
    return this.value;
  }
}
```
Notice how the value for the ImmutableValue instance is passed in the constructor. Notice also how there is no setter method. Once an ImmutableValue instance is created you cannot change its value. It is immutable. You can read it however, using the getValue() method.

If you need to perform operations on the ImmutableValue instance you can do so by returning a new instance with the value resulting from the operation. Here is an example of an add operation:  
`注意在构造函数中传递了ImmutableValue实例的值。也要注意到没有setter方法。一旦ImmutableValue实例被创建，就不能再修改它的值了。它是不可变的。但是你可以读取它，通过getValue()方法。`  
`如果需要在ImmutableValue实例上执行操作，你可以在操作中返回新实例。这里是一个加法的操作例子：`  
```
public class ImmutableValue{

  private int value = 0;

  public ImmutableValue(int value){
    this.value = value;
  }

  public int getValue(){
    return this.value;
  }

  
  public ImmutableValue add(int valueToAdd){
    return new ImmutableValue(this.value + valueToAdd);
  }
  
}
```
Notice how the add() method returns a new ImmutableValue instance with the result of the add operation, rather than adding the value to itself.

The Reference is not Thread Safe!
It is important to remember, that even if an object is immutable and thereby thread safe, the reference to this object may not be thread safe. Look at this example:  
`注意到add()方法返回了一个带有加法操作的挤过的新ImmutableValue实例，而不是在它自身上加值。`  
### 引用不是线程安全的
`牢记即使对象是不可变的则线程是安全的，但是该对象的应用也许不是安全的是很重要的。看这个例子：`
```
public class Calculator{
  private ImmutableValue currentValue = null;

  public ImmutableValue getValue(){
    return currentValue;
  }

  public void setValue(ImmutableValue newValue){
    this.currentValue = newValue;
  }

  public void add(int newValue){
    this.currentValue = this.currentValue.add(newValue);
  }
}
```
The Calculator class holds a reference to an ImmutableValue instance. Notice how it is possible to change that reference through both the setValue() and add() methods. Therefore, even if the Calculator class uses an immutable object internally, it is not itself immutable, and therefore not thread safe. In other words: The ImmutableValue class is thread safe, but the use of it is not. This is something to keep in mind when trying to achieve thread safety through immutability.

To make the Calculator class thread safe you could have declared the getValue(), setValue(), and add() methods synchronized. That would have done the trick.  
`Calculator类持有对ImmutableValue实例的引用。注意通过setValue()与add()方法可改变这个引用。因此，即使Calculator类内部使用了不可变的对象，但是它本身仍不是不可变的，因此线程不是安全的。换句话说：ImmutableValue类是线程安全的，但是它的使用却不是。记住可以通过不可变性达到线程安全。`  
`为了使计算器类线程安全，您可以声明getValue()、setValue()和add()方法同步。这样就可以了。`
