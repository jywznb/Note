

### Java内存模型

3y：嗯，在讲之前还是得强调下：Java内存模型它是一种「规范」，Java虚拟机会实现这个规范。



3y：Java内存模型主要的内容，我个人觉得有以下几块吧

3y：1. Java内存模型的抽象结构

3y：2. happen-before规则

3y：3.对volatile内存语义的探讨（这块我后面再好好解释）



### Java内存模型的抽象结构

3y：嗯。Java内存模型定义了：Java线程对内存数据进行交互的规范。

3y：线程之间的「共享变量」存储在「主内存」中，每个线程都有自己私有的「本地内存」，「本地内存」存储了该线程以读/写共享变量的副本。

3y：本地内存是Java内存模型的抽象概念，并不是真实存在的。

3y：顺便画个图吧，看完图就懂了。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gs1g0xg9gfj31ju0u0wus.jpg)

3y：Java内存模型规定了：线程对变量的所有操作都必须在「本地内存」进行，「不能直接读写主内存」的变量

3y：Java内存模型定义了8种操作来完成「变量如何从主内存到本地内存，以及变量如何从本地内存到主内存」

3y：分别是read/load/use/assign/store/write/lock/unlock操作

3y：看着8个操作很多，对变量的一次读写就涵盖了这些操作了，我再画个图给你讲讲

![](https://tva1.sinaimg.cn/large/008i3skNgy1gs99k2g1muj315i0u0e3n.jpg)

3y：懂了吧？无非就是读写用到了各个操作（：

### 什么是happen-before

3y：按我的理解下，happen-before实际上也是一套「规则」。Java内存模型定义了这套规则，目的是为了阐述「操作之间」的内存「可见性」

3y：从上次讲述「指令重排」就提到了，在CPU和编译器层面上都有指令重排的问题。

3y：指令重排虽然是能提高运行的效率，但在并发编程中，我们在兼顾「效率」的前提下，还希望「程序结果」能由我们掌控的。

3y：说白了就是：在某些重要的场景下，这一组操作都不能进行重排序，「前面一个操作的结果对后续操作必须是可见的」。



面试官：...

3y：于是，Java内存模型就提出了happen-before这套规则，规则总共有8条

3y：比如传递性、volatile变量规则、程序顺序规则、监视器锁的规则...（具体看规则的含义就好了，这块不难）

3y：只要记住，有了happen-before这些规则。我们写的代码只要在这些规则下，前一个操作的结果对后续操作是可见的，是不会发生重排序的。



### volatile

3y：嗯，volatile是Java的一个关键字

3y：为什么讲Java内存模型往往就会讲到volatile这个关键字呢，我觉得主要是它的特性：可见性和有序性(禁止重排序)

3y：Java内存模型这个规范，很大程度下就是为了解决可见性和有序性的问题。



### 原理，volatile这个关键字是怎么做到可见性和有序性的

3y：Java内存模型为了实现volatile有序性和可见性，定义了4种内存屏障的「规范」，分别是LoadLoad/LoadStore/StoreLoad/StoreStore

3y：回到volatile上，说白了，就是在volatile「前后」加上「内存屏障」，使得编译器和CPU无法进行重排序，致使有序，并且写volatile变量对其他线程可见。

3y：Java内存模型定义了规范，那Java虚拟机就得实现啊，是不是？



面试官：...

3y：之前看过Hotspot虚拟机的实现，在「汇编」层面上实际是通过Lock前缀指令来实现的，而不是各种fence指令（主要原因就是简便。因为大部分平台都支持lock指令，而fence指令是x86平台的）。

3y：lock指令能保证：禁止CPU和编译器的重排序（保证了有序性）、保证CPU写核心的指令可以立即生效且其他核心的缓存数据失效（保证了可见性）。

面试官：...



### volatile和MESI协议关系

3y：它们没有直接的关联。

3y：Java内存模型关注的是编程语言层面上，它是高维度的抽象。MESI是CPU缓存一致性协议，不同的CPU架构都不一样，可能有的CPU压根就没用MESI协议...

3y：只不过MESI名声大，大家就都拿他来举例子了。而MESI可能只是在「特定的场景下」为实现volatile的可见性/有序性而使用到的一部分罢了（：

面试官：...



3y：为了让Java程序员屏蔽上面这些底层知识，快速地入门使用volatile变量

3y：Java内存模型的happen-before规则中就有对volatile变量规则的定义

3y：这条规则的内容其实就是：对一个 volatile 变量的写操作相对于后续对这个 volatile 变量的读操作可见

3y：它通过happen-before规则来规定：只要变量声明了volatile 关键字，写后再读，读必须可见写的值。（可见性、有序性）



### 总结

**为什么存在Java内存模型**：Java为了屏蔽硬件和操作系统访问内存的各种差异，提出了「Java内存模型」的规范，保证了Java程序在各种平台下对内存的访问都能得到一致效果

**Java内存模型抽象结构**：线程之间的「共享变量」存储在「主内存」中，每个线程都有自己私有的「本地内存」，「本地内存」存储了该线程以读/写共享变量的副本。线程对变量的所有操作都必须在「本地内存」进行，而「不能直接读写主内存」的变量

**happen-before规则**：Java内存模型规定在某些场景下（一共8条），前面一个操作的结果对后续操作必须是可见的。这8条规则成为happen-before规则

**volatile**：volatile是Java的关键字，修饰的变量是可见性且有序的（不会被重排序）。可见性由happen-before规则完成，有序性由Java内存模型定义的「内存屏障」完成，实际HotSpot虚拟机实现Java内存模型规范，汇编底层通过Lock指令来实现。





参考资料：

- https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&tempkey=MTEyMF9ybkFGTjlJWlZYOEhXMjMvSkhHd19WaTYzMVlqdDZWYnVoZ1lhclJzOXlBTGQ5TDVVelFMcmticlNmSUljd09JTExLRk1Yb0ZTOTVJVi0yXzhZcFNSMjJkd0dGX2xrMWhUN0FJbUl1TTN1bkNkZzhSNWJXdFFueE1oUEVBTmRudlp6X0N1ZUtWNy1ibVozNkE2bTl5T1FVYmhFU1ZRMkQzdU9tTzhnfn4%3D&chksm=6bd75ba75ca0d2b1d384e840405aef7fc58384e5768966ba3bba73f3e97b1695d2461d443a39#rd
- 《深入理解Java虚拟机》
- 《Java并发编程的艺术》
- 《Java并发编程实战》
- https://blog.csdn.net/u011080472/article/details/51337422  【深入理解JVM】：Java内存模型JMM
- https://blog.csdn.net/qq_26222859/article/details/52235930 volatile与lock前缀指令
- https://zhuanlan.zhihu.com/p/29881777  Java内存模型（JMM）总结
- https://zhuanlan.zhihu.com/p/56191979  漫画：什么是volatile关键字？
- https://www.zhihu.com/question/296949412  既然CPU有缓存一致性协议（MESI），为什么JMM还需要volatile关键字？





