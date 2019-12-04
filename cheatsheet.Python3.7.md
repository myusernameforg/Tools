## 学习资料
- [x] CS224预备知识
- [x] 图灵丛书《深度学习入门》·第一章 3.4.1
- [x] 图灵丛书《Python编程导论（第二版）》 3.5  1~8章
- [ ] [官方教程](https://docs.python.org/zh-cn/3.7/tutorial/index.html)
- [ ] [官方文档](https://docs.python.org/zh-cn/3.7/)

## 参考资料
1. [官方文档](https://docs.python.org/zh-cn/3.7/)
2. [PyPI](https://pypi.org/)

## 安装包（是否应该转移到shell那一块？）
```bash
pip install [-i https://pypi.tuna.tsinghua.edu.cn/simple] [-U] package1 [package2 ...]
```
* ```-i```是修改Pypi源
* ```-U```是进行升级
* ```-q```是静默安装，不建议使用
### 其他命令选项
* --version 查看版本
* uninstall 卸载包
* list 查看现有的所有包
* show [package] 查看某个包的信息
### 问题
在不使用docker的时候，偶尔遇到pip源自于local，会导致pip和python来源不同，无法协作。这时只要把.local/bin里面的pip复制成pip.bk就行，推测是旧的python local pip优先级较高的缘故。推荐一开局就安装anaconda就van事了，没有幺蛾子。

## 自带特性
* 排序
	* inplace排序
		* *list*.sort(reverse=True)
	* 取出排序id
        ```python
		>>> s = [2, 3, 1, 4, 5]
        >>> sorted(range(len(s)), key=lambda k: s[k])
        [2, 0, 1, 3, 4]
        ```
* copy库
	* 深度拷贝：和Python往往只传递了引用不同，深度拷贝将遍历以确保复制整个对象
    	* *target* = copy.deepcopy(*object*)
* random库
	* 列表随机inplace排序
    	* random.shuffle(*list*)
* breakpoint() 进入断点
	* n next
	* c continue
* [过滤器](https://www.runoob.com/python/python-func-filter.html)
	* *target* = filter(*function*, *iterable*)
		* 个人一般配合lambda使用
* [lambda表达式](https://www.cnblogs.com/wanpython/archive/2010/11/01/1865919.html)
* collections库
	* collections.Counter
* os库
	* os.environ，是个dict，用来查看和修改环境变量
		* os.environ['CUDA_VISIBLE_DEVICES'] = '0'
	* os.popen，用以执行bash命令并返回结果作为文件来读取
		* *result* = os.popen(*command_str*).read()
	* os.system，用以执行bash命令，不返回结果
		* os.system(*command_str*)
### 弃用
* pathlib.Path，要用/延长路径倒是很方便，但是如果要增加文件名后缀，我目前还不知道怎么做。考虑到可以被os库相关操作完全替代，而且也没有方便多少，遂弃用。
* logging库，有些import的包error不输出到此log，还是直接更改stdout输出，让所有输出都确保能到对应文件罢。
* collections.default_dict，和dict不同，default_dict一引用某不存在值就会自动创造键，有时候要担心这个特性带来意想不到的后果。