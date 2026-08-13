# JAVA
最新版本Java24 2025年3月18日发布

## Java8 到现版本的新特性及最流行两大版本

1. Java8：
    
    1. Lambda 表达式与函数式接口：如(a,b) -> a+b
        
    2. Stream API：提供声明式集合操作（如`filter()`/`map()`/`reduce()`），简化数据处理，流式操作
        
    3. Optional：封装可能为 `null` 的对象，减少空指针异常，`Optional.ofNullable(value).orElse("default")`
        
    4. 默认方法与静态方法：接口中可定义带实现的`default`方法，便于接口扩展
        
    5. CompletableFuture：增强异步编程能力，支持链式调用和组合操作
        
    6. 日期时间API（java.time）：替代`SimpleDateFormat`和`Calendar`，线程安全且易用
        
2. Java17：稳定与安全提升
    
    1. 增强的伪随机数生成器：Java 17 为伪随机数生成器 （pseudorandom number generator，PRNG，又称为确定性随机位生成器）增加了新的接口类型和实现，使得开发者更容易在应用程序中互换使用各种 PRNG 算法。（依赖于一个初始值）
        
    2. Switch 语句的模式匹配（预览）
        
    3. 向量API：优化数值计算性能
        
3. Java21：
    
    1. Switch 语句的模式匹配（转正）：允许在`switch`的`case`标签中使用模式匹配，使操作更加灵活和类型安全
        
    2. 数组模式：将模式匹配扩展到数组中，`if (arr instanceof int[] {1, 2, 3})`，可以直接判断数组`arr`是否匹配指定的模式。
        
    3. 虚拟线程：轻量级并发，通过共享堆栈的方式降低内存消耗
        
    4. Scoped Values（范围值）：提供了一种在线程间共享不可变数据的新方式，避免使用传统的线程局部存储，促进了更好的封装性和线程安全，可用于在不通过方法参数传递的情况下，传递上下文信息，如用户会话或配置设置。
        
    5. 字符串模板：更简洁、更直观的方式来动态构建字符串。通过使用占位符`${}`，我们可以将变量的值直接嵌入到字符串中，而不需要手动处理。
        
4. Java24：
    
    1. 密钥派生函数API：为不同的加密目的（如加密、认证等）生成多个不同的密钥，避免密钥重复使用带来的安全隐患。
        
    2. 提前加载类和连接
        
    
    ##   [#](https://xiaolincoding.com/interview/java.html#%E5%BA%8F%E5%88%97%E5%8C%96)序列化
    

## static关键字

1. **修饰变量**：静态变量（类变量），属于类本身，所有实例共享，存储在方法区，可通过类名直接调用。
    
2. **修饰方法**：静态方法（类方法），属于类，不能访问非静态成员（实例变量、实例方法），可通过类名直接调用。
    
3. **修饰代码块**：静态代码块，类加载时执行，且仅执行一次，用于初始化静态变量。
    
4. **修饰内部类**：静态内部类，不依赖于外部类的实例，可直接实例化，不能访问外部类的非静态成员。
    

## final关键字

`final`关键字主要有以下三个方面的作用：用于修饰类、方法和变量。

类不能被继承、方法不能被重写、变量值不能修改
![[Pasted image 20260326184536.png]]
## finally关键字

Java 异常处理的关键字，只能用于 try-catch 语句块中

### try-catch-finally 如何使用？

- `try`块：用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。
    
- `catch`块：用于处理 try 捕获到的异常。
    
- `finally` 块：无论是否捕获或处理异常，`finally` 块里的语句都会被执行。当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。
    

**注意：不要在 finally 语句块中使用 return!** 当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。这是因为 try 语句中的 return 返回值会先被暂存在一个本地变量中，当执行到 finally 语句中的 return 之后，这个本地变量的值就变为了 finally 语句中的 return 返回值。

### 用 try - catch 抛出异常的时候，会先抛出父类异常还是子类异常

子类！

错误写法：

```Java
try {
    // 可能抛出 NullPointerException（子类异常）
    String str = null;
    str.length();
} catch (Exception e) { // 父类异常先捕获
    System.out.println("捕获到 Exception");
} catch (NullPointerException e) { // 子类异常后捕获（永远无法执行）
    System.out.println("捕获到 NullPointerException");
}
```

### 可以在 Finally 语句中抛出异常吗？

- 语法上允许：`finally` 中可以抛出异常。
    
- 风险极大：会覆盖原始异常，掩盖错误真相，增加调试难度。
    
- 最佳实践：`finally` 中应避免抛异常，若必须处理可能的异常，应在内部捕获或包裹原始异常后再抛出。
    

### [finally 中的代码一定会执行吗？](https://javaguide.cn/java/basis/java-basic-questions-03.html#finally-%E4%B8%AD%E7%9A%84%E4%BB%A3%E7%A0%81%E4%B8%80%E5%AE%9A%E4%BC%9A%E6%89%A7%E8%A1%8C%E5%90%97)

不一定的！在某些情况下，finally 中的代码不会被执行。

就比如说 finally 之前虚拟机被终止运行的话，finally 中的代码就不会被执行

另外，在以下 2 种特殊情况下，`finally` 块的代码也不会被执行：

1. 程序所在的线程死亡。
    
2. 关闭 CPU
    

## Final、finally、finalize 的区别

finalize是 `Object` 类的一个方法，用于对象被垃圾回收前的资源清理（如释放非 Java 资源）。由垃圾回收器自动调用，无法保证执行时机和顺序，且 Java 9 后已标记为过时（推荐用 try-with-resources 替代）。

final 用于限制修改，finally 用于异常中的资源释放，finalize 用于垃圾回收前的清理（已过时），三者无直接关联。

## Stream 流常用函数及应用场景

- 常用函数分类：
    
    - 中间操作（返回 Stream，可链式调用）：
        
        - 过滤：`filter(Predicate)`（如筛选列表中大于 10 的元素）；
            
        - 映射：`map(Function)`（如将 `List<String>` 转为 `List<Integer>`）、`flatMap(Function)`（如将 `List<List<String>>` 拆分为单个 `String` 的 `Stream`）；
            
        - 排序：`sorted()`（自然排序）、`sorted(Comparator)`（自定义排序，如按对象属性排序）；
            
        - 去重：`distinct()`（基于`equals()`去重）；
            
        - 限制 / 跳过：`limit(long)`（取前 N 个元素）、`skip(long)`（跳过前 N 个元素）。
            
    - 终端操作（返回非 Stream，触发流计算）：
        
        - 收集：`collect(Collector)`（如`Collectors.toList()`、`Collectors.toMap()`、`Collectors.groupingBy()`分组）；
            
        - 判断：`anyMatch(Predicate)`（是否存在满足条件的元素）、`allMatch(Predicate)`（是否所有元素满足条件）、`noneMatch(Predicate)`（是否无元素满足条件）；
            
        - 统计：`count()`（元素个数）、`max(Comparator)`/`min(Comparator)`（最大 / 小元素）；
            
        - 遍历：`forEach(Consumer)`（遍历元素执行操作）。

# JAVA集合

## 常**见**的集合有哪些？(537/1759=30.5%)

Java 集合，也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 、 `Queue`。

### 说说 List, Set, Queue, Map 四者的区别？

- `List`(对付顺序的好帮手): 存储的元素是有序的、可重复的。
    
- `Set`(注重独一无二的性质): 存储的元素不可重复的。
    
- `Queue`(实现排队功能的叫号机): 按特定的排队规则来确定先后顺序，存储的元素是有序的、可重复的。
    
- `Map`(用 key 来搜索的专家): 使用键值对（key-value）存储，类似于数学上的函数 y=f(x)，"x" 代表 key，"y" 代表 value，key 是无序的、不可重复的，value 是无序的、可重复的，每个键最多映射到一个值。
    

## ArrayList和LinkedList的区别？(479/1759=27.2%)

- **是否保证****线程安全****：** `ArrayList` 和 `LinkedList` 都是不同步的，也就是不保证线程安全；
    
- **底层数据结构：** `ArrayList` 底层使用的是 **`Object`** **数组**；`LinkedList` 底层使用的是 **双向链表** 数据结构（JDK1.6 之前为循环链表，JDK1.7 取消了循环。注意双向链表和双向循环链表的区别，下面有介绍到！）
    
- **插入和删除是否受元素位置的影响：**
    
    - `ArrayList` 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行`add(E e)`方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`），时间复杂度就为 O(n)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。
        
    - `LinkedList` 采用链表存储，所以在头尾插入或者删除元素不受元素位置的影响（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()`、 `removeLast()`），时间复杂度为 O(1)，如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)`，`remove(Object o)`,`remove(int index)`）， 时间复杂度为 O(n) ，因为需要先移动到指定位置再插入和删除。
        
- **是否支持快速****随机访问****：** `LinkedList` 不支持高效的随机元素访问，而 `ArrayList`（实现了 `RandomAccess` 接口） 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。
    
- **内存****空间占用：** `ArrayList` 的空间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。
    

## LinkedHashMap 的数据结构

`LinkedHashMap` 基于 **“****哈希表** **+** **双向链表****”** 实现：

- **哈希表**：继承自 `HashMap`，底层用数组（哈希桶）存储键值对，通过 key 的哈希值计算索引，解决哈希冲突（同 `HashMap` 的链表 / 红黑树结构）。
    
- **双向链表**：在哈希表基础上，为每个键值对节点（`Entry`）增加 `prev` 和 `next` 指针，形成双向链表，用于维护键值对的 “插入顺序” 或 “访问顺序”（可通过构造函数 `LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)` 设置，`accessOrder=true` 表示访问顺序，`false` 表示插入顺序）。
    

TreeMap：红黑树（自平衡的排序二叉树）

Select s.student_name, sc.score

From student s

Left join score sc on s.student_id = sc.student_id

Left join course c on c.course_id = s.course_id

Where c.course_name='数学'

And sc.score =(

Select Distinct score

From score

Join course on score.course_id = course_id

Where course.course_name='数学'

Order by score desc

Limit 1 offset 1

);

  

## HashMap的原理是什么？(643/1759=36.6%)

“JDK 1.8 相比 1.7 在 HashMap 上主要有三个大的变化：

1. **数据结构：** 1.7 是‘数组+链表’；1.8 变成了‘数组+链表+**红黑树**’。
    
    1. 当链表长度大于 8 且数组长度大于等于 64 时，链表会转为红黑树。
        
    2. 这主要是为了解决哈希冲突严重时，查询效率从 **O(n)** 退化的问题，红黑树可以将查询效率提升到 **O(****log** **n)**。
        
2. **插入方式：** 1.7 使用**头插法**，并发扩容时可能导致链表死循环；1.8 改为了**尾插法**，解决了这个问题。
    
3. **扩容机制：** 1.8 的扩容计算更简单，元素要么在原位置，要么是‘原位置 + 旧容量’，不再像 1.7 那样需要重新 Rehash，性能更好。”
    

#### **JDK1.8 之前**

所谓 **“拉链法”** 就是：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YjlkOTVmMjdmNjJlNTIxNDFiN2Y5ODRlODE2MjViMjJfQ096djh0R21ueTNFRzdkQ1RVTzFBSUppa1V2bnFUd21fVG9rZW46U0ZhdGJKYnVjb2tPWGt4bjgwcmN4OWtTbnNmXzE3NzM2NDk0ODI6MTc3MzY1MzA4Ml9WNA)

#### JDK1.8 之后

相比于之前的版本， JDK1.8 之后在**解决哈希冲突**时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树。

这样做的目的是减少搜索时间：链表的查询效率为 O(n)（n 是链表的长度），红黑树是一种自平衡二叉搜索树，其查询效率为 O(log n)。当链表较短时，O(n) 和 O(log n) 的性能差异不明显。但当链表变长时，查询性能会显著下降。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2E4YjlhNDc1OTY0ZTBjMmJkMmM1YzJkZWU3NDA0YjdfU0lqdWtwVFRsbFRVbG00cHo0M3lWTGY3bTY2TXNlMUZfVG9rZW46Q2JaeWI5amlhb2NkSll4ZTFVaGN3RmxYbnRYXzE3NzM2NDk0ODI6MTc3MzY1MzA4Ml9WNA)

**为什么优先扩容而非直接转为红黑树？**

数组扩容能减少哈希冲突的发生概率（即将元素重新分散到新的、更大的数组中），这在多数情况下比直接转换为红黑树更高效。

红黑树需要保持自平衡，维护成本较高。并且，过早引入红黑树反而会增加复杂度。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTFjNmVhZWI3MzZlZjg4ZDgyNzRhMjU2YzAwNjM2NzBfU2ViNW4weXo2MlZrRU02SnlNU2xpd2lHdUR5V2lZVnZfVG9rZW46Ukh0SGJ6SW1Qb2VyUVR4eVJJd2NLWkU5blViXzE3NzM2NDk0ODI6MTc3MzY1MzA4Ml9WNA)

**为什么选择阈值 8 和 64？**

1. 泊松分布表明，链表长度达到 8 的概率极低（小于千万分之一）。在绝大多数情况下，链表长度都不会超过 8。阈值设置为 8，可以保证性能和空间效率的平衡。
    
2. 数组长度阈值 64 同样是经过实践验证的经验值。在小数组中扩容成本低，优先扩容可以避免过早引入红黑树。数组大小达到 64 时，冲突概率较高，此时红黑树的性能优势开始显现。
    

## HashMap如何扩容？(671/1759=38.1%)

在 HashMap 中有一个阈值的概念，HashMap 在元素数量超过阈值时，就会触发扩容，例如，如果我们创建一个大小为 16 的 HashMap，那么默认的阈值为 16 * 0.75 = 12。这意味着一旦 HashMap 中的元素数量超过 12，就会触发扩容。

扩容时HashMap 会先新建一个数组，新数组的大小是老数组的两倍，然后会将 HashMap 内的元素重新哈希，映射并搬运到新的数组中。

在JDK 1.8 及之后对扩容进行了改进，通过位运算判断新下标位置，而不需要每个节点都重新计算哈希值，提高了效率。

举例来说，如果旧数组的长度是 16（在二进制中表示为 010000），而新数组的长度是 32（在二进制中表示为 100000）。

所以重点就在 key 的 hash 值（32位）的从右往左数第五位（最高位，如果时32和64则为第六位）是否是 1，如果是1说明需要搬迁到新位置，且新位置的下标就是原下标+16(原数组大小)，如果是0说明吃不到新数组长度的高位，那就还是在原位置，不需要迁移。

Hashmap的最大容量为**2的30次方**，在Java中，整数类型（int）是32位的，其中最高位是符号位。因此，实际可用于表示数据的位数是31位。在HashMap中，为了避免整数溢出并保持效率，最大容量被限制为2的30次方。

**为什么扩容是原来的 2 倍？**

当数组初始长度为16的时候，每次扩容都为之前的2倍，那么就保证了每次扩容之后新数组的最大索引值对应的**二进制数****为全1**。通过(n-1)&hash计算映射的位置，全1保证足够分散，如果是10000，与运算之后必然只有第一位可能为1，会限制映射范围

## HashMap在并发场景中有什么问题

[www.yuque.com](https://www.yuque.com/hollis666/gg1x9v/ph44ot)

JDK1.7及以前版本，扩容的时候会将元组插入链表头部（即**头插法**），导致在多线程并发扩容的时候产生了**循环引用**的问题

JDK1.8之后改为了**尾插法**修复了这一问题

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjYwNzM3NjU4YjIwYzYyMzVhZmQ4MWY2MjZmMTQzY2RfWXRSdjN4alR6N2hZUDZDSzdCVWd0UGVHbEp6dXFQM2dfVG9rZW46SzNxY2JtZlFhb0F0cHp4OEJCemM4Z3E1bm1jXzE3NzM2NDk0ODI6MTc3MzY1MzA4Ml9WNA)

# JAVASE

## 常用注解

- @Autowired
    
- @Resource
    
- @Service
    
- @Component
    
- @Data
    
- @Transactional
    
### Spring中@Service、@Component、@Repository等注解区别是什么?
- @Component：是一个通用的组件声明注解，表示该类是一个Spring组件
- @Service：通常用于标记服务层的组件
- @Repository：用于标记数据访问层的组件，这个注解除了将类标识为Spring组件之外，还能让Spring为它提供一些持久化特定的功能，比如异常转换。
- @Controller：用于标记控制层的组件，这个注解通知Spring该类应当作为控制器处理HTTP请求。

**这些注解在Spring框架中的主要区别在于它们的语义意图，在功能上几乎没有差异!**

### @Autowired与@Resource区别

- 来源不同：@Autowired是Spring提供的，@Resource是java本身提供的
    
- 注入方式：@Autowired是通过类型进行注入，可以与`@Qualifier`注解一起使用，@Resource是默认通过名称进行注入，如果未指定名称会尝试通过类型进行匹配
    
- 属性：@Autowired需要通过 `@Qualifier` 注解来显式指定名称，`@Resource`可以通过 `name` 属性来显式指定名称。
    
- 依赖性：@Autowired需要Spring框架
    
- 使用场景：如果需要细粒度的控制注入过程，推荐@Resource，否则@Autowired更常见，可以自动装配。`@Resource` 主要用于字段和方法上的注入，不支持在构造函数或参数上使用。

### @Transactional

#### 使用方式

作用范围：

1. 方法：只能在public方法上
    
2. 类：该注解对该类中所有的 public 方法都生效。
    
3. 接口：不推荐在接口上使用
    

参数：

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NzVjNmI4MGI1MGRkODljYTFkY2JjNDE1Mzk0MzkzOTZfSllGSTFhaEw1cGdDV3dUNUZrSEVMOEZzcHR5d1EzUWVfVG9rZW46R1R2UWJwUzVLbzNENTl4WUlycmNmb1dVbnBmXzE3NzM2NDk1NTY6MTc3MzY1MzE1Nl9WNA)

#### @Transactional在什么场景会失效

1. 未捕获异常：如果一个事务方法中发生了未捕获的异常，并且异常未被处理或传播到事务边界之外，那么事务会失效，所有的数据库操作会回滚。
    
2. 事务传播属性设置不当：如果在多个事务之间存在事务嵌套，且事务传播属性配置不正确，可能导致事务失效。
    
3. 跨方法调用事务：如果一个事务方法内部调用另一个方法，而这个被调用的方法没有 @Transactional 注解，这种情况下外层事务可能会失效。
    
4. 事务在非公开的方法中失效：标注在私有方法或非public方法上会失效
    
5. 非受检异常
    
6. 多数据源的事务管理：
    
7. 被 `@Transactional` 注解的方法所在的类必须被 Spring 管理，否则不生效
    
8. 底层使用的数据库必须支持事务机制，否则不生效；
    

#### **如果异步链路里包含数据库事务，事务回滚会不会有问题？为什么？**

  

## 异常分类

1. Error（错误）：表示运行时环境的错误。错误是程序无法处理的严重问题，如系统崩溃、虚拟机错误、动态链接失败等。通常，程序不应该尝试捕获这类错误。例如，OutOfMemoryError、StackOverflowError等。
    
2. Exception（异常）：表示程序本身可以处理的异常条件。异常分为两大类：
    
    1. 非运行时异常：在编译阶段必须被处理的异常。如果程序中可能抛出这类异常，必须通过_try-catch_语句捕获它们，或者通过_throws_关键字声明它们。例如，文件不存在（FileNotFoundException）和类未找到（ClassNotFoundException）就是非运行时异常。
        
    2. 运行时异常：Java虚拟机正常运行期间可能抛出的异常的超类。例如，`NullPointerException`和`ClassCastException`（类型转换异常）就是运行时异常。这些异常是由程序运行时的操作错误引起的，编译器不会检查这些异常，程序员可以选择捕获处理它们，也可以不处理。
        

## 面向对象编程有哪些特性？(371/1759=21.1%)

面向对象编程（Object-Oriented Programming，简称 OOP）是一种以对象为核心的编程范式，它通过模拟现实世界中的事物及其关系来组织代码。OOP 具有三大核心特性：封装、继承、多态。接下来我会逐一详细说明这些特性。

第一，封装（Encapsulation）。

封装是指将数据（属性）和行为（方法）捆绑在一起，并对外隐藏对象的内部实现细节。通过访问修饰符（如 private、protected 和 public），我们可以控制哪些部分是对外可见的，哪些是内部私有的。这种机制提高了代码的安全性和可维护性。例如，在 Java 中，我们通常会将类的属性设置为 private，并通过 getter 和 setter 方法提供受控的访问方式。

第二，继承（Inheritance）。

继承允许一个类（子类）基于另一个类（父类）来构建，从而复用父类的属性和方法。通过继承，子类不仅可以拥有父类的功能，还可以扩展或重写父类的行为。Java 中使用 extends 关键字实现继承。例如，我们可以通过定义一个通用的 Animal 类，然后让 Dog 和 Cat 类继承它，这样就避免了重复编写相同的代码。继承体现了“is-a”的关系，比如“狗是一个动物”。

第三，多态（Polymorphism）。

多态是指同一个方法调用可以根据对象的实际类型表现出不同的行为。多态分为两种形式：编译时多态（方法重载）和运行时多态（方法重写）。运行时多态是通过动态绑定实现的，即程序在运行时决定调用哪个方法。例如，如果父类 Animal 有一个 makeSound() 方法，子类 Dog 和 Cat 可以分别重写这个方法，当调用 animal.makeSound() 时，具体执行的是 Dog 或 Cat 的实现。多态使得代码更加灵活和可扩展。

## 接口、普通类和抽象类有什么区别和共同点(412/1759=23.4%)

在 Java 中，接口、普通类和抽象类是构建面向对象程序的三种重要结构。普通类用于描述具体的对象，抽象类用于定义具有共性的基类，而接口则用于定义行为规范。它们各自有不同的用途和特点，但也有一定的共同点。接下来，我会从定义、方法实现、继承关系以及成员变量这4个方面详细讲解它们的区别，然后再总结它们的共同点。

第一个是定义上的区别。

普通类是一个完整的、具体的类，可以直接实例化为对象。它包含属性和方法，并且可以有构造方法。

抽象类是一个不能直接实例化的类，通常用来作为其他类的基类。它可以包含抽象方法（没有实现的方法）和具体方法（有实现的方法）。

接口是一种完全抽象的结构，用于定义行为规范。它只包含抽象方法（**Java 8 之后可以包含默认方法和****静态方法****、jdk9有私有方法（private）**）。

第二个是方法实现上的区别。

普通类的所有方法都可以有具体实现（即方法体）。

抽象类可以包含具体方法和抽象方法。

接口默认只包含抽象方法（Java 8 后可以包含默认方法和静态方法）。

第三是继承关系上的区别。

普通类支持单继承（一个类只能继承一个父类）。

抽象类也支持单继承（一个类只能继承一个抽象类）。

接口支持多实现（一个类可以实现多个接口）。

第四是成员变量上的区别。

普通类和抽象类都可以有各种类型的**成员变量**（实例变量、静态变量等）。

接口只能有常量（public static final）。

接下来讲一下共同点，一共有3点。

首先，它们都是面向对象编程的基础结构，都可以用来组织代码，实现封装、继承和多态等特性。

其次，它们都可以包含方法，尽管接口中的方法默认是抽象的。

最后，它们都可以被继承或实现，普通类可以通过继承扩展功能，抽象类和接口则需要子类继承或实现后才能使用。

## 深拷贝和浅拷贝区别了解吗？(603/1759=34.3%)

深拷贝和浅拷贝的核心区别在于是否递归地复制对象内部的引用类型数据，接下来，我会从定义、实现方式以及使用场景三个方面详细讲解它们的区别。

首先是定义上的区别，

浅拷贝是指创建一个新对象，但新对象中的引用类型字段仍然指向原对象中引用类型的内存地址。换句话说，浅拷贝只复制了对象本身，而没有复制对象内部的引用类型数据。修改新对象中的引用类型数据会影响原对象。

深拷贝是指创建一个新对象，并且递归地复制对象内部的所有引用类型数据。换句话说，深拷贝不仅复制了对象本身，还复制了对象内部的所有引用类型数据。修改新对象中的引用类型数据不会影响原对象。

其次是实现方式上的区别，

浅拷贝可以使用 Object 类的 clone() 方法，也可以使用实现 Cloneable 接口并重写 clone() 的方法。

深拷贝可以手动对引用类型字段进行递归拷贝，也可以使用序列化（Serialization）的方式将对象序列化为字节流，再反序列化为新对象。

最后是使用场景上的区别，

浅拷贝适用于当对象内部的引用类型数据不需要独立复制的情况。

深拷贝适用于当对象内部的引用类型数据需要完全独立的情况。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=M2Y5MDZkMjZjMzBiYWNhYzVjODQwM2FmZmFiMWRhY2VfOEZEdkhHaWEzOVpLNkhwNXd4YjdqZ1Jlb3lVRkZ2aFBfVG9rZW46T0ZxRGI3WGx4b0dwYUF4VE5tWmNMb0FQblhiXzE3NzM2NDk1NTY6MTc3MzY1MzE1Nl9WNA)

`BeanUtils.copyProperties`的过程是浅拷贝

## int和Integer的区别(576/1759=32.7%)

int 和 Integer 是 Java 中用于表示整数的两种类型，接下来我会从定义、使用方式以及使用场景这3个方面来详细说明。

第一个是定义上的区别，

int 是 Java 的基本数据类型，直接存储数值，占用固定的 4 字节内存空间，范围是从 -2,147,483,648 到 2,147,483,647。

而 Integer 是 int 的包装类，它是一个对象，通过引用指向存储的数值，因此除了存储数值本身外，还需要额外的内存开销。

第二个是使用方式上的区别，

int 是一种原始类型，可以直接声明和赋值。

而 Integer 必须实例化后才能使用，它提供了更多的功能，比如支持泛型、序列化、缓存以及一些实用方法。

第三个是使用场景上的区别，

当需要高效处理整数时，优先使用 int。

当需要将整数作为对象使用时，选择 Integer。

1.为什么需要 int 和 Integer？

**(1) 基本****数据类型****的需求**

在计算机底层，整数是最基础的数据类型之一，直接映射到硬件层面的操作（如寄存器运算）。为了高效处理整数运算，Java 引入了基本数据类型 int，它直接存储数值，占用固定的 4 字节内存空间。

这种设计使得 int 非常高效，但它的局限性也很明显：无法作为对象使用，不能参与面向对象编程中的某些操作（如泛型、集合类等）。

**(2) 面向对象的需求**

Java 是一门面向对象的语言，很多场景需要将数据封装成对象。例如：泛型（Generics）要求参数必须是对象类型，而不能是基本数据类型。序列化（Serialization）需要对象支持，以便将数据持久化或通过网络传输。缓存机制需要对整数进行复用，以提高性能和节省内存。

因此，Java 设计了 Integer 作为 int 的包装类，解决了这些面向对象的需求。

## 什么是自动拆箱/装箱？(587/1759=33.4%)

自动拆箱和装箱是为了提高代码的简洁性，它简化了基本数据类型与对应的包装类之间的转换。接下来我会详细解释什么是自动装箱和自动拆箱，以及它们的注意事项。

首先说一下自动装箱，

自动装箱是指将基本数据类型（如 int、double、boolean 等）自动转换为对应的包装类对象（如 Integer、Double、Boolean 等）。这个过程由编译器自动完成，无需手动调用包装类的构造方法或静态方法。

当存储一个基本数据类型到需要用到对象的场景中(例如集合)，Java 编译器会检测到基本数据类型需要被转换为包装类对象，编译器会自动调用包装类的 valueOf() 方法来创建对应的包装类对象，生成的对象会被存储到目标位置。

接下来说一下自动拆箱，

自动拆箱是指将包装类对象（如 Integer、Double、Boolean 等）自动转换为对应的基本数据类型（如 int、double、boolean 等）。同样，这个过程也是由编译器自动完成的。

当你从一个需要对象的场景中取出值并赋给基本数据类型时，Java 编译器会检测到目标变量是一个基本数据类型。编译器会自动调用包装类的 xxxValue() 方法，比如 intValue()、doubleValue() 等，来获取基本数据类型的值。返回的基本数据类型值会被赋给目标变量。

最后说一下注意事项，一共有3点需要注意

第一个是性能问题，频繁的自动装箱和拆箱可能会导致额外的性能开销，因为每次都需要创建或转换对象。

第二个是空指针异常，如果对一个 null 的包装类对象进行自动拆箱操作，会抛出 NullPointerException。

第三个是缓存机制，某些包装类（如 Integer、Boolean 等）会对常用值进行缓存。

包装类是对Java中基本类型的封装，在 JDK5 中引入了包装类的缓存机制，有助于节省内存。实现方式是在类初始化的时，提前创建好会频繁使用的包装类对象，当需要使用某个类的包装类对象时，如果该对象包装的值在缓存的范围内，就返回缓存的对象，否则就创建新的对象并返回。

**使用构造函数创建对象时不使用缓存。**例如：`Integer a = new Integer(123);`

在包装类中，浮点数类型的包装类`Float`,`Double`并没有实现常量池技术。

|   |   |   |
|---|---|---|
|基本数据类型|包装类型|缓存范围|
|byte|Byte|-128 ~ 127|
|short|Short|-128 ~ 127|
|int|Integer|-128 ~ 127|
|long|Long|-128 ~ 127|
|char|Character|0 ~ 127|
|boolean|Boolean|true,false|
|float|Float|无|
|double|Double|无|

扩展：

1. 为什么使用整型包装类时，大家多推荐使用使用`valueOf()`方法，少使用`parseXXX()`方法？
    

> 因为 Integer、Long 这种包装类有缓存机制，valueOf 方法会从缓存中取值，如果命中缓存，会减少资源的开销，parseXXX 方法没有这个机制。

2. switch语句能否作用在byte上，能否作用在long上，能否作用在string上？
    

> byte的存储范围小于int，可以向int类型进行隐式转换，所以switch可以作用在byte上
> 
> long的存储范围大于int，不能向int进行隐式转换，只能强制转换，所以switch不可以作用在long上
> 
> string在1.7版本之前不可以，1.7版本之后switch就可以作用在string上了

## 重载和重写的区别？(655/1759=37.2%)

重载常用于提供多种调用方式，而重写则用于实现多态性，增强代码的灵活性和可扩展性。接下来我会从6个方面详细说一下它们的区别。

第一是发生位置的不同，重载发生在同一个类中，而重写发生在父子类之间 。

第二是参数列表的不同，重载要求方法名相同，但参数列表必须不同。重写要求方法名和参数列表完全相同。

第三是返回值类型的不同，重载的返回值类型可以不同，而重写的返回值类型必须相同或是父类返回值类型的子类型。

第四是访问修饰符的不同，重载对访问修饰符没有限制，而重写的访问修饰符不能比父类更严格。

第五是异常声明的不同，重载对异常声明没有限制，而重写时，子类方法抛出的异常不能比父类方法抛出的异常范围更大。

第六是绑定关系的不同，重载是静态绑定，编译时确定调用哪个方法，而重写是动态绑定，运行时根据对象的实际类型决定调用哪个方法。

**方法的重写要遵循“两同两小一大”**（以下内容摘录自《疯狂 Java 讲义》，[issue#892](https://github.com/Snailclimb/JavaGuide/issues/892) ）：

- “两同”即方法名相同、形参列表相同；
    
- “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
    
- “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。
    

## ==和 equals 的区别？(667/1759=37.9%)

`==` 对于基本类型和引用类型的作用效果是不同的：

- 对于基本数据类型来说，`==` 比较的是值。
    
- 对于引用数据类型来说，`==` 比较的是对象的内存地址。
    

> 因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。

**`equals()`** 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。`equals()`方法存在于`Object`类中，而`Object`类是所有类的直接或间接父类，因此所有的类都有`equals()`方法。

`equals()` 方法存在两种使用情况：

- **类没有重写** **`equals()`****方法**：通过`equals()`比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 `Object`类`equals()`方法。
    
- **类重写了** **`equals()`****方法**：一般我们都重写 `equals()`方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(即，认为这两个对象相等)。
    

  

== 和 equals 是 Java 中用于比较的两种方式，接下来我会从5个方面来说一下它们的区别。

第一个是比较内容上，== 比较的是内存地址（引用类型）或实际值（基本数据类型），而equals 比较的是逻辑上的相等性，具体取决于类是否重写了 equals 方法。

第二个是适用范围上，== 可用于基本数据类型和引用数据类型，而 equals 只能用于引用数据类型。

第三个是默认行为上，== 始终比较的是内存地址或实际值，而equals 在未重写时与 == 行为一致，但在某些类中（如 String、Integer 等）被重写以实现内容比较。

第四个是可扩展性上，== 是操作符，无法被修改或扩展，而equals 是方法，可以在自定义类中重写以实现特定的比较逻辑。

第五个是性能上，== 性能更高，因为它直接比较内存地址或值，而equals 性能可能较低，尤其是在复杂对象中需要逐个比较属性值。

## [hashCode() 有什么用？](https://javaguide.cn/java/basis/java-basic-questions-02.html#hashcode-%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8)

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MWY4ODBmZDRmMWE4YTdhMGUyYzViY2MxZjYwNzAxMzVfVzJYcVREUG93VzRDV2tXN2Nuck5oUThxUWRwdEJQcEpfVG9rZW46QVIzRWIyMXFwb0IxNEF4VXZpUmNjWDk5bkM4XzE3NzM2NDk1NTY6MTc3MzY1MzE1Nl9WNA)

`hashCode()` 定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是：`Object` 的 `hashCode()` 方法是本地方法，也就是用 C 语言或 C++ 实现的。

### 为什么要有 hashCode？

我们以“`HashSet` 如何检查重复”为例子来说明为什么要有 `hashCode`？

下面这段内容摘自我的 Java 启蒙书《Head First Java》:

> 当你把对象加入 `HashSet` 时，`HashSet` 会先计算对象的 `hashCode` 值来判断对象加入的位置，同时也会与其他已经加入的对象的 `hashCode` 值作比较，如果没有相符的 `hashCode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashCode` 值的对象，这时会调用 `equals()` 方法来检查 `hashCode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 `equals` 的次数，相应就大大提高了执行速度。

其实， `hashCode()` 和 `equals()`都是用于比较两个对象是否相等。

**那为什么 JDK 还要同时提供这两个方法呢？**

总结下来就是：

- 如果两个对象的`hashCode` 值相等，那这两个对象不一定相等（哈希碰撞）。
    
- 如果两个对象的`hashCode` 值相等并且`equals()`方法也返回 `true`，我们才认为这两个对象相等。
    
- 如果两个对象的`hashCode` 值不相等，我们就可以直接认为这两个对象不相等。
    

### 为什么重写 equals() 时必须重写 hashCode() 方法？

因为两个相等的对象的 `hashCode` 值必须是相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**思考**：重写 `equals()` 时没有重写 `hashCode()` 方法的话，使用 `HashMap` 可能会出现什么问题。

**总结**：

- `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。
    
- 两个对象有相同的 `hashCode` 值，他们也不一定是相等的（哈希碰撞）。
    

## 什么是泛型？有什么作用？(501/1759=28.5%)

**Java** **泛型****（Generics） 是 JDK 5 中引入的一个新特性。使用泛型参数，可以增强代码的可读性以及稳定性。

编译器可以对泛型参数进行检测，并且通过泛型参数可以指定传入的对象类型。比如 `ArrayList<Person> persons = new ArrayList<Person>()` 这行代码就指明了该 `ArrayList` 对象只能传入 `Person` 对象，如果传入其他类型的对象就会报错。

泛型的作用主要有4点，

第一点是提高代码的复用性，它允许我们编写与类型无关的通用代码。

第二点是增强类型安全性，**在没有泛型的情况下，集合类（如 ArrayList）默认存储的是 Object 类型，取出元素时需要手动进行类型转换，容易引发 ClassCastException。而泛型在编译时就会进行类型检查，避免了运行时的类型错误。

第三点是简化代码，**使用泛型后，我们无需显式地进行类型转换，减少了冗余代码，提高了代码的可读性和维护性。

第四点是支持复杂的类型约束，**泛型可以通过通配符（如 ? extends T 和 ? super T）实现更复杂的类型限制，满足特定场景下的需求。

### [泛型的使用方式有哪几种？](https://javaguide.cn/java/basis/java-basic-questions-03.html#%E6%B3%9B%E5%9E%8B%E7%9A%84%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D)

泛型一般有三种使用方式:泛型类、泛型接口、泛型方法。

**1.****泛型类**：

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}
```

**2.****泛型****方法**：通过在方法返回值前添加类型参数（如 `<T>`）来定义泛型方法。

```java
   public static < E > void printArray( E[] inputArray )
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
```

使用：

```java
// 创建不同类型数组：Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```

> 注意: `public static < E > void printArray( E[] inputArray )` 一般被称为静态泛型方法;在 java 中泛型只是一个占位符，必须在传递类型后才能使用。类在实例化时才能真正的传递类型参数，由于静态方法的加载先于类的实例化，也就是说类中的泛型还没有传递真正的类型参数，静态的方法的加载就已经完成了，所以静态泛型方法是没有办法使用类上声明的泛型的。只能使用自己声明的 `<E>`
> 
> 在 Java 泛型中，`E` 和 `T` 只是命名约定上的不同标识符，并没有本质上的区别。它们都只是占位符，用于表示某种类型，并且可以在编译时替换为具体的类型。通常的约定是：
> 
> - **E**：常用来表示集合（例如 List、Set）中的“Element”（元素）。
>     
> - **T**：通常用来表示“Type”（类型），适用于更通用的场景。
>     

### [项目中哪里用到了泛型？](https://javaguide.cn/java/basis/java-basic-questions-03.html#%E9%A1%B9%E7%9B%AE%E4%B8%AD%E5%93%AA%E9%87%8C%E7%94%A8%E5%88%B0%E4%BA%86%E6%B3%9B%E5%9E%8B)

## 什么是反射？应用？(607/1759=34.5%)

反射（Reflection）是 Java 中一种强大的机制，它允许程序在运行时动态地获取类的信息并操作类的属性、方法和构造器。接下来我会详细解释反射的定义和应用场景。

首先说一下什么是反射，

反射是一种在运行时动态获取类信息的能力。通过反射，我们可以在程序运行时加载类、获取类的结构（如字段、方法、构造器等），甚至可以调用类的方法或修改字段的值。

其次，反射主要应用在这5个场景，

第一个是框架开发，很多 Java 框架都有使用反射，比如如 Spring、Hibernate 等。

第二个是动态代理，动态代理是反射的一个重要应用，常用于 AOP（面向切面编程）。通过反射，我们可以在运行时动态生成代理类，拦截方法调用并添加额外逻辑。

第三个是注解处理，注解本身不会对程序产生任何影响，但通过反射，我们可以在运行时读取注解信息并执行相应的逻辑。

第四个是插件化开发，在某些场景下，我们需要动态加载外部的类或模块。反射可以帮助我们在运行时加载这些类并调用其方法，从而实现插件化开发。

第五个是测试工具，单元测试框架（如 JUnit）利用反射来发现和运行测试方法，而无需手动指定每个测试用例。

  

使用反射实现动态加载和调用

Java 的反射机制允许程序在运行时动态加载类并调用其方法，从而解决了上述问题。

动态加载类

反射提供了 Class.forName(String className) 方法，可以根据类的全限定名（Fully Qualified Name）动态加载类。

示例代码：

```
String className = "com.example.MyPlugin"; // 从配置文件读取
Class<？> clazz = Class.forName(className); // 动态加载类
Object instance = clazz.getDeclaredConstructor().newInstance(); // 创建实例
```

动态调用方法

反射还提供了 Method 类，用于表示类中的方法，并允许在运行时调用这些方法。

示例代码：

```java
String methodName = "execute"; // 从配置文件读取
Method method = clazz.getMethod(methodName); // 获取方法
method.invoke(instance); // 调用方法
```

为什么反射能解决这个问题？

动态性：反射允许程序在运行时根据外部输入（如配置文件）动态加载类和调用方法，而无需在编译时确定。这使得程序可以适应不断变化的需求，例如用户更换插件或扩展功能。

灵活性：反射不依赖于具体的类或方法名称，而是通过字符串参数动态操作类和方法。这种灵活性非常适合框架开发、插件系统等需要高度可扩展性的场景。

解耦：使用反射后，程序与具体的类和方法解耦，用户只需提供符合约定的类和方法即可，无需修改主程序代码。

### 反射的优缺点？

反射可以让我们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利。

不过，反射让我们在运行时有了分析操作类的能力的同时，也增加了安全问题，比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

## String、StringBuilder和StringBuffer的区别

String是不可变的，StringBuilder和StringBuffer是可变的。而StringBuffer是线程安全的（方法使用`synchronized` 进行声明），而StringBuilder是非线程安全的。

String的不可变性：（1）Stringl类被final修饰，不能被继承于是方法不会被覆盖（2）用final修饰字符串内容的char[]（**JDK 9 及以后：** `private final byte[] value;`）

（3）没有提供用于修改字符串内容的公共方法，如果需要修改会创建一个新的string对象

String为什么设计成不可变的？[www.yuque.com](https://www.yuque.com/hollis666/gg1x9v/hhkgh2nsrlnf2g0g)

String '+' 怎么实现？StringBuilder.append

## 请说说 StringBuffer 的特点(383/1759=21.8%)

`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。

首先说一下什么是 StringBuffer？

StringBuffer 是一个可变的字符序列，与 String 不同，StringBuffer 的内容是可以被修改的。它的核心特点是线程安全和高效的字符串操作。

然后说一下 StringBuffer 的4个特点**，**

第一个是它具有可变性，我们可以在原有对象上直接修改字符串内容，而无需创建新的对象。

第二个它是线程安全的，StringBuffer 的所有方法都通过 synchronized 关键字修饰，因此它是线程安全的。 在多线程环境下，多个线程可以同时操作同一个 StringBuffer 对象，而不会引发数据竞争或不一致问题。

第三个是性能相对较好，StringBuffer 内部使用一个可扩容的字符数组来存储数据，当容量不足时会自动扩展。相比于 String 的不可变性（每次修改都会生成新对象），StringBuffer 在频繁修改字符串时性能更高。而相比于非线程安全的 StringBuilder ，性能略低。

第四个是包含丰富的 API，比如：append()：追加内容到字符串末尾。 insert()：在指定位置插入内容。delete()：删除指定范围的内容。 reverse()：反转字符串内容。 toString()：将 StringBuffer 转换为 String。

## 静态方法可以被继承吗?静态方法和实例方法的区别

[Java中static方法(子类能否继承，重写父类的static方法)_static方法可以被继承吗-CSDN博客](https://blog.csdn.net/qq_51598480/article/details/122439465)

父类的静态方法**可以**被子类继承，但不能被"重写"，这里的重写指我们一般默认的重写，是基于动态绑定来说的，按动态绑定来说向上转型之后在运行时调用方法时，若子类重写了该方法会调用子类的该方法。

但对于静态方法来说，不存在动态绑定这一说法，其基于的是静态绑定

另说一点：抽象**abstract修饰符无法与static组合**使用正是因为此原因，abstract方法默认是要被子类重写的（要进行动态绑定），若将其变为static静态方法便导致其为静态绑定，与abstract目的相矛盾，当然就无法组合使用

**1、调用方式**

在外部调用静态方法时，可以使用 `类名.方法名` 的方式，也可以使用 `对象.方法名` 的方式，而实例方法只有后面这种方式。也就是说，**调用静态方法可以无需创建对象** 。

不过，需要注意的是一般不建议使用 `对象.方法名` 的方式来调用静态方法。这种方式非常容易造成混淆，静态方法不属于类的某个对象而是属于这个类。

因此，一般建议使用 `类名.方法名` 的方式来调用静态方法。

```Plain
public class Person {    public void method() {      //......    }    public static void staicMethod(){      //......    }    public static void main(String[] args) {        Person person = new Person();        // 调用实例方法        person.method();        // 调用静态方法        Person.staicMethod()    }}
```

**2、访问类成员是否存在限制**

静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），（即实例成员变量和实例方法），而实例方法不存在这个限制。


# Spring

## 特性

1. IOC(inversionof control)容器：控制反转，编程者不再手动实例化对象，通过IOC容器实例化对象
    
    1. 控制反转只能是当我们想要使用对象时我们不在手而是把对象的实例化交给容器去管理
        
        1. 具体来说依赖注入就是其中的一种实现方式， 这是IOC的一种具体实现方式比如说通过auto wired 或者说的函数set 相关的数据完成降低了代码的耦合度冗余度
            
2. AOP：面向切面编程，把不同业务公用的部分封装起来，减少重复代码，降低耦合
    
    1. 动态代理（**JDK（基于接口），****CGLIB****（基于****继承****）两种代理**）实现AOP：
        
    2. **动态代理的核心特点**
        
        1. **不修改原代码**：房东（目标类）完全不用改代码，功能就被增强了。
            
        2. **动态生成**：代理类（中介）是运行时动态生成的，不需要提前写死。
            
        3. **灵活扩展**：如果想加“维修家电”功能，只需修改中介的`invoke`方法。
            
3. 事务管理
    
4. MVC框架
    
    1. model,view,controll：持久层（DAO）、控制（Controller, Service）、视图层（JS）
        
    2. DAO只做原子操作，直接使用select注解/mybatis操作数据库
        
    3. service负责对若干个DAO操作进行封装
        
    4. Controller负责转发，传参
        

## 动态代理

### JDK动态代理和CGLIB动态代理的区别
使用JDK动态代理的对象必须实现一个或多个接口；而使用cglib代理的对象则无需实现接口，达到代理类无侵入，基于继承，运行时生成目标类的子类。

### 静态代理和动态代理的区别
最大的区别就是静态代理是编译期确定的，但是动态代理却是运行期确定的。
|   |   |   |
|---|---|---|
|**特性**|**静态代理**|**CGLIB****动态代理**|
|**实现方式**|手动编写代理类，直接调用目标对象|运行时动态生成目标类的子类，覆盖方法|
|**是否需要接口**|需要目标对象实现接口|不需要接口，直接代理普通类|
|**性能**|无额外生成开销|生成字节码，首次调用略慢|
|**灵活性**|每个代理类只能代理一种接口|可代理任意类（final类/方法除外）|
|**适用场景**|简单场景，代理方法少|复杂场景，代理多个类或多个方法|

## 事务管理

  

## Spring IoC

### 谈谈自己对于 Spring IoC 的了解

**IoC****（****Inversion of Control****:****控制反转****）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spring 特有，在其他语言中也有应用。

**为什么叫****控制反转****？**

- **控制**：指的是对象创建（实例化、管理）的权力
    
- **反转**：控制权交给外部环境（Spring 框架、IoC 容器）
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODgxODc2ZDJiODI1YjkyNTE2NDZlMTk5MWRlNmU0MDJfRm1iRWNpTWVZVEljc1d2b3RvdmRoTVNUQmZWM2pCOHpfVG9rZW46Vk5XWGJGNHNqb2w5WkV4TTdZM2NLdjVEblFoXzE3NzM2NDk1OTc6MTc3MzY1MzE5N19WNA)

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。

**在实际项目中一个 Service 类可能依赖了很多其他的类，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用** **IoC** **的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。**

在 Spring 中， IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象。

Spring 时代我们一般通过 XML 文件来配置 Bean，后来开发人员觉得 XML 文件来配置不太好，于是 SpringBoot 注解配置就慢慢开始流行起来。

[IoC & AOP详解（快速搞懂）](https://javaguide.cn/system-design/framework/spring/ioc-and-aop.html#%E4%BB%80%E4%B9%88%E6%98%AF-ioc)

IOC的优点：

1. 使用者不用关心引用bean的实现细节
    
2. 不用创建多个相同的bean导致浪费
    
3. bean的修改使用方无需感知
    

  

### Bean获取方式：

1. 编写 `spring.xml` 文件，指定 `<beans>` 中 pojo 类的路径，会自动调用无参构造器构建 bean
    
    ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YTE5MDU3MjljZmU3YzFiNWE2YzgwYTU2Y2NjMDI3NjhfNmNWNlYyZjlzMnVzYnBOT2VWSDFDRFpkYVVRbExBaEtfVG9rZW46VHFaMGJNQXFSb01KNnJ4bkJtS2N5dGpkbkNoXzE3NzM2NDk1OTc6MTc3MzY1MzE5N19WNA)
    
    ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTc1NWMyODc5YmI1NzEzNDg5MWVmNDcxN2UzZTk5N2Ffd0N2VnV5YWFqOGtXaDJFSjNmSGVkNVZhblhBY3dNZjhfVG9rZW46RmIxRWJGQTZlb0NkRTd4Z3VoZmMxUUVlbkRoXzE3NzM2NDk1OTc6MTc3MzY1MzE5N19WNA)
    
2. 简单工厂模式
    
3. factory-bean实例化
    
4. FactoryBean接口实例化
    

### Bean的生命周期

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTczZDEwODU2NzI2ZjUxNTFiZjQwNGExNzUxODI3YmNfU3NQUjhBdUFwUjVROUd1TGpUbUZHVVJYeWo3VXBKVFdfVG9rZW46QkZtZWJmcE5Kb1A0UWR4cDRoT2NWaFBkbmNkXzE3NzM2NDk1OTc6MTc3MzY1MzE5N19WNA)

### Bean 的作用域有哪些?

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。
    
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 `getBean()` 两次，得到的是不同的 Bean 实例。
    

### springBean是单例还是多例

Bean 的默认作用域是单例（Singleton），但也支持多例（Prototype）等其他作用域，具体取决于配置。 `@Scope("prototype")`。

## Spring 的循环依赖

循环依赖指的是两个类中的属性相互依赖对方：例如A类中有B属性，B类中有A属性，从而形成了一个依赖闭环。

主要有三种情况：

- 通过构造方法进行依赖注入时产生的循环依赖问题
    
    ```Java
    @Component
    public class A {
        private final B b;
    
        public A(B b) {
            this.b = b;
        }
    }
    
    @Component
    public class B {
        private final A a;
    
        public B(A a) {
            this.a = a;
        }
    }
    ```
    
- 通过 setter 方法进行依赖注入且在多例（原型）模式下产生的循环依赖问题
    
    ```TypeScript
    @Component
    @Scope("prototype")
    public class C {
        private D d;
    
        public void setD(D d) {
            this.d = d;
        }
    }
    
    @Component
    @Scope("prototype")
    public class D {
        private C c;
    
        public void setC(C c) {
            this.c = c;
        }
    }
    
    @Component
    public class E {
        private final C c;
        private final D d;
    
        public E(C c, D d) {
            this.c = c;
            this.d = d;
        }
    }
    ```
    

当 Spring 容器创建`E`的实例时，会分别创建`C`和`D`的实例。在创建`C`时，发现它依赖`D`，就去创建`D`，而创建`D`时又发现它依赖`C`。因为是原型模式，每次请求都会创建新的实例，Spring 无法解决这种无限循环创建的情况， 虽然 Spring 容器在单例模式下可以通过提前暴露 Bean 的方式解决部分循环依赖，**但在原型模式下，由于每次获取的都是全新实例**，无法利用这种机制，最终会导致创建失败 。

- 通过 setter 方法进行依赖注入且在单例模式下产生的循环依赖问题
    

```TypeScript
@Component
public class F {
    private G g;

    public void setG(G g) {
        this.g = g;
    }
}

@Component
public class G {
    private F f;

    public void setF(F f) {
        this.f = f;
    }
}
```

### Spring 循环依赖了解吗，怎么解决？

只有第三种方式的循环依赖问题被解决了，三级缓存，Spring 在 DefaultSingletonBeanRegistry 类中维护了三个重要的缓存 (Map)

1. `singletonObjects` (一级缓存)：存放的是完全初始化好的、可用的Bean实例
    
2. `earlySingletonObjects` (二级缓存)：存放的是提前暴露的Bean的原始对象引用或早期代理对象引用
    
3. `singletonFactories` (三级缓存)：存放的是 Bean 的 `ObjectFactory` 工厂对象。负责在实例化后立刻暴露对象生成能力，兼顾AOP代理的提前生成。这个工厂的 `getObject()` 方法负责返回该 Bean 的早期引用。（就是放在二级缓存的早期引用）
    

假如AB循环依赖，先创建A，会在三级缓存里放入a的工厂对象，发现依赖b，于是去创建b，在三级缓存里放入b的工厂对象，发现依赖a，一二级缓存都找不到，在三级缓存找到A的工厂，调用`getObject()` 返回早期引用，放入二级缓存，`BeanB`成功持有`BeanA`的引用，b创建完毕，放入一级缓存，返回创建a，Spring会直接复用二级缓存中的代理对象作为最终Bean，完成初始化放入一级缓存。

## Spring 可以在一个 Bean 里面再注入自己吗

可以，有两种方式，一种是使用**@Autowired**_**注解**__，一种是使用ApplicationContextAware，

## - 策略模式下如何自动注入 `Map<String, Strategy>`
策略模式下可以利用 Spring 的依赖注入能力，直接注入 `Map<String, Strategy>`。Spring 会自动收集容器中所有 `Strategy` 实现类，key 是 BeanName，value 是对应策略对象。业务侧只需要根据类型从 `Map` 中取出对应策略执行即可，这样可以避免大量 `if-else` 或 `switch`。

## @Service / @Component 是怎么被扫描注册的

`@Service` 和 `@Component` 会被 `@ComponentScan` 扫描，因为 `@Service` 本质上是一个带有 `@Component` 的派生注解。Spring 在启动时通过 `ConfigurationClassPostProcessor` 解析配置类，调用扫描器把候选类解析为 `BeanDefinition`，然后注册到 `BeanDefinitionRegistry`，通常就是 `DefaultListableBeanFactory` 的 `beanDefinitionMap` 中。后续在容器刷新阶段再实例化 Bean，单例对象最终放入 `singletonObjects` 单例池。

## Bean 的生命周期了解么?(576/1759=32.7%)

1. **创建Bean的实例**：Spring启动，查找（即扫描注册）并加载需要被Spring管理的bean，进行Bean实例化
    
2. **Bean属性赋值/填充**：Bean实例化之后对将Bean的引用和值注入到Bean的属性中，populateBean() 属性注入
    
3. **Bean初始化**：
    
    1. 如果Bean实现了BeanNameAware接口的话，Spring将Bean的id传给setBeanName()方法
        
    2. 如果Bean实现了BeanFactoryAware接口的话，Spring将Bean的id传给setBeanFactory()方法，将BeanFactory容器实例传入
        
    3. 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。
        
    4. 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。
        
    5. 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用
        
    6. 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。
        
4. **销毁Bean**：把 Bean 的销毁方法先记录下来，将来需要销毁 Bean 或者销毁容器的时候，就调用这些方法去释放 Bean 所持有的资源。
    
    1. 如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。
        

## FactoryBean 和 BeanFactory

- `BeanFactory` 是 Spring IoC容器的顶层接口，管理所有 Bean,负责管理所有 Bean 的生命周期。全局 Bean 的管理框架
    
- `FactoryBean` 是 “Bean 的工厂”，用于简化复杂 Bean 的创建，是 Spring 提供的一种 Bean 实例化扩展机制。单个 Bean 的个性化创建逻辑
    

## 谈谈自己对于 Spring IoC 的了解(664/1759=37.7%)

  

## 什么是动态代理？(453/1759=25.8%)

  

## spring AOP的执行流程(635/1759=36.1%)

  

## **AOP**

AOP能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

### **说说** **AOP** **的最基本实现方式。**

（静态代理、JDK 动态代理、CGLIB、字节码增强）动态代理，在运行时动态生成代理对象

### **Spring** **AOP** **生成代理的两种场景/方式分别是什么？**

基于JDK的动态代理：这种方式需要代理的类实现一个或多个接口。

基于CGLIB的动态代理：当被代理的类没有实现接口时，Spring会使用CGLIB库生成一个被代理类的子类作为代理。

## 事务

### 事务管理的方式有几种？

- 编程式事务：需要在代码中手动控制事务的开始、提交和回滚等操作
    
- 声明式事务：通过在配置文件中声明事务的切入点和通知等信息来自动控制事务的行为
    
**声明式（`@Transactional`）优点是代码侵入性小，缺点是**粒度太大（只能作用于方法级别）**，如果方法里有耗时的 RPC 调用或长查询，会导致数据库长事务、占用连接池；编程式事务（如 `TransactionTemplate`）优点是**粒度小**，可以精确包裹必须原子的几行 SQL 代码，缺点是代码侵入性强。

### 事务的传播

**事务传播行为是为了解决业务层方法之间互相调用的事务问题**。

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中运行，也可能开启一个新事务，并在自己的事务中运行。

正确的事务传播行为可能的值如下:

**1.****`TransactionDefinition.PROPAGATION_REQUIRED`**

使用的最多的一个事务传播行为，`@Transactional`注解**默认**传播行为。如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

**2.****`TransactionDefinition.PROPAGATION_REQUIRES_NEW`**

**创建一个新的事务**，如果当前存在事务，则把当前事务挂起。也就是说不管外部方法是否开启事务，`Propagation.REQUIRES_NEW`修饰的内部方法会新开启自己的事务，且开启的事务相互独立，**互不干扰**。

**3.****`TransactionDefinition.PROPAGATION_NESTED`**

如果当前存在事务，则创建一个事务作为当前事务的**嵌套事务**来运行；如果当前没有事务，则该取值等价于`TransactionDefinition.PROPAGATION_REQUIRED`。

**4.****`TransactionDefinition.PROPAGATION_MANDATORY`**

如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

这个使用的很少。

若是错误的配置以下 3 种事务传播行为，事务将不会发生回滚：

- **`TransactionDefinition.PROPAGATION_SUPPORTS`**: 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
    
- **`TransactionDefinition.PROPAGATION_NOT_SUPPORTED`**: 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
    
- **`TransactionDefinition.PROPAGATION_NEVER`**: 以非事务方式运行，如果当前存在事务，则抛出异常。
    

#### @Transactional 注解下 A 事务调用 B 事务的异常处理（默认传播机制）

Spring 事务默认传播机制为`PROPAGATION_REQUIRED`（如果当前存在事务，则加入；否则创建新事务），当 A 事务调用 B 事务并抛出异常时：

1. 异常传播与回滚触发
    
    1. 若 B 事务抛出未被捕获的 RuntimeException 或 Error（默认回滚异常类型），异常会传播到 A 事务。
        
    2. A 事务的事务管理器检测到异常，会将回滚标志位设为 true。
        
2. 最终结果
    
    1. 无论 A 事务是否捕获异常，只要回滚标志位被设置，A 和 B 的操作都会整体回滚（因处于同一事务中）。
        
    2. 若 A 事务捕获异常并压制（如不重新抛出），会导致事务管理器冲突：标志位要求回滚，但代码无异常，最终抛出`UnexpectedRollbackException`。
        

## Spring的事务什么情况下会失效？(362/1759=20.6%)

1. 异常处理不当：默认情况下，Spring事务管理只在运行时异常（RuntimeExpection及其子类）和错误（Error及其子类）时回滚。如果捕获并处理了异常，但没有重新抛出，事务不会回滚。此外需要注意，对于检查异常（Exception及其子类），需要制定rollbackFor属性来触发事务回滚
    
2. **事务在非公开方法中失效**：如果@Transactional注解标注在非public方法上，事务也会失效
    
3. **事务传播属性设置不当**：例如：传播行为`REQUIRES_NEW`会挂起当前事务，创建一个新事物，这可能不是我们想要的
    
4. **数据源****和事务管理器配置不一致：**
    
5. **类内部方法调用：（外层无事务）**在同一个类中调用标注了**`@Transactional`**的方法（即自调用）时，事务管理器不会介入，因为Spring AOP代理无法拦截内部调用。（在同一个类中，用`this.doSomethingElse()`调用带`@Transactional`的方法时，本质是原始对象（Target）直接调用自己的方法，而非通过代理对象（Proxy）调用。）
    
    ```Java
    @Service
    public class MyService {
        public void doSomething() {
            // ...
            doSomethingElse();    
        }
            
        @Transactional    
        public void doSomethingElse() {        
            // ...    
        }
    }
    ```
    

## Spring和SpringBoot的区别

**Spring 和 Spring Boot的最大的区别在于Spring Boot的自动装配原理**

使用 Spring 进行开发各种配置过于麻烦比如开启某些 Spring 特性时，需要用 XML 或 Java 进行显式配置。于是，Spring Boot 诞生了！

Spring 旨在简化 J2EE 企业应用程序开发。Spring Boot 旨在简化 Spring 开发（减少配置文件，开箱即用！）。

Spring Boot 只是简化了配置，如果你需要构建 MVC 架构的 Web 程序，你还是需要使用 Spring MVC 作为 MVC 框架，只是说 Spring Boot 帮你简化了 Spring MVC 的很多配置，真正做到开箱即用！

## springboot 相对于 spring 在功能特性上的优化

- Spring Boot 提供了自动化配置，简化了项目的配置过程，通过约定优于配置的原则，很多常用配置可以自动完成
    
- Spring Boot 提供了快速的项目启动器，通过引入不同的Starter，可以快速集成常用的框架和库
    
- Spring Boot 默认集成了多种内嵌服务器，无需额外配置，即可将应用打包成可执行的 JAR 文件
    
## Springboot的starter？
> Starter 是一组依赖 + 自动配置的封装
1.  `spring-boot-starter-web` ：是用来开发 Web 应用的，里面集成了 Spring MVC、内嵌 Tomcat 和 JSON 解析等组件，可以开箱即用地写接口；
只需要写：`@RestController`就能直接提供接口，不用自己配 servlet、web.xml
2. `spring-boot-starter-test`：用来写 **测试代码** 的 starter，是测试依赖，包含 JUnit、Mockito 等工具，并且如果添加scope 是 test，只在测试阶段生效，不会打进生产包。
## SpringBoot的原理？自动装配？

自动装配就是通过注解或一些简单的配置就可以在SpringBoot的帮助下开启和配置各种功能，比如数据库访问、Web开发。

原理：

1. @SpringBootApplication 注解：组合了 `@EnableAutoConfiguration`（开启自动配置）、`@ComponentScan`（包扫描）、`@Configuration`（标识配置类），是启动类的核心注解。
    
2. @EnableAutoConfiguration 的作用：通过 `@Import(AutoConfigurationImportSelector.class)` 导入自动配置类。
    
3. 加载自动配置类：（扫描类路径）
    
    1. `AutoConfigurationImportSelector` 会读取 `META-INF/spring.factories` 文件（每个 starter 中都有该文件），该文件定义了自动配置类的全路径（如 `WebMvcAutoConfiguration`、`DataSourceAutoConfiguration`）。
        
    2. Spring 会根据类路径下是否存在对应依赖（如引入了 `spring-boot-starter-web` 则加载 `WebMvcAutoConfiguration`），以及配置文件中的属性（如 `application.properties`），动态判断是否需要实例化这些自动配置类中的 Bean。
        
4. 条件判断: 对于每一个发现的自动配置类，`AutoConfigurationImportSelector` 会使用条件判断机制（通常是通过 `@ConditionalOnXxx`注解）来确定是否满足导入条件。
    
5. AutoConfigurationImportSelector 是**导入自动配置类**，不是直接实例化 Bean。 Bean 最终由这些配置类里的 @Bean、@Import 等注册。
    
6. spring.factories 这个说法对 **Spring Boot 2.x** 正确； 如果面试官提 Boot 3，要补一句：主要用 AutoConfiguration.imports。
    
7. 条件判断不只看依赖和配置，还会结合 Bean 是否已存在（@ConditionalOnMissingBean）、Web 环境、classpath 等。
    

你可以用这个 40 秒版本回答：

Spring Boot 的自动装配核心是 @SpringBootApplication，其中 @EnableAutoConfiguration 会通过 AutoConfigurationImportSelector 导入候选自动配置类。Boot 2.x 主要从 spring.factories 读取，Boot 3 主要从 AutoConfiguration.imports 读取。随后通过 @ConditionalOnClass、@ConditionalOnMissingBean、@ConditionalOnProperty 等条件注解筛选，满足条件才让配置类生效并注册 Bean。这样开发者只需要引入 starter 和少量配置，就能完成大部分基础能力装配。

### `@SpringBootApplication` 的功能

Spring Boot 通过 `@EnableAutoConfiguration` 引入 `AutoConfigurationImportSelector`，在启动时从 `spring.factories` 或 `AutoConfiguration.imports` 中加载自动配置类，这些类本身通常标注了 `@Configuration` 并结合条件注解，最终决定哪些 Bean 会被注册到 Spring 容器中。

1. `@SpringBootApplication` 是一个组合注解，包含了 `@SpringBootConfiguration`、`@EnableAutoConfiguration`、`@ComponentScan` 三个注解的功能，同时提供了一些属性配置，也是来自于以上 3 个注解；
    
2. `@SpringBootConfiguration` 包含了 `Configuration` 注解的功能；
    
3. `@EnableAutoConfiguration` 是开启自动装配的关键注解，其中标记了 `@AutoConfigurationPackage`，会将被 `@SpringBootApplication` 标记的类所在的包，包装成 `BasePackages`，然后注册到 spring 容器中；`@EnableAutoConfiguration` 还通过 `@Import` 注解向容器中引入了 `AutoConfigurationImportSelector`，该类会将当前项目支持的自动配置类添加到 spring 容器中；
    
4. `@AutoConfigurationPackage`这个注解主要是`@Import(AutoConfigurationPackages.Registrar.class)` ，它就是将`Registrar`这个组件类导入到容器中，而`Registrar`类作用是扫描主配置类同级目录及其子包里面的所有组件扫描到`Spring`容器中；
    
5. `@Import(AutoConfigurationImportSelector.class)`：它通过将`AutoConfigurationImportSelector`类导入容器中，该类的作用是通过`selectImports`方法执行过程中，会使用内部工具类`SpringFactoriesLoader`，查找`classpath`上所有`jar`包中的`META-INF/spring.factories`进行加载，实现将配置类信息交给`SpringFactory`加载器进行一系列的容器创建过程。
    
6. `@ComponentScan` 定义了包扫描路径，其 `excludeFilters` 值可以用来排除类的扫描，springboot 指定了 `TypeExcludeFilter`，表明我们可以继承该类来自主定义排除的类 ；同时也指定了 `AutoConfigurationExcludeFilter` ，该 `Filter` 可以用来排除自动配置类，也就是说，自动配置类不会进行包描述操作。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NGMyYjVjNTRlNmMzNzdlYTJkNzBlZWJjYmI5N2ViYmVfVEhUeWdCcnp1bm5pc0xHTHE0VVRqRjVEUUJ4Z3RTWUNfVG9rZW46U3FCemJLbVlYb3Z4VjN4ZFZadmNlSGFrbkNnXzE3NzM2NDk1OTc6MTc3MzY1MzE5N19WNA)

### `@SpringBootApplication` 依赖管理

在SpringBoot入门程序中，项目[pom.xml](https://zhida.zhihu.com/search?content_id=164714463&content_type=Article&match_order=1&q=pom.xml&zhida_source=entity)文件中有两个核心依赖：

- `spring-boot-starter-parent`：该文件通过标签对一些**常用**的技术框架的依赖文件进行了统一的版本号管理，如果pom.xml引入的依赖文件不是`spring-boot-starter-parent`管理的，那么在`pom.xml`引入依赖文件时，需要使用标签指定依赖文件的版本号。
    
- `spring-boot-starter-web`： Spring Boot 提供的用于快速构建 Web 应用的启动器

# JUC

## ThreadLocal实现原理

`ThreadLocal` 的核心功能不是存储数据本身，而是为**每个线程**分配独立的 `<T>` 类型数据副本，并提供访问入口。

- 数据实际存储在线程的 `ThreadLocalMap` 中（`key` 是 `ThreadLocal` 实例，`value` 是 `<T>` 类型的数据）。
    
- `ThreadLocal` 本身更像一个 “钥匙”：通过它可以找到当前线程专属的 `<T>` 类型数据。
    

`Thread` 类中有一个 `threadLocals` 和 一个 `inheritableThreadLocals` 变量，它们都是 `ThreadLocalMap` 类型的变量,我们可以把 `ThreadLocalMap` 理解为`ThreadLocal` 类实现的定制化的 `HashMap`。默认情况下这两个变量都是 null，只有当前线程调用 `ThreadLocal` 类的 `set`或`get`方法时才创建它们，实际上调用这两个方法的时候，我们调用的是`ThreadLocalMap`类对应的 `get()`、`set()`方法。

**`ThreadLocal`** **可以理解为只是****`ThreadLocalMap`****的封装，传递了变量值。** `ThrealLocal` 类中可以通过`Thread.currentThread()`获取到当前线程对象后，直接通过`getMap(Thread t)`可以访问到该线程的`ThreadLocalMap`对象。

每个`Thread`中都具备一个`ThreadLocalMap`，而`ThreadLocalMap`可以存储以`ThreadLocal`为 key ，Object 对象为 value 的键值对。

`ThreadLocal` 数据结构如下图所示：

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTVjZWUxNTQxMzA5NjJmYjg3YTAyOTc1ZmY0NDhhMTdfejU1V1VqenlKZjFMOENZRFhBang0TTluSGxQUFNaeXdfVG9rZW46RXBiNWJqVk92b1dvNUx4QWRPSGNXZ1habmtkXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

`ThreadLocalMap`是`ThreadLocal`的静态内部类。

### 内存泄漏

ThreadLocalMap 中的 Entry 对象的 Key 是弱引用，但其 Value 是强引用，因此即使 Key 被回收，Value 仍然存在，导致内存泄漏。

内存泄漏的发生需要同时满足两个条件：

1. `ThreadLocal` 实例不再被强引用；
    
2. 线程持续存活，导致 `ThreadLocalMap` 长期存在。
    

**怎么解决？**

- 在**使用完** **`ThreadLocal`** **后**，**务必调用** **`remove()`** **方法**。 这是最安全和最推荐的做法。 `remove()` 方法会从 `ThreadLocalMap` 中显式地移除对应的 entry，彻底解决内存泄漏的风险。 即使将 `ThreadLocal` 定义为 `static final`，也强烈建议在每次使用后调用 `remove()`。
    
- 在线程池等线程复用的场景下，使用 `try-finally` 块可以确保即使发生异常，`remove()` 方法也一定会被执行。
    

**ThreadLocal 的 value 不能设计成****弱引用**，因为它必须在整个线程生命周期内稳定可用；如果 value 也是弱引用，会在任意一次 GC 后被回收，导致 ThreadLocal 在逻辑上“随机失效”。

## 进程

### 进程的状态

创建状态、就绪状态、运行状态、阻塞状态、结束状态

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjFkYjhkYmExOWMzYjQ5ZTg4NzRkNzQ4MWM3ZGY4ZWJfYU01YmlYakdIUUp0REF3UzQ5QWhvRnZHTFJUdWVnQnRfVG9rZW46SUtMVmJsbmZ2bzNJeVh4MzNLQ2M4OE13bjhiXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

## 协程

协程是一种用户态的轻量级线程，其调度完全由用户程序控制，而不需要内核的参与。拥有自己的寄存器上下文和栈，但与其他协程共享堆内存。协程的切换开销非常小，因为只需要保存和恢复协程的上下文。

### 与线程的区别

- 核心差异：线程是“内核调度、开销大、支持并行”，协程是“用户调度、开销小、串行执行”。协程本质是单线程的（运行在单线程中），无法直接利用多核CPU资源。
    
- 选择原则：
    
    - 如果任务需要大量 CPU 计算（CPU 密集），优先用多线程，充分利用多核；
        
    - 如果任务大部分时间在等待 IO（IO 密集），优先用协程，以极小开销支持超高并发。
        

### 协程数量持续增加的原因有哪些

1. 协程创建后未正常销毁 / 回收（如忘记调用`close()`、`cancel()`等终止方法）
    
2. 任务阻塞导致协程 “堆积”
    
3. 调度器配置不合理
    
4. 无限循环 / 递归创建协程
    

## 线程

### 进程与线程的区别

- 进程是资源（包括内存、打开的文件等）分配的单位，线程是 CPU 调度的单位；
    
- 进程拥有一个完整的资源平台，而线程只独享必不可少的资源，如寄存器和栈；
    
- 线程同样具有就绪、阻塞、执行三种基本状态，同样具有状态之间的转换关系；
    

### 如何创建线程？(582/1759=33.1%)

Java 提供了多种方式来创建和管理线程，最常见的方式一共有四种，接下来我会分别进行讲述。

第一种是通过继承 Thread 类并重写其 run() 方法来创建线程。在run() 方法中定义线程需要执行的任务逻辑，然后

创建该类的实例，调用 start() 方法启动线程，start() 方法会自动调用 run() 方法中的代码逻辑。这种方式简单直观，但由于 Java 不支持多重继承，因此限制了类的扩展性。

第二种是实现 Runnable 接口并将其传递给 Thread 构造器来创建线程。Runnable 是一个函数式接口，其中的 run() 方法定义了任务逻辑。这种方式更加灵活，因为它不占用类的继承关系，同时可以更好地支持资源共享，可以让多个线程共享同一个 Runnable 实例。这种方式适用于需要解耦任务逻辑与线程管理的场景。

第三种是通过实现 Callable 接口来创建有返回值的线程。Callable 接口类似于 Runnable，但它可以返回结果并抛出异常。Callable 的 call() 方法需要通过 FutureTask 包装后传递给 Thread 构造器。通过 Future 对象可以获取线程执行的结果或捕获异常。这种方式适用于需要获取线程执行结果或处理复杂任务的场景。

第四种是通过 Executor 框架创建线程池来管理线程。Executor 框架提供了更高级的线程管理功能，例如线程复用、任务调度等。通过 submit() 或 execute() 方法提交任务，避免频繁创建和销毁线程的开销。它作为最常被使用的方式，广泛用于需要高效管理大量线程的场景。

**想象一个工厂生产玩具车的过程，**

Thread 类 ：就像工厂自己造了一辆专属的车，但这辆车只能按照固定的设计运行，不能改装（Java 不支持多重继承）。

Runnable 接口 ：工厂把车的任务外包给工人（Runnable 实例），多个工人可以共享同一套工具（资源共享）。

Callable 接口 ：工厂派工人去完成任务，并要求他们带回结果（返回值）或者报告问题（异常）。

Executor 框架 ：工厂建立了一个车队管理系统（线程池），统一调度车辆，避免频繁制造新车（减少开销）。

#### run和start的区别

#### （1）`start()` 方法

- 作用：启动一个新线程，将线程状态从 “新建状态（New）” 转为 “就绪状态（Runnable）”。当线程获取到 CPU 时间片后，会自动调用 `run()` 方法执行任务。
    
- 底层机制：`start()` 是一个 native 方法（依赖操作系统实现），它会向操作系统申请创建新线程，操作系统为线程分配资源后，由 JVM 调度执行 `run()` 方法。
    
- 限制：一个线程对象只能调用 `start()` 一次，多次调用会抛出 `IllegalThreadStateException` 异常。
    

#### （2）`run()` 方法

- 作用：定义线程的具体任务逻辑，相当于线程的 “执行体”。
    
- 执行方式：`run()` 是一个普通方法，直接调用时不会启动新线程，而是在当前线程中同步执行（和调用普通方法一样）。
    
- 特点：可以多次调用，也可以被其他线程调用，但不会创建新线程。
    

### 说说线程的生命周期和状态？(606/1759=34.5%)

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：

- NEW: 初始状态，线程被创建出来但没有被调用 `start()` 。
    
- RUNNABLE: 运行状态，线程被调用了 `start()`等待运行的状态。
    
- BLOCKED：阻塞状态，需要等待锁释放。如果线程试图进入一个同步代码块或方法时，发现所需的锁被其他线程占用，则会进入阻塞状态。线程在此状态下等待锁的释放，获得锁后会重新回到可运行状态。
    
- WAITING：等待状态，表示该线程需要等待其他线程做出一些特定动作（通知或中断）。当线程调用不带超时参数的等待方法（例如 Object.wait()、Thread.join() 或 LockSupport.park()）时，它将进入等待状态。在这种状态下，线程会无限期地等待，直到其它线程通过 notify()、notifyAll() 或中断操作将其唤醒。
    
- TIME_WAITING：超时等待状态，可以在指定的时间后自行返回而不是像 WAITING 那样一直等待。
    
- TERMINATED：终止状态，表示该线程已经运行完毕。
    

线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTIxNGFiOTAyODIyNDI2MTg5NDVjNzRkY2JlMmQ2ZThfZDRvcmR3YmlQbmRvNHlmcGh1TW05Ujc4MHJTbzBFWUpfVG9rZW46SzdrMGJtZU93b0hKVVN4OFB5UmNpNEFHbjcxXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

**1.描述一下线程的生命周期图**

线程在创建后首先进入 NEW（新建）状态，调用 start() 方法后进入 READY（就绪）状态，等待 CPU 时间片分配；当线程获得时间片后进入 RUNNING（运行）状态并执行任务。若线程调用 wait() 方法，则进入 WAITING（等待）状态，需依赖其他线程的通知才能恢复运行；而通过 sleep(long millis) 或 wait(long millis) 方法，线程会进入 TIMED_WAITING（超时等待）状态，并在超时结束后返回可运行状态。如果线程试图获取 synchronized 锁但被占用，则进入 BLOCKED（阻塞）状态，直到锁可用。最后，当线程执行完 run() 方法或因异常退出时，进入 TERMINATED（终止）状态，生命周期结束。

#### 一个线程可以 start 两次吗？

Java的线程是不允许启动两次的，第二次调用必然会抛出IllegalThreadStateException，这是一种运行时异常，多次调用start被认为是编程错误。

### 什么是线程上下文切换？(388/1759=22.1%)

线程上下文切换是多线程编程中的一个概念，它直接影响程序的性能和效率。接下来我会详细讲述线程上下文切换的定义、发生时机、过程和影响。

首先讲一下**什么是线程上下文切换**，它是指当 CPU 从一个线程切换到另一个线程时，操作系统需要保存当前线程的执行状态，并加载下一个线程的执行状态，以便它们能够正确地继续运行。执行状态主要包括：寄存器状态、程序计数器（PC）、栈信息、线程的优先级等。

接下来讲一下**发生时机**，通常有四种情况会发生线程上下文切换。

第一种是时间片耗尽，操作系统为每个线程分配了一个时间片，当线程的时间片用完后，操作系统会强制切换到其他线程，这是为了保证多个线程能够公平地共享 CPU 资源。

第二种是线程主动让出 CPU，当线程调用了某些方法，如 Thread.sleep()、Object.wait() 或 LockSupport.park()等，会使线程主动让出 CPU，导致上下文切换。

第三种是调用了阻塞类型的系统中断，比如：线程执行 I/O 操作时，由于 I/O 操作通常需要等待外部资源，线程会被挂起，会触发上下文切换。

第四种是被终止或结束运行。

然后再讲一下**线程上下文切换的过程**，分为四步。

第一步是保存当前线程的上下文，将当前线程的寄存器状态、程序计数器、栈信息等保存到内存中。

第二步是根据线程调度算法，如：时间片轮转、优先级调度等，选择下一个要运行的线程。

第三步是加载下一个线程的上下文，从内存中恢复所选线程的寄存器状态、程序计数器和栈信息。

第四步是 CPU 开始执行被加载的线程的代码。

最后讲一下**线程上下文切换所带来的影响**。线程上下文切换虽然能够实现多任务并发执行，但它也会带来 CPU 时间消耗、缓存失效以及资源竞争等问题。为了减少线程上下文切换带来的性能损失，可以采取减少线程数量、使用无锁数据结构等方式进行优化。

### 线程池在使用的时候需要注意哪些

  

### 线程池的七大参数？(679/1759=38.6%)

```c++
    /**
     * 用给定的初始参数创建一个新的ThreadPoolExecutor。
     */
    public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
                              int maximumPoolSize,//线程池的最大线程数
                              long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
                              TimeUnit unit,//时间单位
                              BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
                              ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
                              RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
                               ) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```

`ThreadPoolExecutor` 3 个最重要的参数：

- `corePoolSize` : 任务队列未达到队列容量时，最大可以同时运行的线程数量。**CPU密集=cpu核心数+1，****io****密集=cpu*（1+平均等待时间/平均计算时间）**
    
- `maximumPoolSize` : 任务队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。**一般设置为cpu核心数*2**
    
- `workQueue`: 新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。
    

`ThreadPoolExecutor`其他常见参数 :

- `keepAliveTime`:当线程池中的线程数量大于 `corePoolSize` ，即有非核心线程（线程池中核心线程以外的线程）时，这些非核心线程空闲后不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁。**一般不超过60s**
    
- `unit` : `keepAliveTime` 参数的时间单位。
    
- `threadFactory` :executor 创建新线程的时候会用到。
    
- `handler` :拒绝策略
    

### **线程池****的工作流程**

1. 线程池首先检查核心线程池（corePoolSize）中是否有线程**空闲**。如果有空闲线程，就使用这些线程执行新任务。如果核心线程都在忙，则进入下一步。
    
2. 判断等待队列是否已满：如果小于，则添加到等待队列中等待。如果已满，进入下一个流程
    
3. 判断是否达到最大线程数量：如果小于，创建一个新的线程执行任务，否则根据拒绝策略处理这个任务
    

### 线程池预热

默认是空的，懒加载，只有提交任务才会创建核心线程来处理任务

- `prestartCoreThread()`：预热 1 个核心线程，若核心线程数未达 `corePoolSize`，则创建 1 个线程并使其处于空闲状态。
    
- `prestartAllCoreThreads()`：预热 所有核心线程，即创建 `corePoolSize` 个核心线程，全部进入空闲状态等待任务。
    

### 线程池execute和submit区别

`execute()`方法只能接收实现了`Runnable`接口的任务，而`submit()`方法既可以接收`Runnable`类型的任务，也可以接收`Callable`类型的任务。这一点是两者在使用上的一个主要区别。

`execute()`方法没有返回值，`submit()`方法返回一个`Future`对象

**异常处理的差异**

使用`execute()`提交的任务，如果在`run()`方法中抛出异常，这些异常会被线程池捕获并打印出来。而使用`submit()`提交的任务，如果任务内部有异常发生，异常信息不会被直接打印出来，除非调用了`Future`对象的`get()`方法。因此，当使用`submit()`方法时，建议在`run()`或`call()`方法中显式地捕获和处理异常，以避免异常信息丢失。

### 如何手写线程池拒绝策略

线程池的拒绝策略需实现 `RejectedExecutionHandler` 接口，重写 `rejectedExecution(Runnable r, ThreadPoolExecutor executor)` 方法，该方法接收两个参数：

- `r`：被拒绝的任务。
    
- `executor`：当前线程池实例。
    

### 线程池四大拒绝策略？(658/1759=37.4%)

如果当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了任务时，`ThreadPoolExecutor` 定义一些策略:

- `ThreadPoolExecutor.AbortPolicy`：抛出 `RejectedExecutionException`来拒绝新任务的处理。
    
- `ThreadPoolExecutor.CallerRunsPolicy`：调用执行者自己的线程运行任务，也就是直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。如果你的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
    
- `ThreadPoolExecutor.DiscardPolicy`：不处理新任务，直接丢弃掉。
    
- `ThreadPoolExecutor.DiscardOldestPolicy`：此策略将丢弃最早的未处理的任务请求。
    

### 核心线程空闲时处于什么状态？

将 `allowCoreThreadTimeOut(boolean value)` 方法的参数设置为 `true`可以回收空闲的核心线程。（时间间隔由`keepAliveTime`指定）

- 设置了核心线程的存活时间：在空闲时线程处于waiting状态，等到获取任务。如果超过了核心线程存活时间，会被回收，状态变为 `TERMINATED` 状态。
    
- 没有设置核心线程的存活时间：一致处于 `WAITING` 状态，等待获取任务。
    

### 队列选择原则

1. bounded 优先 ：尽量使用有界队列（如`ArrayBlockingQueue`），避免无界队列（如默认`LinkedBlockingQueue`）因任务堆积导致 OOM
    
2. 任务特性匹配 ：
    
    1. 任务耗时短、数量多 → 用`SynchronousQueue`配合较大`maximumPoolSize`。
        
    2. 任务有优先级 → 用`PriorityBlockingQueue`。
        
    3. 任务需延迟执行 → 用`DelayQueue`。
        
3. 结合拒绝策略 ：有界队列满时会触发拒绝策略，需根据业务选择（如核心任务用`AbortPolicy`及时报警，非核心任务用`DiscardPolicy`）。
    

#### 任务队列类型

1. `ArrayBlockingQueue`（数组阻塞队列）
    
    1. 基于数组的有界队列，创建时需指定容量（如`new ArrayBlockingQueue(100)`）。
        
    2. 特点：FIFO（先进先出），公平 / 非公平访问策略可选。
        
    3. 适用场景：任务量可预估，需要控制队列大小避免内存溢出（如核心业务场景）。
        
2. `LinkedBlockingQueue`（链表阻塞队列）
    
    1. 基于链表的可选有界队列，默认容量为`Integer.MAX_VALUE`（几乎无界）。
        
    2. 特点：FIFO，吞吐量通常高于`ArrayBlockingQueue`，但默认无界可能导致 OOM。
        
    3. 适用场景：任务量不确定，但需避免频繁创建非核心线程（如`Executors.newFixedThreadPool`默认使用此队列）。
        
3. `SynchronousQueue`（同步队列）
    
    1. 无容量的队列，提交的任务必须立即被线程执行，否则阻塞或创建新线程（直到达最大线程数）。
        
    2. 特点：不存储任务，直接传递给线程，适合任务处理速度快的场景。
        
    3. 适用场景：需要快速响应，任务耗时短（如`Executors.newCachedThreadPool`默认使用此队列）。
        
4. `PriorityBlockingQueue`（优先级阻塞队列）
    
    1. 无界队列，按任务优先级排序执行（需任务实现`Comparable`接口）。
        
    2. 特点：破坏 FIFO，高优先级任务先执行。
        
    3. 适用场景：任务有优先级区分（如紧急任务优先处理）。
        
5. `DelayQueue`（延迟队列）
    
    1. 无界队列，任务需等待指定延迟时间后才会被执行（需任务实现`Delayed`接口）。
        
    2. 特点：按延迟时间排序，仅当任务到期后才会被取出。
        
    3. 适用场景：定时任务（如超时订单关闭、缓存过期清理）。
        

##### 线程池使用无界队列的问题

1. 内存溢出（OOM）
    
    1. 当任务提交速度远快于执行速度时，队列会持续堆积任务，最终耗尽 JVM 内存，导致程序崩溃。
        
2. 最大线程数失效
    
    1. 线程池逻辑：核心线程满后先入队，队列满后才创建非核心线程。
        
    2. 无界队列永远不会满，导致`maximumPoolSize`参数失效，非核心线程永远不会被创建，所有任务仅由核心线程处理，可能导致任务延迟激增。
        
3. 系统响应变慢
    
    1. 大量任务堆积会导致 GC 频繁（对象增多），且线程池处理延迟增加，最终引发系统整体响应变慢。
        

### 线程安全的实现方法有哪些

线程安全是什么？

论多个线程如何并发执行、调度顺序如何，代码总能表现出 “正确的行为”，最终结果与单线程执行时一致。

方法一：使用synchronized关键字

方法二：使用Lock接口下的实现类（ReentrantLock）

方法三：使用线程本地存储ThreadLocal

方法四：使用乐观锁机制

### Thread#sleep() 方法和 Object#wait() 方法对比

**共同点**：两者都可以暂停线程的执行。

**区别**：

- **`sleep()`** **方法没有释放锁，而** **`wait()`** **方法释放了锁** 。
    
- `wait()` 通常被用于线程间交互/通信，`sleep()`通常被用于暂停执行。
    
- `wait()` 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify()`或者 `notifyAll()` 方法。`sleep()`方法执行完成后，线程会自动苏醒，或者也可以使用 `wait(long timeout)` 超时后线程会自动苏醒。
    
- `sleep()` 是 `Thread` 类的静态本地方法，`wait()` 则是 `Object` 类的本地方法。为什么这样设计呢？下一个问题就会聊到。
    

#### 为什么 wait() 方法不定义在 Thread 中？

`wait()` 是让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。每个对象（`Object`）都拥有对象锁，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要操作对应的对象（`Object`）而非当前的线程（`Thread`）。

类似的问题：**为什么** **`sleep()`** **方法定义在** **`Thread`** **中？**

因为 `sleep()` 是让当前线程暂停执行，不涉及到对象类，也不需要获得对象锁。

### 可以直接调用 Thread 类的 run 方法吗？

调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。

### 想要让父线程在所有子线程都结束之后再工作，怎么实现

核心是利用 `Thread` 类的 `join()` 方法。`join()` 方法会使当前线程（父线程）阻塞，直到调用该方法的子线程执行结束。

## 死锁的概念和解决方法

线程死锁：多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

死锁预防和死锁避免：

**如何预防****死锁****？** 破坏死锁的产生的必要条件即可：

1. **破坏请求与保持条件**：一次性申请所有的资源。
    
2. **破坏不剥夺条件**：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
    
3. **破坏循环等待条件**：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。
    

如何避免死锁？

避免死锁就是在资源分配时，借助于算法（比如**银行家算法**）对资源分配进行计算评估，使其进入安全状态。

## CurrentHashMap的原理？(488/1759=27.7%)

#### JDK1.8 之前

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmIwMjAwM2QyYzVhOWMxMWYxZjgxMjdiMGQ2YTlkMzhfM0tsVWhqUnRvR2doOFpGN0pVU1lSZm5PckZxU3ZmY01fVG9rZW46UDBNdWJ1VG1kb2xiU1p4ZWlMMWNoN2tHbmhoXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

首先将数据分为一段一段（这个“段”就是 `Segment`）的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

**`ConcurrentHashMap`** **是由** **`Segment`** **数组结构和** **`HashEntry`** **数组结构组成**。

`Segment` 继承了 **`ReentrantLock`**,所以 `Segment` 是一种可重入锁，扮演锁的角色。`HashEntry` 用于存储键？？值对数据。

```Java
static class Segment<K,V> extends ReentrantLock implements Serializable {}
```

一个 `ConcurrentHashMap` 里包含一个 `Segment` 数组，`Segment` 的个数一旦**初始化就不能改变**。 `Segment` 数组的大小默认是 16，也就是说默认可以同时支持 16 个线程并发写。

`Segment` 的结构和 `HashMap` 类似，是一种数组和链表结构，一个 `Segment` 包含一个 `HashEntry` 数组，每个 `HashEntry` 是一个链表结构的元素，每个 `Segment` 守护着一个 `HashEntry` 数组里的元素，当对 `HashEntry` 数组的数据进行修改时，必须首先获得对应的 `Segment` 的锁。也就是说，对同一 `Segment` 的并发写入会被阻塞，不同 `Segment` 的写入是可以并发执行的。

#### JDK1.8 之后

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTNiY2I2YmVhYzgzMzY2ODhkYzg1MWUzODliYzU0NGFfeldYQmE2Y2d0TTFHR3dIY1FFSFhCaWRMeDVtQWdTd0hfVG9rZW46QjAwaGJ3NmI2b3dOTjZ4c00yZWNudDFibjNiXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

Java 8 几乎完全重写了 `ConcurrentHashMap`，代码量从原来 Java 7 中的 1000 多行，变成了现在的 6000 多行。

`ConcurrentHashMap` 取消了 `Segment` 分段锁，采用 `Node + CAS + synchronized` 来保证并发安全。数据结构跟 `HashMap` 1.8 的结构类似，数组+链表/红黑二叉树。Java 8 在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为 O(N)）转换为红黑树（寻址时间复杂度为 O(log(N))）。

### JDK 1.7 和 JDK 1.8 的 ConcurrentHashMap 实现有什么不同？

- **线程安全****实现方式**：JDK 1.7 采用 `Segment` 分段锁来保证安全， `Segment` 是继承自 `ReentrantLock`。JDK1.8 放弃了 `Segment` 分段锁的设计，采用 `Node + CAS + synchronized` 保证线程安全，锁粒度更细，`synchronized` 只锁定当前链表或红黑二叉树的首节点。
    
- **Hash 碰撞解决方法** : JDK 1.7 采用拉链法，JDK1.8 采用拉链法结合红黑树（链表长度超过一定阈值时，将链表转换为红黑树）。
    
- **并发度**：JDK 1.7 最大并发度是 Segment 的个数，默认是 16。JDK 1.8 最大并发度是 Node 数组的大小，并发度更大。
    

## synchronized （内置锁）底层原理了解吗？(543/1759=30.9%)

**JVM****监视器锁**

synchronized 用于保证多线程环境下的数据一致性。接下来我会详细讲述 synchronized 的定义和底层实现。

首先说一下**什么是synchronized** **，**它是一种内置的锁机制，它可以作用于方法或代码块，用于控制多个线程对共享资源的访问。当一个线程进入 synchronized 保护的代码区域时，它会尝试获取锁；如果锁已被其他线程占用，则当前线程会被阻塞，直到锁被释放。锁的持有者在退出同步代码块或方法时会自动释放锁，从而允许其他线程继续执行。

接下来说一下 **synchronized 的底层是如何实现**的，它的依赖于 JVM 的监视器锁（Monitor）机制，每个对象有一个监视器锁（monitor）。当 monitor 被占用时就会处于锁定状态，线程执行 monitorenter 指令时尝试获取锁，会判断 monitor 的进入数是否为 0 ，如果为 0 则该线程进入monitor，然后将进入数设置为 1，该线程即为monitor的所有者；如果不为 0，说明已有线程占有该monitor，那么线程就会进入并处于阻塞状态，直到monitor的进入数为 0，才会重新尝试获取monitor的所有权。

退出同步代码块时，线程会执行 monitorexit，该线程必须是 objectref 所对应的 monitor 的所有者。指令执行时，monitor 的进入数减 1，如果减 1 后进入数为 0，那线程退出 monitor，不再是这个 monitor 的所有者。其他被这个 monitor 阻塞的线程可以尝试去获取这个 monitor 的所有权。

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取而代之的是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。如果是实例方法，JVM 会尝试获取实例对象的锁。如果是静态方法，JVM 会尝试获取当前 class 的锁。

**不过，两者的本质都是对对象监视器 monitor 的获取。**

## ReentrantLock用法
Java语言直接提供了synchronized关键字用于加锁，但这种锁一是很重，二是获取时必须一直等待，没有额外的尝试机制。
java.util.concurrent.locks包提供的ReentrantLock用于替代synchronized加锁，ReentrantLock 内部是基于 AbstractQueuedSynchronizer (简称AQS) 实现的。
ReentrantLock是可重入锁，它和synchronized一样，一个线程可以多次获取同一个锁。
### 如何实现可重入的？
ReentrantLock加锁的时候，看下当前持有锁的线程和当前请求的线程是否是同一个，一样就可重入了。只需要简单地将state+1，记录当前线程地冲入次数即可。释放地时候要确保state=0的时候才能执行释放资源的动作。
## synchronized可以实现锁升级吗？

可以，锁主要存在四种状态，依次是：**无锁**状态、**偏向锁**状态、**轻量级锁**状态、**重量级锁**状态，他们会随着竞争的激烈而逐渐升级。

[浅析synchronized锁升级的原理与实现 - 小新成长之路 - 博客园](https://www.cnblogs.com/star95/p/17542850.html)

## synchronized和ReentrantLock的区别？(642/1759=36.5%)
[✅synchronized和reentrantLock区别？](https://www.yuque.com/hollis666/gg1x9v/bitupp)
都是可重入锁。

- synchronized依赖于jvm而ReentrantLock依赖于api
    
- ReentrantLock 比 synchronized 增加了一些高级功能：
    
    - **等待可中断** : 通过 `lock.lockInterruptibly()` 来实现中断等待锁的线程的机制。lock()会忽略异常继续等待获取锁，而lockInterruptibly()则会抛出InterruptedException异常。
        
    - **可实现公平锁** : `ReentrantLock`可以指定是公平锁还是非公平锁。而`synchronized`只能是非公平锁。
        
    - **可实现选择性通知（锁可以绑定多个条件）**: `synchronized`关键字与`wait()`和`notify()`/`notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以实现，但是需要借助于`Condition`接口与`newCondition()`方法。
        
    - **支持超时** ：`ReentrantLock` 提供了 `tryLock(timeout)` 的方法，可以指定等待获取锁的最长等待时间，如果超过了等待时间，就会获取锁失败，不会一直等待。
        

**哪个性能更优？**

**性能**上的区别，synchronized 和 ReentrantLock 在不同场景下各有优势。

对于低竞争场景，由于synchronized 经过多次优化（如偏向锁、轻量级锁），一般与 ReentrantLock 相当甚至更好。

对于高竞争场景，ReentrantLock 提供了更多的灵活性（如公平锁、可中断锁等），更适合复杂需求。

  

synchronized 和 ReentrantLock 是 Java 中实现线程同步的两种主要方式，它们都能保证多线程环境下的数据一致性。

第一个是**基本概念**上的区别，synchronized 是 Java 的内置关键字，它是隐式的，通过 JVM 提供的监视器锁机制实现同步，使用简单，无需手动管理锁的获取和释放；而 ReentrantLock 是 java.util.concurrent.locks 包中的一个类，它是显式的，提供了更灵活的锁机制，需要开发者手动调用 lock() 和 unlock() 方法来控制锁的生命周期。

第二个是**功能特性**上的区别，ReentrantLock 提供了比 synchronized 更丰富的功能，比如：ReentrantLock 支持在等待锁的过程中响应中断，而 synchronized 不支持中断；还有ReentrantLock 提供了 tryLock() 方法，允许线程尝试获取锁并在指定时间内返回结果，而 synchronized 必须一直等待锁释放。

第三个是**性能**上的区别，synchronized 和 ReentrantLock 在不同场景下各有优势。

对于低竞争场景，由于synchronized 经过多次优化（如偏向锁、轻量级锁），一般与 ReentrantLock 相当甚至更好。

对于高竞争场景，ReentrantLock 提供了更多的灵活性（如公平锁、可中断锁等），更适合复杂需求。

第四个是锁的释放与异常处理上的区别，synchronized 在退出同步代码块时会自动释放锁，即使发生异常也不会导致死锁；而ReentrantLock 需要开发者手动调用 unlock() 方法释放锁，因此必须在 finally 块中确保锁的释放，否则可能导致死锁。。

## Volatile的作用

- 保证变量的可见性，如果一个变量声明为volatile说明这个变量是共享且不稳定的，每次使用它都到主存中进行读取
    
- 禁止指令重排：通过插入特定的 内存屏障 的方式来禁止指令重排序
    

## 什么是乐观锁？(635/1759=36.1%)

不加锁，只是在提交修改的时候去验证对应的资源（也就是数据）是否被其它线程修改了（具体方法可以使用**版本号**机制或 **CAS** 算法）。

### CAS 三大问题，以及怎么解决

1. ABA问题
    

- 问题：当变量从 A 被修改为 B，再改回 A 时，CAS 会误认为值未变而成功更新，导致逻辑错误（如链表节点复用场景）。
    
- 解决：
    
    - 引入版本号：每次更新时版本号 + 1，CAS 比较值的同时比较版本号（如`AtomicStampedReference`）。
        
    - 增加额外标记：记录变量的修改次数或状态，避免仅通过值判断。
        

2. 循环时间长、开销大
    

- 问题：CAS 失败时会自旋重试，若高并发下长期失败，会导致 CPU 占用率飙升。
    
- 解决：
    
    - 限制自旋次数：超过阈值后改用阻塞锁（如`ConcurrentHashMap`中`synchronized`与 CAS 结合）。
        
    - 自适应自旋：根据历史重试情况动态调整自旋次数（JVM 的优化）。
        

3. 只能保证单个变量的原子操作
    

- 问题：CAS 仅支持单个变量的原子更新，无法直接处理多个变量的原子操作。
    
- 解决：
    
    - 合并变量：将多个变量封装为一个对象（如用`AtomicReference`包装自定义对象）。
        
    - 借助锁：复杂场景下使用`synchronized`或`ReentrantLock`保证原子性。
        

#### AtomicStampedReference 如何解决 ABA 问题

内部维护两个字段 ——`value`（实际值）和`stamp`（版本号），两者作为一个整体参与 CAS 操作。

## 倒计时锁（CountDownLatch）、循环屏障（CyclicBarrier）和信号量（Semaphore）的区别
CountDownLatch适用于一个线程等待多个线程完成操作的情况
CyclicBarrier适用于多个线程在同一个屏障处等待(而且是循环使用的)
Semaphore适用于一个线程需要等待获取许可证才能访问共享

## 有三个线程T1,T2,T3如何保证顺序执行？
[✅有三个线程T1,T2,T3如何保证顺序执行？](https://www.yuque.com/hollis666/gg1x9v/wwqs6n658n4ip0ed#zITf4)

常见用CountDownLatch或者join

熟悉一下CyclicBarrier的用法
```
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierThreadExecute {

    public static void main(String[] args) throws Exception {
        // 创建 CyclicBarrier 对象，用来做线程通信
        CyclicBarrier barrier = new CyclicBarrier(2);

        // 创建并启动线程 T1
        Thread t1 = new Thread(new MyThread(barrier), "T1");
        t1.start();

        // 主线程等待线程 T1 执行完
        barrier.await();

        // 创建并启动线程 T2
        Thread t2 = new Thread(new MyThread(barrier), "T2");
        t2.start();

        // 等待线程 T2 执行完
        barrier.await();

        // 创建并启动线程 T3
        Thread t3 = new Thread(new MyThread(barrier), "T3");
        t3.start();

        // 等待线程 T3 执行完
        barrier.await();
    }
}

class MyThread implements Runnable {
    private CyclicBarrier barrier;

    public MyThread(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            // 模拟执行任务
            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName() + " 执行完毕");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // 等待其他线程完成
            try {
                barrier.await();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```
`CyclicBarrier barrier = new CyclicBarrier(2);`
表示屏障需要 **2 个线程都调用 `await()`** 才会放行。

## 并发

### ⭐️线程池处理任务的流程

1. **如果当前运行的****线程数****小于核心线程数，那么就会新建一个线程来执行任务。**
    
2. 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，那么就把该任务放入到任务队列里等待执行。
    
3. 如果向任务队列投放任务失败（任务队列已经满了），但是当前运行的线程数是小于最大线程数的，就新建一个线程来执行任务。
    
4. 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，拒绝策略会调用`RejectedExecutionHandler.rejectedExecution()`方法。
    

再提一个有意思的小问题：**线程池****在提交任务前，可以提前创建线程吗？**

答案是可以的！`ThreadPoolExecutor` 提供了两个方法帮助我们在提交任务之前，完成核心线程的创建，从而实现**线程池****预热**的效果：

- `prestartCoreThread()`:启动一个线程，等待任务，如果已达到核心线程数，这个方法返回 false，否则返回 true；
    
- `prestartAllCoreThreads()`:启动所有的核心线程，并返回启动成功的核心线程数。
    

### ⭐️线程池中线程异常后，销毁还是复用？

直接说结论，需要分两种情况：

- **使用****`execute()`****提交任务**：当任务通过`execute()`提交到线程池并在执行过程中抛出异常时，如果这个异常没有在任务内被捕获，那么该异常会导致当前线程终止，并且异常会被打印到控制台或日志文件中。线程池会检测到这种线程终止，并创建一个新线程来替换它，从而保持配置的线程数不变。
    
- **使用****`submit()`****提交任务**：对于通过`submit()`提交的任务，如果在任务执行中发生异常，这个异常不会直接打印出来。相反，异常会被封装在由`submit()`返回的`Future`对象中。当调用`Future.get()`方法时，可以捕获到一个`ExecutionException`。在这种情况下，线程不会因为异常而终止，它会继续存在于线程池中，准备执行后续的任务。
    

简单来说：**使用****`execute()`****时，未捕获异常导致线程终止，****线程池****创建新线程替代；使用****`submit()`****时，异常被封装在****`Future`****中，线程继续复用**。

这种设计允许`submit()`提供更灵活的错误处理机制，因为它允许调用者决定如何处理异常，而`execute()`则适用于那些不需要关注执行结果的场景。

### 如何给线程池命名？

  

### [如何设定线程池的大小？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#%E5%A6%82%E4%BD%95%E8%AE%BE%E5%AE%9A%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E5%A4%A7%E5%B0%8F)

- **CPU 密集型任务(N+1)**
    
- **I/O** **密集型任务(2N)**
    

### ⭐️如何设计一个能够根据任务的优先级来执行的线程池？

  

### 一个任务需要依赖另外两个任务执行完之后再执行，怎么设计？

  

### AQS 的原理是什么？
[✅如何理解AQS？](https://www.yuque.com/hollis666/gg1x9v/qka9yt)
AbstractQueuedSynchronizer(抽象队列同步器，以下简称AQS)
在AQS内部，维护了一个FIFO队列和一个volatile的int类型的state变量。state=1时表示当前对象锁已经被占有了，state的修改通过CAS完成。
![[Pasted image 20260318002934.png]]
持有锁的线程释放锁时，AQS会将等待队列中的第一个线程唤醒
（如果是公平锁，第一步先判断队列中是否有前序节点，如果有直接入队，不会进行第一次的CAS，非公平锁的话是先CAS，枷锁失败之后入队）


### 悲观锁（Pessimistic Lock）和乐观锁（Optimistic Lock）

悲观锁通常多用于写比较多的情况（多写场景，竞争激烈），这样可以**避免频繁失败和重试影响性能**，悲观锁的开销是固定的。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如`LongAdder`），也是可以考虑使用乐观锁的，要视实际情况而定。

乐观锁通常多用于写比较少的情况（多读场景，竞争较少），这样可以避免频繁加锁影响性能。不过，乐观锁主要针对的对象是单个共享变量（参考`java.util.concurrent.atomic`包下面的原子变量类）。

  

## CompletableFuture

### CompletableFuture与Future的区别

- Future表示异步计算的结果，只能通过**阻塞或****轮询**的方式获取结果，而且**不支持设置****回调**方法。Java8之前回调一般使用ListenableFuture，但会引入回调地狱（也就是很多层的嵌套）
    
- CompletableFuture对Future进行了扩展，可以通过设置**回调**的方式处理计算结果，同时也支持**组合**操作，支持进一步的编排，同时一定程度解决了回调地狱的问题。
    

### CompletableFuture常用的两个方法及区别

- supplyAsync：创建 `CompletableFuture` 对象，接受的参数是 `Supplier<U>`，u是返回结果值的类型
    
- runAsync()：创建 `CompletableFuture` 对象，接受的参数是 `Runnable`，不允许返回值
    
- thenCompose()： 按顺序链接两个 `CompletableFuture` 对象，将前一个任务的返回结果作为下一个任务的输入参数
    
- thenCombine()：会在两个任务都执行完成后，把两个任务的结果合并。两个任务是并行执行的，它们之间并没有先后依赖顺序。
    
- `thenApply()`：接受一个 `Function` 实例，用它来处理结果。
    
- `thenRun()`：不能访问异步计算的结果
    

### 不传线程池时CompletableFuture默认使用什么

默认会使用 **ForkJoinPool.commonPool** 作为线程池。**在高并发、阻塞型任务或复杂业务场景中，直接依赖默认线程池可能引发性能问题**

## 并发容器-并发安全

- **`ConcurrentHashMap`** : 线程安全的 `HashMap`
    
- **`CopyOnWriteArrayList`** : 线程安全的 `List`，在读多写少的场合性能非常好，远远好于 `Vector`。
    
- **`ConcurrentLinkedQueue`** : 高效的并发队列，使用链表实现。可以看做一个线程安全的 `LinkedList`，这是一个非阻塞队列。
    
- **`BlockingQueue`** : 这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。表示阻塞队列，非常适合用于作为数据共享的通道。
    

### ConcurrentHashMap

#### ConcurrentHashMap如何保证线程安全？

jdk1.7：对数组进行了分段，每把锁只锁容器其中一部分数据

jdk1.8：取消了分段锁，采用`CAS+Synchronized`来保证并发安全，`synchronized` 只锁定当前链表或红黑二叉树的首节点，这样只要 hash 不冲突，就不会产生并发，就不会影响其他 Node 的读写，效率大幅提升。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2YwOGNiNzI1MzNiZDYzM2M1Y2MzOTI1M2MwNWE4ZGFfTWhrQk44alhhV1oxRXkwdGU1UlkyNEZJM0E1TUtjQlNfVG9rZW46TjRFaGJTenlwb1JIcVV4MGZ3aWNhVGF0blVPXzE3NzM2NDk2NTM6MTc3MzY1MzI1M19WNA)

#### HashTable与ConcurrentHashMap区别

- `Hashtable` 是线程安全的,因为 `Hashtable` 内部的方法基本都经过`synchronized` 修饰。效率低，基本被淘汰
    
- HashTable 的全局锁机制导致高并发时性能瓶颈，所有线程必须串行化操作
    
- HashTable 扩容时会锁住整个表

# JVM

JVM是运行Java字节码（.class）的虚拟机，针对不同的系统有特定的实现。“一次编译，随处可以运行”

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmU5OWZhNGI5MTU3YzIxNjIxODBlNGY0YjExZTAxNThfU1k4TlJ1MmxOZVB3S3c0cUhmSzVUZmEzd05ubGsyQzVfVG9rZW46UjZ1VGJ0OEVPbzhEZjV4OExZWGMwY0hmbkNmXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

## jdk7和jdk8方法区不同的区别

在 JDK 1.8 及以后的版本中，方法区被**元空间**取代，使用本地内存，即直接使用操作系统的内存，不再占用 JVM 堆内存，默认情况下受操作**系统**可用**内存**限制。用于存储已被虚拟机加载的**类信息**、**常量、静态变量**等数据。

JDK 1.7：JVM 堆内存的一部分（逻辑上是堆的永久区域），受 JVM 内存参数限制

## 对象创建的过程了解吗？(392/1759=22.3%)

1. 类加载检查：首先检查能否在常量池中定位到一个类的符号引用，然后检查这个类是否已被加载、解析和初始化过，如果没有需要先加载类
    
2. 分配空间：在堆中划分一块内存
    
3. 初始化为零值（不包括对象头）
    
4. 必要设置（对象头）：必要设置：对象是哪个**类**的实例、**如何**才能**找**到类的元数据信息、对象的**哈希**码、对象的GC分代**年龄**
    
5. 构造函数，执行init：构造对象需要的其他资源和状态信息
    

## **[JVM 参数详解](https://www.cnblogs.com/ysocean/p/11109018.html)**

标准参数

标准参数是所有 JVM 实现都必须支持的参数，例如，`-version` 参数用于显示 Java 的版本信息，而 `-help` 参数可以列出所有标准参数。

**非标准参数（-X）**

非标准参数是 JVM 的扩展参数，它们可能在未来的 JVM 版本中发生变化。这些参数以 `-X` 开头，例如 `-Xms` 和 `-Xmx`用于设置 JVM 堆内存的初始大小和最大大小。

**非稳定参数（-XX）**

非稳定参数是 JVM 中最常用的参数类型，主要用于 JVM 调优和调试。这些参数的稳定性相对较低，可能会随着 JVM 版本的变化而改变。非稳定参数分为两类：Boolean 类型和 Key-Value 类型。

Boolean 类型的参数格式为 `-XX:[+-]<name>`，用于启用或禁用某个特性，如 `-XX:+UseG1GC` 启用 G1 垃圾收集器。

Key-Value 类型的参数格式为 `-XX:<name>=<value>`，用于设置特定属性的值，如 `-XX:MaxGCPauseMillis=500` 设置 GC 的最大停顿时间为 500 毫秒。

## 类载入过程 JVM 会做什么？（类加载）(364/1759=20.7%)

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=N2ZiZjA0N2RhMGI3ZGFhMDlhM2M1NWNlYTk3NDZjYjRfT0J4aERUaFRTbzVHMHhPZ3BINjViQ3hnSGFjSnoxcWJfVG9rZW46QW1HaWJTczdZb1ZyOWV4bkVSRmN1aFdhbjRjXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

1. 加载：通过全类名获取定义此类的二进制字节流，将字节流所代表的静态存储结构转换为方法区的运行时数据结构，在内存中生产一个代表该类的`class`对象，作为方法区这些数据的访问入口
    
2. 连接：
    
    1. 验证：确保class文件的字节流中包含的信息符合当前虚拟机的要求，不会危害虚拟机自身的安全
        
    2. 准备：为类变量分配内存并设置类变量初始值
        
    3. 解析：虚拟机将常量池内的符号引用替换为直接引用
        
3. 初始化：执行初始化方法`<clinit> ()`的过程，这一步 JVM 才开始真正执行类中定义的 Java 程序代码(字节码)
    
4. 使用：使用类或创建对象
    
5. 卸载：该类的 Class 对象被 GC
    
      卸载类需要满足 3 个要求:
    
    2. 该类的所有的实例对象都已被 GC，也就是说堆不存在该类的实例对象。
        
    3. 该类没有在其他任何地方被引用
        
    4. 该类的类加载器的实例已被 GC
        

## Java中的类什么时候会被加载?

1. 创建类的实例
    
2. 使用类的静态变量或方法
    
3. 通过反射机制访问类
    
4. jvm启动时会自动加载一些基础类
    

Java中的类加载其实是延迟加载的，除了一些基础的类之外，其他的类都是在需要使用类时才会进行加载。

## 如何破坏类加载机制？你了解哪些破坏类加载机制的实现？

在 Java 中，类加载机制遵循**双亲委派模型**（Parents Delegation Model），其核心是 “子类加载器委托父类加载器加载类，父类无法加载时子类才尝试加载”，以此保证类加载的安全性和唯一性。“破坏类加载机制” 本质是通过技术手段绕过或修改双亲委派模型，实现 “父类加载器加载子类加载器专属的类”“自定义类加载逻辑覆盖默认行为” 等非常规加载效果。

1. 线程上下文类加载器（适配 SPI（服务提供者接口））
    

线程上下文类加载器的引入是为了**解决父类加载器无法加载子类加载器的类**的问题。很多服务提供者接口（SPI）需要动态加载实现类，例如 JDBC、JNDI、JCE 等。这些 SPI 的接口由引导类加载器加载，而实现类通常由系统类加载器加载。由于引导类加载器无法直接访问系统类加载器加载的类，因此需要通过线程上下文类加载器来加载这些实现类。

使用线程上下文类加载器时，需要注意以下几点：

- **类加载器****的一致性**：确保多个需要通信的线程使用相同的类加载器，以避免类型转换异常（ClassCastException）。
    
- **破坏双亲委派模型**：线程上下文类加载器的使用会破坏双亲委派模型，因此需要谨慎使用
    

2. 热部署：动态替换已加载的类
    

默认类加载机制中，一个类被加载后（`Class`对象被放入方法区），JVM 不允许重复加载该类（同一个类加载器 + 全限定类名唯一）。热部署通过 “卸载旧类 + 用新类加载器加载新类”，打破 “类不可重复加载” 的约束，实现 “不重启 JVM 更新代码”。

3. 重写`loadClass()`/`findClass()`
    

JDK 1.2 之前，`java.lang.ClassLoader`的`loadClass()`方法并未强制实现双亲委派逻辑，子类加载器可直接重写该方法，完全绕过父类加载器。

JDK 1.2 后，`loadClass()`已强制双亲委派（先委托父类），但开发者可通过 “自定义类加载器 + 重写`findClass()`”，并结合 “改变类的加载路径 / 优先级”，实现对特定类的 “优先自定义加载”，间接破坏双亲委派的 “父类唯一加载权”。

## 什么是双亲委派模型？(425/1759=24.2%)

- **原理**：当类加载器收到类加载请求时，首先将请求委派给父类加载器，直到顶层的启动类加载器（Bootstrap ClassLoader）。只有当父类加载器无法加载该类时，子加载器才会尝试自己加载。核心是 “**父优先**”。
    
- **意义**：
    
    - 避免类重复加载，保证同一个类在 JVM 中只有一个版本。
        
    - 保证核心类的安全性，比如 java.lang.String 不会被自定义类加载器篡改。
        

## JVM的内存区域(631/1759=35.9%)

- 虚拟机栈：每个线程都有自己独立的Java虚拟机栈，每个方法在执行时都会创建一个栈帧，用于存储局部变量表、操作数栈、动态链接、方法出口等信息。可能会抛出 **StackOverflowError** 和 **OutOfMemoryError** 异常
    
- 本地方法栈：执行Native 方法时会创建栈帧
    
- 程序计数器：存储当前线程正在执行的 Java 方法的 JVM **指令****地址，**不会有OutOfMemoryError
    
- 堆：JVM中最大的一块内存区域，线程共享，存放对象实例。新生代、老年代、Eden 区和两个 Survivor 区
    
- 方法区（元空间jdk1.8之后，使用本地内存）：用于存储已被虚拟机加载的类信息、常量、静态变量等数据
    
- 运行时常量池：**方法区**的一部分，用于存放编译期生成的各种字面量和符号引用
    
## JAVA对象都在堆上吗？
new对象时，首先会进行逃逸分析，未逃逸，则jvm可能会通过对象拆分将其分配到栈上，随栈桢弹出释放，否则在堆上分配。

创建对象时，编译器会对对象进行逃逸分析，未发生逃逸的，会进行标量替换，将其拆分成若干个独立的局部变量放入栈中，随**栈帧**弹出释放（虚拟机栈内部有，局部变量表，操作数栈，动态链接，方法返回地址）。发生逃逸的，存入堆中。
什么是逃逸分析：是编译器的一种静态代码分析技术，判断当前**对象的生命周期是否仅仅存在于当前方法。
## 从编写 Java 代码层面说说怎么减少内存碎片

1. 避免频繁创建短命小对象 内存碎片多由大量短命小对象频繁创建和回收导致
    
    1. 复用对象
        
    2. 避免循环内创建对象
        
2. 合理使用数据结构，减少内存浪费
    
    1. 选择紧凑的数据结构：用 `ArrayDeque` 代替 `LinkedList`
        
    2. 避免使用大对象拆分：不要将一个完整对象拆分为多个小对象存储（如用多个 `HashMap` 存储相关属性），尽量用一个大对象集中存储，减少跨内存块的引用。
        
3. 优化长生命周期对象
    
    1. 避免长生命周期对象引用短生命周期对象
        
    2. 控制大对象的创建（尽量复用或拆分，需权衡拆分带来的碎片）
        

## 创建的对象什么时候晋升到老年代

- 15次GC之后
    
- 大对象直接进入老年代
    
- 或者年龄大于Survivor大部分对象的年龄（动态对象年龄判断）
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTFjOTAzZjQ4YzQxZWM3YWM0NjYyYjEwOThmOTM2MjhfT05sUTNrU0hOT01RY2NqaFBJN2RIVFJZdDZCUGl6UERfVG9rZW46TFlYdGJmOGFxb2xjQjB4bmJYcGNjY1Z3bldnXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

## 分代收集机制中Eden与Survivor的作用

堆内存通常分为：

1. 新生代：Eden 区、两个 Survivor 区 S0 和 S1
    
2. 老年代
    
3. 永久代（元空间）
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzljYmM1NjJjOWY1ODIwNjNkOTY0NDFkMGIwNGJhYWRfRzRJNExGQlNXRGkzNk96N2h0dUFWY0pyeFVXdmJHYzBfVG9rZW46RDlmUGIwYzNXb2NxYnF4WVd5aWM5dWFtbmpoXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

**Eden区（占新生代的80%）**是**新创建**对象的主要分配区域，大部分新对象都会被分配到这里

**Survivor区（占新生代的20%）**通常分为两个部分：**S0（Survivor 0）**和**S1（Survivor 1）**。这些区域用于存放经过**多次****垃圾回收****后仍然存活**的对象。

## 垃圾回收算法(673/1759=38.3%)

标记-清除

复制

标记-整理

## 死亡对象判断方法

### 引用计数法

给对象中添加一个引用计数器，统计其被引用的次数，目前主流的虚拟机中并没有选择这个算法来管理内存，其最主要的原因是它很难解决对象之间**循环引用**的问题

### 可达性分析算法

通过一系列的称为 **“****GC** **Roots”** 的对象作为起点，从这些节点开始向下搜索，节点所走过的路径称为引用链，当一个对象到 GC Roots 没有任何引用链相连的话，则证明此对象是不可用的，需要被回收。

整个过程都需要STW，以避免对象的状态发生改变

即使在可达性分析法中不可达的对象，也并非是“非死不可”的，要真正宣告一个对象死亡，至少要经历两次标记过程

### 三色标记算法

将对象状态分为三种：白色、灰色、黑色，标记过程分为三个阶段：初始标记，并发标记和重新标记，初始和重新标记阶段需要STW

- 白色：未被访问，即还未进行可达性分析
    
- 灰色：已被访问过，但其引用对象未进行可达性分析
    
- 黑色：已被访问过，且其引用对象也进行了可达性分析
    

### 引用类型总结

强引用：只要还有强引用指向对象，对象就存活，垃圾回收器不会处理存活对象。普通对象引用

软引用：内存不足时回收，缓存（如图片、数据缓存）

弱引用：只要垃圾回收机制一运行，不管JVM的内存空间是否够，都会回收该对象的占用内存。临时缓存、避免内存泄漏

虚引用：发生 GC 时回收，不能通过get()获取对象，跟踪对象回收状态、管理直接内存

## GC的意义

自动监测对象是否超过作用域从而达到自动回收内存的目的，自动管理内存

## 垃圾回收器(659/1759=37.5%)

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTg1YTI0M2FiOGQ3NTBkZjgwZGNjYzI4MDUwYmYxNmJfbkpsODlza1FUVWNncG5rc0pNUEEwUm9iZHFBcXEwMmpfVG9rZW46UnRKUmJySmVWb3NUbzN4NVJRemNHZVhEbm9oXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

### [Serial 收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#serial-%E6%94%B6%E9%9B%86%E5%99%A8)

Serial（串行）收集器是最基本、历史最悠久的垃圾收集器了。它的 **“单线程”** 的意义不仅仅意味着它只会使用一条垃圾收集线程去完成垃圾收集工作，更重要的是它在进行垃圾收集工作的时候必须暂停其他所有的工作线程（ **"Stop The World"** ），直到它收集结束。

新生代采用标记-复制算法，老年代采用标记-整理算法。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MDRlODA2ZmM0NzdlYWYwOGQyZmEyYjc2ZmFkZjRjNmNfMVp0T0N5c2h6c2RHNk42cXJGMjMyMVJSSG1jWWJsZ1ZfVG9rZW46REFwemJiTWRSbzdQZlJ4c3JvdWNYeU9JbnBkXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

### ParNew 收集器

ParNew 收集器其实就是 Serial 收集器的多线程版本，除了使用多线程进行垃圾收集外，其余行为（控制参数、收集算法、回收策略等等）和 Serial 收集器完全一样。

**新生代采用标记-复制算法，老年代采用标记-整理算法。**

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGFjNzIxY2Y3ZTcwZTU0Y2NlOTBmYWYxOGIxZDhmNTRfRjhLdXZPV0ZYSlNrSU9hNlRwa3lDdmI0ZXByZ21IVFRfVG9rZW46SE5lWWJ6T3BKb1F5OUZ4N2hhZ2N5cGRNbnNjXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

它是许多运行在 Server 模式下的虚拟机的首要选择，除了 Serial 收集器外，只有它能与 CMS 收集器（真正意义上的并发收集器，后面会介绍到）配合工作。

### [Parallel Scavenge 收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#parallel-scavenge-%E6%94%B6%E9%9B%86%E5%99%A8)

Parallel Scavenge 收集器也是使用标记-复制算法的多线程收集器，它看上去几乎和 ParNew 都一样。 那么它有什么特别之处呢？

Parallel Scavenge 收集器关注点是吞吐量（高效率的利用 CPU）。CMS 等垃圾收集器的关注点更多的是用户线程的停顿时间（提高用户体验）。所谓吞吐量就是 CPU 中用于运行用户代码的时间与 CPU 总消耗时间的比值。 Parallel Scavenge 收集器提供了很多参数供用户**找到最合适的停顿时间或最大吞吐量**，如果对于收集器运作不太了解，手工优化存在困难的时候，使用 Parallel Scavenge 收集器配合自适应调节策略，把内存管理优化交给虚拟机去完成也是一个不错的选择。

**新生代采用标记-复制算法，老年代采用标记-整理算法。**

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjdlOTQwMzNlZTZlODFkMDJkMjRhZWU4MDI5YzZjMjdfdldzVVM1VzdtOWg3VjZtSlN2eER6MWlHZ3BoSU9CbkpfVG9rZW46Rkc1b2JNSDlyb1J0Nnl4Zm1IWWNnNGhRbktmXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

### [CMS 收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#cms-%E6%94%B6%E9%9B%86%E5%99%A8)

CMS（Concurrent Mark Sweep）收集器是一种以获取**最短回收停顿时间**为目标的收集器。它非常符合在注重用户体验的应用上使用。

**CMS（****Concurrent** **Mark Sweep）收集器是 HotSpot** **虚拟机****第一款真正意义上的并发收集器，它第一次实现了让****垃圾收集****线程与****用户线程****（基本上）同时工作。**

从名字中的**Mark Sweep**这两个词可以看出，CMS 收集器是一种 **“标记-清除”算法**实现的，它的运作过程相比于前面几种垃圾收集器来说更加复杂一些。整个过程分为四个步骤：

- **初始标记：** 短暂停顿，标记直接与 root 相连的对象（根对象）；
    
- **并发标记：** 同时开启 GC 和用户线程，用一个闭包结构去记录可达对象。但在这个阶段结束，这个闭包结构并不能保证包含当前所有的可达对象。因为用户线程可能会不断的更新引用域，所以 GC 线程无法保证可达性分析的实时性。所以这个算法里会跟踪记录这些发生引用更新的地方。
    
- **重新标记：** 重新标记阶段就是为了修正并发标记期间因为用户程序继续运行而导致标记产生变动的那一部分对象的标记记录，这个阶段的停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段时间短
    
- **并发清除：** 开启用户线程，同时 GC 线程开始对未标记的区域做清扫。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDlkZGY5YmM0YmYyZWViZDA5OGY0ZWM5OGMyMWZjNWRfcTE3YVU4aTZlamd1WENxMFFua0pOUlFTbGJkM0hoM0NfVG9rZW46VWE3T2JiOE5hb2pVMnR4OFB6WWNLVTNyblRlXzE3NzM2NDk3MTk6MTc3MzY1MzMxOV9WNA)

从它的名字就可以看出它是一款优秀的垃圾收集器，主要优点：**并发收集、低停顿**。但是它有下面三个明显的缺点：

- **对 CPU 资源敏感；**
    
- **无法处理浮动垃圾；**
    
- **它使用的回收算法-“标记-清除”算法会导致收集结束时会有大量空间碎片产生。**
    

**CMS** **垃圾回收****器在 Java 9 中已经被标记为过时(deprecated)，并在 Java 14 中被移除。**

### G1 收集器

**G1 (Garbage-First) 是一款面向服务器的****垃圾收集器****,主要针对配备多颗处理器及大容量****内存****的机器. 以极高概率满足** **GC** **停顿时间要求的同时,还具备高****吞吐量****性能特征。**

被视为 JDK1.7 中 HotSpot 虚拟机的一个重要进化特征。它具备以下特点：

- **并行****与并发**：G1 能充分利用 CPU、多核环境下的硬件优势，使用多个 CPU（CPU 或者 CPU 核心）来缩短 Stop-The-World 停顿时间。部分其他收集器原本需要停顿 Java 线程执行的 GC 动作，G1 收集器仍然可以通过并发的方式让 java 程序继续执行。
    
- **分代收集**：虽然 G1 可以不需要其他收集器配合就能独立管理整个 GC 堆，但是还是保留了分代的概念。
    
- **空间整合**：与 CMS 的“标记-清除”算法不同，G1 从整体来看是基于“标记-整理”算法实现的收集器；从局部上来看是基于“标记-复制”算法实现的。
    
- **可预测的停顿**：这是 G1 相对于 CMS 的另一个大优势，降低停顿时间是 G1 和 CMS 共同的关注点，但 G1 除了追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定在一个长度为 M 毫秒的时间片段内，消耗在垃圾收集上的时间不得超过 N 毫秒。
    

G1 收集器的运作大致分为以下几个步骤：

- **初始标记**： 短暂停顿（Stop-The-World，STW），标记从 GC Roots 可直接引用的对象，即标记所有直接可达的活跃对象
    
- **并发标记**：与应用并发运行，标记所有可达对象。 这一阶段可能持续较长时间，取决于堆的大小和对象的数量。
    
- **最终标记**： 短暂停顿（STW），处理并发标记阶段结束后残留的少量未处理的引用变更。
    
- **筛选回收**：根据标记结果，选择回收价值高的区域，复制存活对象到新区域，回收旧区域内存。这一阶段包含一个或多个停顿（STW），具体取决于回收的复杂度。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjNhOGE4N2E1MTExZGJmNDY4Y2RhODIzZGJlZjA2MzZfNlBiVjA1ckFWSzhlaXJ4enc5VjNjMHlwSm5aV3ZJVFpfVG9rZW46TlpZVWJ0dmNtb3dHWEJ4NjdWVWNJUGx5bnI1XzE3NzM2NDk3MjA6MTc3MzY1MzMyMF9WNA)

**G1 收集器在后台维护了一个优先列表，每次根据允许的收集时间，优先选择回收价值最大的 Region(这也就是它的名字 Garbage-First 的由来)** 。这种使用 Region 划分内存空间以及有优先级的区域回收方式，保证了 G1 收集器在有限时间内可以尽可能高的收集效率（把内存化整为零）。

**从 JDK9 开始，G1** **垃圾收集器****成为了默认的垃圾收集器。**

#### G1回收器如何建立可预测的停顿时间模型

G1收集器根据**历史回收数据**构建一个预测模型，预测在给定的停顿时间内可以回收哪些区域（Region）。这个模型会分析过去回收同样大小内存所需的时间，并基于此预测未来回收操作。

|   |   |   |
|---|---|---|
|特性|CMS (Concurrent Mark Sweep)|G1 (Garbage First)|
|内存布局|传统分代：物理上连续的新生代和老年代。|Region 模式：将堆划分为一个个大小相等的独立区域（Region），逻辑分代，物理不连续。|
|回收算法|老年代使用 标记-清除（容易产生内存碎片）。|整体看是 标记-整理，局部（两个Region间）是 复制。（没有内存碎片）|
|核心优势|并发收集，低延迟。|可预测停顿，无碎片，适合大内存服务器。|
|致命弱点|产生大量碎片，导致 Full GC；对 CPU 敏感。|内存占用（Footprint）比 CMS 高。|

### ZGC
极低延迟，支持超大堆
GC算法：并发标记-整理+可扩展的区域分配
## STW的线程挂起机制

GC 设全局标志 $$\rightarro$$ 业务线程轮询 $$\rightarro$$ 业务线程跑到最近的**安全点**主动挂起 $$\rightarro$$ GC 凑齐所有线程进入等待屏障 $$\rightarro$$ 执行回收

## 常见的OOM

- java.lang.OutOfMemo1.ryError: PermGen space：Java7 永久代（方法区）溢出，
    
- java.lang.StackOverflowError：**虚拟机****栈溢出**，一般是由于程序中存在 **死循环或者深度****递归****调用** 造成的
    
- java.lang.OutOfMemoryError: Java heap space：**Java** **堆内存****溢出**，溢出的原因一般由于 JVM 堆内存设置不合理或者内存泄漏导致，如果是内存泄漏，可以通过工具查看泄漏对象到 GC Roots 的引用链。掌握了泄漏对象的类型信息以及 GC Roots 引用链信息，就可以精准地定位出泄漏代码的位置
    

### 解决方法

查看JVM内存分布：假设 Java 应用 PID 为 15162，jmap -heap 15162

dump文件分析：Dump 文件是 Java 进程的内存镜像，其中主要包括 **系统信息**、**虚拟机****属性**、**完整的线程 Dump**、**所有类和对象的状态** 等信息

# 数据库

## 关系型数据库和非关系型数据库有哪些区别？

1. 关系型数据库以**表**的形式进行存储数据，而非关系型以**键值对**的形式存储数据
    
2. 关系型数据库需要保证事务的ACID，而非关系型数据库中的事务一般无法回滚
    
3. 关系型数据库可以通过一张表中的任意字段进行查询，非关系型数据库需要通过key进行查询
    
4. 一般来说关系型数据库基于硬盘存储，非关系型基于内存存储（Mongodb基于磁盘存储）
    
5. 关系型支持各种范围查询、公式计算等
    

### 有了关系型数据库，为什么还需要NOSQL?

NOSQL数据库**无需提前设计表结构**，数据可以根据需要自由地存储和组织，而且相对于关系型数据库，NOSQL高效灵活，非常适合那些复杂、高变化、高并发量得场景中。

## SQL 中 UNION 和 JOIN 的区别

### UNION 和 UNION ALL 的区别

1. `UNION ALL`：直接合并多个结果集，**不过滤重复行**，性能通常更好。
    
2. `UNION`：合并后会做**去重**（等价于 `DISTINCT` 语义），通常比 `UNION ALL` 更慢。
    
3. 使用条件：
    
    1. 多个 `SELECT` 的列数必须一致；
        
    2. 对应列的数据类型需要兼容；
        
    3. 最终结果列名通常以第一条 `SELECT` 为准。
        
4. 面试回答建议：业务不要求去重时优先用 `UNION ALL`，只有明确需要唯一结果时再用 `UNION`。
    
示例：

```sql
SELECT name FROM t1
UNION ALL
SELECT name FROM t2;  -- 保留重复

SELECT name FROM t1
UNION
SELECT name FROM t2;  -- 去重
```

### JOIN、LEFT JOIN、RIGHT JOIN 的区别

1. `JOIN` 默认就是 `INNER JOIN`：只返回两张表**匹配成功**的行。
    
2. `LEFT JOIN`：返回左表全部行，右表匹配不上时右表列补 `NULL`。
    
3. `RIGHT JOIN`：返回右表全部行，左表匹配不上时左表列补 `NULL`。
    
4. `LEFT JOIN` 和 `RIGHT JOIN` 本质可以互换（交换左右表顺序即可）。
    
示例：

```sql
-- INNER JOIN：只保留匹配行
SELECT *
FROM A
JOIN B ON A.id = B.a_id;

-- LEFT JOIN：保留 A 全部行
SELECT *
FROM A
LEFT JOIN B ON A.id = B.a_id;

-- RIGHT JOIN：保留 B 全部行
SELECT *
FROM A
RIGHT JOIN B ON A.id = B.a_id;
```

补充（高频追问）：

- `LEFT JOIN` 时，针对右表的过滤条件优先写在 `ON` 中；如果写在 `WHERE`，可能把外连接效果“过滤掉”，结果接近内连接。
#### 表连接算法
1. Nested Loop Join：小表驱动大表
2. Merge Join：有序数据，类似归并
3. Hash Join：用 hash 表匹配，适用于大数据量

### HAVING 和 WHERE 的区别

1. `WHERE`：在**分组前**过滤原始数据，作用对象是一行一行的记录。
    
2. `HAVING`：在 `GROUP BY` **分组后**过滤结果，作用对象是分组后的结果集。
    
3. `WHERE` 一般不能直接使用聚合函数，如 `COUNT()`、`SUM()`、`AVG()`；`HAVING` 可以配合聚合函数使用。
    
4. 执行顺序可以理解为：`FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY`
    
5. 面试回答：`WHERE` 是对原始数据过滤，`HAVING` 是对分组后的结果过滤。
    
示例：

```sql
SELECT dept_id, COUNT(*) AS cnt
FROM employee
WHERE salary > 5000
GROUP BY dept_id
HAVING COUNT(*) > 5;
```

含义：

- `WHERE salary > 5000`：先过滤原始员工数据
- `GROUP BY dept_id`：再按部门分组
- `HAVING COUNT(*) > 5`：最后筛选人数大于 5 的部门

## MySQL

### 三大范式

1. 不可分割：要求数据库表的每一列都是不可分割的原子数据项
    
2. 完全依赖：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）（针对联合主键而言）
    
3. 直接相关：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）
    

### 执行流程（原理）

MySQL的架构共分为两层：Server层和存储引擎层

- Server层负责建立连接、分析和执行SQL。
    
- 存储引擎层负责数据的存储和提取。
    

一条 SQL 查询语句的执行流程

1. 连接器：建立连接，管理连接、校验用户身份；
    
2. 查询缓存：mysql8.0之后不会经过查询缓存这个阶段
    
3. 解析sql：解析器进行词法分析和语法分析，然后构建语法树，方便后续模块读取表名、字段、语句类型；
    
4. 执行sql：
    
    1. 预处理：检查表或字段是否存在；将 select * 中的 * 符号扩展为表上的所有列。
        
    2. 优化：选择查询成本最小的执行计划
        
    3. 执行
        

### InnoDB和MyISAM有什么区别?
|        | InnoDB              | MyISAM   |
|--------|---------------------|----------|
| 事务   | 支持                | 不支持   |
| 外键   | 支持                | 不支持   |
| 聚簇索引 | 支持              | 不支持   |
| 锁级别 | 支持行级锁、表级锁   | 表级锁   |
| 行数保存 | 不支持            | 支持     |
| 默认版本 | 5.5 之后          | 5.5 之前 |
| 全文索引 | 5.6以后支持       | 支持     |
### 索引
### 索引、联合索引设置要考虑什么问题
1. 最左匹配原则
2. 查询场景：要结合 `where`、`order by`、`group by` 一起看，不是只看单个字段。
3. 覆盖索引：查询直接命中索引返回，减少回表
4. 更新成本：更新频繁的字段、区分度很低的字段，不建议构建索引
5. 补充：等值匹配优先于范围查询，避免索引失效（函数运算，隐式类型转换，%）
#### 索引失效问题怎么排查

通过explain分析sql语句，查看其执行计划，主要关注type，key和extra

如果发现没走索引，可能是以下几种情况：没有正确创建索引；索引区分度不高；表太小；查询语句中索引字段用到了函数、类型不一致导致索引失效[[JAVA#^b81053]]

#### 聚簇索引

聚簇索引：叶子节点存放的是**实际数据**（默认是主键索引），查询**效率更高**，一个表中**只能有一个**聚簇索引

缺点：数据的插入和更新操作较慢，索引的维护成本高，数据的顺序访问效率高，但随机访问效率低

二级索引（非聚簇索引）：叶子节点存放的是主键值，需要回表查询

#### 数据结构

B树和B+树

1. 单点查询：b树最快查询一次就可以查询到，但是b+树一定要访问到叶子节点才能查询到数据
    
2. 插入/删除效率，B+树有大量的冗余节点，这样使得删除一个节点的时候可以直接从叶子节点中删除
    
3. 范围查询：b+的叶子节点采用双向链表，适合范围查询
    
4. b+树只在叶子节点存储数据，而b树的每个节点都包含数据（索引+记录），b+树的磁盘io次数更少
    

b+树分为主键索引和二级索引两种

- **主键****索引**的 B+Tree 的叶子节点存放的是**实际数据**，所有完整的用户记录都存放在主键索引的 B+Tree 的叶子节点里；
    
- **二级索引**的 B+Tree 的叶子节点存放的是**主键****值**，而不是实际数据。
    

**影响****MySQL** **B+ 树高度的因素**

主要原因，什么时候会导致高度增加？比如说第一层的所有索引正好放满一页，此时再增加一个索引就会导致分页，于是高度增加。（类似于多级页表，第一级就一张页表）

1. 索引字段大小：字段越大（如 VARCHAR (255)），每个索引页（默认 16KB）能存储的索引项越少，树高越高。
    
2. 数据量：数据总行数越多，需要的叶子节点越多，可能导致树高增加。
    
3. 页大小：MySQL 索引页默认 16KB，页越大，单个节点能存储的索引项越多，树高越低（可通过 `innodb_page_size` 调整）。
    
4. 索引类型：
    
    1. 聚簇索引（主键索引）：叶子节点存储完整数据行，数据行大小影响叶子节点数量。
        
    2. 二级索引：叶子节点存储主键，主键大小间接影响二级索引树高。
        
5. 数据插入顺序：无序插入可能导致页分裂频繁，间接影响树结构高度。
    

**回表**

如果查询的数据能够在二级索引中查询到，就不需要回表，此时是覆盖索引。否则需要根据主键值再检索主键索引，这个过程就是回表

#### Mysql的索引的最佳实践

1. 什么时候适用索引
    
    1. 字段有**唯一性限制**，如商品编码
        
    2. **经常用于****`where`****查询条件的字段**，这样能够提高整个表的查询速度，如果查询的不是一个字段，可以建立联合索引
        
    3. **经常用于****`group by`****和****`order by`****的字段**，这样在查询的时候就不需要再去做一次排序
        
2. 什么时候不需要创建索引
    
    1. **`where`****、****`group by`****和****`order by`**用不到的字段
        
    2. 字段中有**大量重复数据**，比如性别字段
        
    3. **数据太少**时不需要创建索引
        
    4. **经常更新的字段**不用创建索引
3. 考虑联合索引
4. 考虑索引覆盖：联合索引可以通过索引覆盖而避免徽标查询
5. 避免创建过多的索引：创建过多的索引会占用大量的磁盘空间
6. 避免使用过长的索引：索引列的长度越长，索引效率越低。
7. 索引失效场景 ^b81053
    
    1. 范围查询、最左匹配原则
        
    2. 模糊查询
        
    3. 字段中有**大量重复数据**
        
    4. 使用函数、进行表达式计算、隐式类型类型转换
        
    5. 在 **WHERE 子句中，如果在 OR** 前的条件列是索引列，而在 OR 后的条件列不是索引列，那么索引会失效。
        
    6. is not null
        

##### MySQL用了函数一定会索引失效吗？

在MySQL8.0之后就不一定了，因为有了函数索引，他就是用来优化函数的。

函数索引不是直接在表的列上创建的，而是基于列的某个表达式创建的。这个表达式可以是简单的数学运算，也可以是字符串函数、日期函数等。创建了函数索引后，MySQL可以在执行涉及该表达式的查询时使用这个索引，从而提高查询效率。

#### 联合索引

最左匹配原则。

联合索引的最左匹配原则，在遇到范围查询（如 >、<）的时候，就会停止匹配，也就是范围查询的字段可以用到联合索引，但是在范围查询字段的后面的字段无法用到联合索引。注意，对于 >=、<=、BETWEEN、like 前缀匹配的范围查询，并不会停止匹配。（但是只有在等于时才会匹配之后的条件，否则依然无法筛选）

##### 索引下推

[五分钟搞懂MySQL索引下推大家好，我是老三，分享一个小知识点。面试时候问到索引，常常会顺嘴问一句索引下推。给我五分钟， - 掘金](https://juejin.cn/post/7005794550862053412)

索引下推的目的是为了减少回表次数，也就是要减少IO操作。对于**`InnoDB`**的**聚簇索引**来说，数据和索引是在一起的，不存在回表这一说。

在二级索引的情景下，且遇到了范围查询，如果没有下推就需要回表查询所有数据行之后再对比b字段的值，如果有下推，就会再联合索引遍历过程中对b的值先做判断，进行筛选，减少回表次数，explain中Extra 为 `Using index condition`，说明使用了索引下推

##### 索引优化

- 索引最好设置为not null：如果存在null，优化器在做索引选择的时候更加复杂，此外空值没有意义，但会占用物理空间
    
- 防止索引失效：
    
- 覆盖索引：SQL 中 query 的所有字段能够从二级索引中查询到，避免回表
    
- 前缀索引：使用某个字段中字符串的前几个字符建立索引
    
- 主键索引自增：主键是聚簇索引，数据被存放在了B+树的叶子节点上，如果采用自增主键，那么相当于是追加，不需要移动数据，如果采用非自增主键，插入的位置是随机的，需要移动数据甚至引起页分裂
    

### Mysql中分区分表后在代码层面如何解决原本只查一个表后续要查多个表的问题

1. 使用分表中间件
    
2. 代码层封装分表路由逻辑：定义一个分表工具类
    

### 分表中非分片键查询

1. 尽量将非分片键查询转为分片键查询（根源优化）:按`user_id`分表存储订单数据，若业务需要 “查询 2024 年 9 月创建的所有订单”（非分片键查询），可调整为 “先按用户维度查询（含`user_id`分片键），再过滤时间”（如用户中心筛选目标用户，再查对应分表的订单）；
    
2. 构建非分片键的 “全局索引表”（常用方案）: 单独创建一张 “索引表”，存储 “非分片键→分片键” 的映射关系，查询时先查索引表定位分片键，再查目标分表。
    

### 事务

#### 特性

原子性、一致性、隔离性、持久性

- 持久性通过redo log （重做日志）实现
    
- 原子性通过undo log（回滚日志）实现
    
- 隔离性通过MVCC或锁机制
    
- 一致性则是通过持久性+原子性+隔离性来保证；
    

并发事务存在的问题：脏读、不可重复读、幻读

四种隔离级别：读未提交、读已提交、可重复读、串行

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YjI1YmNkNWVjOWQ0ZjkzYTBmMmFjOWJkYzI3ZmI3NDdfM3J2M3lWWHdqSjNvWW1jdXFqNldDQlNDNmZLa0dhNHpfVG9rZW46REhIYmI1QjV5b1NyaFV4OWZTQ2NMSGxJbjNjXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)

#### MVCC

MVCC 的核心设计目标是 “**解决已存在行的版本可见性问题**”，它通过 “快照” 锁定了 “事务启动时的行版本状态”，但无法锁定 “行的数量”—— 因为：

- 对于 新增的行：事务启动时不存在，没有历史版本，MVCC 无法追溯其 “是否在事务启动前存在”，只能根据 Read View 判断其修改事务是否已提交，若已提交则会被读取到，导致行数增加；
    
- 对于 删除的行：事务启动时存在，但被其他事务删除后，该行使版本链的 “最新版本” 变为 “删除标记”，MVCC 会过滤掉该版本，导致行数减少（也属于幻读的一种）。
    

##### MVCC 在 RC （读已提交）和 RR （可重复读）下的区别

核心区别：Read View 生成时机

1. RC：事务每次查询都生成新的Read View
    
2. RR：事务内只生成一次Read View
    

**InnoDB 如何真正解决****幻读****？—— Next-Key Lock**

##### MVCC用到的数据结构

1. **隐藏列（对于使用 InnoDB 存储引擎的数据库表，它的****聚簇索引****记录中都包含下面两个隐藏列）**
    
    1. 每行数据除用户定义的列外，包含 3 个隐藏列：
        
        - `TRX_ID`：最近一次修改该记录的事务 ID（64 位）。
            
        - `ROLL_PTR`：回滚指针，指向 undo log 中该记录的历史版本（形成版本链）。
            
        - `ROW_ID`：若表无主键，InnoDB 会自动生成该列作为行唯一标识。
            
2. **Undo** **Log****（回滚日志）**
    
    1. 存储数据的历史版本，通过 `ROLL_PTR` 串联成版本链（链表结构）。
        
    2. 事务回滚或 MVCC 读取历史版本时使用，当版本不再被需要时会被 purge 线程清理。
        
3. **Read View（读视图）**
    
    1. 一个数据结构，用于判断当前事务可见的数据版本范围，包含：
        
        - `m_ids`：当前活跃事务 ID 列表（未提交的事务）。
            
        - `min_trx_id`：活跃事务中最小的 ID。
            
        - `max_trx_id`：当前系统即将分配的下一个事务 ID（非活跃事务的最大值）。
            
        - `creator_trx_id`：创建该 Read View 的事务 ID。
            
    2. 作用：通过比较记录的 `DB_TRX_ID` 与 Read View 中的值，判断记录是否可见（见 171 详解）。
        
4. **事务 ID 生成机制**
    
    1. 事务启动时分配唯一递增的 64 位事务 ID，用于标记修改操作和 Read View 快照。
        

**流程示例**

- 事务 A（ID=100）读取一条记录，生成 Read View（`m_ids=[101,102]`，`min=101`，`max=103`）。
    
- 记录最新版本的 `trx_id=99`（小于 `min_trx_id=101`）→ 可见。
    
- 若记录版本 `trx_id=101`（在 `m_ids` 中）→ 不可见，需通过 `DB_ROLL_PTR` 找更早版本。
    
- 直到找到符合规则的版本，或版本链结束（无可见版本，视为记录不存在）。
    

#### 当前读和快照读

假设在可重复读的隔离级别下，线程 A 开启事务先 select 了一个 id 为 1 的记录（表中没有），然后线程 B 开启事务 insert 了 id 为 1 的记录，这时候 A 再 select 可以看到吗？怎么样 A 才可以看到？（先答的提交事务之后再开启事务）那不提交事务呢？

上述 “看不到” 的前提是 A 执行的是 **快照读**（普通 `select`，不加锁）。若 A 执行 当前读（如 `select ... for update`、`select ... lock in share mode`），则会跳过快照，直接读取数据库最新数据 —— 但这属于 “打破可重复读的一致性”，并非默认行为。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MWUzOGQxNGE2NzhlZWViOTRkNDhmYjcwYjlmY2NhOGNfQ01wVE91SDhSNE92ZGpPOW55Y28zeklQVjdvcDFGZTNfVG9rZW46SGdBYmJ5SVljbzdtbnh4U2xpT2NMeEw3bkxnXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)

MVCC 的快照机制只对已有行生效，而无法阻止新行的插入干扰查询结果。

### 日志

#### Binlog

二进制日志，追加日志，写满了就会新建一个文件继续写（不会覆盖原有的数据），是全量日志，只记录对数据的修改操作（DML和DDL），不记录select和show等查询类操作，用于**备份恢复，主从复制**。

binlog 有 3 种格式类型，分别是 STATEMENT（默认格式）、ROW、 MIXED，区别如下：

STATEMENT：记录操作（相当于记录了逻辑操作，所以针对这种格式， binlog 可以称为逻辑日志）（存在问题：如果使用了uuid和now这些函数会导致主从库的数据不一致）

ROW：记录修改后的行数据（存在问题：如果使用update批量更新数据会记录多条数据，但是如果是STATEMENT只有update一句）

MIXED：根据不同情况自动选择以上两种方法

##### binlog 如果特别大，影响性能如何解决

1. **定期清理 Binlog 文件**：及时清理不再需要的 Binlog 文件，可以释放磁盘空间，提高数据库性能。
    
2. **合理配置 Binlog 类型**：MySQL 支持三种 Binlog 类型：STATEMENT、ROW 和 MIXED。合理选择 Binlog 类型可以减少日志量，从而降低性能开销。例如，对于只读操作较多的数据库，可以选择 ROW 类型。
    

##### 如何开启binlog日志

修改mysql的`my.cnf`配置文件

一般默认是在`/etc/my.cnf`路径下

简单开启binlog demo

在mysqld下添加第一种方式

```SQL
#第一种方式:
#开启binlog日志
log_bin=ON
#binlog日志的基本文件名
log_bin_basename=/var/lib/mysql/mysql-bin
#binlog文件的索引文件，管理所有binlog文件
log_bin_index=/var/lib/mysql/mysql-bin.index
#配置serverid
server-id=1
```

##### MySQL 主从同步（原理 + 问题解决）

MySQL 主从同步用于实现**读写分离**（主库写，从库读），提高数据库并发能力，核心原理是 “binlog 复制”。

###### （1）主从同步流程

1. **主库（****Master****）操作**：
    
    1. 主库执行写操作（`INSERT`/`UPDATE`/`DELETE`）后，将操作记录写入 binlog（二进制日志）；
        
    2. 主库有一个 IO 线程，监听 binlog 变化，当从库连接时，将 binlog 事件发送给从库。
        
2. **从库（****Slave****）操作**：
    
    1. 从库有一个 IO 线程，接收主库发送的 binlog 事件，写入本地的 relay log（中继日志）；
        
    2. 从库有一个 SQL 线程，读取 relay log，解析并执行其中的 SQL 语句，同步主库数据。
        

###### （2）主从同步模式

- **异步复制**（默认）：主库写入 binlog 后立即返回，不等待从库接收，可能导致主库宕机后数据丢失；
    
- **半同步复制**：主库写入 binlog 后，等待至少一个从库接收并确认 relay log 后才返回，减少数据丢失风险；
    
- **全同步复制**：主库等待所有从库接收并执行完 relay log 后才返回，数据一致性最高，但性能最差。
    

###### （3）常见问题与解决

- **主从延迟**：
    
    - 原因：从库 SQL 线程执行慢（如大事务）、网络延迟；
        
    - 解决：1. 主库拆分大事务；2. 从库使用多 SQL 线程（MySQL 5.6+ 支持）；3. 读写分离时，热点读请求优先走主库。
        
- **主库宕机**：
    
    - 解决：部署哨兵（Sentinel）或 MGR（MySQL Group Replication），自动检测主库状态，主库宕机后从库选举新主库，实现故障转移。
        

#### Undolog

撤销回滚操作，保证了事务ACID中的原子性

- undo log 记录了此次事务「开始前」的数据状态，记录的是更新之前的值；
    

#### RedoLog

保证了事务ACID中的持久性，物理日志

- redo log 记录了此次事务「完成后」的数据状态，记录的是更新之后的值；
    
- 事务提交之前发生了崩溃，重启后会通过 undo log 回滚事务，事务提交之后发生了崩溃，重启后会通过 redo log 恢复事务
    
- 将写操作从「随机写」变成了「顺序写」
    

##### 刷盘时机

Redo Log（重做日志）的刷盘时机由`innodb_flush_log_at_trx_commit`参数控制，有 3 种策略：

1. `innodb_flush_log_at_trx_commit=1`（默认，最安全）
    
    1. 事务提交时，立即将 Redo Log 从内存（log buffer）刷到磁盘（物理写入），确保崩溃后数据不丢失。
        
2. `innodb_flush_log_at_trx_commit=0`（性能最优，安全性最低）
    
    1. 事务提交时不刷盘，仅写入 log buffer，由后台线程每 1 秒刷盘一次。崩溃可能丢失 1 秒内的事务。
        
3. `innodb_flush_log_at_trx_commit=2`（平衡）
    
    1. 事务提交时写入操作系统缓存（OS cache），不立即刷到磁盘，由 OS 每 1 秒刷盘一次。崩溃可能丢失未刷盘的事务，但比 0 更安全（OS 缓存比 log buffer 更可靠）
        

建议：金融等核心业务用 1，非核心业务追求性能可用 2。

### 调优

MySQL调优主要分为三个步骤：监控报警、 排查慢SQL、MySQL调优

1. 监控工具（例如Prometheus+Grafana）监控MySQL，发现查询性能变慢，报警提醒运维人员
    
2. 排查慢sql：
    
    1. 开启慢查询日志
        
    2. 找出最慢的集体几条sql
        
    3. 分析查询计划
        
3. MySQL调优
    

#### 项目中怎么实现的SQL调优？单select改成批量查询要注意什么？（游标方式）追问游标方式的查询中的退出条件（返回数据量＜批量大小），如果每次查10条一共50条记录，需要查几次（6次），怎么优化？

##### （1）项目中 SQL 调优的核心手段

1. **索引优化**：
    
    1. 为 `WHERE`、`JOIN`、`ORDER BY` 字段建索引（如 “订单表” 为 `user_id`、`create_time` 建联合索引）；
        
    2. 避免索引失效（如不用 `NOT IN`、`!=`、函数操作索引字段，如 `DATE(create_time) = '2025-01-01'` 改为 `create_time BETWEEN '2025-01-01 00:00:00' AND '2025-01-01 23:59:59'`）；
        
    3. 使用覆盖索引（如 `SELECT user_id, order_no FROM order WHERE create_time BETWEEN ...`，索引包含 `create_time, user_id, order_no`，避免回表）。
        
2. **SQL** **语句优化**：
    
    1. 拆分复杂 `JOIN`（如 5 表 JOIN 拆分为 “主表 + 子查询”，减少关联次数）；
        
    2. 禁用 `SELECT *`，只查必要字段（减少 IO 和内存占用）；
        
    3. 大表分页用 “主键定位” 替代 `LIMIT OFFSET`（如 `SELECT * FROM order WHERE id > 1000 LIMIT 10`，避免全表扫描）。
        
3. **批量操作优化**：
    
    1. 单条 `INSERT` 改批量 `INSERT`（如 `INSERT INTO user (id, name) VALUES (1, 'a'), (2, 'b')`，减少网络交互次数）；
        
    2. 单条 `UPDATE` 改 `CASE WHEN` 批量更新（如 `UPDATE product SET stock = CASE id WHEN 1 THEN 10 WHEN 2 THEN 20 END WHERE id IN (1,2)`）。
        

##### （2）游标批量查询的优化（以 MySQL 为例）

- **游标批量查询原理**：通过游标（Cursor）按批次读取大表数据（如每次读 10 条），避免一次性加载全表数据导致内存溢出，适合数据量百万级以上的场景。
    
- **原始流程与问题**：
    
    - 原始逻辑：每次读取 10 条，若返回数据量＜10 则退出（认为数据读取完成）；
        
    - 问题：若总数据量为 50 条，需查询 6 次（前 5 次各 10 条，第 6 次返回 0 条，触发退出），多一次无效查询。
        
- **优化方案**：
    
    - **记录当前最大 ID，按 ID 范围批量查询**（推荐）：
        
        1. 首次查询：`SELECT * FROM order WHERE id > 0 LIMIT 10`，记录最后一条数据的 ID（如 10）；
            
        2. 后续查询：`SELECT * FROM order WHERE id > 10 LIMIT 10`，直到返回数据为空；
            
        3. 优势：总查询次数 = ceil (50/10)=5 次，无无效查询；依赖主键索引，查询效率高（避免游标遍历的全表扫描）。
            
    - **优化****游标****退出条件**：
        
        1. 记录已读取的数据总数，当已读取数≥总数据量时退出（总数据量可提前通过 `SELECT COUNT(*) FROM order` 获取，但需注意数据实时变化的问题）。
            
#### 分库分表怎么分？哪些字段不能分？
先看瓶颈是容量还是并发，再决定分库、分表还是都分。
- 分库：解决并发连接过多，单机mysql扛不住的问题
- 分表：为了解决单表数据量太大，导致查询性能下降的问题。
**怎么分？**
- 水平分片：按某个分片键把同结构的数据拆开，比如：
	- 按 `user_id`
	- 按 `order_id`
	- 按 `tenant_id`
	- 按时间
	比如订单表量很大，一般优先考虑按 `user_id` 或者 `buyer_id` 分，因为很多查询天然就是“查某个用户的订单”。
- 垂直拆分：把不同业务或冷热字段拆开。比如用户中心、订单中心、库存中心分不同库；或者订单主表和订单扩展表拆开。
**哪些字段不能乱分**
1. 经常改的字段：分片键一旦修改，数据迁移成本很高
2. 分布不均的字段：比如性别、省份，很容易热点
3. 与查询维度不一致的字段：比如按`user_id` 分，但系统大多数是按 `order_no` 查
4. 范围查询特别多但离散性差的字段（比如性别离散型就很差，只有男女，`user_id`高离散，每个人都不同）因为这类字段无法把查询路由到单个分片
5. 业务上可能为空的字段：空值分片很麻烦
分片键优选：稳定、高基数、分布均匀、查询经常用、尽量不修改的字段
#### 慢SQL

查询速度很慢，解决方案：

- 分析查询语句：explain分析sql执行计划，查看索引使用情况
    
- 创建或优化索引：常用字段增加索引，联合索引符合最左匹配原则[八股](https://mqgxxkfr5d4.feishu.cn/docx/ULV0ddNmDoZJeYxlkrccnDZ3n4R#share-WvixdruwzomOKdxBRvFchhSfnSh)
    
- 避免索引失效：不要使用左模糊匹配、函数计算、表达式计算
    
- 优化查询语句：避免使用select *，使用覆盖索引，联表查询要用小表驱动大表，被驱动的字段要有索引
    
- 分库分表：可以从垂直（纵向）和 水平（横向）两种纬度进行拆分
        
- 分页优化：深分页查询优化，
    
- 缓存机制：使用redis存储热点数据和频繁查询的结果，要考虑缓存一致性问题，
    

#### 执行计划explain

explain是查看sql的执行计划

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OWMzMmViZTMyODg5NDhiNDYxMGIwZDE2ZmVlMTA2MDdfS2RyZ1NpY0VaMkJTN3hQYm9XOHFmaVBpdGd2dUFJdGdfVG9rZW46UlA5TGJNYXZEb1l2Rld4RGhGQWN2bEhwbkxnXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)

参数有：

- type 表示数据扫描类型：all（全表）,index（全索引）,range（索引范围扫描,ref（非唯一索引）,eq_ref（唯一索引）,const（使用了主键或唯一索引与常量比较）,type = NULL，MYSQL不用访问表或者索引就直接能到结果。
    
- possible_keys 字段表示可能用到的索引；
    
- key 字段表示实际用的索引，如果这一项为 NULL，说明没有使用索引；
    
- key_len 表示索引的长度；
    
- rows 表示扫描的数据行数
    

可以使用force index，强制走索引

### 锁

`SELECT ... LOCK IN SHARE MODE`（显式读锁）

不同查询场景的加锁逻辑：（update操作）

（1）主键索引：加行级写锁

（2）唯一索引：加行级写锁

（3）非唯一索引：临键锁（行锁+间隙锁）

（4）未命中索引：表级写锁

（5）范围查询（命中索引）：锁范围区间+间隙

|   |   |
|---|---|
|查询场景|加锁逻辑|
|条件命中主键索引（唯一）|加行级 X 锁（如 UPDATE user SET name = 'a' WHERE id = 1，仅锁定 id=1 的行）。|
|条件命中唯一索引|加行级 X 锁（如 UPDATE user SET name = 'a' WHERE phone = '13800138000'，phone 是唯一索引，仅锁定该 phone 对应的行）。|
|条件命中非唯一索引|加临键锁（行锁 + 间隙锁），避免幻读（如 UPDATE user SET name = 'a' WHERE age = 20，age 是非唯一索引，锁定 age=20 的行及相邻间隙，防止插入 age=20 的新行）。|
|条件无索引（全表扫描）|升级为表级 X 锁（如 UPDATE user SET name = 'a' WHERE name = '张三'，name 无索引，InnoDB 无法定位行，锁定整张表）。|
|范围查询（如 BETWEEN）|加临键锁，锁定范围区间及相邻间隙（如 UPDATE user SET name = 'a' WHERE id BETWEEN 10 AND 20，锁定 id=10~20 的行及 id>20 的间隙，防止插入 id=15 的新行）。|

### 死锁

```SQL
-- 查看最近一次死锁详情（MySQL 8.0+支持）
SHOW ENGINE INNODB STATUS;
```

- `LATEST DETECTED DEADLOCK` 区块：包含死锁发生时间、涉及的事务（`TRANSACTION`）、持有的锁（`HOLDS THE LOCK`）、等待的锁（`WAITING FOR THIS LOCK TO BE GRANTED`）。
    
- 例：事务 1 持有行锁`lock_mode X locks rec but not gap`，等待事务 2 的行锁；事务 2 持有另一行锁，等待事务 1 的行锁，形成循环依赖。
    

根据日志中的 SQL 和锁类型，还原死锁产生的条件：

- 检查事务中 SQL 的执行顺序（如两个事务交叉更新不同行）；
    
- 分析索引使用情况（无索引会导致表锁，增加死锁概率）；
    
- 确认隔离级别（`RR`比`RC`更容易产生间隙锁，可能引发死锁）。
    

### CHAR 与 VARCHAR 的区别

|   |   |   |
|---|---|---|
|维度|CHAR|VARCHAR|
|长度|固定长度（1~255 字节）|可变长度（1~65535 字节）|
|存储方式|不足长度用空格填充|仅存储实际数据，加 1~2 字节记录长度|
|读取效率|快（固定长度，易定位）|稍慢（需计算长度）|
|适用场景|长度固定的数据（如手机号、性别）|长度可变的数据（如用户名、地址）|

- 长度固定且较短 → 用 CHAR（如身份证号、手机号）。
    
- 长度可变或较长 → 用 VARCHAR（如文章内容、用户简介）。
    

#### CHAR 的不可替代的好处

尽管 VARCHAR 更灵活，但 CHAR 在特定场景下更优：

- 查询效率更高：固定长度便于数据库快速定位记录（无需计算偏移量），适合频繁查询的字段。
    
- 节省存储空间：对于极短且固定长度的数据（如性别 `char(1)`），CHAR 比 VARCHAR 更省空间（VARCHAR 需额外存储长度信息）。
    
- 避免空格处理问题：某些数据库（如 MySQL）对 VARCHAR 的尾部空格处理不一致，而 CHAR 的填充空格在查询时会自动截断，更稳定。
    

## Sql注入

### 防止sql注入

1. 使用预处理语句（Prepared Statements）
    
2. 对输入进行过滤和验证
    
3. 避免动态拼接SQL语句
    
4. 最小权限原则
    

## Redis

redis的核心特性（高性能、高可用）

### 为什么要把热点数据存在 redis 上

放入缓存中以减少对数据库的访问效率，从而减少数据库的压力，提高程序的性能。【在内存中存储】

数据库（如 MySQL）基于磁盘存储，即使有索引优化，单次查询耗时通常在 50~200ms；而 Redis 是纯内存数据库，数据读写直接操作内存，单次响应时间可低至 1~5ms，**性能差距达 10~50 倍**。

### Redis为什么快？(高性能)

- 大部分操作都在内存中完成，并且采用了高效的数据结构
    
- redis单线程模型避免了多线程之间的竞争，省去了多线程切换带来的时间和性能上的开销
    
- 采用IO多路复用机制处理大量的客户端请求
    

##### 多路复用机制

一个线程处理多个IO流，内核监听socket上的连接请求或数据请求，一旦有请求到达，就会交给 Redis 线程处理，这就实现了一个 Redis 线程处理多个 IO 流的效果。

### 集群（高可用）

主从复制模式、哨兵模式、Cluster模式

#### 主从模式

**主**节点负责处理**写**操作，同时从节点会同步主节点的数据，客户端可以从**从**节点**读**取数据，实现读写分离。

**有哪些形式？**

一主多从复制架构、多级复制架构、双主(Dual Master)复制架构和多源(Multi-Source)复制架构。

#### 哨兵模式

主从模式下，当 Redis 的主从服务器出现故障宕机时，需要手动进行恢复。

哨兵模式可以监控主从服务器，并且提供主从节点故障转移的功能。

#### 切片集群模式

将数据分布在不同的服务器上，降低对单主节点的依赖。

#### 脑裂

#### redis宕机了怎么处理

1. 用 MQ 削峰填谷
    
2. 本地缓存兜底
    
3. 熔断降级、Redis 主从切换
    
4. Redis 主从切换
    

### 缓存穿透、缓存击穿、缓存雪崩

- 缓存穿透： **key 是不合理的**，**根本不存在于缓存中，也不存在于数据库中**
    
    - **布隆过滤器**：和hash类似，如果key的映射结果存在，则可能存在数据库中，否则一定不存在，可直接拒绝请求
        
    - **数据校验**：根据规则确定key是否合法，可以对key加上校验位，防止随意伪造
        
    - **缓存无效 key**：对于查询到不存在的key，可缓存对应的空值
        
    - 接口限流
        
- 缓存击穿：请求的 key 对应的是 **热点数据，**不存在redis但存在数据库中
    
    - **提前预热（推荐）**
        
    - 永不过期（不推荐）：或者过期时间比较长
        
    - 加锁：保证同一时间只有一个业务线程更新缓存，未能获取互斥锁的请求，要么等待锁释放后重新读取缓存，要么就返回空值或者默认值。
        
- 缓存雪崩：大量的key失效
    
    - 均匀设置过期时间：过期时间加上随机数，避免在同一时间过期大量的数据
        

#### 布隆过滤器

布隆过滤器由“初始值都为0的位图数组”和“N个哈希函数”两部分组成。

- 采用N个哈希函数分别对数据做哈希计算，得到N个哈希值
    
- 将哈希值对数组长度取模
    
- 对应位置值设为1
    

**哈希函数****越多越好吗**

哈希函数越多，发生冲突的概率越小，但存在一个最优值，超过这个值之后会导致整体位数组被打 1 的密度迅速升高，提高误判率

### 数据类型

Redis 五种数据类型的应用场景：

- String 类型的应用场景：缓存对象、常规计数、分布式锁、共享 session 信息等。（数据结构实现主要是 int 和 SDS（简单动态字符串））
    
    - alloc字段：表示分配给该字符数组的总长度
        
    - len字段：表示字符串现有长度
        
- List 类型的应用场景:消息队列(但是有两个问题:1.生产者需要自行实现全局唯一ID;2.不能以消费组形式消费数据)等。
    
- Hash 类型:缓存**对象**、购物车等。**配置项****存储**
    
- Set 类型:聚合计算(并集、交集、差集)场景，比如点赞、共同关注、抽奖活动等。
    
    - 有序集合：元素按分数进行排序，支持范围查询，适用于排行榜或优先级队列。
        
- Zset 类型:排序场景，比如排行榜、电话和姓名排序等。
    

内部实现：

- String 类型的底层的数据结构实现主要是 SDS（简单动态字符串）
    
- List 类型的底层数据结构是由双向链表或**压缩列表**实现的：
    
- Hash类型的底层数据结构是由**压缩列表**或哈希表实现的：
    
- Set 类型的底层数据结构是由哈希表或整数集合实现的：
    
- Zset 类型的底层数据结构是由**压缩列表**或跳表实现的+哈希表（以常数复杂度获取元素权重）：
    

#### zset除了跳表，还有其他的底层存储格式吗

`zset`也有两种不同的实现，分别是`zipList`和`skipList`。

1. 压缩列表ziplist（新的 Redis 版本，将 Hash 对象和 Zset 对象的底层数据结构实现之一的压缩列表，替换成由 listpack 实现）
    

ziplist 编码的 Zset 使用紧挨在一起的压缩列表节点来保存，第一个节点保存 member，第二个保存 score。ziplist 内的集合元素按 score 从小到大排序，其实质是一个双向链表。虽然元素是按 score 有序排序的， 但对 ziplist 的节点指针只能线性地移动，所以在 REDIS_ENCODING_ZIPLIST 编码的 Zset 中， 查找某个给定元素的复杂度为 O(N)。（查找第一个或者最后一个可以通过表头信息直接定位是O(1)）

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OGU0ODYzNGRjYmNkNzNmYzI1N2VmODRiNGQ3NDYxZDdfVlBrZ0k2STY2TWp3TTVPQlFxR0Eyb2ZwOEtoNGVUMThfVG9rZW46VTFJZWI2YU9Pb3Y4dGJ4U3ZiTmN0UjNObnNlXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)

每个节点包含三部分内容：

- _prevlen_，记录了「前一个节点」的长度，目的是为了实现从后向前遍历；
    
- _encoding_，记录了当前节点实际数据的「类型和长度」，类型主要有两种：字符串和整数；
    
- _data_，记录了当前节点的实际数据，类型和长度都由 `encoding` 决定；
    

如果新插入的元素较大，可能会导致后续元素的prevlen 占用空间都发生变化，从而引起连锁更新问题。

为什么会存在连锁更新？因为prevlen的存储空间**不是固定的**，而是根据前一个节点的实际长度动态变化：

- 当前一个节点长度 ≤ 254 字节 时，`prevlen`用 1 字节 存储；
    
- 当前一个节点长度 ＞254 字节 时，`prevlen`用 5 字节 存储（1 字节标记位 + 4 字节长度值）。
    

**Listpack**不保存_prevlen，通过len_只记录当前节点（encoding+data）的长度，避免连锁更新问题

2. 跳表的查询复杂度O(logN)，多层的有序链表，通过**多级索引**实现高效的查找、插入和删除操作
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTQyZTkyNTUwMDkwZGFhYjFlZTY5N2IyMzQ1MjViZThfZ1JXcUZadVppaXRYMTR2Nm9Rck9mazU5cHdXa3VBNUVfVG9rZW46VG1SN2JaNFhyb2ZwaFV4OGp0WWNnQndrbkRmXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)

  

##### 为什么要用跳表而不是红黑树来实现

- 从内存占用上来比较，跳表比平衡树更灵活一些。
    
- 在做范围查找的时候，跳表比平衡树操作要简单。zset的一个主要使用场景是范围查询，跳表底层是一个双向链表可以按顺序访问得到范围查询的救国，红黑数虽然可以通过中序遍历得到有序的结果，但是其操作路径不连续，需要维护栈
    
- 从算法实现难度上来比较，跳表比平衡树要简单得多。对于插入/删除操作：红黑树需要通过旋转来维持平衡，开销较大
    

### 持久化

三种数据持久化的方式：

- AOF 日志：每执行一条写**操作命令**，就把该命令以追加的方式写入到一个文件里；
    
- RDB 快照：将某一时刻的内存数据，以**二进制**的方式写入磁盘；
    
- 混合持久化方式：Redis 4.0 新增的方式，集成了 AOF 和 RDB 的优点；
    

1. AOF实现：先执行命令，再把数据写入日志
    

好处：避免额外的检查开销（可能有语法问题）；不会阻塞当前写操作命令的执行

坏处：数据可能会丢失；可能阻塞其他操作

其实这两个风险都有一个共性，都跟「 AOF 日志写回硬盘的时机」有关

- **AOF** **写回策略有几种**？三种
    
      
    
    ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjU1YzIzMjZiMzY1MmJkNGJkZWY0OGQxZTNlMzcwOTNfWjZBNU4wMjJtZHRNSnhCTXhiaFdXOVFHUEY1Zk5ZbmJfVG9rZW46VENJdWJYajlXb29DYld4V3VJR2Nla0djbktIXzE3NzM2NDk3NTg6MTc3MzY1MzM1OF9WNA)
    
    - 1.将命令追加到 `server.aof_buf` 缓冲区
        
    - 2.通过 write() 系统调用，将 aof_buf 缓冲区的数据写入到 AOF 文件
        
    - 3.系统决定什么时候写入硬盘（根据写入的时机可分为三种：always（立即），every（每隔一秒），no（操作系统决定））
        
    - 但是都无法能完美解决「**主进程阻塞**」和「**减少数据丢失**」的问题
        
- **AOF****重写**
    
    - 一种压缩方法，假设前后执行了「_set name xiaolin_」和「_set name xiaolincoding_」这两个命令的话，会记录两条日志，如果采用重写只记录最新的数据
        
    - 重写时会先写入新的aof文件，完成后覆盖旧的日志
        
    - **后台重写**：由后台子进程 _bgrewriteaof_来完成的

2. RDB快照：恢复数据的效率比AOF更好，因为不需要一条一条执行操作
    

生成快照的两种方式：save（会阻塞主线程）和bgsave（创建一个子进程来生成快照，避免阻塞）

全量快照，频繁执行影响性能

在bgsave时如果主线程需要修改数据，产生的修改只能交给下次bgsave

3. 混合： AOF 重写日志时，子进程全量写rdb进入aof文件，修改以aof方式写入aof
    
4. 大key对redis持久化的影响：
    

aof的always策略下，如果遇到大key， 执行fsync() 的阻塞时间较长

影响页表大小，或出需要写时复制会影响复制的速度（阻塞父进程）

### 分布式锁

Redis分布式锁无法解决主从不一致的问题（Redisson的红锁实现比较复杂，会降低效率，如果非要保证强一致性可以考虑采用zookeeper实现分布式锁）Redis本来就是AP的，保证高可用

[blog.csdn.net](https://blog.csdn.net/asd051377305/article/details/108384490)

用于分布式场景下并发控制的一种机制，用于控制某个资源在同一时刻只能被一个应用所使用

SET lock_key lock_value nx px 1000

- Nx:key不存在才插入
    
- Px：设置过期时间
    
- value：区分来自不同客户端的加锁操作
    

删除：lua脚本，先判断value是否一致再进行删除，保证原子性

```Lua
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

redis原生实现的三大致命问题：

- 未提供自动续期机制（业务执行时间超过锁的过期时间（如锁设置为30秒，业务执行了40秒），锁自动释放后，其他线程获取锁。此时原线程完成业务后，会删除新线程的锁，导致互斥失效。**误删锁**）
    
- 不可重入导致的死锁（如递归调用，由于未记录“持有锁的线程ID”，无法判断是否是同一线程重复获取。）
    
- 单点故障导致锁失效（没有同步到从节点）
    

#### 除Redis外还能用什么实现分布式锁

1. redisson：
    

Redisson 基于Redis的Java客户端，封装了**RedLock**算法，提供了很多开箱即用的功能，比如多种分布式锁的实现、延时队列。

传统的单节点Redis锁（如`SET key value NX PX`）存在**单点故障**风险：若Redis主节点宕机且未同步到从节点，锁可能被重复获取。

RedLock算法：向N个独立Redis节点依次申请锁，若在多数节点（N/2+1）成功获取锁，则认为加锁成功。（过半）

Redisson基于RedLock算法，提供了以下增强功能：

- 可重入锁：同一线程可多次获取同一锁（通过lockCount计数器实现）；
    
- 公平锁：按请求顺序分配锁（通过Semaphore队列实现）；
    
- 锁续期：若业务执行时间超过锁过期时间，自动延长锁的有效期（“看门狗”机制）；
    
- 异步锁：支持lockAsync()/unlockAsync()异步操作，避免阻塞线程。
    

2. zookeeper：利用临时顺序节点来实现分布式锁的获取和释放。
    
3. 基于mysql：通过for update排他锁，对特定数据记录加排他锁。
    

##### Redisson原理

[弄懂Redis的儿子Redisson，只需这个15问题-阿里云开发者社区](https://developer.aliyun.com/article/1365378)
##### watchdog
Watchdog 的触发条件是：线程成功拿到锁，并且**没有显式传 leaseTime**（如果制定了锁的租期时间，会导致看门狗失效）。此时框架会给这把锁注册一个后台续期任务，默认 watchdog 超时是 30 秒，看门狗线程会在后台每隔十秒检查一次，如果业务逻辑还没执行完，就把锁的 TTL 延期到30s。调用`unlock()`后会取消续期，并通过 Pub/Sub 通知等待队列里的线程重新竞争锁。  
如果我自己实现，我会用 Lua 保证加锁、重入、解锁原子性；用 ownerId 防止误解锁和误续期；用调度线程池维护续期任务；等待线程不自旋，而是挂在本地阻塞队列并结合 Redis Pub/Sub 唤醒。这样既能保证长任务不提前过期，也能在进程宕机后靠 TTL 自动释放。
### redis中删除一个key后整个过程是怎么样的？是否 “删除完立马在内存里删除”

- DEL 命令：是，同步删除会立即释放内存（但大键可能阻塞主线程）。
    
- UNLINK 异步模式 / FLUSH ASYNC：否，内存释放由后台线程异步处理，有延迟。
    
- 过期 key 的惰性 / 定期删除：否，删除时机不确定，可能延迟释放内存。
    

### 数据过期

设置过期时间之后Redis 会把该 key 带上过期时间存储到一个过期字典

常见的过期删除策略：定时删除、惰性删除、定期删除

- 定时删除：在设置key的过期时间时，同时创建一个定时事件，自动执行删除操作
    
    - 优点：删除及时
        
    - 缺点：如果过期key比较多，占用cpu时间
        
- 惰性删除：每次访问key时检测key是否过期
    
    - 优点：对cpu友好
        
    - 缺点：对内存不友好
        
- 定期删除：每隔一段时间随机选取一定数量的key检查是否过期
    
    - 前两者居中，缺点难以确定操作执行的时长和频率
        

Redis的过期删除策略：惰性删除+定期删除（默认每秒十次从过期字典中抽取20个key检查是否过期，如果过期的key数量超过25%继续抽查）

### 内存淘汰

当 Redis 的运行内存已经超过 Redis 设置的最大内存之后，则会使用内存淘汰策略删除符合条件的 key

大体分为**不进行数据淘汰**和**进行数据淘汰**两类策略

1. 不进行数据淘汰的策略
    

- noeviction：超过最大设置内存之后，报错通知禁止写入（如果没有新数据写入就不影响）
    

2. 进行数据淘汰的策略
    

- volatile-**random**：**随机**淘汰设置了过期时间的任意键值
    
- volatile-**ttl**：优先淘汰更早过期的键值(**先进先出**)
    
- volatile-**lru**：过期的键值中**最久未使用**的键值
    
- volatile-**lfu**：过期的键值中**最少使用**的键值
    

在所有数据范围内进行淘汰：

- allkeys-**random**：随机淘汰任意键值
    
- allkeys-**lru**：最久未使用
    
- allkeys-**lfu**：最少使用
    

### 项目中对Redis的使用

1. String 类型（用于 List 缓存的 Key）：
    

使用 StringRedisTemplate 将缓存键（cacheKey）存储为字符串类型。

该键用于标识一次 Excel 校验过程中生成的缓存数据。

2. List 类型（实际存储数据）：
    

使用 stringRedisTemplate.opsForList().rightPush() 方法将 MesItemCategoryImportResultDto 对象以 JSON 字符串的形式推入 Redis 的 List 中。

这种结构用于临时存储 Excel 校验结果，便于后续确认导入时读取和处理。

3. 过期时间设置：
    

通过 stringRedisTemplate.expire() 设置缓存键的过期时间为 15 分钟（由 CacheTimeConstant.EXPIRED_MIN15 定义），确保 Redis 中的数据不会长期驻留。

4. 通过 `RedisAtomicInteger`（Redis 的原子整数）实现计数器自增，同时用分布式锁避免多线程 / 多服务实例并发操作时的计数冲突，最终返回一个递增的整数（可作为订单编号的一部分）。
    

### redis 和本地缓存是怎么设计的，如果 redis 挂掉怎么办

1. 应用先查本地缓存，命中则直接返回；
    
2. 本地缓存未命中，再查Redis，命中则返回并同步到本地缓存；
    
3. 两者均未命中，查询数据库，将结果写入 Redis 和本地缓存后返回。
    

如果redis挂掉：

- 熔断隔离：使用熔断器（如 Sentinel、Resilience4j），当 Redis 连接失败率超过阈值时，自动熔断对 Redis 的调用，避免请求长时间阻塞。
    
- 临时依赖本地缓存：熔断后，仅使用本地缓存提供服务（虽可能有数据不一致，但保证部分可用性）。
    
- 主从切换：若 Redis 采用主从架构，主节点挂掉后，哨兵（Sentinel）自动将从节点晋升为主节点，客户端通过哨兵发现新主节点并连接。
    

## 数据库与缓存的一致性问题

1. 双写策略
    
    1. 先写DB，再删除缓存（推荐）
        
    2. 写操作先删缓存，再更新DB
        
    3. 写DB，然后发消息到MQ异步更新redis
        
    4. 分布式事务，用事务保证 “库更新” 和 “缓存操作” 原子性（实现强一致性，开销大，不建议）
        
2. 怎么保证数据一致性，缓存数据库其中一个宕机了怎么办
    

Redis:降级：接口限流；恢复流程：修复缓存集群（如重启节点、切换从库为主库；预防：缓存集群部署（主从 + 哨兵 / Redis Cluster），避免单点故障

MySql：降级：写请求返回 “服务临时不可用”，或者读请求切换到「从库」；恢复流程：若主库不可修复：将从库提升为主库，更新应用的数据库连接地址，恢复写请求；预防：数据库主从复制（如 MySQL 半同步复制），避免主库数据丢失

3. 先改数据库然后删掉缓存怎么防止缓存穿透
    
    1. 缓存预热，更新后主动写缓存
        
    2. 互斥锁（缓存缺失时单线程查库重建）
        
    3. 限流降级（数据库压力大时拦截请求）
        

## ElasticSearch

Elasticsearch擅长海量数据的搜索、分析和计算。

Elasticsearch是面向文档存储的，可以是数据库中的一条商品数据，一个订单信息。文档数据会被序列化为json格式后存储在Elasticsearch中。

## MyBatis

传统 JDBC 开发需要手动处理加载驱动、创建连接、编写 SQL、设置参数、处理结果集、释放资源等步骤，且代码冗余、硬编码严重（如 SQL 语句写在 Java 代码中）。

MyBatis 的核心作用是将这些重复工作抽象为配置和 API。

1. **消除重复代码**（如连接管理、资源释放）；
    
2. 实现 **SQL** **与 Java 代码**分离（通过 XML 或注解配置）；
    
3. 自动完成参数设置和结果集映射（ORM 的简化实现）；
    
4. 提供**动态** **SQL** 功能（根据条件拼接 SQL，避免字符串拼接的麻烦）。
    

### MyBatis 一级缓存和二级缓存

当我们使用Mybatis进行数据库的操作时候，会创建一个SqlSession来进行一次数据库的会话，会话结束则关闭SqlSession对象。

Mybatis对缓存提供支持，一级缓存是**默认**使用的，二级缓存需要**手动开启**（`@CacheNamespace`注解）。

- 一级缓存的作用域是一个sqlsession（会话）内；
    
- 二级缓存作用域是Mapper 级缓存（范围更大）；
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MmEwZTNjMTZkMGRhYjUwYTMyMmMxMzYxMDU2NTVkZWRfQjQ1WTRSYnJlMjZzbVFzUWJpa3JDaWhvZzJSWUdjVW5fVG9rZW46V3Zqc2JIRVI0b3lkc3N4M0xiY2NoZ2gwbkllXzE3NzM2NDk4MjE6MTc3MzY1MzQyMV9WNA)

mybatis一级缓存的范围有**SESSION**和**STATEMENT**两种，默认是SESSION。

如果不想使用一级缓存，可以把一级缓存的范围指定为STATEMENT，**这样每次执行完一个Mapper中的语句后都会将一级缓存清除。**

**区别**：

1）一级缓存 Mybatis的一级缓存是指SQLSession，一级缓存的作用域是SQlSession, Myabits默认开启一级缓存。

在同一个SqlSession中，执行相同的SQL查询时；第一次会去查询数据库，并写在缓存（ “自身一级缓存” 和 “Mapper 的二级缓存”）中，第二次会直接从缓存中取。 当执行SQL时候两次查询中间发生了增删改的操作，则SQLSession的缓存会被清空。

每次查询会先去缓存中找，如果找不到，再去数据库查询，然后把结果写到缓存中。 Mybatis的内部缓存使用一个HashMap，key为hashcode+statementId+sql语句。Value为查询出来的结果集映射成的java对象。

SqlSession执行insert、update、delete、commit、close等操作后会清空该SQLSession缓存。

2） Mybatis二级缓存是默认不开启的，作用于一个Application，是Mapper级别的，多个SqlSession使用同一个Mapper的sql能够使用二级缓存。

二级缓存在增删改操作提交后会清空。

### 性能优化

1. SQL优化
    
    1. 只查询必要的字段
        
    2. 避免 `SELECT *` 与强制索引
        
    3. 分页优化，避免offset深分页
        
2. 批量操作
    
3. 开启二级缓存
    
4. 结果集映射优化：禁用自动映射
    

### PageHelper分页的原理是什么?

https://www.yuque.com/hollis666/gg1x9v/ogng83zwfrqblu5e

主要是用来做物理分页

当我们在代码中使用 `PageHelper.startPage(int pageNum, int pageSize)`设置分页参数之后，其实PageHelper会把他们存储到ThreadLocal中。在query方法执行之前（userMapper.findByKeyword），从ThreadLocal中再获取分页参数信息，页码和页大小，然后执行分页算法，计算需要返回的数据块的起始位置和大小。最后，PageHelper会通过修改SQL语句的方式，在SQL后面动态拼接limit语句，限定查询的数据范围，从而实现物理分页的效果。并在查询结束后再清除ThreadLocal中的分页参数。

```Java
public PageInfo<User> searchUsers(int pageNum, int pageSize, String keyword) {
    // 在当前处理这个请求的线程上下文中，贴了一张“我要分页”的便签。
    PageHelper.startPage(pageNum, pageSize);
    // 自定义查询方法
    List<User> users = userMapper.findByKeyword(keyword);
    // 传进来的 users 实际上是一个携带了总记录数等隐藏信息的 Page 对象，
    // PageInfo 的构造函数会提取 Page 对象里的总条数、当前页码等数据，
    // 组装成一个包含 total、pages、list 等丰富字段的分页对象，
    // 直接返回给前端。
    return new PageInfo<>(users);
}
// PageHelper.startPage 必须紧挨着 MyBatis 查询方法，
// 中间绝对不允许有任何可能跳过查询的代码逻辑。否则可能造成线程污染
```

### mybatis中＃和$的区别？

[MyBatis 中 #{} 和 ${} 的区别详解-CSDN博客](https://blog.csdn.net/isolusion/article/details/146440960)

1. `#{}`:用于动态替换 SQL 语句中的参数。会自动为String类型的参数加上‘’单引号
    

```XML

<select id="getUser" resultType="User">
    SELECT * FROM users WHERE username = #{username} AND password = #{password}
</select>
```

MyBatis 会将上述 SQL 转换为：

```XML
SELECT * FROM users WHERE username = ? AND password = ?
```

2. `${}`:用于直接替换 SQL 语句中的字符串。`${}` 主要用于 **动态表名、列名、排序字段等非参数值** 的场景，
    

|   |   |   |
|---|---|---|
|特性|#{}|${}|
|处理方式|使用 PreparedStatement 预编译|直接拼接字符串|
|安全性|防止 SQL 注入|存在 SQL 注入风险|
|参数转义|自动转义特殊字符|不转义特殊字符|
|适用场景|动态参数值|动态表名、列名等非参数值场景|

## 如果设计一个高可用的缓存架构，需要考虑什么

一个高可用性的分布式缓存系统应该具备以下特征：

- 故障容忍性：当某个节点发生故障时，系统能够继续正常工作，不影响整体性能。
    
- 数据复制：数据应该在多个节点之间进行复制，以提供冗余和容错能力。
    
- 数据一致性：复制的数据应该保持一致性，即所有节点上的数据应该保持同样的状态。
    
- 数据分片：数据应该被分成多个片段，存储在不同的节点上，以提高性能和可扩展性。
    

## 除 Redis 外的其他缓存方式

（1）本地缓存（进程内缓存）：缓存数据存储在应用进程内存中，Java：Caffeine（基于 LRU 算法，支持过期时间、异步加载，性能优于 Guava Cache）、Guava Cache；

（2）数据库缓存

（3）CDN 缓存（内容分发网络）：将静态资源（图片、JS、CSS、视频）缓存到全球各地的 CDN 节点，用户就近访问；

（4）应用层框架缓存：集成在 Web 框架中，缓存 HTTP 响应或业务数据；

# 操作系统

## 用户态和内核态的区别

- 用户态：用户态运行的进程可以直接读取用户程序的数据，拥有较低的权限。当应用程序需要执行某些需要特殊权限的操作，例如读写磁盘、网络通信等，就需要向操作系统发起系统调用请求，进入内核态。
    
- 内核态：内核态运行的进程几乎可以访问计算机的任何资源包括系统的内存空间、设备、驱动程序等，不受限制，拥有非常高的权限。当操作系统接收到进程的系统调用请求时，就会从用户态切换到内核态，执行相应的系统调用，并将结果返回给进程，最后再从内核态切换回用户态。
    

由于进入内核态需要付出较高的开销（需要进行一系列的上下文切换和权限检查），应该尽量减少进入内核态的次数

**用户态和内核态切换的三种方式：**

系统调用（Trap）、中断（Interrupt）、异常（Exception）

## 分页分段

### 内存分段

程序是由若干个逻辑分段组成的，如可由代码分段、数据分段、栈段、堆段组成。不同的段是有不同的属性的，所以就用分段（_Segmentation_）的形式把这些段分离出来。

好处：能产生连续的内存空间

不足之处：

1. 内存碎片问题（内部内存碎片/外部内存碎片）
    
2. 内存交换的效率低的问题（内存交换是解决外部内存碎片的方法）
    

内存交换：将数据写到硬盘中，然后从硬盘上读回来到内存里。不过再读回的时候需要将数据紧跟现有的数据，减少碎片

为了解决内存分段的“外部内存碎片和内存交换效率低”的问题，出现了**内存分页**

### 内存分页

分页是把指望个虚拟和物理内存空间切成一段段**固定尺寸**的大小。

虚拟地址和物理地址直接通过页表来映射。页表是存储在内存里的，内存管理单元做地址映射工作。

不会有外部碎片（页与页之间是紧密排列的）但会有内部碎片，最小的分配单位是一页。

#### 多级页表

减少页表要占用的内存

如果使用了二级分页，一级页表就可以覆盖整个 4GB 虚拟地址空间，但如果某个一级页表的页表项没有被用到，也就不需要创建这个页表项对应的二级页表了，即可以在需要时才创建二级页表。

#### TLB

专门存放程序最常访问的页表项的 Cache，这个 Cache 就是 TLB（_Translation Lookaside Buffer_），快表

### 段页式内存管理

内存分段和内存分页并不是对立的，可以先分段再分页，实现段页式内存管理

## read和write过程
- `read`：用户态发起系统调用，CPU从用户态切换到内核态，内核先查看页缓存Page Cache里有没有对应数据，如果已经在页缓存里，直接把数据从内核页缓存拷贝到用户缓冲区buf，如果缓存未命中，内核就要发起磁盘IO，当前进程通常会阻塞，然后再拷贝到用户缓冲区，最后CPU从内核态回到用户态。
- `write`：用户态切到内核态，把数据从用户缓冲区拷贝到内核也缓存，然后把相应页标记为脏页dirty page，异步刷盘，返回用户态。


## 零拷贝

主要实现方式有两种：

- mmap+write
    
- sendfile
    

**mmap****+write**

`mmap()` （memory map内存映射）系统调用函数会直接把内核缓冲区里的数据「**映射**」到用户空间，这样，操作系统内核与用户空间就不需要再进行任何的数据拷贝操作。

具体过程如下：

- 应用进程调用了 `mmap()` 后，DMA 会把磁盘的数据拷贝到内核的缓冲区里。接着，应用进程跟操作系统内核「共享」这个缓冲区；
    
- 应用进程再调用 `write()`，操作系统直接将内核缓冲区的数据拷贝到 socket 缓冲区中，这一切都发生在内核态，由 CPU 来搬运数据；
    
- 最后，把内核的 socket 缓冲区里的数据，拷贝到网卡的缓冲区里，这个过程是由 DMA 搬运的。
    

通过使用 `mmap()` 来代替 `read()`， 可以减少一次数据拷贝的过程。

但这还不是最理想的零拷贝，因为仍然需要通过 CPU 把内核缓冲区的数据拷贝到 socket 缓冲区里，而且仍然需要 4 次上下文切换，因为系统调用还是 2 次。
![[Pasted image 20260326233830.png]]

**sendfile**

具体过程如下：

- 第一步，通过 DMA 将磁盘上的数据拷贝到内核缓冲区里；
    
- 第二步，缓冲区描述符和数据长度传到 socket 缓冲区，这样网卡的 SG-DMA 控制器就可以直接将内核缓存中的数据拷贝到网卡的缓冲区里，此过程不需要将数据从操作系统内核缓冲区拷贝到 socket 缓冲区中，这样就减少了一次数据拷贝；
![[Pasted image 20260326234106.png]]


## 线程进程

- **进程（Process）：** 是指计算机中正在运行的一个程序实例。
    
- **线程（Thread）：**也被称为轻量级进程，更加轻量。多个线程可以在同一个进程中同时执行，并且共享进程的资源比如内存空间、文件句柄、网络连接等。
    

多个线程共享进程的堆和方法区 (JDK1.8 之后的元空间)资源，但是每个线程有自己的程序计数器、虚拟机栈 和 本地方法栈。

### 进程之间怎么保护它的内存不被其他进程所占用

页表+MMU（Memory Management Unit，内存管理单元）

### 进程通信方式

1. **管道/匿名管道(Pipes)** ：用于具有亲缘关系的父子进程间或者兄弟进程之间的通信。
    
2. **有名管道(Named Pipes)** : 匿名管道由于没有名字，只能用于亲缘关系的进程间通信。为了克服这个缺点，提出了有名管道。有名管道严格遵循 **先进先出(First In First Out)** 。有名管道以磁盘文件的方式存在，可以实现本机任意两个进程通信。
    
3. **信号(Signal)** ：信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生；
    
4. **消息队列(Message Queuing)** ：消息队列是消息的链表,具有特定的格式,存放在内存中并由消息队列标识符标识。管道和消息队列的通信数据都是先进先出的原则。与管道（无名管道：只存在于内存中的文件；命名管道：存在于实际的磁盘介质或者文件系统）不同的是消息队列存放在内核中，只有在内核重启(即，操作系统重启)或者显式地删除一个消息队列时，该消息队列才会被真正的删除。消息队列可以实现消息的随机查询,消息不一定要以先进先出的次序读取,也可以按消息的类型读取.比 FIFO 更有优势。消息队列克服了信号承载信息量少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。
    
5. **信号量(Semaphores)** ：信号量是一个计数器，用于多进程对共享数据的访问，信号量的意图在于进程间同步。这种通信方式主要用于解决与同步相关的问题并避免竞争条件。
    
6. **共享内存(Shared memory)** ：使得多个进程可以访问同一块内存空间，不同进程可以及时看到对方进程中对共享内存中数据的更新。这种方式需要依靠某种同步操作，如互斥锁和信号量等。可以说这是最有用的进程间通信方式。
    
7. **套接字(Sockets)** : 此方法主要用于在客户端和服务器之间通过网络进行通信。套接字是支持 TCP/IP 的网络通信的基本操作单元，可以看做是不同主机之间的进程进行双向通信的端点，简单的说就是通信的两方的一种约定，用套接字中的相关函数来完成通信过程。
    

### 线程间的同步的方式有哪些？

1. **互斥锁(Mutex)** ：采用互斥对象机制，只有拥有互斥对象的线程才有访问公共资源的权限。因为互斥对象只有一个，所以可以保证公共资源不会被多个线程同时访问。比如 Java 中的 `synchronized` 关键词和各种 `Lock` 都是这种机制。
    
2. **读写锁（Read-Write Lock）**：允许多个线程同时读取共享资源，但只有一个线程可以对共享资源进行写操作。
    
3. **信号量(Semaphore)** ：它允许同一时刻多个线程访问同一资源，但是需要控制同一时刻访问此资源的最大线程数量。
    
4. **屏障（Barrier）**：屏障是一种同步原语，用于等待多个线程到达某个点再一起继续执行。当一个线程到达屏障时，它会停止执行并等待其他线程到达屏障，直到所有线程都到达屏障后，它们才会一起继续执行。比如 Java 中的 `CyclicBarrier` 是这种机制。
    
5. **事件(Event) :**Wait/Notify：通过通知操作的方式来保持多线程同步，还可以方便的实现多线程优先级的比较操作。
    

  

  

## 浅拷贝和深拷贝区别

- 浅拷贝：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
    
- 深拷贝：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。
    
- 引用拷贝：两个不同的引用指向同一个对象。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTRkMjlmYjQ5ZTMyZGU2YzQ5NjFhYjJmOWZlZTFkZmZfaloxTmdNU05BOUM3R1lydzlNQnRxQXdRRkRuVXRYR0dfVG9rZW46TjloMWJTSmdHb3plUmF4VFI3dWNhMm5kbnlkXzE3NzM2NDk4NTg6MTc3MzY1MzQ1OF9WNA)

## 置换算法的lru和lfu知道吗？这两个置换算法的缺点是什么？

- **最近最久未使用页面置换算法（****LRU** **，Least Recently Used）**：最近**最久未使用**的页面予以淘汰。LRU 算法是根据各页之前的访问情况来实现，因此是易于实现的。OPT 算法是根据各页未来的访问情况来实现，因此是不可实现的。
    
- **最少使用页面置换算法（****LFU****，****Least Frequently Used****）** : 和 LRU 算法比较像，不过该置换算法选择的是之前一段时间内**使用最少**的页面作为淘汰页。
    
- **最佳页面置换算法（****OPT****，Optimal）**：优先选择淘汰的页面是以后永不使用的，或者是在最长时间内不再被访问的页面，这样可以保证获得最低的缺页率。
    
- **先进先出页面置换算法（****FIFO****，First In First Out）**：性能并不是很好。
    

缺点：

LRU：偶发性的、周期性的批量查询操作（包含冷数据）会淘汰掉大量的热点数据，导致 LRU 命中率急剧下降

LFU：最近加入的数据总是易于被剔除，早期的热点数据一直占据缓存

# 计网

## OSI七层

- 应用层
    
- 表示层
    
- 会话层
    
- 传输层
    
- 网络层
    
- 数据链路层
    
- 物理层
    

## 浏览器输入URL后发生的全流程

1. URL 解析：浏览器解析网址，分离协议（如 HTTP/HTTPS）、域名、端口、路径等。
    
2. 浏览器通过 **DNS** 协议，获取域名对应的 IP 地址：
    
    1. 检查浏览器缓存 → 操作系统缓存 → 本地 DNS 服务器 → 根域名服务器 → 顶级域名服务器 → 权威域名服务器。
        
3. 根据IP和端口号，向目标服务器发起一个**TCP**连接请求
    
4. 在 TCP 连接上，向服务器发送一个 **HTTP** 请求报文，请求获取网页的内容
    
5. 服务器收到 HTTP 请求报文后，处理请求，并**返回 HTTP 响应**报文给浏览器
    
6. 浏览器收到 HTTP 响应报文后，解析响应体中的 HTML 代码，渲染网页的结构和样式
    
7. 关闭TCP连接
    

### 浏览器如何知道用户按了回车

事件监听机制：浏览器的渲染引擎会监听键盘事件（`keydown`、`keyup` 等）。

### 应用程序如何知道客户端发送了请求

1. 网络层监听：应用程序通常通过 Socket 绑定端口并监听（如 Web 服务器监听 80/443 端口）。
    
2. 操作系统转发：客户端请求到达服务器网卡后，操作系统内核通过 TCP/IP 协议栈解析，根据端口号将数据转发到对应应用程序的 Socket。
    
3. I/O 事件通知：应用程序通过 I/O 多路复用机制（如 epoll、select）监听 Socket 上的可读事件，当数据到达时触发通知。
    

## 应用层

### HTTP
#### GET与POST的区别
**GET 的语义是从服务器获取指定的资源，POST 的语义是根据请求负荷（报文body）对指定的资源做出处理**，GET方法是安全且幂等的
#### 状态码

- 200：请求成功；
    
- 301 请求的网页已永久移动到新位置。服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。（永久重定向）
    
- 302：临时重定向；
    
- 304 自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。
    
- 404：无法找到此页面；405：请求的方法类型不支持；
    
- 500：服务器内部出错
    

#### http1,http1.1,http2

对比 HTTP 1.0 和 HTTP 1.1：

- 响应状态码：**HTTP 1.1新增了 24 种错误响应状态码**，比如说，`100 (Continue)`——在请求大资源前的预热请求，`206 (Partial Content)`——范围请求的标识码，`409 (Conflict)`——请求与当前资源的规定冲突，`410 (Gone)`——资源已被永久转移，而且没有任何已知的转发地址。
    
- 缓存处理：在 HTTP1.0 中主要使用 header 里的 If-Modified-Since,Expires 来做为缓存判断的标准，HTTP1.1 则引入了更多的缓存控制策略例如 Entity tag，If-Unmodified-Since, If-Match, If-None-Match 等更多可供选择的缓存头来控制缓存策略。
    
- 连接方式：HTTP 1.0 为短连接，HTTP 1.1 支持长连接，**默认启用Keep-Alive**（连接复用）。HTTP 协议的长连接和短连接，实质上是 TCP 协议的长连接和短连接。
    
- Host 头处理：HTTP/1.1 在请求头中加入了`Host`字段。
    
- 带宽优化：HTTP/1.1 引入了范围请求（range request）机制，以避免带宽的浪费。
    

对比 HTTP 1.1 和 HTTP 2.0：

- **头部压缩**：HTTP 2.0会压缩头（Header），如果同时发出多个请求，他们的头是一样的或是相似的，那么，协议会帮你消除重复的部分。只发送索引号
    
- **二进制格式**：HTTP 2.0会将所有传输的信息分割为更小的消息和帧，并采用二进制格式编码
    
- **并发传输**：引入了stream概念，多个stream复用在一条TCP连接，**解决了HTTP/1.1 队头阻塞的问题**
    
- **服务器主动推送资源**：服务端不再是被动地响应，可以主动向客户端发送消息

HTTP/3
- 使用**QUIC**协议(基于UDP)替代 TCP。QUIC不仅仅承担了传输层协议的职责，还具备了TLS的安全性相关能力
- **消除了TCP层的队头阻塞**。
- 支持**0-RTT建连**，极大降低连接延迟。
- 更安全：**强制加密传输**。
- 更适应现代网络(如移动网络、弱网)
    

#### http和https的区别

**端口号**：http的端口号是80，https的端口号是443

url前缀不同：http:// https://

**安全性**和资源消耗：http协议运行在tcp之上，https是运行在ssl/tls之上的http协议，ssl/tls运行在tcp之上。http传输的内容都是**明文**，且无法进行**身份验证**。https传输的内容经过了加密，采用对称加密，对称加密的密钥用服务器方的证书进行了非对称加密。

**SEO**(搜索引擎优化)：搜索引擎通常会更青睐使用 HTTPS 协议的网站

#### https原理

HTTPS 的核心是 **混合加密**，结合了对称加密和非对称加密的优点。

#### HTTPS握手

（tcp先握手tls再握手，或者同时发生）

即SSL / TLS 握手

TLS握手是启动HTTPS通信的过程，类似于TCP建立连接时的三次握手。TLS握手过程中通信双方交换消息以进行**身份认证**，并确立他们要使用的加密算法以及**会话密钥**。

- 商定双方通信所使用的TLS版本；
    
- 确定双方所要使用的密码组合（随机数、密码信息等）；
    
- 客户端通过公钥和数字证书上的数字前面验证服务端的身份；
    
- 生成会话密钥
    

### DNS

#### dns两种查询方式

迭代法、递归法

层级：根 DNS 服务器、顶级域 DNS 服务器（TLD 服务器）、权威 DNS 服务器、本地 DNS 服务器

1. 迭代：请求主机到本地DNS服务器的查询是递归的，其余的查询是迭代的。
    
2. 递归：全部递归
    

## 传输层

### UDP 和 TCP 有什么区别呢？分别的应用场景是？

TCP**面向连接的**、**可靠的**、**基于****字节流**的传输层通信协议、一对一的两点服务

UDP面向无连接的通信服务，协议简单，头部只有8字节（源端口、目的端口、包长度、校验和）

1. 可靠性：TCP有拥塞控制机制、流量控制、重传机制以及确认机制，确保消息的可达性，但是UDP不保证消息一定能够送达
    
2. 数据传输形式：TCP是以流的方式传输数据，UDP是报文
    
3. 首部长度：TCP的首部包括窗口大小、序列号、校验位等信息，总共占用20字节，UDP8字节
    
4. 是否有连接：TCP有三次握手和四次挥手，需要先建立链接
    
5. 效率：TCP的效率更低
    

应用场景：

- TCP：ftp文件传输，http/https
    
- UDP：包总量较少的通信，如 `DNS` 、`SNMP` 等；视频、音频等多媒体通信；广播通信
    

### QUIC协议/UDP怎么保证可靠性

可靠的UDP协议...

- 连接迁移：网络变化时快速迁移网络
    
- 重传机制
    
- 前向纠错：使用前向纠错技术，在接收端修复部分丢失的数据，降低重传的需求
    
- 拥塞控制：根据网络状况动态调整传输速率
    

### 三次握手

1. 首先由发起方一般是客户端，发送SYN包，并附带序列号Seq=x
    
2. 服务器收到之后发送SYN+ACK，并附带序列号Seq=y，ack=x+1，表示收到了x及之前消息
    
3. 客户端发送ACK，ack=y+1，此时可以附带消息。
    

#### 为什么是三次握手？不是两次、四次？

- 三次握手才可以阻止重复历史连接的初始化（主要原因）（可能第一个syn包发送过程中遇到网络阻塞，然后发送了一个新的syn包，但是旧的包也到达了服务器会建立连接）
    
- 三次握手才可以同步双方的初始序列号
    
- 三次握手才可以避免资源浪费
    

#### 初始序列号ISN是如何随机产生的？

起始 `ISN` 是基于时钟的，每 4 微秒 + 1。

ISN=M + F(localhost, localport, remotehost, remoteport)。

- `M` 是一个**计时器**，这个计时器每隔 4 微秒加 1。
    
- `F` 是一个 **Hash 算法**，根据源 IP、目的 IP、源端口、目的端口生成一个随机数值。要保证 Hash 算法不能被外部轻易推算得出，用 MD5 算法是一个比较好的选择。
    

#### 为什么每次建立 TCP 连接时，初始化的序列号都要求不一样呢

- 为了防止历史报文被下一个相同四元组的连接接收（**重要**）
    
- 为了安全性，防止黑客伪造的相同序列号的TCP报文被对方接收
    

  

### 四次挥手

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTdmOTZjNmU0MjY0YTIwMTE2MGQyMDE0OWM3YjhjNzdfWVNGVUxlWHYxZXllTEdoZTZDcXBJU2o1TTZZa1B0dGVfVG9rZW46Qk05NmJEWnZKb2o3R0l4OUlocGNvTWZObldlXzE3NzM2NDk4NTg6MTc3MzY1MzQ1OF9WNA)

1. 发起方（假如是客户端）发送FIN包，并附带序列号Seq=x
    
2. 服务器收到之后回复ACK=x+1，附带序列号y
    
3. 服务器不需要传送数据之后，发送FIN，序列号z
    
4. 客户端回复ack=z+1
    

#### TIME_WAIT状态的作用

- 能够接收到一段时间内来自对方的延迟数据包。可以方式旧的数据包干扰新的连接
    
- 确保被动关闭连接的一方能正确关闭
    

### 如果建立了连接后服务器出错了怎么办

1. 服务器进程崩溃（但操作系统正常），操作系统会检测到 “进程崩溃”，自动释放该进程占用的所有 TCP 连接资源，并向客户端发送**RST****（复位）报文**（强制关闭连接，无需四次挥手）
    
2. 服务器完全宕机（断电 / 系统崩溃），客户端处理：依赖 TCP 的**超时重传机制**和应用层**心跳检测**：
    

### 滑动窗口
[4.2 TCP 重传、滑动窗口、流量控制、拥塞控制 | 小林coding | Java面试学习](https://xiaolincoding.com/network/3_tcp/tcp_feature.html#%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3)
> 引入窗口概念的原因
TCP 是每发送一个数据，都要进行一次确认应答。当上一个数据包收到了应答了， 再发送下一个，这种方式的缺点是效率比较低。
有了窗口，就可以指定窗口大小，窗口大小就是指**无需等待确认应答，而可以继续发送数据的最大值**。
> 窗口大小由哪一方决定
通常由接收方的窗口大小决定，在tcp头里有一个字段叫window也就是窗口大小。
> 发送方的滑动窗口
#### 累计确认
假设窗口大小为 `3` 个 TCP 段，那么发送方就可以「连续发送」 `3` 个 TCP 段，并且中途若有 ACK 丢失，可以通过「下一个确认应答进行确认」。
![[Pasted image 20260323183403.png]]


### 拥塞控制

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWVkMTE0NzA0NTJhYWQ1YzYyOGI2ZTE0YjM2YjY0NGZfM3UzSGh0UFFiRGM3WXB6ZTZvV0duT0p1TzU0dTdicXNfVG9rZW46WVJEamJEYjhXb09Ham54aGc5emNUNTJpbkhnXzE3NzM2NDk4NTg6MTc3MzY1MzQ1OF9WNA)

TCP 的拥塞控制采用了四种算法，即 **慢开始、 拥塞避免、快重传 和 快恢复**。

- 慢开始：cwnd （拥塞窗口）初始值为 1，每经过一个传播轮次，cwnd 加倍。直至ssthresh进入拥塞避免
    
- 拥塞避免：每经过一个往返时间 RTT 就把发送方的 cwnd 加 1
    
- 快重传和快恢复（fast retransmit and recovery，FRR）：没有 FRR，如果数据包丢失了，TCP 将会使用定时器来要求传输暂停，有了 FRR，如果接收机接收到一个不按顺序的数据段，它会立即给发送机发送一个重复确认。如果发送机接收到三个重复确认，它会假定确认件指出的数据段丢失了，并立即重传这些丢失的数据段，不会因为重传时要求的暂停被耽误。
    
- ②超时重传：ssthresh 设为 cwnd/2，cwnd 重置为 1（是恢复为 cwnd 初始化值，我这里假定 cwnd 初始化值 1），然后开始慢启动
    

**发生拥塞之后窗口如何减小？减小到多少？就或者说我收到了三个重复的****ack** **以后，这个窗口会怎么样？**

如果是超时，cwnd减小到1，ssthresh 设为 cwnd/2，开始慢启动

如果是三个重复的ack， cwnd = cwnd/2 ，也就是设置为原来的一半; ssthresh = cwnd;（因为三个ack说明拥塞的情况没有特别严重）

## SYN洪流攻击

最原始、最经典的 **DDoS**（Distributed Denial of Service，分布式拒绝服务）攻击之一

SYN Flood利用了TCP的三次握手机制，攻击者利用工具或者控制僵尸主机向服务器发送海量的变源IP地址或者变源端口的TCP SYN报文，服务器响应了这些报文候就会生产大量的半连接，当系统资源被耗尽后，服务器将无法提供正常的服务。

防御 SYN Flood 的关键在于判断哪些连接请求来自于真实源，屏蔽非真实源的请求以保障正常的业务请求能得到服务。

**如何缓解**

- **扩展积压工作队列**：增加操作系统允许的最大半开连接数目，如果系统没有足够的内存，无法应对增加的积压工作队列规模，将对系统性能产生负面影响，但仍然好过拒绝服务。
    
- **回收最先创建的****TCP****半开连接**：填充积压工作后覆盖最先创建的半开连接
    
- **SYN** **Cookie**：修改三次握手协议，服务器在收到SYN请求并发送SYN+ACK响应时，不立即分配资源，而是根据SYN请求计算出一个cookie值。这个cookie值被用作SYN+ACK响应中的初始序列号。当服务器收到客户端的ACK响应时，它会根据ACK中的确认序列号重新计算cookie值，如果计算结果与之前发送的SYN+ACK中的初始序列号相匹配，那么服务器就确认这是一个合法的连接请求，随后才分配资源并建立连接。

# 设计模式

## 设计原则

SOLID 五大原则：单一职责原则（SRP）、**开放 - 封闭原则**（OCP）、里氏替换原则（LSP）、接口隔离原则（ISP）、依赖倒置原则（DIP）

开放封闭原则：对扩展开放、对修改关闭，开闭原则的本质是 “通过抽象隔离变化”

## 单例设计模式(357/1759=20.3%)

单例设计模式是一种创建型设计模式，核心思想是保证一个类在整个应用中只有一个实例，并提供一个全局的访问入口。它常用于管理共享资源，比如**配置、缓存或数据库连接**等场景。

**1.****单例模式****的经典实现方式对比**

(1)饿汉式（静态常量）

```java
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

特点：类加载时立即初始化，线程安全。

优点：实现简单，无并发问题。

缺点：即使未使用也会占用资源，可能造成浪费。

(2)懒汉式（线程安全）

```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

特点：首次调用时初始化，通过synchronized保证线程安全。

缺点：每次调用getInstance()都会同步，性能较低。

（3）双重校验锁实现对象单例（线程安全）（DCL，即 double-checked locking）

```java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    private Singleton() {}
    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

## 工厂设计模式(411/1759=23.4%)

“通过工厂类统一管理对象的创建过程，隐藏对象实例化的细节”

1. 类对象的创建通过工厂来管理，类的创建逻辑发生了变化，那么只需要修改工厂类
    

(1)简单工厂模式（Simple Factory）

简单工厂模式并不是一种正式的设计模式，但它是最基础的形式。它通过一个工厂类根据传入的参数决定创建哪种具体产品。

```typescript
public class SimpleFactory {
    public static Product createProduct(String type) {
        if ("A".equals(type)) {
            return new ConcreteProductA();
        } else if ("B".equals(type)) {
            return new ConcreteProductB();
        }
        throw new IllegalArgumentException("Unknown type");
    }
}

// 抽象产品
public interface Product {
    void use();
}

// 具体产品A
public class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

// 具体产品B
public class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}
```

特点：集中管理对象创建逻辑，易于理解和实现。

缺点：违反开闭原则，新增产品时需修改工厂类。

(2)工厂方法模式（Factory Method）

工厂方法模式通过定义一个创建对象的接口，让子类决定实例化哪个类。它将对象的创建延迟到子类中完成。

```java
// 抽象工厂
public abstract class Factory {
    public abstract Product createProduct();
}

// 具体工厂A
public class ConcreteFactoryA extends Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

// 具体工厂B
public class ConcreteFactoryB extends Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}

// 抽象产品
public interface Product {
    void use();
}

// 具体产品A
public class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

// 具体产品B
public class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}
```

特点：符合开闭原则，新增产品只需添加新的工厂和产品类。

缺点：每新增一个产品都需要增加对应的工厂类。

(3)抽象工厂模式（Abstract Factory）

抽象工厂模式提供了一组相关或依赖对象的创建接口，而无需指定它们的具体类。适用于需要创建一系列产品的场景。

```typescript
// 抽象工厂
public interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

// 具体工厂1
public class ConcreteFactory1 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

// 具体工厂2
public class ConcreteFactory2 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}

// 抽象产品A
public interface ProductA {
    void use();
}

// 具体产品A1
public class ConcreteProductA1 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using Product A1");
    }
}

// 抽象产品B
public interface ProductB {
    void use();
}

// 具体产品B1
public class ConcreteProductB1 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using Product B1");
    }
}
```

特点：支持创建一组相关的产品，适合复杂系统。

缺点：增加了系统的复杂性，可能需要大量的工厂和产品类。

## 生产者消费者设计模式(389/1759=22.1%)

## 责任链模式

当客户端发出一个请求时，该请求会从链的头部开始传递，依次经过每个处理者。每个处理者在接收到请求后，会判断自己是否能够处理该请求，如果可以，则进行处理；如果不能，则将请求传递给链中的下一个处理者，直到请求被处理或者到达链的末尾。

使多个对象都有机会处理同一请求，从而避免请求的发送者和接受者之间的耦合关系，每个对象都是一个处理节点，将这些对象连成一条链，并沿着这条链传递该请求。

## 享元模式

[设计模式第14讲——享元模式(Flyweight)-CSDN博客](https://blog.csdn.net/weixin_45433817/article/details/131335574)

享元模式是一种结构型的设计模式。它的主要目的是通过共享对象来减少系统中对象的数量，其本质就是缓存共享对象，降低内存消耗。

享元模式将需要重复使用的对象分为两个部分：内部状态和外部状态。

内部状态是不会变化的，可以被多个对象共享，而外部状态会随着对象的使用而改变。比如，连接池中的连接对象，保存在连接对象中的用户名、密码、连接URL等信息，在创建对象的时候就设置好了，不会随环境的改变而改变，这些为内部状态。而当每个连接要被回收利用时，我们需要将它标记为可用状态，这些为外部状态。

## 策略模式

### 应用场景

#### 4.1 生活场景

- 共享单车：在共享单车系统中，每辆车可以看做是一个享元对象。共享的部分是单车的基本属性，比如颜色、价格、型号等。相同型号、价格、颜色的单车可以共享同一个对象，不需要每次创建新的对象。
    
- 车票预订：在一个车站的车票预订系统中，有许多订票窗口，每个窗口可以处理多个订票请求。每个订票请求都可以看作一个享元对象，而订票窗口可以看作是“享元工厂”。订票窗口根据请求的参数来创建或共享对象，避免创建过多的重复对象，提高了系统的性能。
    
- 汉字输入法：在汉字输入法中，由于汉字数量很多，为了避免重复创建，使用享元模式，即相同的汉字只需要在内存中创建一次，这样就大大减少了内存的消耗，也加快了输入法的执行速度。
    

#### 4.2 java场景

- String 类型：内部维护了一个字符串对象池（即字符串常量池），相同的字符串只会在池中创建一次，之后它们会被多个对象共享，这样可以减少字符串对象的数量，降低内存的使用，并提升系统的效率。
    
- Integer 类型：对常用的整数值（-127~128）进行了缓存，避免了每次创建新的 Integer 对象，提高了系统的性能。
    
- 数据库连接池：数据库连接是非常消耗资源的，通过享元模式将连接池内部的连接对象进行复用，减少连接对象的创建和销毁，提高系统的效率和性能。
    
- 线程池：通过享元模式将线程池中创建的线程对象进行复用，减少了线程的创建和销毁，从而提高系统的性能。
    

1. Builder模式
    
    1. 一个类的创建包含很多步骤，而且这些步骤不是严格同步执行，不同的步骤组合会有不同的表示
        

## 工厂模式和策略模式的区别

工厂模式关注的是**对象的创建**：好比想要一台电脑、想要一台计算器，工厂给你生产出来

策略模式关注的是**行为的封装**：好比要开发一台电脑或者计算器，你想实现加减法。是 a+b 还是 b+a，由你决定；是 a×10÷10+b 还是 (a+b)，也由你决定。对外暴露的就是加减功能，用户能知道有这俩功能就行

# 分布式

## CAP

一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）

- 一致性：分布式存储系统的数据是否都一致（所有节点能否都访问最新的数据）（分为弱一致性、强一致性和最终一致性）
    
- 可用性：部分节点故障之后，集群整体能否响应读写请求
    
- 分区容错性：通信时限要求，系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDBhZGU1YThlZTQxNTA0NmQ1YjE0NmY1YmFlZDQ2MGVfUUZ2MkdiT0dMWmNOVWF5akhJbGE3ZENMbkdRczhkdDFfVG9rZW46UTZzcWJ1VEx4b01LSk94Z3FkRmNBQllFbmtiXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

# Linux

## linux 命令：一个目录底下有 100 个文件，根据文件大小排序，过滤出最大的 10 个文件，然后删除 7 天之前创建的文件，该如何实现

1. 按文件大小排序并筛选最大 10 个
    

```Bash
# 按大小降序排序，显示前10个文件（含详细信息）
ls -lhS | head -n 11(第 1 行为总大小信息)
ls -lhS：以长格式显示文件，按大小降序排序；
```

2. 删除 7 天前创建的文件
    

- `find . -type f -ctime +7 -delete`
    
    - `.`：当前目录；
        
    - `-type f`：仅处理文件（排除目录）；
        
    - `-ctime +7`：状态改变时间超过 7 天（`+7`表示 7 天前及更早）；Linux **ext4 等文件系统一般不保存“创建时间（birth time）”（这是豆包给的答案）**
        
    - `-delete`：直接删除符合条件的文件（谨慎使用）。
        

## linux系统比较卡如何排查

1. **查看****内存****使用情况**
    

free -g

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MmE5Mjk1ODdiM2VlZTYzNjVkOTA1ZWQ4ZDY3ZmM2YmFfM2NmbXFIQVMzck1xWjVuNHNNU2xuNUpMM0d6NkY5dFFfVG9rZW46SkFKVGJidVNtb1d0dWJ4Sms0NmNZMTU1bkJkXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

当观察到free栏已为0的时候，表示内存基本被吃完了，那就释放内存吧

2. **查看磁盘使用情况**
    

df -h

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmI3NzllY2JhOGRhNjlhMTU4MmEzZGFmZjg1NTgwNjFfanp1WG1oQWlWb2l6b1l4dFhaOUgySnJRNFZQUno2Z2VfVG9rZW46SWs3OWJzcUVxb0R2T2Z4b3psM2NtVzVhbkxkXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

当发现磁盘使用率很高时，那就要释放磁盘空间了，删除一些不必要的文件

3. **查看磁盘****IO****使用情况**
    

iostat -x 1

1表示1秒刷新一次

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTg1MjFmMzBhZmU5NzBjMTM3NjZmMTJjNzIwOWNhYjNfZURDbnhucGhNU0h4Mkg4MTJTY3gzbmlsNzFhM1NDaXBfVG9rZW46TnpJVGJqYnJCbzRoMkR4M1VjU2NwSHJibkxkXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

当发现最右侧%util很高时，表示IO就很高了，若想看哪个进程占用IO，执行iotop命令查看

4. **查看cpu使用情况**
    

top

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGUxNTRjZTcxNWRjM2ViNGY3NGEyODZhMTQ3ZTU4MmRfTmZnWDZEaFlDdXpkYmZJTUx6ZnJvYUVXMGpkR1libkFfVG9rZW46TnNibWJrSzIyb0Z6YXV4Ymp4VmNvMTVhbmNkXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

下图中红框里表示cpu使用情况，最右侧的%id表示剩余，若很低，则表示cpu被吃完了，在top界面按shift+p对进程使用cpu排序，能看到哪些进程占用cpu较多

## 查看java进程

Jps

jstack

## **如何查看那个进程比较占****内存****，命令是什么?线上发现一个内存跑满了，怎么查看是哪个线程造成的?**

1. **查看进程占用内存的情况：**
    

`top`

然后按 **M** 键，按内存占用量排序，这样可以看到哪些进程占用了最多的内存。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MDc2MDcxMTM4M2JkZDFiMzc0NjM1ZmZjZjM2MTBlYmJfc0JqazdnWTJOV3hZV1NSUW5lcTdEcDV3dnBhY3gxc01fVG9rZW46S1V4RGJPbDE4b3Y3NWZ4N0JZUGNIQlRnbjdlXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

2. **查看更详细的内存使用情况：**
    

`ps aux --sort=-%mem | head`

这将显示占用最多内存的前几个进程。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NGMwYTRjYTRjZjMyNDFjY2RjZDYxYWZhY2Q5Nzg2ZWJfWUVIbDY3MkdIN1Z2S2ZzNGd2SFpGb1lBVEh1VlB2eGpfVG9rZW46Vm1pQ2JqejRrb1c0ZlF4d0RIa2NLMWhHbmFjXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

如果你想进一步了解某个进程中的具体线程占用了多少内存，可以使用：

3. **查看特定进程中的线程：**
    

首先，找到进程的PID，然后运行：

`top -H -p <PID>`

这里 `-H` 表示查看线程信息，`-p <PID>` 指定要查看的进程。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YTZmZGIxMjM0MzMyMTA1MTAxMTY2NzlmMzA0YjhiNzRfVTdONnZVWGMxYUI1TDljZmVlTDBId2szdWVBcmw2bG9fVG9rZW46WW1GSGJJUUlsb0dMMWJ4clFEdWNqdzNSbkpoXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

这里的PID是 线程ID。在这个视图下，也可以执行top进程视图下的操作，例如按CPU、内存排序等。

4. **查看线程占用内存的详细信息：**
    

你也可以使用 `ps` 命令查看线程的内存使用情况：

`ps -eLf | grep <PID>`

这个命令会列出所有属于该进程的线程，并显示它们的资源使用情况。

## 虚拟内存

主要作用是作为进程访问主存的桥梁并简化内存管理

提供了下面这些能力：

- **隔离进程**：每个进程都认为自己拥有了整个物理内存，进程之间彼此隔离，一个进程中的代码无法更改正在由另一进程或操作系统使用的物理内存。
    
- **提供更大的可使用****内存****空间**：物理内存不够用时，可以利用磁盘充当（但会影响读写速度）
    
- **提升****物理内存****利用率**：操作系统只需要将进程当前正在使用的部分数据或指令加载入物理内存
    
- **简化****内存****管理**：不需要和真正的物理内存打交道
    
- **多个进程共享****物理内存**：操作系统的动态库在内存中实际只会加载一份，对于每个进程都是公用的
    
- **提高****内存****使用安全性**：隔离不同进程的访问权限，提高系统安全性
    

# 测试

## 逻辑覆盖测试

逻辑覆盖测试法的六种类型：语句覆盖、判定覆盖、条件覆盖、判定-条件覆盖、条件组合覆盖和路径覆盖

[动态白盒测试——逻辑覆盖测试法-CSDN博客](https://blog.csdn.net/weixin_44048668/article/details/115460791)

**Alpha 测试**是一种验收测试；在将最终产品发布给最终用户之前，执行以识别所有可能的问题和错误。Alpha 测试由作为组织内部员工的测试人员执行。主要目标是确定典型用户可能执行的任务并对其进行测试。

尽可能简单地说，这种测试被称为 alpha 只是因为它在软件开发的早期、接近尾声和 beta 测试之前完成。alpha 测试的主要焦点是使用黑盒和白盒技术模拟真实用户。

**Beta 测试**是由软件应用程序的“真实用户”在“真实环境”中进行的，它可以被认为是一种外部用户验收测试。这是将产品运送给客户之前的最终测试。来自客户的直接反馈是 Beta 测试的主要优势。此测试有助于在客户环境中测试产品。

该软件的 Beta 版发布给有限数量的产品最终用户，以获取对产品质量的反馈。Beta 测试降低了产品故障风险，并通过客户验证提高了产品质量

# MQ

## 消息队列

### 三大作用

- 解耦：将原本通过网络之间的调用的方式改为使用MQ进行消息的异步通讯，这样项目之间不会存在耦合，就算一个系统挂了，也只是消息挤压在MQ里面没人消费而已
    
- 异步：对于不需要同步完成的多个步骤之间，可以通过异步实现
    
- 削峰：如果流量太大，可以将用户的大量消息直接放到MQ里面，然后我们的系统去按自己的最大消费能力去消费这些消息，就可以保证系统的稳定
    

### 消息队列如何选型

![[Pasted image 20260322233022.png]]

1.性能和吞吐量:如果需要处理海量数据，需要高性能和高吞吐量，那么Kafka是一个不错的选择。

2.可靠性:如果需要保证消息传递的可靠性，包括数据不丢失和消息不重复投递，那么RocketMQ和RabbitMQ都提供了较好的可靠性保证。

3.消息传递模型:如果需要支持发布-订阅和点对点模型，那么RocketMQ和RabbitMQ是一个不错的选择。如果只需要发布-订阅模型，Kafka则是一个更好的选择。

4.消息持久化:如果需要更快地持久化消息，并且支持高效的消息查询，那么Kafka是个不错的选择。如果需要更加传统的消息持久化方式，那么RocketMQ和RabbitMQ可以满足需求。

5.开发和部署复杂度:Kafka比较简单，易于使用和部署，但在实现一些高级功能时需要进行一些复杂的配置。RocketMQ和RabbitMQ提供了更多的功能和选项，也更加灵活，但相应地会增加开发和部署的复杂度。

为什么选择RabbitMQ？

RabbitMQ 提供了多层次的可靠性机制，能最大限度避免消息丢失，完善的容错与重试机制，应对服务临时故障。

**灵活的消息路由**：通过Direct、Topic、Fanout、Headers四种Exchange类型实现精确匹配、通配符、广播及基于消息头的路由，适配复杂业务逻辑。

在中小规模场景下可实现微秒级响应，适合实时性要求高的业务。

RabbitMQ 提供了**全链路可靠性机制**：

- 消息持久化（交换机、队列、消息均支持持久化到磁盘），即使 Broker 重启，消息也不会丢失；
    
- 生产者确认（Publisher Confirm）机制，确保消息成功投递到 Broker 后才返回，避免网络波动导致的消息丢失；
    
- 消费者手动 ACK 机制，只有下游服务（如库存服务）确认消息处理完成后，Broker 才删除消息，防止服务崩溃导致的消息丢失。
    

RabbitMQ 提供了**死信队列**（DLX）+ **重试机制**：

- 若下游服务处理失败（如返回 NACK），消息会被重新放入队列或延迟队列，按指数退避策略重试（如间隔 1s、5s、10s）；
    
- 多次重试失败后，消息进入死信队列，避免阻塞正常消息，同时可人工排查失败原因（如短信模板错误），保证业务连续性。
    

### 消息队列如何保证最终一致性

rocketMq支持事务消息

https://blog.csdn.net/weixin_42222436/article/details/123155310

### 什么时候会出现重复消费问题？

重复消费的本质是“消息消费成功，但消费结果未被RocketMQ正确记录”

1. 消费端确认机制异常(消费端是业务逻辑完成之后才发送ack)
    
    1. 消费端宕机（服务已完成，但还发送ack就宕机了）
        
    2. 消费端ACK发送失败（网络波动未送达broker）
        
    3. 消费逻辑超时（默认15分钟，超时会判定为消费失败）
        
2. broker重试机制触发
    
    1. broker主从切换（主节点宕机，从节点升级，可能存在部分消息为同步）
        
3. 生产端重复发送
    
    1. 未收到发送确认（可能网络超时）
        

### 消息队列幂等性如何设计
[✅RabbitMQ如何防止重复消费](https://www.yuque.com/hollis666/gg1x9v/epqupbq473z9mkew)
一锁二判三更新
一锁：第一步，先加锁。可以加分布式锁、或者悲观锁都可以。但是一定要是一个互斥锁!
二判：第二步，进行幂等性判断。可以基于状态机、流水表、唯一性索引等等进行重复操作的判断。
三更新：第三步，进行数据的更新，将数据进行持久化。
```
//一锁：先加一个分布式锁
@DistributeLock(scene = "OEDER", keyExpression = "#request.identifier", expire = 3000)
public OrderResponse apply(OrderRequest request) {
    OrderResponse response = new OrderResponse();
    //二判：判断请求是否执行成功过
    OrderDTO orderDTO = orderService.queryOrder(request.getProduct(), request.getIdentifier());
    if (orderDTO != null) {
        response.setSuccess(true);
        response.setResponseCode("DUPLICATED");
        return response;
    }

    //三更新：执行更新的业务逻辑
    return orderService.order(request);
}
```
重复消费会怎样？重复消费的危害取决于业务场景是否 “幂等”（即 “重复执行同一操作，结果与执行一次一致”）危害：库存重复扣减、数据重复插入、金额计算错误、资源浪费。

- 唯一标识：全局唯一id（通用方案）
    
- 数据库事务+乐观锁：通过版本号或状态字段控制并发更新
    
- 数据库唯一约束：（贴合业务）
    
- 分布式锁
    
- 消息去重
    

辅助方案：**控制重试次数上限**，通过`setMaxReconsumeTimes`设置重试次数（如 5 次），超过次数后将消息放入 “死信队列”（DLQ），避免无限重试浪费资源。后续可通过人工排查死信队列（如你的项目中 “库存变更失败” 的死信消息），分析失败原因并处理。

兜底：为 RocketMQ 配置死信队列监控（如通过 Prometheus 采集死信消息数量，设置告警阈值），当死信消息超过 10 条时触发告警；开发死信消息补偿程序（如定时任务），对死信消息进行人工审核或自动重试（如确认是网络问题导致的重复，可重新投递），确保核心业务消息不丢失。

### 投递语义(kafka)
[✅为什么Kafka没办法100%保证消息不丢失？](https://www.yuque.com/hollis666/gg1x9v/vwy7vz63qax9c7ab)
Kafka提供的Producer和Consumer之间的消息传递保证语义有三种，所谓消息传递语义，其实就是Kafka的消息交付可靠保障，主要有以下三种:
- Atmost once（至多一次）一消息可能会丢，但绝不会重复传递;
- At least once（至少一次）（默认）一消息绝不会丢，但可能会重复传递;
- Exactly once（精确一次）一每条消息只会被精确地传递一次：既不会多，也不会少;

## RabbitMQ
### 数据存储结构
[深度解析RabbitMQ网络存储与生产消费核心架构-开发者社区-阿里云](https://developer.aliyun.com/article/1409948)
1. RabbitMQ消息有两种类型：持久化和非持久化消息
2. RabbitMQ 的存储模块也包含元数据存储与消息数据存储两部分。
3. 消息数据存储：**RabbitMQ 消息数据的最小存储单元是 Queue**
![[Pasted image 20260322175900.png]]
![[Pasted image 20260322175938.png]]
- Queue：逻辑概念，表示一条消息队列，负责消息顺序、投递状态、ack 状态。
- rabbit_queue_index：每个队列各自有一份索引文件，记录“这个队列里有哪些消息、顺序是什么、消息状态怎样、消息体在哪里”。
- rabbitmq_msg_store：共享消息存储，不是按队列分开的，通常同一 vhost 下多个队列共用。追加写
- msg_store_persistent / msg_store_transient：共享消息存储再按“持久化消息/非持久化消息”分成两套。

msg_store可以粗略理解成键值对形式保存：
- key：消息 ID / 消息在存储中的标识
- value：消息体内容

**一条消息从写入到消费，结构关系是这样的**

生产者发来消息后：

1. RabbitMQ 先把消息体写到共享 msg_store
2. 如果路由到多个队列，就给每个队列各写一条 queue index 记录
3. 消费某个队列时，RabbitMQ 先看这个队列自己的 queue index
4. 再根据索引去 msg_store 找到消息体
5. 当所有引用它的队列都确认删除后，msg_store 里的消息体才真正可清理

### 工作模式

[RabbitMQ六种工作模式RabbitMq的工作模式其实大致都是基于四种类型的交换机来划分的，simple简单模式、w - 掘金](https://juejin.cn/post/6997217228743507981)

1. simple简单模式：生产者->消息队列->消费者
    
2. work工作模式：多个消费者监听同一个消息队列
    
3. Publish/Subscribe发布订阅模式：每个消费者一个队列，由交换机将消息投递到不同的队列当中（交换机类型Fanout）
    
4. Routing路由模式：生产者发送消息时指定routing key，交换机会根据routing key将消息投递到指定的队列（交换机类型Direct）
    
5. Topic主题模式：（交换机类型Topic）支持模糊匹配，消息能被投递到一个或多个队列中。根据routing key投递消息
    
6. RPC模式（很少见，感觉不需要掌握）
    

#### 交换机类型

1. Direct(routing key)：根据routing key完整匹配进行转发，一个消息队列可以通过多个binding key与一个交换机进行绑定
    
2. fanout：不处理routing key，会转发到与该交换机绑定的所有对了，类似于广播
    
3. topic（模糊匹配的routing key）：
    
      `#` ：代表匹配一个多或多个、或者一个也匹配不到,支持多级
    
      `*` ：代表必须匹配一个，且只能是一级（比如`a.b.sms`，和`*.sms`不匹配）
    
4. headers：基于基于消息的headers数据进行路由，交换机绑定队列时需要指定参数Arguments
    

### RabbitMQ的高可用的原理

RabbitMQ 是比较有代表性的，因为是基于主从（非分布式）做高可用性的，我们就以 RabbitMQ 为例子讲解第一种 MQ 的高可用性怎么实现。RabbitMQ 有三种模式：单机模式、普通集群模式、镜像集群模式。

**单机模式**

Demo 级别的，一般就是你本地启动了玩玩儿的?，没人生产用单机模式。

**普通集群模式**

意思就是在多台机器上启动多个 RabbitMQ 实例，每个机器启动一个。你创建的 queue，只会放在一个 RabbitMQ 实例上，但是每个实例都同步 queue 的元数据（元数据可以认为是 queue 的一些配置信息，通过元数据，可以找到 queue 所在实例）。

你消费的时候，实际上如果连接到了另外一个实例，那么那个实例会从 queue 所在实例上拉取数据过来。这方案主要是提高吞吐量的，就是说让集群中多个节点来服务某个 queue 的读写操作。

**镜像集群模式**

这种模式，才是所谓的 RabbitMQ 的高可用模式。跟普通集群模式不一样的是，在镜像集群模式下，你创建的 queue，无论元数据还是 queue 里的消息都会存在于多个实例上，就是说，每个 RabbitMQ 节点都有这个 queue 的一个完整镜像，包含 queue 的全部数据的意思。然后每次你写消息到 queue 的时候，都会自动把消息同步到多个实例的 queue 上。RabbitMQ 有很好的管理控制台，就是在后台新增一个策略，这个策略是镜像集群模式的策略，指定的时候是可以要求数据同步到所有节点的，也可以要求同步到指定数量的节点，再次创建 queue 的时候，应用这个策略，就会自动将数据同步到其他的节点上去了。

这样的好处在于，你任何一个机器宕机了，没事儿，其它机器（节点）还包含了这个 queue 的完整数据，别的 consumer 都可以到其它节点上去消费数据。坏处在于，第一，这个性能开销也太大了吧，消息需要同步到所有机器上，导致网络带宽压力和消耗很重！RabbitMQ 一个 queue 的数据都是放在一个节点里的，镜像集群下，也是每个节点都放这个 queue 的完整数据。

### rabbitMQ如何实现延迟消息?
给消息设定一个ttl，（投递到一个普通队列当中）但不消费这个消息，等它过期，这个消息会进入私信队列，然后再监听死信队列的消息进行消费。
RabbitMQ中的ttl可以设置任意时长，比RocktetMQ只支持一些固定的时长更加灵活。
#### 存在问题
可能造成队头阻塞，因为mq会定时检查队头的消息是否过期，但不会逐个检查队列中的所有消息是否过期
也有相应的延迟消息插件实现
## 零拷贝技术

### 如何实现零拷贝技术

- mmap + write
    
- sendfile
    

# Zookeeper

## 重要概念

### 存储结构

树形结构

我们通常是将 znode 分为 4 大类：

- **持久（PERSISTENT）节点**：一旦创建就一直存在即使 ZooKeeper 集群宕机，直到将其删除。
    
- **临时（EPHEMERAL）节点**：临时节点的生命周期是与 **客户端会话（session）** 绑定的，**会话消失则节点消失**。并且，**临时节点只能做****叶子节点** ，不能创建子节点。
    
- **持久顺序（PERSISTENT_SEQUENTIAL）节点**：除了具有持久（PERSISTENT）节点的特性之外， 子节点的名称还具有顺序性。比如 `/node1/app0000000001`、`/node1/app0000000002` 。
    
- **临时顺序（EPHEMERAL_SEQUENTIAL）节点**：除了具备临时（EPHEMERAL）节点的特性之外，子节点的名称还具有顺序性
    

每个 znode 由 2 部分组成:

- stat：状态信息
    
- data：节点存放的数据的具体内容
    

### Watcher（事件监听器）

Watcher（事件监听器），是 ZooKeeper 中的一个很重要的特性。ZooKeeper 允许用户在指定节点上注册一些 Watcher，并且在一些特定事件触发的时候，ZooKeeper 服务端会将事件通知到感兴趣的客户端上去，该机制是 ZooKeeper 实现分布式协调服务的重要特性。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Nzc5MGI0NmUyYTkwNjg4NzlkNjUwMDgzMTE3ZmEzY2JfNGZUMjFaa2JUUk5TcWtZR24wZzlvTmVkUmd1U2VFMThfVG9rZW46SzlIamJOTExYb1YxS0h4YnNBRWN0aEI3blBlXzE3NzM2NDk5MDU6MTc3MzY1MzUwNV9WNA)

_破音：非常有用的一个特性，都拿出小本本记好了，后面用到_ _ZooKeeper_ _基本离不开 Watcher（事件__监听器__）机制。_

### [会话（Session）](https://javaguide.cn/distributed-system/distributed-process-coordination/zookeeper/zookeeper-intro.html#%E4%BC%9A%E8%AF%9D-session)

  

## 集群

### ZooKeeper 集群 Leader 选举过程

  

### ZooKeeper 集群为啥最好奇数台？

  

### ZooKeeper 选举的过半机制防止脑裂

**何为集群脑裂？**

  

## 一致性问题

### 2PC

### 3PC

# 其他

## 如何解决接口幂等的问题

https://www.yuque.com/hollis666/gg1x9v/gz2qwl

一锁、二判、三更新

## 性能调优

### 压测在性能调优中的作用

通过模拟真实用户行为和负载情况，发现系统在高压力下的性能瓶颈，并针对性地进行优化。 这一过程不仅能够提高系统的**吞吐量**和**响应速度**，还能增强系统的**稳定性**和**可扩展性**。

我在压测中验证过我们系统的承压能力。 在 2C4G、带宽 1Mbps 的环境下，系统在 **500** **TPS** **/ 100** **QPS** 时表现稳定，错误率低于 0.2%。 当时的瓶颈主要出现在带宽和硬件限制，而不是应用本身。 如果换到更高配置（比如 4C8G + 10Mbps，或者使用分布式 JMeter），系统能够轻松扩展到 **千级 TPS** 的并发压力。

### 指标

核心性能指标（需结合业务场景定义阈值）：

- 接口响应时间：如 “商品详情接口 P99 响应时间 ≤ 500ms”“下单接口 P95 响应时间 ≤ 1s”；
    
- 系统吞吐量（TPS/QPS）：如 “首页接口 QPS ≥ 1000”“支付接口 TPS ≥ 500”；
    
- 资源利用率：CPU 使用率 ≤ 70%、内存使用率 ≤ 80%、数据库连接池使用率 ≤ 80%；
    
- 错误率：接口错误率 ≤ 0.1%（如超时、异常响应）。
    

#### 核心调优场景与落地方案（附项目案例）

结合电商项目常见瓶颈，分 “应用层、数据库层、缓存层、架构层” 拆解调优方案：

1. 应用层调优（代码 / 配置优化）
    

- 案例 1：接口序列化耗时久 问题：商品列表接口用 Jackson 序列化复杂对象（包含 20+ 字段），单次序列化耗时 200ms，占接口总耗时的 40%。 调优：
    
    - 用 FastJSON2 替换 Jackson（FastJSON2 序列化速度比 Jackson 快 30%+）；
        
    - 按需序列化：用 `@JSONField(serialize = false)` 排除不需要返回的字段（如商品内部状态字段）；
        
    - 结果：序列化耗时降至 80ms，接口总耗时从 500ms 降至 380ms。
        
- 案例 2：频繁创建大对象导致 GC 频繁 问题：订单接口每次请求创建 10+ 临时对象（如 StringBuilder、HashMap），JVM 年轻代 GC 频率从 5 分钟 / 次升至 1 分钟 / 次，GC 耗时从 50ms 升至 200ms。 调优：
    
    - 复用对象：用 ThreadLocal 缓存线程私有对象（如 StringBuilder），避免重复创建；
        
    - 用数组替代集合：简单场景下用数组（如 String []）替代 ArrayList，减少集合 overhead；
        
    - 结果：年轻代 GC 频率恢复至 5 分钟 / 次，GC 耗时降至 60ms。
        

2. 数据库层调优（SQL / 索引 / 配置）
    

- 案例 1：慢 SQL 导致接口超时 问题：用户订单查询接口（`SELECT * FROM order WHERE user_id = 123 AND create_time > '2024-01-01'`）耗时 1.2s，触发接口超时（阈值 1s），慢查询日志显示 “全表扫描”。 调优：
    
    - 加联合索引：`ALTER TABLE order ADD INDEX idx_user_create (user_id, create_time)`（遵循 “最左前缀原则”，匹配查询条件）；
        
    - 避免 `SELECT *`：只查询需要的字段（如 `id, order_no, amount`），减少数据传输量；
        
    - 结果：SQL 耗时从 1.2s 降至 50ms，接口响应时间降至 300ms。
        
- 案例 2：数据库连接池耗尽 问题：秒杀活动期间，下单接口并发量从 200 QPS 升至 1000 QPS，数据库连接池（HikariCP）配置为 “最大连接数 20”，出现 “连接池耗尽” 错误，接口错误率升至 5%。 调优：
    
    - 调整连接池参数：最大连接数从 20 增至 50（需结合数据库最大连接数，MySQL 默认最大连接数 151，避免超过上限）；
        
    - 加缓存：将 “用户地址”“商品库存” 等高频查询数据缓存到 Redis，减少数据库查询次数；
        
    - 结果：连接池使用率从 100% 降至 60%，接口错误率恢复至 0.1%。
        

3. 缓存层调优（Redis 优化）
    

- 案例 1：Redis 命中率低导致数据库压力大 问题：首页商品推荐接口，Redis 缓存命中率仅 60%，大量请求穿透到数据库，数据库 CPU 使用率升至 90%。 调优：
    
    - 优化缓存 key 设计：将 “用户个性化推荐” 拆分为 “热门商品缓存”（公共缓存，命中率高）和 “用户专属缓存”（私有缓存），公共缓存占比提升至 70%；
        
    - 调整过期时间：热门商品缓存过期时间从 1 小时增至 4 小时，减少缓存失效频率；
        
    - 结果：Redis 命中率升至 92%，数据库 CPU 使用率降至 40%。
        
- 案例 2：Redis 大 key 导致阻塞 问题：统计接口用 `hgetall` 获取 “商品销量哈希表”（包含 10000+ 字段），单次命令耗时 800ms，导致 Redis 主线程阻塞，其他命令响应延迟。 调优：
    
    - 拆分大 key：将 “商品销量哈希表” 按商品分类拆分为多个小哈希表（如 `sales:category:1`、`sales:category:2`）；
        
    - 用 `hscan` 替代 `hgetall`：分批次获取数据（每次扫描 100 个字段），避免单次命令阻塞；
        
    - 结果：命令耗时从 800ms 降至 50ms，Redis 主线程阻塞消失。
        

4. 架构层调优（扩容 / 降级 / 异步）
    

- 案例：秒杀场景吞吐量不足 问题：秒杀商品下单接口，单实例 TPS 仅 300，无法满足 1000 TPS 的需求，出现接口超时。 调优：
    
    - 水平扩容：将应用部署从 2 实例增至 5 实例，通过 Nginx 负载均衡分发请求；
        
    - 异步化：将 “订单创建后发短信”“记录日志” 等非核心逻辑用 MQ（RabbitMQ）异步处理，减少主流程耗时；
        
    - 限流降级：用 Sentinel 对秒杀接口设置 QPS 阈值 1000，超过阈值返回 “稍后再试”，避免系统过载；
        
    - 结果：接口 TPS 升至 1200，超时率从 10% 降至 0.5%。
        

## 用户规模大后怎么解决QPS高的问题？

高 QPS 问题的解决需要 “分层治理”：

- 前端 / CDN 层：减少无效请求，加速静态资源。
    
- 应用层：负载均衡、服务拆分、本地缓存，分散流量。
    
- 缓存层：分布式缓存抗住大部分读压力。
    
- 数据库层：读写分离、分库分表，优化存储效率。
    
- 全局控制：限流、熔断、异步化，保障系统稳定性。
    

## 限流与熔断的区别

限流：给入口“拧水龙头”，控制**并发/速率**，防止把自己或下游压垮。

熔断：发现下游“故障/高延迟”时，**快速失败**并隔离故障，避免雪崩。

## 负载均衡算法有哪些

1. 简单轮询：将请求按顺序分发给后端服务器上，不关心服务器当前的状态
    
2. 加权轮询：根据服务器**自身的性能**给服务器设置不同的权重，将请求按顺序和权重分发给后端服务器，可以让性能高的机器处理更多的请求
    
3. 简单随机：
    
4. 加权随机：根据性能加权然后随机
    
5. 一致性哈希：根据请求参数通过哈希算法得到一个数组，取模（这个模值是确定的，普通哈希的模值是服务器数量）映射
    
6. 最小活跃数：
    

### **Nginx** **负载均衡****策略**

1. **轮询****（****`默认策略`****）**
    
2. **加权轮询**
    
3. **IP** **哈希**:这种策略适用于需要**保持会话状态**的场景
    
4. **最少连接**
    

#### 选择策略

- 静态资源 / 无状态服务：轮询（带权重）
    
- 需会话保持：ip_hash
    
- 动态服务 / 长连接：least_conn
    
- 静态资源缓存优化：url_hash
    

## 提高接口性能

1. 批量操作：将多次单条操作改为批量操作
    
2. 热点数据缓存：将高频访问数据（如首页推荐、商品详情）提前缓存
    
3. 异步处理：非核心流程（如日志、通知）通过线程池或消息队列异步执行
    
4. 合理设置线程池参数：根据 CPU 核心数调整核心线程数和队列大小
    

## 设计供下游调用的接口时需要关注的问题

- 接口定义的清晰性
    
    - 明确接口的功能边界（单一职责，避免一个接口做太多事情）；
        
    - 统一参数命名和数据格式（如日期用 `yyyy-MM-dd`，状态码用枚举）；
        
    - 提供详细文档（使用 Swagger 等工具，说明参数含义、必填项、返回值、错误码）。
        
- 兼容性与版本控制
    
    - 确保接口升级时向下兼容（新增字段而非删除 / 修改原有字段）；
        
    - 预留版本号（如 `/api/v1/user`），便于迭代时区分新旧接口。
        
- 安全性
    
    - 鉴权与授权（如使用 Token、API 密钥，限制下游调用权限）；
        
    - 防滥用（限流、防重放攻击，如添加时间戳和签名）；
        
    - 数据脱敏（返回敏感信息时加密或隐藏，如手机号显示为 `138****5678`）。
        
- 可靠性
    
    - 超时控制（设置合理的超时时间，避免下游阻塞）；
        
    - 错误处理（返回明确的错误码和描述，而非模糊的 “系统错误”）；
        
    - 降级与熔断（当接口异常时，返回默认值或拒绝请求，保护自身服务）。
        
- 性能与可扩展性
    
    - 优化响应速度（减少不必要的数据库查询，使用缓存）；
        
    - 支持批量操作（如批量查询用户，减少调用次数）；
        
    - 考虑并发场景（接口是否线程安全，是否需要分布式锁）。
        

### 前端查询请求较慢的问题定位步骤

前端查询请求慢的原因可能分布在前端、网络、后端服务、数据库等多个环节，需逐步排查：

1. 前端排查
    
    1. 检查浏览器控制台（Network 面板）：查看请求的总耗时（`Duration`），区分是网络传输慢（`Waiting (TTFB)` 长）还是前端渲染慢（接收数据后渲染耗时久）。
        
    2. 查看是否有冗余请求：是否重复发送相同请求，或请求了不必要的数据（如大图片、冗余字段）。
        
    3. 检查前端逻辑：是否在请求后执行了复杂计算（如循环遍历大量数据），导致页面阻塞。
        
2. 网络排查
    
    1. 检查网络链路：通过 `ping`、`traceroute` 确认前端到后端服务器的网络延迟，是否有丢包。
        
    2. 检查 HTTPS 握手耗时：HTTPS 首次连接需要 TLS 握手，若耗时过长可能是证书问题或网络波动。
        
    3. 检查接口返回数据量：若返回数据过大（如几万条记录未分页），会导致传输耗时增加，可通过 `Content-Length` 确认。
        
3. 后端服务排查
    
    1. 查看接口日志：记录请求进入后端的时间、处理开始 / 结束时间，定位是否在后端业务逻辑中耗时（如复杂计算、第三方服务调用）。
        
    2. 检查线程池状态：若后端服务线程池满（如 Tomcat 线程耗尽），请求会排队等待，导致响应慢（可通过监控工具如 JConsole 查看线程状态）。
        
    3. 排查缓存是否失效：若接口依赖缓存（如 Redis），缓存失效后直接查数据库可能导致耗时增加。
        
4. 数据库排查
    
    1. 查看 SQL 执行耗时：通过数据库慢查询日志（如 MySQL 的 `slow_query_log`）定位是否有慢 SQL。
        
    2. 分析执行计划：用 `explain` 检查 SQL 是否走索引、是否全表扫描、关联查询是否低效。
        
    3. 检查数据库负载：CPU、内存、IO 是否过高（如锁等待、表空间满），导致 SQL 执行阻塞。
        
5. 分布式链路追踪
    
    1. 若为微服务架构，通过链路追踪工具（如 SkyWalking、Zipkin）查看请求在各服务间的流转耗时，定位瓶颈服务。