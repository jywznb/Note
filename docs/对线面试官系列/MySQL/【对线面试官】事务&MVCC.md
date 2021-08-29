![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgpafiqdj30ku112qc9.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgp16y18j30ku112dkm.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgpi2b00j30ku112tdz.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgpm65s5j30ku112ahv.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgovkknsj30ku112tgk.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgozeqe8j30ku112gsg.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgpukfbbj30ku112gtk.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgpyh0vzj30ku112qaa.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgopyaisj30ku112459.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgq6abrnj30ku112ahn.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgoxc6exj30ku112wmc.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgp4gaeij30ku1120z8.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgp5ai5sj30ku112gts.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgqeqqagj30ku112ai7.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgoowu3lj30ku112qax.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgoqwiuzj30ku112wlu.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgosrn78j30ku112wm8.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgorp36xj30ku112108.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgp0awmaj30ku112dms.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgoo7h0aj30ku112108.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grdgp24zxkj30ku112thi.jpg)

一不小心就写了这么长了，总结下今天的知识点吧（**赞和转发是肯定要的，别想了，又不用钱**）：

- 事务为了保证数据的最终一致性
- 事务有四大特性，分别是原子性、一致性、隔离性、持久性
  - 原子性由undo log保证
  - 持久性由redo log 保证
  - 隔离性由数据库隔离级别供我们选择，分别有read uncommit,read commit,repeatable read,serializable
  - 一致性是事务的目的，一致性由应用程序来保证
- 事务并发会存在各种问题，分别有脏读、重复读、幻读问题。上面的不同隔离级别可以解决掉由于并发事务所造成的问题，而隔离级别实际上就是由MySQL锁来实现的
- 频繁加锁会导致数据库性能低下，引入了MVCC多版本控制来实现读写不阻塞，提高数据库性能
- MVCC原理即通过read view 以及undo log来实现

