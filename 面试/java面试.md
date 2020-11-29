# 基础知识

## 面向对象的特征有哪些方面？

- 抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。
- 继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段
- 封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口
- 多态性：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。方法重载（overload）实现的是编译时的多态性（也称为前绑定，同名方法参数不同），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：1). 方法重写（子类继承父类并重写父类中已有的或抽象的方法）；2). 对象造型（用父类型引用引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。

## 基本的数据类型

Java中的基本数据类型只有8个：byte、short、int、long、float、double、char、boolean；除了基本类型（primitive type）和枚举类型（enumeration type），剩下的都是引用类型（reference type）。

## &和&&的区别

&运算符有两种用法：(1)按位与；(2)逻辑与。&&运算符是短路与运算。逻辑与跟短路与的差别是非常巨大的，虽然二者都要求运算符左右两端的布尔值都是true整个表达式的值才是true。&&之所以称为短路运算是因为，如果&&左边的表达式的值是false，右边的表达式会被直接短路掉，不会进行运算。很多时候我们可能都需要用&&而不是&，例如在验证用户登录时判定用户名不是null而且不是空字符串，应当写为：username != null &&!username.equals("")，二者的顺序不能交换，更不能用&运算符，因为第一个条件如果不成立，根本不能进行字符串的equals比较，否则会产生NullPointerException异常。注意：逻辑或运算符（|）和短路或运算符（||）的差别也是如此。

## 最有效率的方法计算2乘以8

 2 << 3（左移3位相当于乘以2的3次方，右移3位相当于除以2的3次方）

## 创建数组方式

```java
int[] arr = {1,2,4, …}; 
int[][] arr = {{1,2,3},{4,5,6}
int[] arr = new int[3]
int[] arr = new int[]{1,2,3, …};

```

## 获取数组的长度

数组没有length()方法，有length 的属性。String 有length()方法。JavaScript中，获得字符串的长度是通过length属性得到的，这一点容易和Java混淆。

## JVM加载class文件的原理机制

JVM中类的装载是由类加载器（ClassLoader）和它的子类来实现的，Java中的类加载器是一个重要的Java运行时系统组件，它负责在运行时查找和装入类文件中的类。
由于Java的跨平台性，经过编译的Java源程序并不是一个可执行程序，而是一个或多个类文件。当Java程序需要使用某个类时，JVM会确保这个类已经被加载、连接（验证、准备和解析）和初始化。类的加载是指把类的.class文件中的数据读入到内存中，通常是创建一个字节数组读入.class文件，然后产生与所加载类对应的Class对象。加载完成后，Class对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括验证、准备（为静态变量分配内存并设置默认的初始值）和解析（将符号引用替换为直接引用）三个步骤。最后JVM对类进行初始化，包括：1)如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；2)如果类中存在初始化语句，就依次执行这些初始化语句。

类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。从Java 2（JDK 1.2）开始，类加载过程采取了父亲委托机制（PDM）。PDM更好的保证了Java平台的安全性，在该机制中，JVM自带的Bootstrap是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM不会向Java程序提供对Bootstrap的引用。下面是关于几个类加载器的说明：

​    Bootstrap：一般用本地代码实现，负责加载JVM基础核心类库（rt.jar）；
​    Extension：从java.ext.dirs系统属性所指定的目录中加载类库，它的父加载器是Bootstrap；
​    System：又叫应用类加载器，其父类是Extension。它是应用最广泛的类加载器。它从环境变量classpath或者系统属性java.class.path所指定的目录中记载类，是用户自定义加载器的默认父加载器。

## char 型变量中能不能存贮一个中文汉字

char类型可以存储一个中文汉字，因为Java中使用的编码是Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个char类型占2个字节（16比特）

## 抽象类（abstract class）和接口（interface）

抽象类和接口都不能够实例化，但可以定义抽象类和接口类型的引用。一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽象方法全部进行实现，否则该类仍然需要被声明为抽象类。接口比抽象类更加抽象，因为抽象类中可以定义构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法全部都是抽象方法。抽象类中的成员可以是private、默认、protected、public的，而接口中的成员全都是public的。抽象类中可以定义成员变量，而接口中定义的成员变量实际上都是常量。有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法。

## 如何实现对象克隆

- 实现Cloneable接口并重写Object类中的clone()方法

  ```java
  	public Object clone() {  
          Address addr = null;  
          try{  
              addr = (Address)super.clone();  
          }catch(CloneNotSupportedException e) {  
              e.printStackTrace();  
          }  
          return addr;  
      }  
  ```

- 实现Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆

  ```java
      public static <T> T clone(T obj) throws Exception {
          ByteArrayOutputStream bout = new ByteArrayOutputStream();
          ObjectOutputStream oos = new ObjectOutputStream(bout);
          oos.writeObject(obj);
  
          ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
          ObjectInputStream ois = new ObjectInputStream(bin);
          return (T) ois.readObject();
          // 说明：调用ByteArrayInputStream或ByteArrayOutputStream对象的close方法没有任何意义
          // 这两个基于内存的流只要垃圾回收器清理对象就能够释放资源，这一点不同于对外部资源（如文件流）的释放
      }
  ```

注意：基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检查是编译器完成的，不是在运行时抛出异常，这种是方案明显优于使用Object类的clone方法克隆对象。让问题在编译的时候暴露出来总是优于把问题留到运行时。

## java8中时间的获取和修改

获取时分秒

```java
	public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();
        System.out.println(cal.get(Calendar.YEAR));
        System.out.println(cal.get(Calendar.MONTH));    // 0 - 11
        System.out.println(cal.get(Calendar.DATE));
        System.out.println(cal.get(Calendar.HOUR_OF_DAY));
        System.out.println(cal.get(Calendar.MINUTE));
        System.out.println(cal.get(Calendar.SECOND));

        // Java 8
        LocalDateTime dt = LocalDateTime.now();
        System.out.println(dt.getYear());
        System.out.println(dt.getMonthValue());     // 1 - 12
        System.out.println(dt.getDayOfMonth());
        System.out.println(dt.getHour());
        System.out.println(dt.getMinute());
        System.out.println(dt.getSecond());
    }
```

当前时间

```java
    public static void main(String[] args) {
        SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy/MM/dd");
        Date date1 = new Date();
        System.out.println(oldFormatter.format(date1));

        // Java 8
        DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
        LocalDate date2 = LocalDate.now();
        System.out.println(date2.format(newFormatter));
    }
```

昨天

```java
	public static void main(String[] args){
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DATE, -1);
        System.out.println(cal.getTime());
    
    	//java8
    	LocalDateTime today = LocalDateTime.now();
        LocalDateTime yesterday = today.minusDays(1);

        System.out.println(yesterday);
    }
```

## ArrayList、Vector、LinkedList的存储性能和特性

ArrayList 和Vector都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号索引元素，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快而插入数据慢，Vector中的方法由于添加了synchronized修饰，因此Vector是线程安全的容器，但性能上较ArrayList差，因此已经是Java中的遗留容器。LinkedList使用双向链表实现存储（将内存中零散的内存单元通过附加的引用关联起来，形成一个可以按序号索引的线性结构，这种链式存储方式与数组的连续存储方式相比，内存的利用率更高），按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。Vector属于遗留容器（Java早期的版本中提供的容器，除此之外，Hashtable、Dictionary、BitSet、Stack、Properties都是遗留容器），已经不推荐使用，但是由于ArrayList和LinkedListed都是非线程安全的，如果遇到多个线程操作同一个容器的场景，则可以通过工具类Collections中的synchronizedList方法将其转换成线程安全的容器后再使用（这是对装潢模式的应用，将已有对象传入另一个类的构造器中创建新的对象来增强实现）。

```java
List<String> list = Collections.synchronizedList(new ArrayList<String>());
list.add("1");
list.add("2");
list.add("3");
synchronized (list) {
    Iterator i = list.iterator(); // Must be in synchronized block
    while (i.hasNext()) {
        //foo(i.next());
        System.out.println(i.next());
    }
}
```

## 多线程程序有几种实现方式

Java 5以前实现多线程有两种实现方法：一种是继承Thread类；另一种是实现Runnable接口。两种方式都要通过重写run()方法来定义线程的行为，推荐使用后者，因为Java中的继承是单继承，一个类有一个父类，如果继承了Thread类就无法再继承其他类了，显然使用Runnable接口更为灵活。

```java
public class AddMoneyThread implements Runnable {
    private Account account;    // 存入账户
    private double money;       // 存入金额
    public AddMoneyThread(Account account, double money) {
        this.account = account;
        this.money = money;
    }
    @Override
    public void run() {
        synchronized (account) {
            account.deposit(money); 
        }
    }
}

public class Test01 {
    public static void main(String[] args) {
        Account account = new Account();
        ExecutorService service = Executors.newFixedThreadPool(100);
        for(int i = 1; i <= 100; i++) {
            service.execute(new AddMoneyThread(account, 1));
        }
        service.shutdown();

        while(!service.isTerminated()) {}

        System.out.println("账户余额: " + account.getBalance());
    }
}
```

Java 5以后创建线程还有第三种方式：实现Callable接口，该接口中的call方法可以在线程执行结束时产生一个返回值

```java
class MyTask implements Callable<Integer> {
    private int upperBounds;
    public MyTask(int upperBounds) {
        this.upperBounds = upperBounds;
    }
    @Override
    public Integer call() throws Exception {
        int sum = 0; 
        for(int i = 1; i <= upperBounds; i++) {
            sum += i;
        }
        return sum;
    }
}

class Test {
    public static void main(String[] args) throws Exception {
        List<Future<Integer>> list = new ArrayList<>();
        ExecutorService service = Executors.newFixedThreadPool(10);
        for(int i = 0; i < 10; i++) {
            list.add(service.submit(new MyTask((int) (Math.random() * 100))));
        }
        int sum = 0;
        for(Future<Integer> future : list) {
            // while(!future.isDone()) ;
            sum += future.get();
        }
        System.out.println(sum);
    }
}
```

## 什么是线程池

如果并发的线程数量很多，并且每个线程都是执行一个时间很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。

最核心的ThreadPoolExecutor类：

- corePoolSize：核心池的大小。默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中；
- maximumPoolSize：线程池最大线程数，这个参数也是一个非常重要的参数，它表示在线程池中最多能创建多少个线程；
- keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。
- unit：参数keepAliveTime的时间单位，有7种取值
- workQueue：一个阻塞队列，用来存储等待执行的任务

```java
     public static void main(String[] args) {   
         ThreadPoolExecutor executor = new ThreadPoolExecutor(5, 10, 200, TimeUnit.MILLISECONDS,
                 new ArrayBlockingQueue<Runnable>(5));
          
         for(int i=0;i<15;i++){
             MyTask myTask = new MyTask(i);
             executor.execute(myTask);
             System.out.println("线程池中线程数目："+executor.getPoolSize()+"，队列中等待执行的任务数目："+
             executor.getQueue().size()+"，已执行玩别的任务数目："+executor.getCompletedTaskCount());
         }
         executor.shutdown();
     }
```

不过在java doc中，并不提倡我们直接使用ThreadPoolExecutor，而是使用Executors类中提供的几个静态方法来创建线程池：

```java
Executors.newCachedThreadPool();        //创建一个缓冲池，缓冲池容量大小为Integer.MAX_VALUE
Executors.newSingleThreadExecutor();   //创建容量为1的缓冲池
Executors.newFixedThreadPool(int);    //创建固定容量大小的缓冲池
```

从它们的具体实现来看，它们实际上也是调用了ThreadPoolExecutor，只不过参数都已配置好了。

使用方式可以看上面的多线程实现方式

## 如何实现序列化

序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决对象流读写操作时可能引发的问题（如果不进行序列化可能会存在数据乱序的问题）。
要实现序列化，需要让一个类实现Serializable接口，该接口是一个标识性接口，标注该类对象是可被序列化的，然后使用一个输出流来构造一个对象输出流并通过writeObject(Object)方法就可以将实现对象写出（即保存其状态）；如果需要反序列化则可以用一个输入流建立对象输入流，然后通过readObject方法从流中读取对象。序列化除了能够实现对象的持久化之外，还能够用于对象的深度克隆

## 数据库编程时，连接池有什么作用

由于创建连接和释放连接都有很大的开销（尤其是数据库服务器不在本地时，每次建立连接都需要进行TCP的三次握手，释放连接需要进行TCP四次握手，造成的开销是不可忽视的），为了提升系统访问数据库的性能，可以事先创建若干连接置于连接池中，需要时直接从连接池获取，使用结束时归还连接池而不必关闭连接，从而避免频繁创建和释放连接所造成的开销，这是典型的用空间换取时间的策略（浪费了空间存储连接，但节省了创建和释放连接的时间）。池化技术在Java开发中是很常见的，在使用线程时创建线程池的道理与此相同。基于Java的开源数据库连接池主要有：C3P0、Proxool、DBCP、BoneCP、Druid等。

## 事务的ACID是指什么

\- 原子性(Atomic)：事务中各项操作，要么全做要么全不做，任何一项操作的失败都会导致整个事务的失败； - 一致性(Consistent)：事务结束后系统状态是一致的； - 隔离性(Isolated)：并发执行的事务彼此无法看到对方的中间状态； - 持久性(Durable)：事务完成后所做的改动都会被持久化，即使发生灾难性的失败。通过日志和同步备份可以在故障发生后重建数据。

## 简述设计模式

- 工厂模式：工厂类可以根据条件生成不同的子类实例，这些子类有一个公共的抽象父类并且实现了相同的方法，但是这些方法针对不同的数据进行了不同的操作（多态方法）。当得到子类的实例后，开发人员可以调用基类中的方法而不必考虑到底返回的是哪一个子类的实例。

- 单例模式

  ```java
  public class Singleton {
      private static Singleton instance = null;
      private Singleton() {}
      public static synchronized Singleton getInstance(){
          if (instance == null) instance ＝ new Singleton();
          return instance;
      }
  }
  ```

## synchronized 和 volatile 的区别

- volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
- volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。
- volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性。
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
- volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。



## 什么是分布式和集群

集群：多个人在一起作同样的事 。

分布式 ：多个人在一起作不同的事 。

**将一套系统拆分成不同子系统部署在不同服务器上（这叫分布式），**

**然后部署多个相同的子系统在不同的服务器上（这叫集群），**

**部署在不同服务器上的同一个子系统应做负载均衡。** 



## 为什么不能用Executors

https://www.cnblogs.com/ants/p/11343657.html#autoid-3-3-2

首先我们看下 `ThreadPoolExecutor` 的完全构造函数

```java
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```

corePoolSize

> 核心池大小，除非设置了 `allowCoreThreadTimeOut` 否则哪怕线程超过空闲时间，池中也要最少要保留这个数目的线程。

> 需要注意的是，corePoolSize所需的线程并不是立即创建的，需要在提交任务之后进行创建，所以如果有大量的缓存线程数可以先提交一个空任务让线程池将线程先创建出来，从而提升后续的执行效率。

maximumPoolSize

> 允许的最大线程数。

keepAliveTime

> 空闲线程空闲存活时间，核心线程需要 `allowCoreThreadTimeOut` 为true才会退出。

unit

> 与 `keepAliveTime` 配合，设置 `keepAliveTime` 的单位，如：毫秒、秒。

workQueue

> 线程池中的任务队列。上面提到线程池的主要作用是复用线程来处理任务，所以我们需要一个队列来存放需要执行的任务，在使用池中的线程来处理这些任务，所以我们需要一个任务队列。

1. 当线程数小于核心线程数时，创建线程。
2. 当线程数大于等于核心线程数，且任务队列未满时，将任务放入任务队列。
3. 当线程数大于等于核心线程数，且任务队列已满
   1. 若线程数小于最大线程数，创建线程
   2. 若线程数等于最大线程数，调用拒绝执行处理程序（默认效果为：抛出异常，拒绝任务）

之所以不使用，是因为在 `workQueue` 参数直接 使用了 `new LinkedBlockingQueue()` 理论上可以无限添加任务到线程池。

如果提交到线程池的任务由问题，比如 sleep 永久，会造成内存泄漏，最终导致OOM。









# 数据

## sql优化方案

1、在表中建立索引，优先考虑where、group by使用到的字段。

2、尽量避免使用select *，返回无用的字段会降低查询效率。如下：

3、尽量避免使用in 和not in，会导致数据库引擎放弃索引进行全表扫描。如下：

4、尽量避免使用or，会导致数据库引擎放弃索引进行全表扫描。如下：

5、尽量避免在字段开头模糊查询，会导致数据库引擎放弃索引进行全表扫描。如下：

6、尽量避免进行null值的判断，会导致数据库引擎放弃索引进行全表扫描。如下：

7、尽量避免在where条件中等号的左侧进行表达式、函数操作，会导致数据库引擎放 弃索引进行全表扫描。

in 和 exists的区别: 如果子查询得出的结果集记录较少，主查询中的表较大且又有索引时应该用in, 反之如果外层的主查询记录较少，子查询中的表大，又有索引时使用exists。

## oracle和mysql的区别

(1) 对事务的提交

  MySQL默认是自动提交，而Oracle默认不自动提交，需要用户手动提交，需要在写commit;指令或者点击commit按钮

(2) 分页查询

  MySQL是直接在SQL语句中写"select... from ...where...limit  x, y",有limit就可以实现分页;而Oracle则是需要用到伪列ROWNUM和嵌套查询

(6) 并发性

  MySQL以表级锁为主，对资源锁定的粒度很大，如果一个session对一个表加锁时间过长，会让其他session无法更新此表中的数据。

 虽然InnoDB引擎的表可以用行级锁，但这个行级锁的机制依赖于表的索引，如果表没有索引，或者sql语句没有使用索引，那么仍然使用表级锁。

 Oracle使用行级锁，对资源锁定的粒度要小很多，只是锁定sql需要的资源，并且加锁是在数据库中的数据行上，不依赖与索引。所以Oracle对并发性的支持要好很多。

## 什么是数据库索引？有哪些类型？

数据库索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息。在数据库中，索引的含义与日常意义上的“索引”一词并无多大区别（想想小时候查字典），它是用于提高数据库表数据访问速度的数据库对象。

四种索引：唯一索引、非唯一索引、主键索引和聚集索引。

## 索引原理 为什么会提高效率？

索引常用两种数据结构实现：1.B-Tree 索引。2.B+Tree 索引

B+Tree区别B-Tree（B树）的地方在于，B+Tree的非叶子结点只存储导航信息，数据全部存储在叶子结点处并且用链表连接。

B+树的层级更少：相较于B树B+每个非叶子节点存储的关键字数更多，树的层级更少所以查询数据更快；

B+树查询速度更稳定：B+所有关键字数据地址都存在叶子节点上，所以每次查找的次数都相同所以查询速度要比B树更稳定;

B+树全节点遍历更快：B+树遍历整棵树只需要遍历所有的叶子节点即可，而不需要像B树一样需要对每一层进行遍历，这有利于数据库做全表扫描。

## 为什么 redis 读写速率快、性能好

1.Redis将数据存储在内存上，避免了频繁的IO操作
2.Redis其本身采用字典的数据结构，时间复杂度为O(1),且其采用渐进式的扩容手段
3.采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；

4.使用多路I/O复用模型，非阻塞IO；







# spring

## 什么是IoC和DI？DI是如何实现的？

IoC叫控制反转，是Inversion of Control的缩写，DI（Dependency Injection）叫依赖注入，是对IoC更简单的诠释。控制反转是把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象组件的装配和管理。所谓的"控制反转"就是对组件对象控制权的转移，从程序代码本身转移到了外部容器，由容器来创建对象并管理对象之间的依赖关系。

依赖注入的基本原则是应用组件不应该负责查找资源或者其他依赖的协作对象。配置对象的工作应该由容器负责，查找资源的逻辑应该从应用组件的代码中抽取出来，交给容器来完成。DI是对IoC更准确的描述，即组件之间的依赖关系由容器在运行期决定，形象的来说，即由容器动态的将某种依赖关系注入到组件之中。

## 什么叫AOP

AOP（Aspect-Oriented Programming）指一种程序设计范型，该范型以一种称为切面（aspect）的语言构造为基础，切面是一种新的模块化机制，用来描述分散在对象、类或方法中的横切关注点（crosscutting concern）。

会影响到整个应用程序的关注功能，它跟正常的业务逻辑是正交的，没有必然的联系，但是几乎所有的业务逻辑都会涉及到这些关注功能。通常，事务、日志、安全性等关注就是应用中的横切关注功能。

##  springboot 的自动装载机制

简单的说 springboot 的自动装载机制就是通过配置@EnableAutoConfiguration 将配置为@Configuration 下的@Bean 方法加载到 spring 容器中，这个过程就是 spring 自动装载机制。首先 springboot 自动装配功能是为了满足啥?是为了满足其他的插件进行扩展，因为有很多外部的 bean 我们没法管理，也不知道具体包的路径，这个时候 springboot 提供了自动装配功能，让我们外部的类能够注入到 spring 项目中。第二，如果说这个是 springboot 的自动装配功能不如说是 spring 的自动装配功能。因为 springboot 使用了 spring3.1 出来的 ImportSelector 动态 bean 的装载实现的自动装载机制，同时使用了 META-INF/spring.factories 中的 SPI 机制实现了 spring 自动扫描到自动装载的 bean 的机制。后面再说几点，spring 发展是由 XML 文件到注解方式的一个循序渐进的过程,比如@Component 以及它的派生注解@Controller 等，最后 spring 直接把 XML 文件变成了@Configuration 注解，这样理解就可以理解成 springboot 自动装载机制是把外部的 xml 文件的 Bean 配置导入到了自己的项目中，让 Bean 在自己的项目中运行。而起到关键作用的@EnableAutoConfiguration 只是作为了一个中介者的作用。





# 具体题目

## Interger缓存的使用

```
public static void main(String[] args) {
    Integer f1 = 100, f2 = 100, f3 = 150, f4 = 150;
    System.out.println(f1 == f2);
    System.out.println(f3 == f4);
}
true
false
```

四个变量都是Integer对象引用，所以下面的==运算比较的不是值而是引用。当我们给一个Integer对象赋一个int值的时候，会调用Integer类的静态方法valueOf，如果看看valueOf的源代码就知道发生了什么。

```
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

IntegerCache里面简单的说，如果整型字面量的值在-128到127之间，那么不会new新的Integer对象，而是直接引用常量池中的Integer对象

## 静态方法合构造方法的执行顺序

```java
class A {
    static {
        System.out.print("1");
    }
    public A() {
        System.out.print("2");
    }
}

class B extends A{

    static {
        System.out.print("a");
    }
    public B() {
        System.out.print("b");
    }
}

public class Hello {
    public static void main(String[] args) {
        A ab = new B();
        ab = new B();
    }
}
//输出
1a2b2b
```

## 字符串的反转及替换

可以自己写实现也可以使用String或StringBuffer/StringBuilder中的reverse方法。

```java
   public static String reverse(String originStr) {
        if(originStr == null || originStr.length() <= 1)
            return originStr;
        return reverse(originStr.substring(1)) + originStr.charAt(0);
    }
```

## 编程实现文件拷贝

使用FileStreams复制

```java
	public static void fileCopy(String source, String target) throws IOException {
        try (InputStream in = new FileInputStream(source)) {
            try (OutputStream out = new FileOutputStream(target)) {
                byte[] buffer = new byte[4096];
                int bytesToRead;
                while((bytesToRead = in.read(buffer)) != -1) {
                    out.write(buffer, 0, bytesToRead);
                }
            }
        }
    }


```

使用FileChannel复制

    public static void fileCopyNIO(String source, String target) throws IOException {
        try (FileInputStream in = new FileInputStream(source)) {
            try (FileOutputStream out = new FileOutputStream(target)) {
                FileChannel inChannel = in.getChannel();
                FileChannel outChannel = out.getChannel();
                ByteBuffer buffer = ByteBuffer.allocate(4096);
                while(inChannel.read(buffer) != -1) {
                    buffer.flip();
                    outChannel.write(buffer);
                    buffer.clear();
                }
            }
        }
    }
    private static void copyFileUsingFileChannels(File source, File dest) throws IOException {
            FileChannel inputChannel = null;
            FileChannel outputChannel = null;
        try {
            inputChannel = new FileInputStream(source).getChannel();
            outputChannel = new FileOutputStream(dest).getChannel();
            outputChannel.transferFrom(inputChannel, 0, inputChannel.size());
        } finally {
            inputChannel.close();
            outputChannel.close();
        }
    }

使用Java7的Files类复制

```jav
private static void copyFileUsingJava7Files(File source, File dest)
        throws IOException {    
        Files.copy(source.toPath(), dest.toPath());
}
```

## 文件夹遍历

```java
    public static void main(String[] args) throws IOException {
        Path initPath = Paths.get("E:\\books");
        Files.walkFileTree(initPath, new SimpleFileVisitor<Path>() {

            @Override
            public FileVisitResult visitFile(Path file, BasicFileAttributes attrs)
                    throws IOException {
                System.out.println(file.getFileName().toString());
                return FileVisitResult.CONTINUE;
            }
        });
    }
```



## 冒泡排序



## 二分查找



