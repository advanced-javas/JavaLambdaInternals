# 1 2

lambda不是类

lambda是外部类的私有方法，通过invokeddirect指令进行调用

lambda的this是外部对象

# 3 

### Map新方法

putIfAbsent

merge ： 将新的错误信息拼接到原来的信息上

compute ： 把计算结果关联到key上

computeIfAbsent ： 只有在当前Map中不存在key值的映射或映射值为null时，才调用mappingFunction，并在mappingFunction执行结果非null时，将结果跟key关联．

### 集合新方法

...

# 4

stream并不是某种数据结构，它只是数据源的一种视图。这里的数据源可以是一个数组，Java容器或I/O channel等。

* 无存储。stream不是一种数据结构，它只是某种数据源的一个视图，数据源可以是一个数组，Java容器或I/O channel等。

* 为函数式编程而生。对stream的任何修改都不会修改背后的数据源，比如对stream执行过滤操作并不会删除被过滤的元素，而是会产生一个不包含被过滤元素的新stream。

* 惰式执行。stream上的操作并不会立即执行，只有等到用户真正需要结果的时候才会执行。

* 可消费性。stream只能被“消费”一次，一旦遍历过就会失效，就像容器的迭代器那样，想要再次遍历必须重新生成。

☆ stream 方法

# 5

stream方法

reduce

joining

collect

...

# 6

Stream流水线

1. 对于表中返回boolean或者Optional的操作（Optional是存放 一个 值的容器）的操作，由于值返回一个值，只需要在对应的Sink中记录这个值，等到执行结束时返回就可以了。

2. 对于归约操作，最终结果放在用户调用时指定的容器中（容器类型通过收集器指定）。collect(), reduce(), max(), min()都是归约操作，虽然max()和min()也是返回一个Optional，但事实上底层是通过调用reduce()方法实现的。

3. 对于返回是数组的情况，毫无疑问的结果会放在数组当中。这么说当然是对的，但在最终返回数组之前，结果其实是存储在一种叫做Node的数据结构中的。Node是一种多叉树结构，元素存储在树的叶子当中，并且一个叶子节点可以存放多个元素。这样做是为了并行执行方便。

# 8

测试方法和测试数据

性能测试并不是容易的事，Java性能测试更费劲，因为虚拟机对性能的影响很大，JVM对性能的影响有两方面：

1. GC的影响。GC的行为是Java中很不好控制的一块，为增加确定性，我们手动指定使用CMS收集器，并使用10GB固定大小的堆内存。具体到JVM参数就是-XX:+UseConcMarkSweepGC -Xms10G -Xmx10G

2. JIT(Just-In-Time)即时编译技术。即时编译技术会将热点代码在JVM运行的过程中编译成本地代码，测试时我们会先对程序预热，触发对测试函数的即时编译。相关的JVM参数是-XX:CompileThreshold=10000。

Stream并行执行时用到ForkJoinPool.commonPool()得到的线程池，为控制并行度我们使用Linux的taskset命令指定JVM可用的核数。

测试数据由程序随机生成。为防止一次测试带来的抖动，测试4次求出平均时间作为运行时间。