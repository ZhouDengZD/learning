**一、Docker的基本概念**

![](https://ftp.bmp.ovh/imgs/2019/12/b8f4542de524e395.png)

docker包括三个基本概念：Image(镜像)，Container(容器)，Repository(仓库)。

镜像是 Docker 运行容器的前提，仓库是存放镜像的场所，可见镜像更是 Docker 的核心。

Docker 镜像可以看作是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。**镜像不包含任何动态数据，其内容在构建之后也不会被改变（只读）**。镜像运行后，就成了容器。

镜像有多种生成方法：

1. 从无到有开始创建镜像
2. 下载并使用别人创建好的现成的镜像
3. 在现有镜像上创建新的镜像

**我们可以将镜像的内容和创建步骤描述在一个文本文件中，这个文件被称作 Dockerfile ，通过执行 docker build  命令可以构建出 Docker 镜像**

容器可以理解为一个轻量级的沙箱， Docker 利用容器来运行和隔离应用。

**Docker 仓库是集中存放镜像文件的场所**。镜像构建完成后，可以很容易的在当前宿主上运行，但是， 如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry (仓库注册服务器)就是这样的服务。有时候会把仓库 (Repository) 和仓库注册服务器 (Registry) 混为一谈，并不严格区分。Docker 仓库的概念跟 Git 类似，注册服务器可以理解为 GitHub 这样的**托管服务**。实际上，一个 Docker Registry 中可以包含多个仓库 (Repository) ，每个仓库可以包含多个标签 (Tag)，每个标签对应着一个镜像。所以说，镜像仓库是 Docker 用来集中存放镜像文件的地方类似于我们之前常用的代码仓库。

通常，**一个仓库会包含同一个软件不同版本的镜像**，而**标签就常用于对应该软件的各个版本** 。我们可以通过<仓库名>:<标签>的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签.。

**仓库又可以分为两种形式：public(公有仓库)及 private（私有仓库) **

Docker Registry 公有仓库是开放给用户使用、允许用户管理镜像的 Registry 服务。一般这类公开服务允许用户免费上传、下载公开的镜像，并可能提供收费服务供用户管理私有镜像。

除了使用公开服务外，用户还可以在本地搭建私有 Docker Registry 。**Docker 官方提供了 Docker Registry镜像，可以直接使用做为私有Registry服务。当用户创建了自己的镜像之后就可以使用 push命令将它上传到公有或者私有仓库，这样下次在另外一台机器上使用这个镜像时候，只需要从仓库上pull下来就可以了。**

**二、Docker构架**

![](https://ftp.bmp.ovh/imgs/2019/12/ca6995e8273096d3.png)

Docker 使用 C/S 结构，即**客户端/服务器**体系结构。 Docker 客户端与 Docker 服务器进行交互，Docker服务端负责构建、运行和分发 Docker 镜像（即：当我们使用 docker 命令行运行命令时， **Docker 客户端将这些命令发送给服务器端，服务端将执行这些命令。 **docker 命令使用 docker API 。 Docker 客户端可以与多个服务端进行通信。）。 Docker 客户端和服务端可以运行在一台机器上，也可以通过 RESTful 、 stock 或网络接口与远程 Docker 服务端进行通信。

![](https://ftp.bmp.ovh/imgs/2019/12/315e30fd0cb70d81.png)

这张图展示了 Docker 客户端、服务端和 Docker 仓库（即 Docker Hub 和 Docker Cloud ），**默认情况下Docker 会在 Docker 中央仓库寻找镜像文件**，这种利用仓库管理镜像的设计理念类似于 Git ，当然这个仓库是可以通过修改配置来指定的，甚至我们可以创建我们自己的私有仓库。

**Docker 的核心组件包括：**

1. Docker Client（客户端）
2. Docker daemon（服务端）
3. Docker Image
4. Docker Registry
5. Docker Container

**客户端**，其实就是 Docker 提供命令行界面 (CLI) 工具，是用户与 Docker 进行交互的主要方式。客户端可以构建，运行和停止应用程序，还可以远程与Docker_Host进行交互。**最常用的 Docker 客户端就是 docker 命令**，我们可以通过 docker 命令很方便地在 host 上构建和运行 docker 容器

**Docker daemon** **是服务器组件**，以 Linux 后台服务的方式运行，是 Docker 最核心的后台进程，我们也把它称为守护进程。它**负责响应来自 Docker Client 的请求**，然后将这些请求翻译成系统调用完成容器管理操作。该进程会在后台启动一个 API Server ，负责接收由 Docker Client 发送的请求，**接收到的请求将通过Docker daemon 内部的一个路由分发调度，由具体的函数来执行请求**。大致可以分为以下三部分：

- Docker Server
- Engine
- Job

![](https://ftp.bmp.ovh/imgs/2019/12/d6f26d0a4db1048a.png)

Docker Daemon 可以认为是通过 Docker Server 模块接受 Docker Client 的请求，并在 Engine 中处理请求，然后根据请求类型，创建出指定的 Job 并运行。 Docker Daemon 运行在 Docker host 上，负责创建、运行、监控容器，构建、存储镜像。

运行过程的作用有以下几种可能：

- 向 Docker Registry 获取镜像
- 通过 graphdriver 执行容器镜像的本地化操作
- 通过 networkdriver 执行容器网络环境的配置
- 通过 execdriver 执行容器内部运行的执行工作

**镜像**有多种生成方法：

1. 从无到有开始创建镜像
2. 下载并使用别人创建好的现成的镜像
3. 在现有镜像上创建新的镜像

**我们可以将镜像的内容和创建步骤描述在一个文本文件中，这个文件被称作 Dockerfile ，通过执行 docker build  命令可以构建出 Docker 镜像**

**Docker registry** 是存储 docker image 的仓库，它在 docker 生态环境中的位置如下图所示：

![](https://ftp.bmp.ovh/imgs/2019/12/d86523cfb9f7931b.jpeg)

运行docker push、docker pull、docker search时，实际上是通过 docker daemon 与 docker registry 通信。

**Docker 容器就是 Docker 镜像的运行实例**，是真正运行项目程序、消耗系统资源、提供服务的地方。 Docker Container 提供了系统硬件环境，我们可以使用 Docker Images 这些制作好的系统盘，再加上我们所编写好的项目代码， run 一下就可以提供服务啦。