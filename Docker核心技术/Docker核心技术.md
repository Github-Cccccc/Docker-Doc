# Docker核心技术

##### 一 Docker是什么?

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的[Linux](https://baike.baidu.com/item/Linux)机器或Windows 机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。

**Docker官方有一句话:一次构建,处处运行**

---------------

**Docker为什么会出现?**

通常我们在开发和运维工程师之间往往会出现这样的问题,开发工程师开发完项目后在自己本地跑是没有任何问题的,但当交给运维工程师,项目部署上线时,往往会出现开发交给运维工程师的项目在开发本地跑没有任何问题,交给运维后却错误满满,甚至运行不起来!导致这种现象的原因就是项目的环境以及配置文件不统一,由此Docker应运而生,解决了这一大难题

---

**Docker解决了什么问题?**

解决了运行环境和配置问题软件容器,方便做持续集成并有助于整体发布的容器虚拟化技术

一个完整的Docker有以下几个部分组成：

> 1. DockerClient客户端
> 2. Docker Daemon守护进程
> 3. Docker Image镜像
> 4. DockerContainer容器 

------

##### 二 Docker三要素

> 镜像[images] 
>
> 容器[container] 
>
> 仓库[respoistory]

![](D:\socra\Documents\Typora\Docker核心技术\img\sanyaosu.jpg)

**我们可以通过相关镜像创建容器也可以反过来通过容器提交一个镜像,镜像通常在仓库拉取,也可以通过容器提交或者通过Dockerfile文件构建自定义的镜像文件,**

![](D:\socra\Documents\Typora\Docker核心技术\img\2441456-af6c270b1a64912a.jpg)

仓库就类似于Maven,git中的中央仓库,

镜像可以想象为我们Java代码的具体的一个类,容器可以理解为这个Java代码的具体的一个类的实例

通过镜像我们可以创建容器,反过来通过容器我们也可以生成镜像 ,

类似于Java中的反射,

--------

##### 三 Docker常用命令

**3.1 帮助命令:**

```json
docker --help   帮助命令

docker info     详细信息

docker version  版本信息

```

**3.2 镜像命令:**

```json
docker images                      列出本机所有镜像
       docker images -a            列出本机所有镜像,包含中间映像层
       docker images -q            列出本机所有镜像的id
       docker images -aq           列出本机所有镜像,包含中间层镜像的id
       docker images --no-trunc    不截断输出镜像信息.
       docker images --digests     显示镜像的摘要信息

docker search  xxx                 查找某一个镜像
       docker search -s 10  xxx    查找某一个镜像.只显示star数大于10的惊醒


docker pull  xxx      拉取某一个镜像
     Usage: docker pull NAME[:TAG]
     默认拉取镜像为最新版latest
     docker pull xxx  xxx     拉取多个

docker rmi xxx                        删除某一镜像
     docker rmi xxx xxx               删除多个镜像
     docker rmi $(docker images aq)   删除全部镜像
```

**3.3 容器命令**

```json
Usage:	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

OPTIONS参数命令
--name     启动容器后的容器名
-i         以交互方式启动
-t         打开终端命令行
-d         后台启动,即守护式启动
-p(小写)    指定端口映射   localhostport : containerport 
-P(大写)    随机端口映射

示例如下:
这里我用centos:7 镜像作为示例
docker run -it --name centos centos:7 /bin/bash   交互式运行某个镜像并命名容器名为centos,运行容器后执行 /bin/bash命令

docker run -d --name centos centos:7 /bin/bash    以守护进程方式运行某个镜像并命名容器名centos,运行容器后执行 /bin/bash命令

这里用tomcat:8.5.3镜像作为示例
docker run -it --name tomcat  -p 80:8080 tomcat tomcat:8.5.3   以交互式运行某个镜像,并重命名为tomcat 指定映射端口为本机80端口映射容器内8080端口

docker run -d  --name tomcat  -P tomcat tomcat:8.5.3           以守护进程方式运行某个镜像,并重命名为tomcat 随机映射本机端口到容器内的tomcat端口

#Docker重要知识点:docker容器后台运行,就必须有一个前台进程,容器运行的命令如果不是那些一直挂起的命令(比如运行top,tail),那么容器就会自动退出


docker ps  列出当前所有正在运行的容器
    docker ps -a      列出所有运行过的容器记录
    docker ps -aq     列出当前所有正在运行的容器的id
    docker ps -l      列出最近运行的容器
    docker ps -n 3    列出最近运行的三个容器 


容器退出的两种方式
exit 直接退出,不保留后台进程 
Ctrl+Q+P  退出,保留后台进程

启动容器
docker start 容器id或名字
停止容器
docker stop 容器id或名字  温和停止
docker kill 容器id或名字  直接停止
重启容器
docker restart 容器id或名字 

强制删除容器
rm -f contrainer
删除已经停止的容器
rm contrainer 

查看容器日志
docker log -f -t -tail 容器id
-f  代表最新日志  -t代表加上时间戳  -tail代表显示尾部几条数据


查看容器内运行的进程
docker top 容器id

查看容器内部细节
docker inspect 容器id

进入容器内并以命令行交互
docker exec -it 容器id bashshell  是在容器中打开新的终端,并且可以启动新的进程

docker attach 容器id     直接进入容器并启动命令终端,不会创建新的进程


把容器内的文件拷贝到主机上
docker cp 容器id:容器内目录 主机内目录
```

![u=3911531038,490596358&fm=26&gp=0](D:\socra\Documents\Typora\Docker核心技术\img\u=3911531038,490596358&fm=26&gp=0.jpg)  

##### 四 Docker镜像原理

**镜像是什么?**

轻量级,可执行的独立软件包,用来打包软件运行环境和基于运行环境开发的软件,它包含运行某个软件所需的所有内容,包括代码,运行时,库,环境变量和配置文件

Docker联合文件系统

联合文件系统（[UnionFS](https://en.wikipedia.org/wiki/UnionFS)）是一种分层、轻量级并且高性能的文件系统，它支持对**文件系统的修改作为一次提交来一层层的叠加**，同时可以将不同目录挂载到**同一个虚拟文件系统**下。
联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

![](D:\socra\Documents\Typora\Docker核心技术\img\2441456-74c323a4b53e341f.jpg)

**Docker镜像commit操作补充**

```
docker commit -a  -m  容器Id    imagename:tag
-a 为镜像作者
-m 为镜像描述信息
```

----

##### 五 容器数据卷

**5.1 容器数据卷的作用:**

> 对容器内的数据进行持久化操作,
>
> 容器之间继承以及共享数据

**卷就是目录或者文件**,存在于一个或多个容器中,由docker挂载到容器中,卷的设计目的就是数据的持久化,完全独立于容器的生存周期,因此Docker不会再容器删除时删除其挂载的数据

**特点:**

1.数据卷可在容器之间共享数据

2.卷中的更改可以直接生效

3.数据卷中的更改不会包含在镜像的更新中

4.数据卷的生命周期一直持续到没有容器使用它为止

----

**数据卷添加方式**

**1.直接命令添加:**

```json
docker run -it -v  /宿主机绝对目录 : / 容器内目录  镜像名

带权限
docker run -it -v  /宿主机绝对目录 : / 容器内目录:ro  镜像名    容器内的目录只支持读操作 only read
```

如何查看数据卷是否挂载成功 

```json
docker inspect 容器id
```

-----

**2.Dockerfile添加:**

示例:

```json
FROM centos 

VOLUME ["/data","data01"]  这里的数据卷目录为容器内的目录,宿主机对应的目录可利用 docker inspect 容器id查看

CMD  echo "finished----------success"

CMD /bin/bash 
```

利用Dockerfile文件构建镜像

```json
docker bulid   -f docker文件路径  -t  镜像:tag .  #小数点为当前路径下
```

----

##### 六 数据卷容器

**定义:命名的容器挂载数据卷,其他容器通过挂载这个数据卷(父容器)实现其数据共享,挂载数据的容器,成为数据卷容器**

**作用:容器之间的数据传递共享**

```json
示例:

比如当前容器中有host01,host01,host03三个容器,host01为数据卷容器即host01挂载了数据,host02,host03通过继承host01就可以实现数据共享

docker run -it --name host01 -v /宿主机目录 : /容器内目录   image:tag

docker run -it --name host02 --volumes-from host01  image:tag

docker run -it --name host03 --volumes-from host01 image:tag 

docker run -it --name host04  --volumes-from host03 image:tag

当删除父容器host01后,host02与host03之间仍然可以数据共享
当删除父容器host03时,host02与host04之间仍然可以数据共享

结论:容器之间配置信息的传递,数据卷的生命周期一直持续到没有容器使用它为止

```

---

##### 七 Dockerfile文件解析-1

**7.1** **定义:Dockerfile是用来构建Docker镜像的文件,是由一系列命令和参数构成的脚本**

**7.2** **Dockerfile内容基础知识:**

> 1.每条保留字指令字母必须为大写,并且后面必须跟随参数
>
> 2.指令从上到下,依次执行
>
> 3.#为注释内容
>
> 4.每条指令都会创建一个新的镜像层,并对镜像层进行提交

**7.3 Dockfile文件大致执行流程**

> 1.从基础镜像运行一个容器
>
> 2.执行一条指令对容器进行修改
>
> 3.执行类似 commit 指令的操作提交一个新的镜像层
>
> 4.再基于刚提交的镜像运行一个新容器
>
> 5.执行Dockerfile中的下一条指令直到所有指令都执行完成

从应用软件的角度来看,Dockerfile,Docker镜像,与Docker容器分别代表软件的三个不同阶段

- [x] Dockerfile是软件的原材料
- [x] Docker镜像是软件的交付品
- [x] Docker容器则可以认为是软件的运行态

Dockerfile面向开发,Docker镜像成为交付标准,Docker容器则涉及部署与运维,三者缺一不可,合力充当Docker体系的基石

![](D:\socra\Documents\Typora\Docker核心技术\img\三要素.png)

1.Dockerfile,需要定义一个Dockerfile,Dockerfile定义了进程所需要的一切东西,Dockerfile涉及的内容包括执行代码或者是文件,环境,变量,依赖包,运行时环境,动态链接库,操作系统发行版,服务进程,和内核进程(当应用进程需要和系统服务和内核进程打交道时,这时需要考虑到如何设计namespace的权限控制等等);



2.Docker镜像,在用Dockerfile定义了一个文件之后,Docker  build时会产生一个Docker镜像,当运行Docker镜像时,会真正的开始提供服务



3.Docker容器,容器是直接提供服务的

----------

##### 八 Dockerfile文件解析-2

　　　　　　　　　　　　　　　**Dockerfile 文件保留字指令**

| １   | FROM            | 基础镜像 指定当前新镜像基于哪个镜像的                        |
| :--- | --------------- | :----------------------------------------------------------- |
| ２   | **MAINTANINER** | 镜像维护者的姓名和邮箱地址                                   |
| ３   | RUN             | 镜像构建时需要运行的命令                                     |
| ４   | EXPOSE          | 当前容器对外暴露的端口                                       |
| ５   | WORKDIR         | 指定在创建容器后,终端默认登陆进来的工作目录,可以理解为落脚点 |
| ６   | ENV             | 用来在构建镜像过程中设置环境变量                             |
| ７   | ADD             | 将宿柱机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包 |
| ８   | COPY            | 类似于ADD,拷贝文件和目录到镜像中，将从构建上下文目录中＜源路径＞的文件／目录复制到新的一层的镜像内的＜目标路径＞位置　　**两种写法** |
| ９   | VOLUME          | 容器数据卷,用于数据保存和持久化操作                          |
| １０ | CMD             | 指定一个容器启动时要运行的命令 ,如果有多个命令,但只有最后一个生效,而且CMD命令会被Docker后的命令替换 |
| １１ | ENTRYPOINT      | 指定一个容器启动时要运行的命令,和CMD命令具有相同目的，但不同的一点是,ENTRYPOINT 后的命令可以有多个，并且都可以生效 |
| １２ | ONBUILD         | 当构建一个被继承的Dockerfile时运行命令，父镜像在被子镜像继承后父镜像的onbuild被触发 |

8 **COPY   指令两种写法**

```json
COPY src dest        　Shell脚本写法

COPY ["src","dest"]    Json写法　
```

----

**Dockerfile 示例:**

​                                                          **Tomcat-Dockerfile示例文件:**

```Json
FROM openjdk:11-jdk-buster

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

ENV TOMCAT_MAJOR 8
ENV TOMCAT_VERSION 8.5.57
ENV TOMCAT_SHA512 720de36bb3e40a4c67bdf0137b12ae0fd733aef772d81a4b8dab00f29924ddd17ecb2a7217b9551fc0ca51bd81d1da13ad63b6694c445e5c0e42dfa7f279ede1

EXPOSE 8080
CMD ["catalina.sh", "run"]
```

​                                                               **Centos-Dockerfile示例文件:**

```json
FROM scratch
ADD centos-7.7-x86_64-docker.tar.xz /

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20191024"

CMD ["/bin/bash"]
```

------

**Dockerfile 自定义 示例:**

**示例一:**自定义centos,改变默认登陆路径,具有vim ifconfig 功能,并暴露80端口

```xml
FROM  centos
MAINTAINER  staticzz<c.dev1@qq.com>
ENV  PATH /usr
WORKDIR  $PATH
RUN yum -y install vim
RUN yum -y install net-tools 
EXPOSE 80

CMD echo $PATH 
CMD echo "success--"
CMD /bin/bash
```

**示例二:**CMD与ENTRYPOINT示例

CMD指令后不能有多个命令,如果有多个命令,则被最后一个命令覆盖

需求:现在我们要查询本机的Ip,并返回http头信息,

```
curl -s http://ip.cn -i  
```

如果利用CMD指令构建的镜像,当容器运行时,加-i 命令会报错,原因是CMD命令不能多个一起使用

```json
docker run -it 镜像id  -i 
```

```json
FROM centos
RUN yum -y install curl 
CMD ["curl","-s","http://ip.cn"]
```

利用ENTRYPOINT指令构建的镜像,当容器运行时,加-i 命令正常运行,ENTRYPOINT指令可多个参数一起运行

```json
docker run -it 镜像id -i
```

```json
FROM centos 
RUN yum -y insatll curl 
ENTRYPOINT ["curl","-s","http://ip.cn"]
```

**示例三:**ONBUILD指令

我们新建两个Dockerfile文件,一个为父文件名为Dockerfile,一个为子文件Dockerfile01

Dockerfile文件内容如下:

```json
FROM　centos

YUN  yum -y install  curl

CMD  ["curl","-s","http://ip.cn"]

ONBUILD RUN echo"father is onbuild"
```

我们现在利用Dockerfile构建我们需要的镜像

```json
docker build  -t statiacc/centos:1.1 . 
```

构建完成后我们接下来编辑我们的Dockerfile01子文件,内容如下:

```json
FROM  构建好的镜像

RUN  yum -y install curl

ENTRYPOINT ["cmd","-s","http://ip.cn"]
```

接下来当我们利用我们的子文件Dockerfile01文件构建镜像时,因为我们继承的是我们的父文件构建的镜像,所以当我们利用子文件构建镜像时,会触发ONBUILD 指令 

**示例四:**构建Tomcat,综合使用保留字命令并区分COPY 与ADD 的区别

**步骤:**

> 1. mkdir -p /usr/local/tomcat
> 2. 在上述目录下touch c.txt
> 3. 将jdk和tomcat安装的压缩包拷贝进上一步目录
> 4. 在/usr/local/tomcat目录下新建Dockerfile文件
> 5. 构建
> 6. 运行
> 7. 验证
> 8. 综合前述步骤部署web服务

Dockerfile文件内容如下:

```json
FROM centos 

MAINTAINER   cc<ss@qq.com

#把宿主机当前的上下文中的c.txt拷贝到容器的/usr/local/tomcat目录下
COPY  c.txt /usr/local/tomcat

#把java与tomcat拷贝到容器的/usr/local/tomcat下
ADD  jdk文件 /usr/local/
ADD  tomcat文件   /usr/local

#安装vim编辑器
RUN yum -y install vim 

设置java与tomcat环境变量
ENV JAVA_HOME  /usr/local/java
ENV CLASSPATH  $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/tomcat
ENV CTATLINA_BASE /usr/local/tomcat

ENV PATH　$PATH:$JAVA_HOME:$CATALINA_HOME/lib:$CTATLINA_HOME/bin

#设置工作路径,即容器登录后的落脚点
ENV  path  /usr/local
WORKDIR   $path

#设置容器运行时监听的端口
EXPOSE  80

#启动时运行tomcat
#ENTRYPOINT ["/usr/local/tomcat/startup.sh"]
#CMD ["/usr/local/tomcat/startup.sh"]
CMD /usr/local/tomcat/bin/startup.sh && tail -F /usr/local/tomcat/bin/logs/catalina.out
```

**Dockerfile小总结图示**

![](D:\socra\Documents\Typora\Docker核心技术\img\u=2833323145,117014179&fm=26&gp=0.jpg)

##### 九 Docker镜像的拉取与推送

**因为docker官方网站较慢,我们可以使用阿里云镜像仓库**

推送命令如下:

```json
docker login --username=xxxx  ##这里写你的仓库名

docker tag [本地要推送的镜像id] xxxx 这里写你的即将要推送的镜像仓库的地址/staticzz/:xxx 这里写你的镜像版本号

docker push xxx这里写你的即将要推送的镜像仓库的地址以及仓库的位置和上一步中已经设置好的镜像名与tag
```

使用方法如下:

```json
$ sudo docker login --username=staticdev registry.cn-hangzhou.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/static_cc/docketr_resp:[镜像版本号]
$ sudo docker push registry.cn-hangzhou.aliyuncs.com/static_cc/docketr_resp:[镜像版本号]
```

