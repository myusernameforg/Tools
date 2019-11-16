0. 以防万一fork到我的库里面了。
     * 提示：这个工具同样可以用来让没有独立IP的私人深度学习主机在局域网外可访问。
1. 在有独立IP，假设为【IP4】吧，的机器上，以aws为例[创建域名并绑定该IP](https://aws.amazon.com/cn/getting-started/tutorials/get-a-domain/)
      * 绑定的时候前部加的子域名不同，可能有的会导致DNS绑定无效，比如www和未指定。我们假设域名为your.domain，子域名为sub，当访问地址的时候显示“sub.your.domain 拒绝了我们的连接请求”而不是“找不到 sub.your.domain 的服务器 IP 地址。”时便成功了
2. [启动服务](https://github.com/RayXu14/frp/blob/master/README_zh.md)
    1. 有绑定域名独立IP的服务器端（我的在aws）
        1. 下载并解压 https://github.com/fatedier/frp/releases/download/v0.29.1/frp_0.29.1_linux_386.tar.gz (或者AMD64)
        2. 进入解压后的文件夹，对frps.ini写入文件内容，这里1104是我内网jupyterlab的端口，为了方便记忆两端设定成一样的。
            ```bash
            [common]
            bind_port = 7000
            vhost_http_port = 1104
            ```
        3. 启动
            ```bash
            ./frps -c ./frps.ini
            ```
    2. 内网的服务器端
        1. 下载并解压 https://github.com/fatedier/frp/releases/download/v0.29.1/frp_0.29.1_linux_386.tar.gz
        2. 进入解压后的文件夹，对frpc.ini写入文件内容，这里1104是我内网jupyterlab的端口。
            ```bash
            [common]
            server_addr = 【IP4】
            server_port = 7000

            [web]
            type = http
            local_port = 1104
            custom_domains = sub.your.domain
            ```
        3. 启动
            ```bash
            ./frpc -c ./frpc.ini
            ```
    3. 访问sub.your.domain:1104，确认成功
3. 映射多个web端口，这个对tensorboard很有用
    1. 有绑定域名独立IP的服务器端
        1. 在之前所说的文件夹，新建21021.ini写入文件内容。两个绑定的端口都应该和之前的区分开，因为占用了。同一个服务倒是可以绑定同样的端口。
            ```bash
            [common]
            bind_port = 21021
            vhost_http_port = 21021
            ```
        2. 启动
            ```bash
            ./frps -c ./21021.ini
            ```
    2. 内网的服务器端
        1. 在之前所说的文件夹，新建21021.ini写入文件内容，这里1104是我内网jupyterlab的端口。注意custom_domains和之前一样就好了，方便记忆。
            ```bash
            [common]
            server_addr = 【IP4】
            server_port = 21021

            [web]
            type = http
            local_port = 21021
            custom_domains = sub.your.domain
            ```
        2. 启动
            ```bash
            ./frpc -c ./21021.ini
            ```
    3. 访问sub.your.domain:21021，确认成功
   4. 开更多端口如法炮制就行。
        * 如果嫌万一重启一个一个再跑太傻，就统一用nohup输出到分别对应的log挂后台。这样就可以批量映射好多个。
4. frpc ssh的配置文件写法注意：server port和remote port不能是同一个
5. 当负载太高的时候，会导致frp这边EOF，jupyter那边zmq这样的错误。
    * 2019-11-16 开了十几个端口外加一个sudo的ssh端口，mobaxterm CPU占用显示100%，梯子在重启时显示内存不足，ssh到服务器端的时候也是很卡。
        * 当时同步盘正在进行大量网络IO。
        * 内存不足？算力不足？
        * 当只开了梯子和3个web端口的时候无此问题，CPU占用显示接近0%，内存0.26/0.45G
    
