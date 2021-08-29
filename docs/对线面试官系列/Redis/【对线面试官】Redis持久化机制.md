![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3p8xgvoj30ku112q9s.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3qohqc6j30ku11212n.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3r9ifrdj30ku112an6.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3rqvx54j30ku112tjz.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3scczrjj30ku112dtr.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3svg27sj30ku112tl4.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tfsai6j30ku1124b9.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tptg6uj30ku112gsm.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tq4je3j30ku112jxn.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3ts50kgj30ku112ag6.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3trr7fkj30ku11243n.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tqrf58j30ku112grq.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3togohrj30ku112jwo.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tpciozj30ku112wjk.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEly1gof3tnwztvj30ku1120xn.jpg)



### 今日总结

**Redis持久化机制**：RDB和AOF



**RDB持久化**：定时任务，BGSAVE命令  fork一个子进程生成RDB文件（二进制）



**AOF持久化**：根据配置将写命令存储至日志文件中，顺序写&&异步刷盘(子线程)，重写AOF文件也是需要 fork 子进程



Redis4.0之后支持混合持久化，用什么持久化机制看业务场景


