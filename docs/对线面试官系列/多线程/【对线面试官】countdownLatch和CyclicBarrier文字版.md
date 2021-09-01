### 场景：现在我有50个任务，这50个任务在完成之后，才能执行下一个「函数」，怎么设计？

3y：可以用JDK给我们提供的线程工具类，CountDownLatch和CyclicBarrier都可以完成这个需求。

3y：这两个类都可以等到线程完成之后，才去执行某些操作

### CountDownLatch和CyclicBarrier有什么什么区别呢？

3y：主要的区别就是CountDownLatch用完了，就结束了，没法复用。而CyclicBarrier不一样，它可以复用。

面试官：那如果是这样的话，那我多次用CountDownLatch不也可以解决问题吗？

面试官：是这样的，我提出了个场景，它确实很像可以用CountDownLatch和CyclicBarrier解决

面试官：我可**没说一个任务就用一个线程处理**哦

### CountDownLatch和CyclicBarrier分别是什么意思

面试官：比如说：CountDownLatch和CyclicBarrier都是线程同步的工具类

面试官：CountDownLatch允许一个或多个线程一直等待，直到这些线程完成它们的操作

面试官：而CyclicBarrier不一样，它往往是当线程到达某状态后，暂停下来等待其他线程，等到所有线程均到达以后，才继续执行

面试官：可以发现这两者的等待主体是不一样的。

面试官：CountDownLatch调用await()通常是主线程/调用线程，而CyclicBarrier调用await()是在任务线程调用的

面试官：所以，CyclicBarrier中的阻塞的是任务的线程，而主线程是不受影响的。

面试官：简单叙述完这些基本概念后，可以特意**抛出这两个类都是基于AQS实现的**



### CountDownLatch和CyclicBarrier区别（源码）

面试官：而不是随便说CountDownLatch是一次性的，而CyclicBarrier可在完成时重置进而重复使用就来敷衍我

面试官：比如说CountDownLatch你就可以回答：前面提到了CountDownLatch也是基于AQS实现的，它的实现机制很简单

面试官：当我们在构建CountDownLatch对象时，传入的值其实就会赋值给 AQS 的关键变量state

面试官：执行countDown方法时，其实就是利用CAS 将state 减一

面试官：执行await方法时，其实就是判断state是否为0，不为0则加入到队列中，将该线程阻塞掉（除了头结点）

面试官：因为头节点会一直自旋等待state为0，当state为0时，头节点把剩余的在队列中阻塞的节点也一并唤醒。



面试官：回到CyclicBarrier上，代码也不难，重点就是await方法

面试官：从源码不难发现的是，它没有像CountDownLatch和ReentrantLock使用AQS的state变量，而CyclicBarrier是直接借助ReentrantLock加上Condition 等待唤醒的功能 进而实现的

面试官：在构建CyclicBarrier时，传入的值会赋值给CyclicBarrier内部维护count变量，也会赋值给parties变量（这是可以复用的关键）

面试官：每次调用await时，会将count -1 ，操作count值是直接使用ReentrantLock来保证线程安全性

面试官：如果count不为0，则添加则condition队列中

面试官：如果count等于0时，则把节点从condition队列添加至AQS的队列中进行全部唤醒，并且将parties的值重新赋值为count的值（实现复用）

再简单**总结下：CountDownlatch基于AQS实现，会将构造CountDownLatch的入参传递至state，countDown()就是在利用CAS将state减-1，await()实际就是让头节点一直在等待state为0时，释放所有等待的线程**

**而CyclicBarrier则利用ReentrantLock和Condition，自身维护了count和parties变量。每次调用await将count-1，并将线程加入到condition队列上。等到count为0时，则将condition队列的节点移交至AQS队列，并全部释放。**
