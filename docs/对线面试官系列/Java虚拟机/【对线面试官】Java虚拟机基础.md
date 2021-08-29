![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vnpt15j30ku112jxg.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vinr46j30ku112n3e.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1veyc6aj30ku1127bb.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vgno41j30ku1120zr.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vlrow2j30ku112jzo.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vh86mtj30ku112dmf.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1ve012hj30ku112qam.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vi6zcuj30ku112ahn.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vk0nkjj30ku112dnt.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vfokayj30ku112qaj.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vjiut7j30ku1120zp.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vmwntlj30ku112tgn.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx1vkwufqj30ku112agj.jpg)

总结下今天的内容，并画了个图（**三连三连！三连可以加快更新速度！**）：

- Java跨平台因为有JVM屏蔽了底层操作系统
- Java源码到执行的过程，从JVM的角度看可以总结为四个步骤：编译->加载->解释->执行
  - 「编译」经过 语法分析、语义分析、注解处理 最后才生成会class文件
  - 「加载」又可以细分步骤为：装载->连接->初始化。装载则把class文件装载至JVM，连接则校验class信息、分配内存空间及赋默认值，初始化则为变量赋值为正确的初始值。连接里又可以细化为：验证、准备、解析
  - 「解释」则是把字节码转换成操作系统可识别的执行指令，在JVM中会有字节码解释器和即时编译器。在解释时会对代码进行分析，查看是否为「热点代码」，如果为「热点代码」则触发JIT编译，下次执行时就无需重复进行解释，提高解释速度
  - 「执行」调用系统的硬件执行最终的程序指令

![](https://tva1.sinaimg.cn/large/008i3skNgy1grx29rcwc5j30zq0gl0yt.jpg)



《对线面试官》系列目前已经连载**26**篇啦！**有深度风趣**的系列！

- [【对线面试官】Java注解](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483821&idx=1&sn=e9003410a8d3c8a092de0c4d2002bedd&scene=21#wechat_redirect)
- [【对线面试官】Java泛型](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483823&idx=1&sn=cc887dc2c7e68a69e8d4d141c2ca9b5e&scene=21#wechat_redirect)
- [【对线面试官】 Java NIO](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483854&idx=1&sn=aa450a03ac0d6e8cf12cf13d4719ede3&scene=21#wechat_redirect)
- [【对线面试官】Java反射 && 动态代理](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483893&idx=1&sn=af51e626f2c2baec8cae4f4a15425957&scene=21#wechat_redirect)
- [【对线面试官】多线程基础](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483918&idx=1&sn=ab8550bb284edcf7cf0c6d0b41e0c2f6&scene=21#wechat_redirect)
- [【对线面试官】 CAS](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483977&idx=1&sn=1a3aa3aec27073aa3b422bc41d7fbe2d&chksm=fdf0ea16ca8763005aff64834eeb7bef08bf4ee2d8febb7e8d4d8e5d1542336e13fac71e2881&scene=21&cur_album_id=1657204970858872832#wechat_redirect)
- [【对线面试官】synchronized](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247483980&idx=1&sn=c9b620834adb889ad8ccedb6afdcaed1&scene=21#wechat_redirect)
- [【对线面试官】AQS&&ReentrantLock](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484035&idx=1&sn=ccaec352e192f1fd40020d9a984e9461&chksm=fdf0eadcca8763ca5c44bd19118fd00e843c163deb40cda444b3fc08430c57760db15eca1ea6&scene=21&cur_album_id=1657204970858872832#wechat_redirect)
- [【对线面试官】线程池](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484036&idx=1&sn=75e9e93a82a811e9c71b8127cf7ac677&chksm=fdf0eadbca8763cd7ab74757f9472d061c0244d2373a1ea85b1cbc833941441fdb1e91ead5b4&scene=21&cur_album_id=1657204970858872832#wechat_redirect)
- [【对线面试官】ThreadLocal](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484118&idx=1&sn=9526a1dc0d42926dd9bcccfc55e6abc2&scene=21#wechat_redirect)
- [【对线面试官】CountDownLatch和CyclicBarrier](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484363&idx=1&sn=743dcdfb84f83cfc38882407f87c7c6d&chksm=fdf0eb94ca87628296d86d16769f25e10acd052bcd78f4a4608f4218e4948aff610b04a41f60&token=960279204&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】为什么需要Java内存模型？](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484535&idx=1&sn=af9676b6defcfd862db297b1ee3f4aea&chksm=fdf0ec28ca87653e84aedd1b5db916d776c46c158afb727467d42a941af85d948046da24b5e5&token=1812893887&lang=zh_CN#rd)
- [【对线面试官】Java从编译到执行，发生了什么？](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484557&idx=1&sn=6fb103a2a322effc564fbb04c3b93a6c&chksm=fdf0ecd2ca8765c4eacc22e54b4bc57888555efee99f1c7e57ee611e07d220b35b2aa658a4ca&token=830702193&lang=zh_CN#rd)
- [【对线面试官】List](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484253&idx=1&sn=532db3941f47502582295cbb003f753d&chksm=fdf0eb02ca8762145c66b33bbb429399f1f0f27b31c22f7cf6c693c235e9a7cffdafb6ce2fdc&token=57394744&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】Map](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484280&idx=1&sn=87cfede653dabc26c909823a1dafd615&chksm=fdf0eb27ca876231095ff99f0b3e30acd7b2ee4cdc7ddb16da0bb6a3b02f531e27324059cf58&token=100834666&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】SpringMVC](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484064&idx=1&sn=3a59514a8262ab61036fc89cf0b0a27e&chksm=fdf0eaffca8763e90002ce1daf365f717a4bda3e50878f65943f52d14bee78fc65e837ef32f9&token=664255414&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】Spring基础](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484147&idx=1&sn=ef282cd54351436fc33c47534b4c2ac1&chksm=fdf0eaacca8763ba9b6c69acdba6b0ae8801405c98295842a0b5d891fe80246d76a2a0470bea&token=1998524575&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】SpringBean生命周期](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484187&idx=1&sn=8f831c40dca9b2a57fdfbd051e4eab44&chksm=fdf0eb44ca87625253ea831471110860d3f27e04488b2748ba90ad442b079aca3d6b95d31bbe&token=1998524575&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】Redis基础](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484227&idx=1&sn=4a124a2dd5ef6ce062abdadf247b5cff&chksm=fdf0eb1cca87620a8679473dfdd50421eb6ccba2459a7cb59ae1652138f7bb508558f3d4649e&token=57394744&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】Redis持久化](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484323&idx=1&sn=c3306b3f9abb6f880e2672f169202a42&chksm=fdf0ebfcca8762eaf9b4873e79cd3445857b1f4476a854acdf9c19fb81e1a02146c65cff5078&token=610975656&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】Kafka基础](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484387&idx=1&sn=5bb2ba58776e65f53b091a4bcdb73755&chksm=fdf0ebbcca8762aadc359066ecd70274fa23ee846f9ba9114017402dcbed415f25f97d3020a6&token=1131755397&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】使用Kafka会考虑什么问题？](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484411&idx=1&sn=9c4aaeb44f4d9e09cc796805ada29921&chksm=fdf0eba4ca8762b234c3f101bb88c5d134554a831cbf4e80b08dc0bfa829e363a4e1e49a8b50&token=649285067&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】MySQL索引](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484431&idx=1&sn=17b9e88233282469481e214a0cd2dc56&chksm=fdf0ec50ca8765460a20af19101855c859a6350a8dfd6680e7f47c2e73f03de48288184a1bf3&token=310857929&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】MySQL 事务&&锁机制&&MVCC](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484480&idx=1&sn=3571b89575e8c37c114c9f290b953a1c&chksm=fdf0ec1fca87650913e6673a453d0ba1614341433aa67dd9977fef7231a3d825f7da4e4a132a&token=1651214636&lang=zh_CN&scene=21#wechat_redirect)
- [【对线面试官】MySQL调优](https://mp.weixin.qq.com/s?__biz=MzU4NzA3MTc5Mg==&mid=2247484508&idx=1&sn=4e81d365409bf32c08e4ea985e3ca593&chksm=fdf0ec03ca876515d59c49f033cf83f72b62fafe356e678b4d162ad3623d31bf60fb6620176f&token=336229290&lang=zh_CN&scene=21#wechat_redirect)

