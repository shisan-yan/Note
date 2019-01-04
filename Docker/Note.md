什么是容器

​	Namespace

​	docker使用的linux的Namespace机制对容器内的进程空间做了手脚，让docker内的进程使用认为自己的PID为1，既看不到宿主机里真正的进程空间，也看不到其他PID Namespace里的具体情况。其实这只是一种"障眼法"，除了刚才的PID Namespace，Linux操作系统还提供了Mount、UTFS、IPC、Network和User这些Namespace，用来对各种不同的进程上下文进行“障眼法”操作<比如，Mount Namespace，用于让被隔离进程只能看到当前Namespace里的挂载点信息；Network Namespace，用于让被隔离进程看到当前Namespace里的网络设置>

​	这，其实就是Linux容器最近本的实现原理了。

​	所谓的docker容器其实就是在创建容器进程时，指定了这个进程所需要启用的一组Namespace参数。这样，容器就只能"看"到当前Namespace所限定的资源、文件、设备、状态或者配置。对于宿主机以及其他不相关的程序，它就完全看不到了

​	所以说，容器，其实就是一种特殊的进程而已

​	用户运行在容器里的应用程序，跟宿主机上的其他进程一样，都有宿主机操作系统统一管理，只不过这些被隔离的进程拥有额外设置的Namespace参数。而Docker在这里扮演的角色，更多的是旁路式的辅助和管理工作

​	Namespace技术实际上修改了应用进程看待整个计算机"视图"，即它的"视线"被操作系统做了限制，只能"看到"某些指定的内容

容器与虚拟机的区别

![1546492200567](F:\gitHub\Note\Docker\Image\容器与虚拟机的区别.png)

​	docker跟虚拟机不同，使用docker的时候，并没有一个真正的"Docker容器"运行在宿主机里面。Docker项目帮助用户启动的还是原来的应用进程，只不过在创建这些进程时，Docker为他们加上了各种各样的Namespace参数。这时，这些容器就会觉得自己是各自PID Namespace里的第1号进程，只能看到Mount Namespace里挂载的目录和文件，只能访问到各自network namespace里的网络设备，就像是运行在一个个"容器"里面，与世隔绝。

​	"敏捷"和"高性能"是容器相较于虚拟机最大的优势，也是它能够在PasS这种更颗粒度的资源管理平台上大行其道的重要原因

Cgroups



什么是Docker的单进程

​	比如我们有一个Docker容器，里面只有一个java进程在运行；但实际上我们连接到Docker容器中时会发现，我们还可以在Docker里面执行ping命令、netstat命令。那么为什么又说Docker是单进程的呢？

​	Docker的单进程是指，只有一个进程是收到Docker控制的，而其他运行的那些进程是不受Docker控制的。所以Docker的单进程不是时候只能运行一个进程，而是只有一个进程是可控的

