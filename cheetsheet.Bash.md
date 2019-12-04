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

## git
* git commit -m '*message*'
* git checkout 切换到
	* -b *branch_name* 创建并切换到对应branch
	* [branch]
	* [commit id]
	* -t origin/[远程分支名]
* git branch
	* -d/--delete *branch_name*
	* -m/--move  *branch_oldname* *branch_newname*
	* -r/--remote
* git remote
	* -v 显示远程分支情况
	* add *name* *url* 添加远程
	* rm *name* 删除远程
* git diff
	* *blob1* *blob2*
	* *commit_id* 对比该commit版本和现在的不同
* git clone *url*
* git add ... 
	* 可以同时添加多个目标
* git log 显示commit的历史概要
* git show 显示上次commit的改动
* git pull 拉取改动
	* 从远程仓库获取所有分支
		```
		git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
		git fetch --all
		git pull --all
		```
* git stash 收起上次commit为止到现在的改动到栈中
	* list 显示收起了几次
	* pop 弹出一次收起
	* drop *id* 去除一次收起
	* clear 清除所有收起
* git status 显示上次commit到现在的改动文件情况
* git merge *另一branch* 从本branch合并另一branch
* git init *repository_name*
* git push 
