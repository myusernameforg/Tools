---
# 结论
框架过于细化，操作订得太死，不建议使用。
---
*2019/4/8*

# 概览

通过浏览，扫清了边边角角，确定要看的部分包括：

[主教程](https://course.fast.ai/index.html)

[进阶教程·前沿技术(18年版本，新版未发布)](http://course18.fast.ai/part2.html)

[数学基础(fast.ai的理念是让普通人可以做AI，所以他们的教程比较易懂，但不会深入)](https://github.com/fastai/numerical-linear-algebra/blob/master/README.md)

[机器学习(fast.ai的理念是让普通人可以做AI，所以他们的教程比较易懂，但不会深入)](http://course18.fast.ai/ml.html)

可以参考的资源有：

[github](https://github.com/fastai/fastai/blob/master/README.md)

[文档](https://docs.fast.ai/)

[论坛](https://forums.fast.ai/)

# [主教程](https://course.fast.ai/index.html)

## Getting started

今天申请提高aws实例数量限额还没得到回复，所以先在83上做了GPU部分的实验，可以看到，GPU在深度学习方面要比CPU快得多。

![alt text](https://github.com/RayXu14/Tools/blob/master/img/CPUvsGPU.png)

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
写下我的收获。由于我事先有基础，已知则不论。

###  idea
* 可以利用google image的数据做一个北大识花app
* Jereny Howard认为强化学习对大多数人都显然没有鸟用，迁移学习才是正道。
* Jereny希望把fast.ai做成不需要coding的软件，让普通人都能够做人工智能。
* Jereny使用随机森林算法寻找最佳超参数，这是AutoML的内容。

### 底层工具
* jupyter的工具确实不会清除之前占用的显存。notebook里面的垃圾回收：令模型=None，然后gc.collect()即可，实测在需要用的时候，pytorch就会延迟回收显存。这样可以避免重开notebook的麻烦。
* fp16有时候结果还更好

### 数据处理
* 数据处理类
    * DataBlock的分离机制我还挺喜欢的。DataBlock的更多作用：
      * 指定预处理
      * 通过制定类别/浮点列表决定是回归问题还是分类问题
    * DataBunch类基于pytorch的DataLoader。
* 迁移学习是“永恒不变的热点”，没有理由不使用预训练数据。
* 没有理由不利用全部数据。

### 调模型
* 建议先用小数据尝试不同方法的优劣，再训全部数据集，以节省时间。
* 在fastai库里面，最好的措施已经是默认值，除了weight decay，为了避免少数情况无法收敛，默认是0.1，最佳其实是0.01。
* train loss和valid loss的关系
    * train loss > valid loss 意味着欠拟合 ->可以增加训练轮次，减小后面的学习率，实在不行只能降低正则化。
    * train loss < valid loss 不意味着过拟合，准确率下降才意味着过拟合。
* loss散度（波动范围）很大时，参数要么很小要么很大，无法继续训，要从头调学习率再来。
* 迁移学习要用discriminate learning rate才能取得如此优异的结果。
* 寻找最佳学习率
    * 解冻前：最大下降斜率处为选择的（最大）学习率，
    * 解冻后：为（最小）学习率，难找的就用最低点除以10（事实上解冻前也是这么选的），
    最大学习率为原来的1/5到1/10
* Adam比SGD好，是momentum+RMSprop。动态学习率，然而还是需要设置学习率。还是得设置学习率退火。不过learner会代劳这些所有事情。
momentum就是动量加权（移动平均梯度），在SGD函数里面一般设置为0.9。
* fit_one_cycle里面学习率先升高后降低时针对总batch数的，而不是每个epoch都升降一次。
    * 这个训练方法能使模型稳定训练并获得更好的泛化能力：一开始参数不对，梯度可能带偏；后期只能微小变化，不然过头。
    * fit_one_cycle的训练loss先大后小是好现象，意味着找到了合适的学习率。loss一开始就向下的话，需要担心陷入局部最优，影响泛化能力。可能需要调大学习率，挑出局部最优。
* 正则化。这些fastai库可以代劳，设置一下参数就好。
    * weight decay（在我看来L2正则化的权重，但他说是做同一件事情的两个方面）
        * 避免过拟合，不一定要使用较少参数。可以用weight decay。因为参数很小的时候作用就可以很小的。
        * 未有0.1而不好的情况，0.1不需要early stop
        * 少数情况0.1难以拟合，需要0.01，0.01则需要early stop
    * dropout。pytorch dropout的时候会把比例除回来，确保量纲一致。
    * batch normalization
        * fast.ai实现的是移动平均的版本
    * data augmentation

### CV
* 用小规模/小图片做预训练，可以得到更快的训练速度和更好的泛化能力。

### GAN
* train gan的时候，由于loss大致不会有大的偏差，所以很难判断结果好坏，基本只能按时查看一下生成的结果。

### 遗留问题
discriminate learning rate 最佳递增减幅度为2.6？对BERT是如此吗？
