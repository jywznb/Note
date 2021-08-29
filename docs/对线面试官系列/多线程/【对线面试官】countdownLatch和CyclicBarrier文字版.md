3y：淦，已经熬到了第18面了

3y：哎，最近真的忙，好久都没面试了

3y：最近也没有准备了，不知道会不会就止步于此

3y：算了，不想了，淦它丫的！



3y：面试官你好，请问面试可以开始了吗？

面试官：嗯，开始吧

面试官：今天来聊聊并发相关的问题吧？

3y：嗯？你说

面试官：我现在有个场景：现在我有50个任务，这50个任务在完成之后，才能执行下一个「函数」，要是你，你怎么设计？

3y：嗯，我想想哈。

3y：可以用JDK给我们提供的线程工具类，CountDownLatch和CyclicBarrier都可以完成这个需求。

3y：这两个类都可以等到线程完成之后，才去执行某些操作

面试官：那既然都能实现的话？那CountDownLatch和CyclicBarrier有什么什么区别呢？

3y：主要的区别就是CountDownLatch用完了，就结束了，没法复用。而CyclicBarrier不一样，它可以复用。

面试官：那如果是这样的话，那我多次用CountDownLatch不也可以解决问题吗？

3y：....

面试官：要不今天面试就到这里就结束了？你有什么想问我的吗？

3y：....

3y：念在我发了这么多次红包的份上，要不来讲讲为什么这次把我挂了？

面试官：是这样的，我提出了个场景，它确实很像可以用CountDownLatch和CyclicBarrier解决

面试官：但是，作为面试者的你可以尝试向我获取更多的信息

面试官：我可没说一个任务就用一个线程处理哦

面试官：放一步讲，即便我是想考察CountDownLatch和CyclicBarrier的知识

面试官：但是过程也是很重要的：我会看你思考的以及沟通的过程

3y：...

面试官：你提到了CountDownLatch和CyclicBarrier这些关键词，不能就直接就抛给我

面试官：我是希望你能讲下什么是CountDownLatch和CyclicBarrier分别是什么意思

面试官：比如说：CountDownLatch和CyclicBarrier都是线程同步的工具类

面试官：CountDownLatch允许一个或多个线程一直等待，直到这些线程完成它们的操作

面试官：而CyclicBarrier不一样，它往往是当线程到达某状态后，暂停下来等待其他线程，等到所有线程均到达以后，才继续执行

面试官：可以发现这两者的等待主体是不一样的。

面试官：CountDownLatch调用await()通常是主线程/调用线程，而CyclicBarrier调用await()是在任务线程调用的

面试官：所以，CyclicBarrier中的阻塞的是任务的线程，而主线程是不受影响的。

3y:....

面试官：简单叙述完这些基本概念后，可以特意抛出这两个类都是基于AQS实现的

面试官：反正你在前几次面试的过程中都说过AQS了，我知道你是懂的，你可以抛出来

面试官：至于问不问，我可能会问，也可能会不问嘛，但问的概率还是挺大的。

3y：草，学到了

面试官：其实我在问CountDownLatch和CyclicBarrier有什么什么区别的时候，你就可以围绕源码可以给我讲讲

面试官：而不是随便说CountDownLatch是一次性的，而CyclicBarrier可在完成时重置进而重复使用就来敷衍我

面试官：比如说CountDownLatch你就可以回答：前面提到了CountDownLatch也是基于AQS实现的，它的实现机制很简单

面试官：当我们在构建CountDownLatch对象时，传入的值其实就会赋值给 AQS 的关键变量state

面试官：执行countDown方法时，其实就是利用CAS 将state 减一

面试官：执行await方法时，其实就是判断state是否为0，不为0则加入到队列中，将该线程阻塞掉（除了头结点）

面试官：因为头节点会一直自旋等待state为0，当state为0时，头节点把剩余的在队列中阻塞的节点也一并唤醒。

面试官：是不是经过解释后，就会让人觉得清晰很多？

3y：你说得对

面试官：回到CyclicBarrier上，代码也不难，重点就是await方法

面试官：从源码不难发现的是，它没有像CountDownLatch和ReentrantLock使用AQS的state变量，而CyclicBarrier是直接借助ReentrantLock加上Condition 等待唤醒的功能 进而实现的

面试官：在构建CyclicBarrier时，传入的值会赋值给CyclicBarrier内部维护count变量，也会赋值给parties变量（这是可以复用的关键）

面试官：每次调用await时，会将count -1 ，操作count值是直接使用ReentrantLock来保证线程安全性

面试官：如果count不为0，则添加则condition队列中

面试官：如果count等于0时，则把节点从condition队列添加至AQS的队列中进行全部唤醒，并且将parties的值重新赋值为count的值（实现复用）

面试官：是不是不难？

面试官：再简单总结下：CountDownlatch基于AQS实现，会将构造CountDownLatch的入参传递至state，countDown()就是在利用CAS将state减-1，await()实际就是让头节点一直在等待state为0时，释放所有等待的线程

面试官：而CyclicBarrier则利用ReentrantLock和Condition，自身维护了count和parties变量。每次调用await将count-1，并将线程加入到condition队列上。等到count为0时，则将condition队列的节点移交至AQS队列，并全部释放。

面试官：等你讲完这一套东西时，时间已经过了好几分钟了

面试官：一般我也不会用一个问题探讨太久，觉得还可以就问下一个问题了。

3y：学到了

3y：我都面了这么多次了，要不再给我个机会吧？这次发挥失常了

面试官：如果「在看」过100，那继续约你下一次面试吧

面试官：不然你就回家等通知吧



参考资料：

- https://www.jianshu.com/p/ddcc8aea4030
- https://blog.csdn.net/qq_32459653/article/details/81486757
- https://blog.51cto.com/14267003/2415153



