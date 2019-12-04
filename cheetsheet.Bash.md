## 学习资料
- [x] https://course.fast.ai/terminal_tutorial.html

## [命令行操作](https://linuxtoy.org/archives/bash-shortcuts.html)
* Ctrl + a ：移到命令行首
* Ctrl + e ：移到命令行尾
* Ctrl + w ：从光标处删除至字首
* Alt + f : 向前移动一个词
    
## 服务器控制相关命令
* tmux 每次回来环境都一样，并在退出登陆后仍能继续运行
    * Ctrl+B
        * % 左右分框
        * " 上下分框
* htop 显示CPU和内存情况
    * t 显示进程数
* watch -n [seconds] [command] 每隔n秒刷新一次命令显示
* nvidia-smi 显示显卡情况
* df -h 显示外存情况
* date 显示时间
* ifconfig | grep global 显示公有IP地址

### 弃用
* screen由tmux替代

## 文件操作相关命令
* grep -rn *str* *path* 搜索某个路径下所有文件和子文件夹中文件包含该字符串的行
* | 管道
* unzip [-d *path*] *zipfile* 解压zip压缩文件[到某个文件夹]
* find / -name [filename] 寻找文件名，支持正则表达式
* diff -u [-r] *path1* *path2*　比较两个文件夹[和子文件夹]中每个文件的不同
* tail -f *file* 代替不再使用的tailf

## bash脚本
* [命令行参数](https://www.runoob.com/linux/linux-shell-passing-arguments.html)
	eg. bash tf-tutorial.sh 'jupyter lab --allow-root'
* [数组及遍历](https://blog.csdn.net/redhat456/article/details/6068409)

## docker
* docker push *name*:*tag*
* docker build -t *name*:*tag* .  -f *dockerfile* 
* docker run [--runtime=nvidia] --rm -it -v *local_path*:*container_path* -p *local_port*:*container_port*
* docker tag *origin name* *new name*

### Dockerfile
* 写路径建议用绝对路径。用~会出错。
