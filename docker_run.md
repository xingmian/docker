# docker run

创建并启动容器

`$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

1. 运行一个在后台执行的容器，同时，还能用控制台管理：
`docker run -i -t -d ubuntu:latest`

2. 运行一个带命令在后台不断执行的容器，不直接展示容器内部信息：
`docker run -d ubuntu:latest ping www.docker.com`

3. 运行一个在后台不断执行的容器，同时带有命令，程序被终止后还能重启继续跑，还能用控制台管理:
`docker run -d --restart=always ubuntu:latest ping www.docker.com`

4. 为容器指定一个名字:
`docker run -d --name=ubuntu_server ubuntu:latest`

5. 容器暴露80端口，并指定宿主机80端口与其通信(: 之前是宿主机端口，之后是容器需暴露的端口):
`docker run -d --name=ubuntu_server -p 80:80 ubuntu:latest`

6. 指定容器内目录与宿主机目录共享(: 之前是宿主机文件夹，之后是容器需共享的文件夹):
`docker run -d --name=ubuntu_server -v /etc/www:/var/www ubuntu:latest`

`-a, --attach=[]`                 登录容器（以docker run -d启动的容器）
`-c, --cpu-shares=0`           设置容器CPU权重，在CPU共享场景使用
`--cap-add=[]`               添加权限，权限清单详见：http://linux.die.net/man/7/capabilities
`--cap-drop=[]`              删除权限，权限清单详见：http://linux.die.net/man/7/capabilities
`--cidfile=""`               运行容器后，在指定文件中写入容器PID值，一种典型的监控系统用法
`--cpuset=""`                设置容器可以使用哪些CPU，此参数可以用来容器独占CPU
`-d`                                  <span style="color:#DAA520;">后台运行容器 </span>
`--device=[]`                添加主机设备给容器，相当于设备直通
`--dns=[]`                   指定容器的dns服务器
`--dns-search=[]`            指定容器的dns搜索域名，写入到容器的/etc/resolv.conf文件
`-e, --env=[]`               指定环境变量，容器中可以使用该环境变量
`--entrypoint=""`            <span style="color:#DAA520;">覆盖image的入口点</span>
`--env-file=[]`              指定环境变量文件，文件格式为每行一个环境变量
`--expose=[]`                指定容器暴露的端口，即修改镜像的暴露端口
`-h, --hostname=""`          指定容器的主机名
`-i`                                <span style="color:#DAA520;">打开STDIN，用于控制台交互</span>
`--link=[]`                  指定容器间的关联，使用其他容器的IP、env等信息
`--lxc-conf=[]`              指定容器的配置文件，只有在指定--exec-driver=lxc时使用
`-m, --memory=""`            指定容器的内存上限
`--name=""`                  指定容器名字，后续可以通过名字进行容器管理，links特性需要使用名字
`--net="bridge"`             容器网络设置，待详述
`-P`                                       <span style="color:#DAA520;">大写，随机端口映射的端口</span>
` -p`                                      <span style="color:#DAA520;">小写，指定端口映射，格式为：主机(宿主)端口:容器端口</span>
`--privileged=false`         指定容器是否为特权容器，特权容器拥有所有的capabilities
`--restart=""`               指定容器停止后的重启策略，待详述
`--rm=false`                 指定容器停止后自动删除容器(不支持以docker run -d启动的容器)
`--sig-proxy=true`           设置由代理接受并处理信号，但是SIGCHLD、SIGSTOP和SIGKILL不能被代理
`-t`                                     <span style="color:#DAA520;">在新容器内指定一个伪终端或终端</span>
`-u, --user=""`              指定容器的用户
`-v`                                    <span style="color:#DAA520;">给容器挂载存储卷，挂载到容器的某个目录</span>
`--volumes-from=[]`          给容器挂载其他容器上的卷，挂载到容器的某个目录
`-w, --workdir=""`           指定容器的工作目录
`--rm` 容器结束时自动清理其所产生的数据

### --restart 的参数：

 `--restart=no`：<span style="color:#3333;">容器退出时不重启</span>
 `--restart=on-failure`：<span style="color:#3333;">容器故障退出（返回值非零）时重启</span>
 `--restart=always`：<span style="color:#3333;">容器退出时总是重启</span>

### -P 端口暴露

1. host上5000号端口，映射到容器暴露的80端口：
`docker run -d -p 5000:80 training/webapp`

2. host上127.0.0.1:5000号端口，映射到容器暴露的80端口
` docker run -d -p 127.0.0.1:5000:80 training/webapp`

3. host上127.0.0.1:随机端口，映射到容器暴露的80端口
`docker run -d -p 127.0.0.1::80 training/webapp`

4. 绑定udp端口
`docker run -d -p 127.0.0.1:5000:5000/udp training/webapp`

### 网络配置

`--net=bridge`：     <span style="color:#3333;">使用docker daemon指定的网桥</span>
`--net=host`：         <span style="color:#3333;">容器使用主机的网络</span>
`--net=container`: <span style="color:#3333;">使用其他容器的网路，共享IP和PORT等网络资源</span>
`--net=none`：         <span style="color:#3333;">容器使用自己的网络（类似--net=bridge），但是不进行配置</span>

### Security configuration
`--security-opt="label:user:USER"`   : <span style="color:#3333;">Set the label user for the container</span>
`--security-opt="label:role:ROLE"`   : <span style="color:#3333;">Set the label role for the container</span>
`--security-opt="label:type:TYPE"`   : <span style="color:#3333;">Set the label type for the container</span>
`--security-opt="label:level:LEVEL"` : <span style="color:#3333;">Set the label level for the container</span>
`--security-opt="label:disable"`     : <span style="color:#3333;">Turn off label confinement for the container</span>
`--secutity-opt="apparmor:PROFILE"`  : <span style="color:#3333;">Set the apparmor profile to be applied  to the container</span>

### Runtime privilege, Linux capabilities, and LXC configuration
`--cap-add`: <span style="color:#3333;">Add Linux capabilities</span>
`--cap-drop`: <span style="color:#3333;">Drop Linux capabilities</span>
`docker run --cap-add=ALL --cap-drop=MKNOD ...` <span style="color:#3333;">容器拥有除了MKNOD之外的所有内核权限</span>
`--privileged`: <span style="color:#3333;">Docker将拥有访问主机所有设备的权限</span>
`--device=[]`: <span style="color:#3333;">允许容器只访问一些特定设备</span>
`docker run --device=/dev/sda:/dev/xvdc --rm -it ubuntu fdisk  /dev/xvdc`
`--lxc-conf=[]`: <span style="color:#3333;">(lxc exec-driver only) Add custom lxc options --lxc-conf="lxc.cgroup.cpuset.cpus = 0,1"</span>

### 覆盖Dockfile
- CMD (Default Command or Options)
- ENTRYPOINT (Default Command to Execute at Runtime)
- EXPOSE (Incoming Ports)
- ENV (Environment Variables)
- VOLUME (Shared Filesystems)
- USER
- WORKDIR 

#### 无法被覆盖的：FROM、MAINTAINER、RUN、ADD

`$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

1. CMD
[COMMAND] 是可选部分，上面命令中新的command来覆盖旧的command，如果镜像中设定了ENTRYPOINT，那么命令中的CMD也可以作为参数追加到ENTRYPOINT中。

2. ENTRYPOINT
指定当容器执行时，需要启动哪些进程。--entrypoint

3. EXPOSE
--expose可以让容器接受外部传入的数据。容器内监听的端口不需要和外部主机的端口相同。比如说在容器内部，一个HTTP服务监听在80端口，对应外部主机的端口就可能是49880。
当使用-P时，Docker会在主机中随机从49153 和65535之间查找一个未被占用的端口绑定到容器。可以使用docker port来查找这个随机绑定端口。
当你使用--link方式时，作为客户端的容器可以通过私有网络形式访问到这个容器。同时Docker会在客户端的容器中设定一些环境变量来记录绑定的IP和PORT。

4. ENV
```
HOME    Set based on the value of USER
HOSTNAME    The hostname associated with the container
PATH    Includes popular directories, such as : /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM    xterm if the container is allocated a psuedo-TTY
```
当容器启动时，会自动在容器中初始化这些变量。docker run 通过`-e`来设定任意的环境变量，甚至覆盖已经存在的环境变量。

5. VOLUME
Dockerfile中可以设定多个volume
-v=[]
--volumes-from=""

6. USER
容器中默认的用户是root，可以通过Dockerfile的USER设定默认的用户，并通过"-u "来覆盖这些参数。

7. WORKDIR
容器中默认的工作目录是根目录（/）可以通过"-w"来覆盖默认的工作目录。
