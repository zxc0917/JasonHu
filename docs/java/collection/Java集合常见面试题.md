# 1 集合常见问题剖析

## 1.1 集合概述

### 1.1.1 Java集合概念 

Java除了以`Map` 结尾的类之外，其他类都实现自`Collection`接口，并且以`Map`结尾的类都实现了`Map`接口（`SetFromMap` 是`Collection的内部类`）

![](C:\Users\Administrator\Desktop\面试\JasonHu\docs\java\collection\images\Java-Collections.jpeg)

### 1.1.2 List ,Set,Map 三者的区别？

- `List`（ 对付顺序的好帮手）：存储的元素是有序、可重复的。

- `Set` (注重独一无二的性质)：存储的元素是无序的、不可重复的。

- `Map`（用key搜索的专家）：使用键值对的方式存储，key是无序、不可重复的的，value是无序的，可重复的，每一个键最多映射到一个值。

### 1.1.3 List  

 常用的三个实现类

- `ArrayList` : `ArrayList`  是最常用的List实现类，内部是通过数组实现的，它允许对元素进行快速随机访问。数组的缺点是每个元素之间不能有间隔，当数组大小不满足是需要增加存储能力，就要将已经有数组的数据复制到新数组的存储空间中。当从`ArrayList`的中插入或者删除元素时，需要对数组进行复制。移动，代价比较高，因此它适合随机查找和遍历，不适合插入和删除。

- `Vector`：`Vector`与`Arrary`一样也是通过数组实现的，不同的是它支持线程同步，，即某一时刻只有一个线程能够写`Vector`避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此它访问比`ArrayList`慢。

- `LinkedList`：`LinkedList` 使用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较慢。另外，他还提供了List接口中没有定义的方法，专门用于操作表头和表尾元素，可以当做堆、栈和双向队列使用。

### 1.1.4 Set

Set注重独一无二的性质，该体系集合用于存储无序（存入和取出的顺序不一定相同）元素，值不能重复。对象的相等性本质是对象`HashCode`值（`java`是依据对象的内存地址计算出此序列号）判断的，如果想要让两个不同的对象视为相等的，就必须覆盖`Object`的`hashCode`方法和`equals`方法.

- `HashSet`: 基于哈希表实现，存入数据是按照哈希值，所以并不是按照存入的顺序排序，为保证存入的唯一性，存入元素哈希值相同时，会使用`equals`方法比较，如果比较出不同就放入同一个哈希桶里。
- `TreeSet`： 基于红黑树实现，支持有序性操作，每一个增加一个对象都会进行排序，将对象插入的二叉树指定位置。
- `LinkHashedSet`(`HashSet`+`LinkedHashMap`)：继承于`HashSet`又是基于`LinkedHashMap`来实现的，具有`HashSet`的查找效率。 

###1.1.5 Map

1. `HashMap`: 根据键的`hashCode`值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的，`HashMap`非线程安全，底层实现为数组+链表+红黑树。
2. `ConcurrentHashMap` ：**支持并发模式操作的`HashMap`**,在`jdk1.7`和`jdk1.8`实现线程安全的方式不同。
3. `HashTable`: `HashTable` 是遗留类，不建议使用，但多映射的常用功能与`HashMap` 类似，通过`synchronized`把整个（table）表锁住来实现线程安全的，效率十分低下。
4. `TreeMap`: 红黑树实现，可排序，需要对一个有序的key集合进行遍历时建议使用。
5. `LinkedHashMap`: 是`HashMap`的一个子类，增加一条双向链表，从而可以保存记录的插入顺序，再用`Iterator`遍历 `LinkedHashMap`时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

### 1.1.5 Queue

  

 ### 1.1.6  如何选用集合

​     根据集合的特点来选用，比如我们需要根据键值获取到元素值时就选用`Map` 接口下的集合，需要排序时选择`TreeMap` ,不需要排序选择`HashMap`，需要保证线程安全就选用`ConcurrentHashMap`.

### 1.1.7 有哪些集合是线程安全的？怎么解决呢？

  我们常用`ArrayList`,`LinkedList`,`HashMap`,`TreeSet`,`PrioritQueue`都是线程不安全的，解决办法很简单，可以使用线程安全的集合来替代。（java.util.concurrent）

1. `ConcurrentHashMap` : 可以看作是线程安全的`HashMap` 
2. `CopyOnWriteArrayList`可以看作是线程安全的`ArrayList`，在读多写少的场合性能非常好，远远好于`Vector`.
3. `ConcurrentLinkedQueue`：高效的并发队列，使用链表实现，可以看做一个线程安全的`LinkedList`,这是一个非阻塞队列。
4. `BlockingQueue`：这是一个接口，jdk 内部通过链表、数组等方式实现了一个线程安全的`LinkedList`,这时一个非阻塞队列。
5. `ConcurrentSkipListMap`： 跳表的实现。这是一个`Map`，使用跳表的数据结构进行快速查找。

## 1.2  Collection 子接口 List

 ### 1.2.1 ArrayList和Vector的区别？

1. Arraylist是list的主要实现类，底层使用Object[]存储，适用于频繁的查找工作，线程不安全；

2. Vector是List的古老实现类，底层使用Object[]存储，线程安全的（在读的同时写入不一定安全）

   

### 1.2.2 ArrayList与LinkedList区别？

1. 是否线程安全：都是不同步的，也就是不保证线程安全；
2. 底层数据结构：`ArrayList` 底层使用**object类型的数组**；`LikedList`底层使用的是**双向链表**数据结构（jdk1.6之前为循环链表）
3. 插入和删除是否受元素位置的影响：** ① ** `ArrayList`采用数组存储，所以插入和删除元素的时间复杂度受元素的位置的影响。





