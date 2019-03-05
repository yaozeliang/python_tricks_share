## Python进阶之路总结 ##

大家好，我的<< Python进阶之路>>到这一期就到此为止了，和 <<Python 基础起步>>不同，在掌握了一些基础知识后，我在这一期里同步和大家分享了我的学习心得，一共主要关注了以下几个知识点：

 - [Python 进阶之路 (一) List 进阶方法汇总][1]
 - [Python 进阶之路 (二) Dict 进阶宝典][2]
 - [Python 进阶之路 (三) Tuple元组使用指南][3]
 - [Python 进阶之路 (四) 先立Flag, 社区最全的Set用法集锦][4]
 - [Python 进阶之路 (五) map, filter, reduce, zip 一网打尽][5]
 - [Python 进阶之路 (六) 九浅一深 lambda，陈独秀你给我坐下！][6]
 - [Python 进阶之路 (七) 隐藏的神奇宝藏：探秘Collections][7]
 - [Python 进阶之路 (八) 最用心的推导式详解][8]
 - [Python 进阶之路 (九) 再立Flag, 社区最全的itertools深度解析（上）][9]
 - [Python 进阶之路 (十) 再立Flag, 社区最全的itertools深度解析（中）][10]
 - [Python 进阶之路 (十一) 再立Flag, 社区最全的itertools深度解析（下）][11]

现在看来，除了标题起的比较活泼外，考虑到我也是小白，内容上我自认为已经是干货满满了，如果你打开任何一篇文章，相信你会发现我并没有夸大，没有任何一篇文章是蜻蜓点水，一带而过，对得起花时间观看文章的每一位朋友，从2月4日第一遍笔记起，22天更新了11篇，应该说高产似母猪了~

今天是进阶之路的最后一篇文章，然而像标题所示，尾声即是开始，现在我们中午掌握了python的基础知识，哪怕可能不够熟练，但是足够开始新的征程了！

我决定新的旅途让我们从pandas开始，原因有很多，我会在结尾和大家说明，现在还是老规矩，我将和大家分享我在学习过程中发现的一些非常不错的资源，没有它们的帮助不论如何我是写不出这些文章的。


## 优秀资源分享 ##

在我的Python基础起步系列的最后，我分享了很多优秀资源给大家，这回也不例外，在此我将把写文章期间参考的非常优秀的资源分享给大家，让我们一起进步：

> 综合教程文章 / 视频：

 - [Corey Schafer的youtube视频教程][12],个人认为是最好的视频教程，需要翻墙
 - [Py4e][13]，密歇根大学的教程，有详细PPT 可以下载
 - [RealPython][14] 由浅入深来学习，我最爱闲逛的网站，我的文章很多是受上面的教程启发
 - [DataCamp][15] 很棒的免费教程，付费部分暂不需要
 - [Tutorial Point][16]不错的免费教程，个人感觉比w3school强
 - [GeeksforGeeks][17]非常棒的综合Python教程平台，我目前碰到的几乎所有问题，都会在上面找到答案
 - [Pythonforbeginers][18]非常不错的起步教程，界面简单明了
 - [廖雪峰老师的教程][19]，不用多说，名声很大了


> 进阶文档
 - [Python设计模式及实用方法][20]
 - [Intermediate Python][21]一个很火的python技巧文档


> 在线测试
 - [onlinegdb][22] 一个非常好用的在线环境，支持所有语言，做一些简单功能测试的时候很方便，有代码补充提示，可以在线保存



## 新专栏Pandas ##

我想了一段时间，不太确定要先和大家分享OOP编程思想还是Pandas，最后还是决定新建一个Pandas专栏教程，因为现在数据分析非常普遍。相关的工作岗位也很多，同时我自己在工作中应用pandas也比较多，对我而言会更容易一些。从实用性角度讲也是如此，如果你平时的工作涉及到大量的 excel 或者 VBA，那么 pandas 无意是你最好的选择。

这次我计划的新专栏会和你常见的不太一样。经过我简单的个人研究，我发现pandas教程遍地都是，但是很多教程的问题如下：
 - 一次能讲完的东西，拆分成好几节课
 - 没有及时更新，很多停留在2016年或者之前
 - 缺少自己的理解和基础模型分享
 - 有些收费的课程水分很大

我这次会尽我所能，为大家带来干货版本的pandas学习心得，争取用5期达到他人10期的效果，同时会加入一些工作中遇到的，自己创建的，别人教我的小模型等。争取让我的免费分享达到别人收费的标准

当然，由于pandas是应用于数据分析的第三方包，分享时会涉及到大量图表，因此我会使用Jupyter Notebook作为环境和编辑器，每期教程后也会和大家分享源代码，这次会正式一些，直接放在 github 里，只希望得到大家的鼓励，并且真正帮助到有需要的人。

**名词解释：**
> ***有需要的人：只要你平时用 excel，就是有需要！！！***

最后想说，多谢很多朋友的鼓励支持，我作为新手注册SegmentFault并开专栏起，已经做好了迎接各种吐槽的准备，然而这种情况并没有出现，除了大家比较善良外，主要原因是没多少人关注~ 不过慢慢地，在写文章的过程中，我发现也有很多朋友和我一样，是以萌新的身份进入编程领域的。

对于这些朋友我想说，你不是一个人在战斗，在学习Python的路上战友越多，就会越轻松，大家互相分享，终会成长，我不否认世上有很多天才，但是大多数时候，一个人的智慧很难和群体抗衡。因此我希望大家可以在我的文章下多多留言，让我在修改错误的同时也能和你一起进步！

就这样，让我们下期 Pandas见


  [1]: https://segmentfault.com/a/1190000018098698
  [2]: https://segmentfault.com/a/1190000018102693
  [3]: https://segmentfault.com/a/1190000018107287
  [4]: https://segmentfault.com/a/1190000018109634
  [5]: https://segmentfault.com/a/1190000018114755
  [6]: https://segmentfault.com/a/1190000018121906
  [7]: https://segmentfault.com/a/1190000018145641
  [8]: https://segmentfault.com/a/1190000018183488
  [9]: https://segmentfault.com/a/1190000018224910
  [10]: https://segmentfault.com/a/1190000018242976
  [11]: https://segmentfault.com/a/1190000018268824
  [12]: https://www.py4e.com/
  [13]: https://www.py4e.com/
  [14]: https://realpython.com/
  [15]: https://www.datacamp.com/
  [16]: https://www.tutorialspoint.com/python/
  [17]: https://www.geeksforgeeks.org/python-programming-language/
  [18]: https://www.pythonforbeginners.com/
  [19]: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000
  [20]: https://python-3-patterns-idioms-test.readthedocs.io/en/latest/Comprehensions.html
  [21]: http://book.pythontips.com/en/latest/
  [22]: https://www.onlinegdb.com/
