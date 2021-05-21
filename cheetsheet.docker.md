## 非root权限安装docker
参考了[官方教程](https://docs.docker.com/engine/security/rootless/)
1. *可能需要```sudo apt update```和```sudo apt update```
2. *可能需要检查uid数量（一般都符合），```sudo apt install uidmap```并按教程进行检查
3. ```curl -fsSL https://get.docker.com/rootless | sh```
4. 若第3步有提示操作，则重新登录并跳到第7步
5. ```export PATH=/home/xurj/bin:$PATH```
6. ```systemctl --user start docker```和```docker context use rootless```，然后重新登录生效
    * 如果没有root权限，推测每次服务器重启后都需启动。任何时候docker不通，可以尝试以上两条。
7. ```docker run hello-world```进行测试

# root权限安装nvidia-docker并在非root权限下使用
*因为没有找到nvidia-docker特别靠谱的非root安装方式，所以和刘畅要了root来安装。只是权宜之计。*
1. sudo权限[安装nvidia-docker 2.0](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-nvidia-container-toolkit)
	```bash
	$distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   		&& curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   		&& curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
	$ sudo apt-get update
	$ sudo apt-get install -y nvidia-docker2
	```
2. [sudo权限修改/etc/nvidia-container-runtime，令no-cgroups = true](https://github.com/moby/moby/issues/38729)
3. [注册用户级别的nvidia runtime](https://kien.ai/docker-rootless)，在~/.config/docker/daemon.json写入
	```
	$ {
		"runtimes": {
			"nvidia": {
				"path": "nvidia-container-runtime",
				"runtimeArgs": []
			}
		}
	}
	```
5. 重启docker并进行测试
	```bash
	$ systemctl --user restart docker1
	```

## docker命令
* ```docker push *name*:*tag*```
* ```docker build -t *name*:*tag* .  -f *dockerfile* ```
* ```docker run [--runtime=nvidia] --rm -it -w *container_working_path* -v *local_path*:*container_path* -p *local_port*:*container_port*```
* ```docker tag *origin name* *new name*```
* 在容器内运行命令```docker exec [OPTIONS] CONTAINER COMMAND [ARG...]```

## Dockerfile
* 写路径建议用绝对路径。用~会出错。
* [CMD和ENTRYPOINT区别](https://blog.csdn.net/u010900754/article/details/78526443)，推荐使用灵活性更大的CMD

## [Docker镜像的导出与拷贝](https://blog.csdn.net/yelllowcong/article/details/76731668)
用于网速过慢的情况
```bash
#将镜像存储
docker save nginx:latest > /root/docker-images/nginx.tar

#导入镜像文件
docker load --input /root/docker-images/nginx.tar

#通过符号的方式来导入
docker load < /root/docker-images/nginx.tar
```

## 对镜像网速过慢时[改用国内镜像](https://yeasy.gitbooks.io/docker_practice/install/mirror.html)
0. [直接设置docker run的参数](https://www.jianshu.com/p/df75f9b5fcf6)或者
1. 修改/etc/docker/daemon.json为
	```
	{"registry-mirrors": ["http://f1361db2.m.daocloud.io"],  # 也可以是http://hub-mirror.c.163.com或者https://docker.mirrors.ustc.edu.cn
    	    "runtimes": {
                "nvidia": {
                    "path": "nvidia-container-runtime",
                    "runtimeArgs": []
        	          }
    		        }
	}
	```
2. 执行
	```bash
	$ sudo systemctl daemon-reload
	$ sudo systemctl restart docker
	```
