## 学习资料
- [x] CS224预备知识
- [x] 图灵丛书《深度学习入门》·第一章 3.4.1
- [x] 图灵丛书《Python编程导论（第二版）》 3.5  1~8章
- [ ] [官方教程](https://docs.python.org/zh-cn/3.7/tutorial/index.html)
- [ ] [Python进阶](https://eastlakeside.gitbook.io/interpy-zh/)

## 参考资料
1. [官方文档](https://docs.python.org/zh-cn/3.7/)
2. [PyPI](https://pypi.org/)

## 安装包
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
	* deepcopy：深度拷贝：和Python往往只传递了引用不同，深度拷贝将遍历以确保复制整个对象
    	* *target* = deepcopy(*object*)
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
	* Counter
* os库
	* environ，是个dict，用来查看和修改环境变量
		* os.environ['CUDA_VISIBLE_DEVICES'] = '0'
	* popen，用以执行bash命令并返回结果作为文件来读取
		* *result* = os.popen(*command_str*).read()
	* system，用以执行bash命令，不返回结果
		* os.system(*command_str*)
* math库
	* log
	* exp
	* pi
	* ...
* functools库
	* [partial](https://zhuanlan.zhihu.com/p/47124891)(*func*, \*args, \*\*keywords)
		* Return a new partial object which when called will behave like func called with the positional arguments args and keyword arguments keywords. If more arguments are supplied to the call, they are appended to args. If additional keyword arguments are supplied, they extend and override keywords.
* 编码问题：[UnicodeEncodeError 'ascii' codec can't encode characters in position 0-1](https://blog.csdn.net/AckClinkz/article/details/78538462)

### 弃用
* pathlib.Path，要用/延长路径倒是很方便，但是如果要增加文件名后缀，我目前还不知道怎么做。考虑到可以被os库相关操作完全替代，而且也没有方便多少，遂弃用。
* logging库，有些import的包error不输出到此log，还是直接更改stdout输出，让所有输出都确保能到对应文件罢。
* collections.default_dict，和dict不同，default_dict一引用某不存在值就会自动创造键，有时候要担心这个特性带来意想不到的后果。

## 第三方库
* tqdm
	* tqdm
		* for *iteritem* in tqdm.tqdm(*iterable*, desc=*str*): ...
			* ```dest```是进度条左边显示的字符串
	* trange(...)
		 * 等价于tqdm.tqdm(range(...))
* [lxml](https://www.jianshu.com/p/e084c2b2b66d)
* numpy
	- [x] CS224预备知识
* pandas
	* DataFrame类
		* to_excel方法
		* to_csv方法
		* ix
		* loc
			* [在修改值时避免线性调用](https://blog.csdn.net/qq_33711966/article/details/79902276)
			* 反序：data.iloc[::-1]
		* iloc
		* [处理缺失值](https://blog.csdn.net/sinat_29957455/article/details/79017363)
		* [*ix*]
			* 取列和修改列
			* [map方法](https://blog.csdn.net/li_0891/article/details/81020657)
		* join(*dataframe*, on=*join_key*)
		* [apply(*function*, axis=1)](https://stackoverflow.com/questions/13331698/how-to-apply-a-function-to-two-columns-of-pandas-dataframe)
		* [drop_duplicates()](https://stackoverflow.com/questions/13331698/how-to-apply-a-function-to-two-columns-of-pandas-dataframe)
		* query(*query_str*)
	* read_csv(*path*, index_col=*index_col*)
	* concat(*objs*, axis : {0/’index’, 1/’columns’}, sort : bool, default None)
