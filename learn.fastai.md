*2019/4/8*

# 概览

通过浏览，扫清了边边角角，确定要看的部分包括：

[主教程](https://course.fast.ai/index.html)

[进阶教程·前沿技术](http://course18.fast.ai/part2.html)

[数学基础](https://github.com/fastai/numerical-linear-algebra/blob/master/README.md)

[机器学习](http://course18.fast.ai/ml.html)

可以参考的资源有：

[github](https://github.com/fastai/fastai/blob/master/README.md)

[文档](https://docs.fast.ai/)

[论坛](https://forums.fast.ai/)

# [主教程](https://course.fast.ai/index.html)

## Getting started

今天申请提高aws实例数量限额还没得到回复，所以先在83上做了GPU部分的实验，可以看到，GPU在深度学习方面要比CPU快得多。

![alt text](https://github.com/RayXu14/Tools/blob/master/images/CPUvsGPU.PNG)

这里我还接触了jupyter notebook/lab里面[%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)这个命令。似乎是一个计时的方法。以后我可以考虑写程序时候计时变为标准化操作，这样可以得知各部分跑的时间，以进行优化。

## Server setup
本来谷歌那个是最好的，有2000美元免费额度。可惜中国大陆境内没有业务，而且GCP我不熟。我怕节外生枝，觉得还是在AWS EC2烧钱稳妥一点。

---
*2019/4/9*

配好了位于东京的p2.xlarge实例，和教程不同的是，我为了方便，采用了我自己的bypass-all安全组策略，并且为了省钱，使用spot实例，不过在竞价方面把最高价设定为和按需使用的价格一样高。

至于jupyter notebook我用的是jupyter lab且在windows mobaXterm采用使用远程连接，具体可看
https://blog.csdn.net/cc1949/article/details/79095494

### 翻车

用spot实例**翻车**了，被terminated，而且拿不回来。

重开，这次不用spot，但是仍然bypass-all（方便jupyter lab远程使用），而且fastai的那个环境使用conda create另开，避免影响base。

又发现一个**问题**， 存储空间75G居然才刚好够放环境？！快点删没用的东西，尤其是在conda info --env里面找一找，不然悔之晚矣。也许本次翻车是这个原因。

## Lessons
