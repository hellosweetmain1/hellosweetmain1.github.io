## Java基础
***
#### &和&&的区别
- 都可以做为逻辑运算符，两边的结果都为true时，整个运算结果才为 true
- && 有短路功能，如果第一个表达式为false，则不会计算第二个表达式
- &都会计算

#### Switch语句
- 可以使用String、int、byte、char、short类型，不能使用long类型

#### Integer
- 常量池范围 -128 ~ +127

#### 八种基本数据类型
- byte、short、char、int、float、double、long、boolean

#### 四舍五入
- ceil 向上取整
```Java
Math.ceil(11.3) // 结果 12
```

- floor 向下取整
```Java
Math.floor(11.6) // 结果 11
```

- round 四舍五入
```java
Math.round(11.5) // 结果 12
```

#### 变量名词
- 实例变量

 类中的成员变量，被private、public 和默认修饰的变量
- 静态变量

  带有static关键字的变量
- 局部变量

 类中方法里的变量

#### 关键字
- native

  表示该方法要用另外一种依赖平台的编程语言实现的，比如用c语言去实现

#### 内部类
- 可以访问外部类的成员变量，不能定义静态成员
- 静态嵌套类

 当内部类使用时，只能访问外部类的静态变量，静态方法，不能访问非静态变量
 ```Java
public class InnerDemo{
  int a = 2; // 普通变量
  static int x = 8;
  static void add(){
	System.out.println(3+7);
}
  static class Inner{ // 静态内部类
   void show_x(){  
     System.out.println(x); // 可以调用静态变量
   }
   void addInner(){
     add(); //可以调用静态方法
   }
//	void show_a(){ // 不能调用非静态变量
//		System.out.println(a);
//	}
 }
public static void main(String[] args) {
	InnerDemo.Inner inner = new Inner();
	inner.show_x();
	inner.addInner();
}
 ```

#### StringBuilder、StringBuffer区别
都可以用来存储字符串，StringBuilder是单线程的，StringBuffer可以在多线程下使用。

#### 为什么不能根据返回类型来区分重载
因为方法重载要求同名的方法需要有不同的参数列表（参数类型不同、参数个数不同或者二者都不同），和方法的返回类型没有关系，所以不能用返回类型区分。

#### 创建对象的调用顺序
1. 先初始化静态成员，如果继承了某个类，那么先初始化父类的静态成员
2. 然后调用父类无参构造器（如果父类没有无参构造器，并且子类没有显示的super调用父类的构造器，则报错）
3. 再初始化非静态成员（有父类先父类）
4. 再调用自身构造器

#### 格式化日期
 ```Java
SimpleDateFormat sf = new SimpleDateFormat("yyyy-MM-dd");
Date d = new Date();
System.out.println(sf.format(d));
System.out.println("--------------------");
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
LocalDate localDate = LocalDate.now();
System.out.println(localDate.format(dateTimeFormatter));
 ```

#### Error和Exception的区别
- Error表示系统级错误和程序不必处理的异常，比如内存溢出。
- Exception表示需要捕捉或者需要程序进行处理的异常，如果程序运行正常，从不会发送的情况。

#### 事务的ACID
- 原子性 Atomic

 事务种的各项操作，要么全做要么全不做，任何一项操作的失败都会导致整个事务的失败
- 一致性 Consistent

 事务结束后系统状态是一致的
- 隔离性 Isolated

 并发执行的事务彼此无法看到对方的中间状态
- 持久性 Durable

 事务完成后所做的改动都会被持久化，即使发生灾难性的失败。通关日志和同步备份可以再故障发生后重建数据。

#### 获得类对象
- String.class
- "hello".getClass
- Class.forName("java.lang.String")

#### 单例模式
- 饿汉式

 ```java
  public class Singleton {

    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
      return instance;
    }
  }
 ```
- 懒汉式
 ```java
public class Singleton {
   private static final Singleton instance = null;

   private Singleton() {}

   public static synchronized Singleton getInstance() {
    if (instance == null) {
      instance = new Singleton();
    }
    return instance;
   }
}
 ```

#### 常见RuntimeException 异常
- DOMException
- NoSuchElementException
- ArrayIndexOutOfBoundsException
- NullPointerException
- ClassCastException

***
#### Collection

##### List
- ArrayList

 底层是Object[]数组。查询快，增删慢，线程不安全，效率高
- LinkedList

 底层是一个链表。查询慢，增删快，线程不安全，效率高
- Vector

 底层是Object[]数组。查询快，增删慢，线程安全，效率低
- Arrays工具类

 ```java
 // 把数组转换成集合。但这个集合的长度不能改变，可以修改元素，但不能删除添加
public static <T> List<T> asList(T... a)
 ```
- 集合转成数组

 ```java
// ArrayList
public Object[] toArray()
 ```

##### Set
- HashSet (无序，唯一)
 1. 底层是哈希表 （单个元素为链表的数组）
 2. 保持Set集合元素是唯一的：hashCode()和equals()
- LinkedHashSet (有序)
 1. 底层是哈希表和链表组成。
 2. 哈希表保证元素的唯一。
 3. 链表保证元素的有序。
- TreeSet(排序)
 1. 底层是红黑树（自平衡的二叉树）
 2. 能对元素进行某种规则的排序
 3. 自然排序，让元素实现 Comparable接口
 4. 比较器排序，让集合构造方法接受Comparable的实现类对象

##### Collections工具类
- 排序（自然顺序）
 ```java
public static <T> void sort(List<T> list)
 ```
- 二分查找
 ```java
public static <T> int binarySearch(List<?> list,T key)
 ```
- 最大值
 ```java
public static <T> T max(Collection<?> coll)
 ```
- 反转
 ```java
public static void reverse(List<?> list)
 ```
- 随机置换
 ```java
public static void shuffle(List<?> list)
 ```

***
#### Map
##### HashMap
- 可以存 (null, null)
- 底层是哈希表（单个元素为链表的数组）
- 保持Map集合元素是唯一的：hashCode()和equals()

##### TreeMap
- key不可以是null
- 底层是红黑树（自平衡的二叉树）
- 能对元素进行某种规则的排序
- 自然排序（让元素类实现Comparable接口）
- 比较器排序 (让集合构造方法接受Comparable的实现类对象)

##### LinkedHashMap
- 可以存 (null, null)
- 底层是哈希表和链表组成
- 哈希表保证元素的唯一
- 链表保存元素的有序。

##### HashTable
- 线程安全
- 不可以存 (null, null)

#### 迭代器的好处
- 能够将遍历序列的操作与序列底层的结构分离
- 任何实现 Iterable接口的类，都可以使用 foreach循环
- 只需要实现
 ```java
Iterator<T> iterator();
 ```
- 数组没有实现 Iterable接口

#### Map和Collection之间的唯一重叠
- Map的entrySet() 和 values() 方法来产生 Collection。

***
#### 多线程
- 进程

 是一个正在执行中的程序。每一个进程执行都有一个执行顺序，该顺序是一个执行路径，或者叫一个控制单元。
- 线程

 就是进程的执行单元，执行路径。
- 意义

 提高CPU的使用率。
- 创建线程

 - 继承Thread类

   ```java
  class C extends Thread {
      public void run() {
        for (int i=0; i<1000; i++) {
          System.out.println("Thread执行run"+i);
        }
      }
  }
  public class ThreadTest {
      public static void main(String[] args) {
        C t1 = new C();
        t1.start();
        for (int i=0; i<1000; i++) {
          System.out.println("Main run"+i);
        }
      }
  }
   ```
 - 实现Runnable接口
   ```java
  class Ticket implements Runnable{
      private int tick = 100;
      @Override
      public void run() {
        while(true){
  		if(tick > 0)
  		  System.out.println(Thread.currentThread().getName()+"卖票："+(tick--));
    	  }
      }
  }
  public class ThreadDemo{
  	public static void main(String[] args) {
  		Ticket t = new Ticket();
  		new Thread(t,"窗口一").start();
  		new Thread(t,"窗口二").start();
  		new Thread(t,"窗口三").start();
  		new Thread(t,"窗口四").start();
  	}
  }

   ```
##### run()和start()的区别

 start()开启一个线程，然后JVM执行该线程的run方法。run()仅仅是对象调用方法，线程虽然创建了，但并没有运行。

##### 实现和继承方式两者的区别
- 实现方式的好处可以避免单继承的局限性。
- 两者的线程代码存放位置不同，实现存放在接口的子类的run方法，而继承则存放在 Thread子类run方法中。

##### 线程状态
![logo](http://a2.qpic.cn/psb?/84da08f1-d1bb-47d3-a1b8-aafbf230431a/Qm.79Yl4Rpwdz3UXmi8AH6bX.KmhKBJzvl70V*E4gSk!/b/dHgBAAAAAAAA&bo=7AFYAewBWAEDCSw!&rf=viewer_4)

##### 为什么操作线程的方法要定义在Object类中呢？
因为这些方法在操作同步线程时，都必须要标识它们所操作线程持有的锁，只有同一个锁上的等待线程，才可以被同一个锁上的notify唤醒。不可以对不同锁中的线程进行唤醒。也就是说，等待和唤醒必须时同一个锁，而锁可以是任意对象，所以可以被任意对象调用的方法定义在Object类中。

***
#### JVM
- 强引用

 Object obj = new Object()只要强引用还存在，垃圾回收器永远不会回收被引能用的对象。
- 软引用

 引用一些有用但并非必须的对象，垃圾回收时，会把这些对象列进回收范围进行二次回收，如果这次回收还没有足够的内存，才会抛出内存溢出异常。
- 弱引用

 强度比软引用更弱一些，被关联的对象只能生存到下一次垃圾收集发生之前。（无论内存是否足够，都会被回收）
- 虚引用

 是一种最弱的引用，也无法通过虚引用来取得一个对象实例。主要目的是为了垃圾回收时收到一个系统通知。

#### 垃圾收集算法
- 标记 – 清除算法

 缺点：效率不高，会产生大量不连续的内存碎片。不利于下次的内存分配。
- 复制算法

 解决了效率问题，代价是内存缩小为原来的一半。
- 标记 – 整理算法

 过程和标记清除算法一样，但后续步骤是让所有存活的对象都向一端移动，然后直接清理掉边界以外的内存。
- 分代收集算法（商用的主流收集算法）

 把Java堆分成新生代和老年代，对新生代采用复制算法，老生代采用“标记-清理”或“标记-整理”算法来进行回收。

#### G1垃圾收集器
- 基于“标记-整理”算法实现，不会产生内存碎片。
- 可以精确控制停顿。
