Dockerfile 是自动构建 docker 镜像的配置文件， 用户可以使用 Dockerfile 快速创建自定义的镜像。

Dockerfile 是由一行行命令语句组成，并且支持以# 开头的注释行。Dockerfile 中的命令非常类似于 linux 下的 shell 命令。

一般来说，可以将 Dockerfile 分为四个部分：

- **基础镜像(父镜像)信息指令 FROM**
- **维护者信息指令 MAINTAINER**
- **镜像操作指令 RUN 、 EVN 、 ADD 和 WORKDIR 等**
- **容器启动指令 CMD 、 ENTRYPOINT 和 USER 等**

一个小例子：

1. FROM python:2.7 
2. MAINTAINER Angel_Kitty <angelkitty6698@gmail.com>
3. COPY . /app
4. WORKDIR /app
5. RUN pip install -r requirements.txt
6. EXPOSE 5000
7. ENTRYPOINT ["python"]
8. CMD ["app.py"]

分析一下上面这个过程：

- 1、从 Docker Hub 上 pull 下 python 2.7 的基础镜像

- 2、显示维护者的信息

- 3、copy 当前目录到容器中的 /app 目录下

- 4、指定工作路径为 /app

- 5、安装依赖包

- 6、暴露 5000 端口

- 7、启动 app

  

**Dockerfile常用指令**

**1.**FROM 是用于指定基础的 images ，一般格式为 ：FROM <image> or FORM <image>:<tag> 。所有的 Dockerfile 都用该以 FROM 开头，FROM 命令指明 Dockerfile 所创建的镜像文件以什么镜像为基础，FROM 以后的所有指令都会在 FROM 的基础上进行创建镜像。可以在同一个 Dockerfile 中多次使用 FROM 命令用于创建多个镜像。

2.MAINTAINER 是用于指定镜像创建者和联系方式，一般格式为 MAINTAINER <name> 。

3.COPY 是用于复制本地主机的 <src> (为 Dockerfile 所在目录的相对路径)到容器中的 <dest>。例如要拷贝当前目录到容器中的 /app 目录下，可以这样操作：

COPY . /app

4.WORKDIR 用于配合 RUN，CMD，ENTRYPOINT 命令设置当前工作路径。可以设置多次，如果是相对路径，则相对前一个 WORKDIR 命令。默认路径为/。一般格式为 WORKDIR /path/to/work/dir 。例如设置/app 路径，可以进行如下操作：

WORKDIR /app

5.RUN 用于容器内部执行命令。**每个 RUN 命令相当于在原有的镜像基础上添加了一个改动层**，原有的镜像不会有变化。一般格式为 RUN <command> 。例如要安装 python 依赖包，做法如下：

RUN pip install -r requirements.txt

6.EXPOSE 命令用来指定对外开放的端口。一般格式为 EXPOSE <port> [<port>...]

7.ENTRYPOINT 可以让容器表现得像一个可执行程序一样。**一个 Dockerfile 中只能有一个 ENTRYPOINT**，如果有多个，则最后一个生效。

ENTRYPOINT 命令也有两种格式：

- ENTRYPOINT ["executable", "param1", "param2"] ：推荐使用的 exec形式
- ENTRYPOINT command param1 param2 ：shell 形式

例如下面这个，要将 python 镜像变成可执行的程序，可以这样去做：

ENTRYPOINT ["python"]

8.**CMD 命令用于启动容器时默认执行的命令**，CMD 命令可以包含可执行文件，也可以不包含可执行文件。不包含可执行文件的情况下就要用 ENTRYPOINT 指定一个，然后 CMD 命令的参数就会作为ENTRYPOINT的参数。

CMD 命令有三种格式：

- CMD ["executable","param1","param2"]：推荐使用的 exec 形式。
- CMD ["param1","param2"]：无可执行程序形式
- CMD command param1 param2：shell 形式。

一个 Dockerfile 中只能有一个CMD，如果有多个，则最后一个生效。而 CMD 的 shell 形式默认调用 /bin/sh -c 执行命令。

CMD 命令会被 Docker 命令行传入的参数覆盖：docker run busybox /bin/echo Hello Docker9(容器启动后，执行的命令) 会把 CMD 里的命令覆盖。

例如要启动 /app ，可以用如下命令实现：

CMD ["app.py"]

