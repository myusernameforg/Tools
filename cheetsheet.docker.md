## docker命令
* ```docker push *name*:*tag*```
* ```docker build -t *name*:*tag* .  -f *dockerfile* ```
* ```docker run [--runtime=nvidia] --rm -it -v *local_path*:*container_path* -p *local_port*:*container_port*```
* ```docker tag *origin name* *new name*```

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
