[toc]

## 前言

> 只有光头才能变强。

> **文本已收录至我的GitHub精选文章，欢迎Star**：[https://github.com/ZhongFuCheng3y/3y](https://github.com/ZhongFuCheng3y/3y)

上次的Mybatis反响很不错阿，本来想着150个在看就心满意足了，没想到有200多个在看，非常感谢各位的支持啦。我看评论区都想看**Spring**，三歪红着眼睛都要把这份Spring肝出来。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdpwc1h7ghj31400u0b29.jpg)

由于Spring家族的东西很多，一次性写完也不太现实。所以这一次先更新Spring「最核心」的知识点：**AOP和IOC**

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdpwiaj1o9j314f0u0qsc.jpg)

无论是入门还是面试，理解AOP和IOC都是非常重要的。在校招的时候，我没被问过Mybatis/Hibernate/Struts2这样的框架，而Spring就经常会被问到。



![](https://tva1.sinaimg.cn/large/00831rSTly1gdalri8hjcg308c08cjsq.gif)

这次的PDF共有「**142**」页，PDF涉及到的内容：

- IOC和AOP的全面讲解
- Spring事务详解和相关问题
- SpringIOC/AOP相关面试题

### 为什么要用Spring

当年的我，刚学Spring的时候，会想：『这IOC和AOP』是什么鬼玩意啊？一大堆的名词「控制反转」「依赖注入」「面向切面编程」。这是在给我搞笑的吧。



![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdq3nelo6jj307d06yq38.jpg)



在最开始学的IOC折腾了一大堆的玩意，结果就是在管「创建对象」的事？？逗我呢？？？我直接`new`一个对象出来不香吗？



有这种想法这种明显就是「**代码写得少了，想得多了**」

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdq3qwoxneg305f03pdrl.gif)



我们写代码，不仅仅是要能实现功能，实现完了以后我们还得对写过的代码「**维护**」。如果我们的代码写得很烂，那「维护」的成本就很高。



维护实际上是做什么事？

1. 出了问题需要找到是哪块的代码有问题
2. 在原有的基础上加入一些新的功能（也就是所谓的迭代）



面对重复的/繁琐的非业务代码：

1. 如果程序出了问题，我们得看吧？谁也保证不了重复的代码就没有问题。
2. 我们要想加一个新的功能，还得按原来的方式写吧？代码量会越来越多，越来越多....



上一期的「Mybatis」教程也讲到了，我们的JDBC写得好好的，运行的效率也是杠杠的。但是JDBC需要我们「自行」处理的细节太多了，我们需要在里边添加各种「重复」的代码。



我们使用ORM框架，那么我们就可以更加「专注」去实现本身的业务，ORM框架把「重复」的代码都屏蔽掉，代码维护起来就比JDBC要方便。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdqu8l1nxcj30gt0fajs9.jpg)

Spring IOC 解决的是**对象管理和对象依赖的问题**。

Spring AOP 解决的是 **非业务代码抽取的问题**。



（这里要是没基础的同学，可能看不太懂，下面再来解释解释一下应该就没问题了）

### Spring IOC

提到Spring IOC，随便去网上一搜，我们就可以看到「依赖注入」「控制反转」这两个词。



很多人都会试图要把这两个词给解释清楚，但是**太难了**，这两个词真的是太难给解释清楚了。



![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr035utjog304604lq3t.gif)



Spring IOC 解决的是**对象管理和对象依赖的问题**。本来我们的对象都是`new`出来的，而我们如果使用`Spring ` 则把对象交给「**IOC容器**」来管理。



三歪这逼搞事情了。「依赖注入」和「控制反转」都没讲，现在还来了个「IOC容器」。



「IOC容器」是什么？我们可以理解为是一个「**工厂**」，我们把对象都交由这个「工厂」来管理，包括对象的创建和对象之间的依赖关系等等。等我们要用到对象的时候，就从这个「工厂」里边取出来。



「控制反转」指的就是：本来是「**由我们自己**」`new`出来的对象，现在交给了IOC容器。把这个对象的「控制权」给「他方」了。「控制反转」更多的是一种**思想**或者说是**设计模式**，把原有由自己掌控的事交给「别人」来处理。



「依赖注入」更多指的是「控制反转」这个思想的**实现方式**：对象**无需自行创建或管理它们的依赖关系**，依赖关系将被**「自动注入」**到需要它们的对象当中去。



最简单理解「依赖注入」和「控制反转」：本来我们的对象都是「**由我们自己**」`new`出来的，现在我们把这个对象的创建权限和对象之间的依赖关系交由「IOC容器」来管理。



> 悄悄话：我个人本身是不太喜欢琢磨每个词的含义的，很多时候大佬们也很难解释清楚。如果是初学的同学，也不用太纠结每个名词的具体含义，深究下去也没有太大的必要。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr129hgztj30a00bedg8.jpg)

现在问题又来了，为什么我们要把对象给「IOC容器」来管理呢？要理解这个，我建议可以先去看看我写过的「**工厂模式**」



理论上，我们可以把「IOC容器」也当做是一个「工厂」，使用IOC的好处就是：

- **将对象集中统一管理，便于修改**
- 降低耦合度（调用方无需自己组装，也无需关心对象的实现，直接从「IOC容器」取就好了）





![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr2qo3az7g304604qac6.gif)

### IOC 需要学什么？

我们在使用Spring的时候，首先我们要学习的就是**怎么把**对象交给「IOC容器管理」



Spring提供了四种方式：

- 注解
- XML
- JavaConfig
- 基于Groovy DSL配置



总的来说：我们以XML配置+注解来装配Bean比较多，其中**注解这种方式占大部分。**



把对象放到「IOC容器」了以后，对象与对象之间是有关系的，我们需要把对象之间的依赖告诉Spring，让它来帮我们解决掉对象的依赖关系。



「对象之间的关系」别想得太复杂了。在日常开发中其实很多时候就是**A对象里边有B对象的属性**而已。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr3hzhobwj30gy0geaco.jpg)

一般来说我们会通过**构造器**或者**属性**(setting方法)的方式来注入对象的依赖



**举个例子：**日常开发中，我们很多时候用`@Component`注解标识将对象放到「IOC容器」中，用`@Autowired`注解将对象注入

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr3tarlcgj310e0meq7p.jpg)





下面这张图就很好总结了以各种方式来**对Bean的定义和注入**。

![](https://user-gold-cdn.xitu.io/2018/5/22/16387d351767a6fd?w=720&h=159&f=png&s=44762)

![](https://user-gold-cdn.xitu.io/2018/5/22/16387d35177cc21a?w=715&h=693&f=png&s=178450)



### Spring AOP

AOP： Aspect Object Programming  「面向切面编程」，听起来是不是很牛逼。



Spring AOP主要做的事情就是：「把重复的代码抽取，在运行的时候往业务方法上**动态植入**“切面类代码”」



举个例子，现在我们有以下的代码：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr4axrexzj314z0u0n46.jpg)

上面的代码其实最核心的就一行代码：「保存user对象到数据库中」

```java
session.save(user);
```

我们的数据库表肯定不止`user`一张表，对数据库的增删改也肯定不止`add()`方法一个。所以我们可以想象到：对数据库的每次操作，**都要写**「开启事务」和「关闭事务」这种代码。



这种代码对我们来说是重复的，于是我们会想把这种代码给「**抽取**」出来。



如果我们单纯用OOP（面向对象）的思想去把代码给优化掉，最终我们的效果可能是这样的：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr9jt66crj30re0ie77r.jpg)

即使这样看起来代码已经很少了，但我们细想一下会发现：`update()/delete()`方法同样也会有`aop.begin()`这样的重复代码的。



我们想要「消灭」掉这些重复代码，可以怎么做？这个时候我们应该能想到「**动态代理**」，通过动态代理，将非业务代码写在要「增强」的逻辑上。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr9olbk32j31600u0k0b.jpg)

完了以后，我们就可以通过「代理对象」去调用方法，最终屏蔽掉「重复代码」

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr9ppe6pnj319u0jeq7e.jpg)

效果可能会如下：

![](https://user-gold-cdn.xitu.io/2018/3/14/16223e1544b379b8?w=1498&h=662&f=png&s=80856)



上面是我们手动写的代理来实现对「非业务代码」的抽取，类似这样的场景会有很多：比如我们要做权限控制，要对参数进行校验等等。



Spring 支持了AOP，让我们可以不用自己「手动」去写代理对象，达到将「非业务代码」的抽取的效果。



我们可以体验一波Spring AOP是怎么弄的，跟上面的对比对比：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdr9y2c9ewj30u0165n83.jpg)

效果如下：

![](https://user-gold-cdn.xitu.io/2018/3/14/16223e15455ecca1?w=1083&h=715&f=png&s=78787)

### 再叨叨

建议：在学习IOC之前，可以先看看「工厂模式」。在学习AOP之前，可以先看看「代理模式」



理解了「工厂模式」那就知道为什么我们不再直接`new`对象，理解了「代理模式」，我们就知道Spring AOP的底层技术其实就是「动态代理」，这样学习IOC和AOP的时候，就会轻松很多。



还要一点就是不要被「名词」给吓唬了，之前不懂某个技术的时候，听别人讲一些名词，我对此完全不懂。那就可能会认为这个技术会很牛逼，其实等真正接触下来，学习完了以后，其实发现也不过如此嘛。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdrbfhitubj308c07qwel.jpg)



现在已经工作有一段时间了，为什么还来写`Spring`呢，原因有以下几个：

- 我是一个对**排版**有追求的人，如果早期关注我的同学可能会发现，我的GitHub、文章导航的`read.me`会经常更换。现在的[GitHub](https://github.com/ZhongFuCheng3y/3y)导航也不合我心意了（太长了），并且早期的文章，说实话排版也不太行，我决定重新搞一波。
- 我的文章会分发好几个平台，但文章发完了可能就没人看了，并且图床很可能因为平台的防盗链就挂掉了。又因为有很多的读者问我：”**你能不能把你的文章转成PDF啊**？“
- 我写过很多系列级的文章，这些文章就几乎不会有太大的改动了，就非常适合把它们给”**持久化**“。



基于上面的原因，我决定把我的系列文章汇总成一个`PDF/HTML/WORD/epub`文档。说实话，打造这么一个文档**花了我不少的时间**。为了防止**白嫖**，关注我的公众号回复「**888**」即可获取。



**Spring电子书，有兴趣的同学可以浏览一波。共有「142」页**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdrbi1zk6mj31ad0u0dz3.jpg)



文档的内容**均为手打**，有任何的不懂都可以直接**来问我**（公众号有我的联系方式）。



如果**这次点赞超过250**，那下周再肝一个系列出来。**想要看什么，可以留言告诉我**


