---

---

nginx日志分析  goaccess

关于SSL

TLS套件解读

![TLS](E:\github\Note\Nginx\Image\TLS.png)

CA证书的颁发与申请

![CA证书](E:\github\Note\Nginx\Image\CA证书.png)

证书类型

![证书类型](E:\github\Note\Nginx\Image\证书类型.png)

浏览器如何确认证书是否有效

​	浏览器会向系统或者自己维护的证书机构来检查颁发证书的机构是否有效<Firefox就自己有维护自己的证书机构>，没有维护的话可以向操作系统来验证，每个操作系统也都有维护一套证书校验的数据

TLS通信过程

![通信过程](E:\github\Note\Nginx\Image\通信过程.png)

对称加密与非对称加密

自建证书搭建HTTPS网站

yum install pythn2-certbot-nginx

生成证书

certbot --ngix --ngix-server-root=/etc/nginx/ -d  www.ykyao.pub

此时会有一个交互，询问是否涉及重定向的问题。如果需要将所有的http重定向到https那就选择2，否则选择1

这样，工具就会自动把私钥、密钥生成，并且修改好了nginx的配置文件

nginx请求处理流程

![请求处理流程](E:\github\Note\Nginx\Image\请求处理流程.png)

nginx的进程结构

![进程结构](E:\github\Note\Nginx\Image\进程结构.png)

nginx采用多进程结构

​	为什么要采用多进程结构：

​		多线程结构中，线程之间是共享内存地址空间的。如果因为某个第三方模块引发了一个地址空间的段错误时，会导致整个nginx进程挂掉

worker进程是主要用来处理具体请求的。master进程负责管理workday进程。虽然nginx支持第三方模块在master进程中实现自己的功能，但是不建议也没人这么做

缓存进程是要被多个worker进程之间共享的，不但会被worker进程使用，还会被cache manager 和cache loader使用

worker跟cache进程之间通过共享内存来交互

cache manager 、cache loader、master进程都会只有一个，worker会有很多个

因为nginx采用了事件驱动以后，希望每个worker进程从头到尾占用一颗CPU，所以我们一般都会将worker进程的数量配置跟cpu核数配置相同，并且还会一一进行绑定

执行nginx -s reload<等同于 kill -SIGHUP masterPID>后，所有的worker进程跟cache进程会优雅的退出并且启动新的进程

如何重启某个worker进程

kill -SIGTEAM  workerPID

nginx进程管理：信号

![信号](E:\github\Note\Nginx\Image\信号.png)

nginx reload流程

​	向master进程发送HUP信号(reload命令)

​	master进程校验配置语法是否正确

​	master进程打开新的监听端口

​	master进程用新的配置启动新的worker子进程

​	master进程向老worker子进程发送QUIT信号

​	老worker进程关闭监听句柄，处理完当前的连接后结束进程

网络事件与nginx事件驱动

![nginx事件驱动](E:\github\Note\Nginx\Image\nginx事件驱动.png)

epoll网络事件收集器的特点

