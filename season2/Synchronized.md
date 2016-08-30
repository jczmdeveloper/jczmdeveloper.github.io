#synchronized
---

在并发编程中，多线程同时并发访问的资源叫做临界资源，当多个线程同时访问对象并要求操作相同资源时，分割了原子操作就有可能出现数据的不一致或数据不完整的情况，为避免这种情况的发生，我们会采取同步机制，以确保在某一时刻，方法内只允许有一个线程。

采用synchronized修饰符实现的同步机制叫做互斥锁机制，它所获得的锁叫做互斥锁。每个对象都有一个monitor(锁标记)，当线程拥有这个锁标记时才能访问这个资源，没有锁标记便进入锁池。任何一个对象系统都会为其创建一个互斥锁，这个锁是为了分配给线程的，防止打断原子操作。每个对象的锁只能分配给一个线程，因此叫做互斥锁。

这里就使用同步机制获取互斥锁的情况，进行几点说明：

1. 如果同一个方法内同时有两个或更多线程，则每个线程有自己的局部变量拷贝。
2. 类的每个实例都有自己的对象级别锁。当一个线程访问实例对象中的synchronized同步代码块或同步方法时，该线程便获取了该实例的对象级别锁，其他线程这时如果要访问synchronized同步代码块或同步方法，便需要阻塞等待，直到前面的线程从同步代码块或方法中退出，释放掉了该对象级别锁。
3. 访问同一个类的不同实例对象中的同步代码块，不存在阻塞等待获取对象锁的问题，因为它们获取的是各自实例的对象级别锁，相互之间没有影响。
4. 持有一个对象级别锁不会阻止该线程被交换出来，也不会阻塞其他线程访问同一示例对象中的非synchronized代码。当一个线程A持有一个对象级别锁（即进入了synchronized修饰的代码块或方法中）时，线程也有可能被交换出去，此时线程B有可能获取执行该对象中代码的时间，但它只能执行非同步代码（没有用synchronized修饰），当执行到同步代码时，便会被阻塞，此时可能线程规划器又让A线程运行，A线程继续持有对象级别锁，当A线程退出同步代码时（即释放了对象级别锁），如果B线程此时再运行，便会获得该对象级别锁，从而执行synchronized中的代码。
5. 持有对象级别锁的线程会让其他线程阻塞在所有的synchronized代码外。例如，在一个类中有三个synchronized方法a，b，c，当线程A正在执行一个实例对象M中的方法a时，它便获得了该对象级别锁，那么其他的线程在执行同一实例对象（即对象M）中的代码时，便会在所有的synchronized方法处阻塞，即在方法a，b，c处都要被阻塞，等线程A释放掉对象级别锁时，其他的线程才可以去执行方法a，b或者c中的代码，从而获得该对象级别锁。
6. 使用synchronized（obj）同步语句块，可以获取指定对象上的对象级别锁。obj为对象的引用，如果获取了obj对象上的对象级别锁，在并发访问obj对象时时，便会在其synchronized代码处阻塞等待，直到获取到该obj对象的对象级别锁。当obj为this时，便是获取当前对象的对象级别锁。
7. 类级别锁被特定类的所有示例共享，它用于控制对static成员变量以及static方法的并发访问。具体用法与对象级别锁相似。
8. 互斥是实现同步的一种手段，临界区、互斥量和信号量都是主要的互斥实现方式。synchronized关键字经过编译后，会在同步块的前后分别形成monitorenter和monitorexit这两个字节码指令。根据虚拟机规范的要求，在执行monitorenter指令时，首先要尝试获取对象的锁，如果获得了锁，把锁的计数器加1，相应地，在执行monitorexit指令时会将锁计数器减1，当计数器为0时，锁便被释放了。由于synchronized同步块对同一个线程是可重入的，因此一个线程可以多次获得同一个对象的互斥锁，同样，要释放相应次数的该互斥锁，才能最终释放掉该锁。

##内存可见性

加锁（synchronized同步）的功能不仅仅局限于互斥行为，同时还存在另外一个重要的方面：内存可见性。我们不仅希望防止某个线程正在使用对象状态而另一个线程在同时修改该状态，而且还希望确保当一个线程修改了对象状态后，其他线程能够看到该变化。而线程的同步恰恰也能够实现这一点。

内置锁可以用于确保某个线程以一种可预测的方式来查看另一个线程的执行结果。为了确保所有的线程都能看到共享变量的最新值，可以在所有执行读操作或写操作的线程上加上同一把锁。下图示例了同步的可见性保证。

![](http://img.blog.csdn.net/20131212211029125)

当线程A执行某个同步代码块时，线程B随后进入由同一个锁保护的同步代码块，这种情况下可以保证，当锁被释放前，A看到的所有变量值（锁释放前，A看到的变量包括y和x）在B获得同一个锁后同样可以由B看到。换句话说，当线程B执行由锁保护的同步代码块时，可以看到线程A之前在同一个锁保护的同步代码块中的所有操作结果。如果在线程A unlock M之后，线程B才进入lock M，那么线程B都可以看到线程A unlock M之前的操作，可以得到i=1，j=1。如果在线程B unlock M之后，线程A才进入lock M，那么线程B就不一定能看到线程A中的操作，因此j的值就不一定是1。

现在考虑如下代码：

```
public class  MutableInteger  
{  
    private int value;  
  
    public int get(){  
        return value;  
    }  
    public void set(int value){  
        this.value = value;  
    }  
}  
```

以上代码中，get和set方法都在没有同步的情况下访问value。如果value被多个线程共享，假如某个线程调用了set，那么另一个正在调用get的线程可能会看到更新后的value值，也可能看不到。

通过对set和get方法进行同步，可以使MutableInteger成为一个线程安全的类，如下：

```
public class  SynchronizedInteger  
{  
    private int value;  
  
    public synchronized int get(){  
        return value;  
    }  
    public synchronized void set(int value){  
        this.value = value;  
    }  
}  
```

对set和get方法进行了同步，加上了同一把对象锁，这样get方法可以看到set方法中value值的变化，从而每次通过get方法取得的value的值都是最新的value值。     
     
	 
	

##Java 多线程：synchronized 关键字用法（修饰类，方法，静态方法，代码块）	
介绍了Java的内存模型，从而可能导致的多线程问题。synchronized就是避免这个问题的解决方法之一。除了 synchronized 的方式，还有 lock，condition，volatile，threadlocal，atomicInteger，cas等方式。
	 
synchronized 用法

它的修饰对象有几种：
1. 修饰一个类，其作用的范围是synchronized后面括号括起来的部分，作用的对象是这个类的所有对象。
2. 修饰一个方法，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象； 
3. 修改一个静态的方法，其作用的范围是整个静态方法，作用的对象是这个类的所有对象； 
4. 修饰一个代码块，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象；

修饰一个类

其作用的范围是synchronized后面括号括起来的部分，作用的对象是这个类的所有对象，如下代码：

class ClassName {
   public void method() {
      synchronized(ClassName.class) {
         // todo
      }
   }
}
修饰一个方法

Synchronized修饰一个方法很简单，就是在方法的前面加synchronized，例如：

public synchronized void method()
{
   // todo
}
另外，有几点需要注意：
1. 在定义接口方法时不能使用synchronized关键字。
2. 构造方法不能使用synchronized关键字，但可以使用synchronized代码块来进行同步。 
3. synchronized 关键字不能被继承。如果子类覆盖了父类的 被 synchronized 关键字修饰的方法，那么子类的该方法只要没有 synchronized 关键字，那么就默认没有同步，也就是说，不能继承父类的 synchronized。

修饰静态方法

我们知道静态方法是属于类的而不属于对象的。同样的，synchronized修饰的静态方法锁定的是这个类的所有对象。如下：

public synchronized static void method() {
   // todo
}
修饰代码块

当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。
当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块。
尤其关键的是，当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞。
第三个例子同样适用其它同步代码块。也就是说，当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。
以上规则对其它对象锁同样适用.
JDK中对 synchronized 的使用举例

对于 synchronized ，个人觉得是一种比较粗糙的加锁，尤其是对整个对象，或者整个类进行加锁的时候。例如，HashTable，它相当于 HashMap的线程安全版本。实现如下：

public synchronized V get(Object key) {
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        for (Entry<?,?> e = tab[index] ; e != null ; e = e.next) {
            if ((e.hash == hash) && e.key.equals(key)) {
                return (V)e.value;
            }
        }
        return null;
    }
可以看到，很多方法都是使用了 synchronized 进行了修饰，那么就意味如果还有别的同步方法x，这个x方法和get方法即使在没有冲突的情况下也需要等待执行。这样效率上 必然会有一定的影响，所以会有 ConcurrentHashMap 进行分段加锁。

另外，在JDK中，Collections有一个方法可以把不是线程安全的集合转化为线性安全的集合，它是这样实现的：

public static <T> Collection<T> synchronizedCollection(Collection<T> c) {
        return new SynchronizedCollection<>(c);
    }
 static class SynchronizedCollection<E> implements Collection<E>, Serializable {
        private static final long serialVersionUID = 3053995032091335093L;

        final Collection<E> c;  // Backing Collection
        final Object mutex;     // Object on which to synchronize

        SynchronizedCollection(Collection<E> c) {
            this.c = Objects.requireNonNull(c);
            mutex = this;
        }

        SynchronizedCollection(Collection<E> c, Object mutex) {
            this.c = Objects.requireNonNull(c);
            this.mutex = Objects.requireNonNull(mutex);
        }

        public int size() {
            synchronized (mutex) {return c.size();}
        }

        ......
可以看到 在构造函数中 有 mutex = this, 然后在具体的方法使用了 synchronized(mutex)，这样就会对调用该方法的对象进行加锁。还是会出现HashTable 出现的那种问题，也就是效率不高。

以上就是 JDK 对于 synchronized 用法的简单举例。