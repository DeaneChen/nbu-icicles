# 查阅=>复制=>粘贴=>跑路！

好了，如果你已经看完了绝大部分视频然后决定继续跟进这部分的记录，那可以跳过本章去看后面的内容。本章重点讲述的是如何使用相关的手册和新建一个非常帅气的工程。

在看手册的时候，一定要将原理图跟参考手册（中文手册）结合起来使用。比方说我想点亮一颗LED灯，我得先把LED灯的对应接口给找出来。
![原理图中的LED引脚](./相关图片/2-1%20原理图LED.png)

可以看到，在STM32F103C8T6这块单片机中（不是什么很高大上的东西，就是野火的这块最小系统板而已），有3个LED，分别是红色（RED）的LED1，绿色（GREEN）的LED2，蓝色（BLUE）的LED3，对应的接口分别是（LED正负极别都不会看了，如果真不会看的话Deepseek一下）：LED1负极接PA1、LED2负极接PA2、LED3负极接PA3。

也就是说，我如果想点亮其中一颗LED，就点亮LED1好了，我就必须得先找到PA1的接口位置（目前就叫接口叭，便于理解），然后让PA1输出一个点亮LED的命令。

（实际上在原理图中可以看出，在这块板子上，LED的正极是始终接3.3V的正电压，而PA1接的是LED的负极，也就是说LED不发光的原因实际上是因为LED两端的电压并没有达到正向导通电压的最低要求（1.2~4V），所以LED不发光。如果说想让他发光，用数电的理解就是给一个低电平信号就可以了。）

所以接下来，我们就该去参考手册中找PA1的位置。那么，问题来了，怎么找？

在参考手册中，第二章存储器和总线架构中有一个系统结构图（总线这种概念如果说玩过MC的格雷科技mod就很好理解了，如果没玩过的话，不推荐去玩，纯纯赤石）。然后我们就要去找对应的IO口（PA1），往PA1中写对应的代码就可以了。

![系统结构图](./相关图片/2-2%20系统结构图.png)

那么我问你，PA1在哪里？

如果你眼睛亮一点，可以看到在APB2箭头指的方向的底下，有一个叫GPIOA的东西，说明APB2是控制这个玩意儿。而这个GPIOA，就是我们要找的PA1。由于STM32引脚数量有限，一个IO所对应的接口数并不为一。换而言之，一个IO口控制的接口可以有很多个。细心点的你会发现，所有的LED都是PA开头的，也就是说，仅这个GPIOA一个IO口就可以控制3个LED的亮灭状态。

（这一块我没有看的很仔细，当时视频中的老师讲的也有点模糊，不知道咋整的就直接找出来了，照我的理解就是PA对应的就是GPIOA，PB就是GPIOB，以此类推。如果说我有说错的还请指正。）

那讲了这么多的IO又是啥个东西？相信大家都选修了C++这一门课。C语言和C++中都有一个叫做输入输出流的东西，像C语言中的常用输入函数是scanf()，常用输出函数是printf()，但是C语言的这两个函数使用起来有很多限制，比如说要 %d 之类的占位符来限定输入输出的类型。而在C++中有一个标准输入输出流，就是常用的 cin 和 cout 。这个流最好不要单纯的直接记，而是分开来记，记成 c_in 和 c_out ，一个是输入in，一个是输出out。

而在STM32中，这个GPIO指的就是通用功能输入输出端口（General Purpose Input Output），过英语四级的相信input和output是看得出来的。简单点来说，这个东西就是用来控制单片机上的各种元器件。

至于我们要如何去建立一个非常帅气的工程文件，我在这份记录的开头也已经说了，这份学习笔记更侧重的是对标准库的使用，所以教你们建立非常帅气的工程文件是不可能的啦。但是我还是会给出我常用的一个Template，也就是模板工程文件夹。以后写代码直接将这个Template文件夹拷贝到工程文件夹下就可以了，也可以复制粘贴之后重命名文件夹。随即也可以看一下我当时看野火教学视频时配置Keil5的一个笔记（相关的资料都在课外拓展学习那一块儿的[学习资料分享](https://pan.baidu.com/s/1AWn1uOjJBhCc_JUNKRK_BA?pwd=icic)中）。

考虑到有些朋友喜欢有VScode进行编译操作什么的，这里也给出Keil5如何在VScode上进行配置的[blog](https://blog.csdn.net/weixin_43576926/article/details/107736692)。
毕竟VScode的高亮看着让人还是很有写程序的欲望的。
配置好后大概长这样：
![配置好后的页面](./相关图片/2-3%20配置好后的页面.png)

（页面长这样是因为我给VScode加了皮肤，喜欢我这样的自己去找咋整的，扩展名是Noctis）

如果学到这里你觉得有点困难了，就可以考虑开始跑路了；如果你觉得还OK的话，那么在下一章中，我们就要开始跑路了——跑通我们的第一个代码。