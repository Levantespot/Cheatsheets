## 基本概念

### 镜像（Image）

Docker 镜像（`Image`）是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 **不包含** 任何动态数据，其内容在构建之后也不会被改变。

### 分层存储

因为镜像包含操作系统完整的 `root` 文件系统，其体积往往是庞大的，因此在 Docker 设计时，就充分利用 [Union FS](https://en.wikipedia.org/wiki/Union_mount) 的技术，将其设计为分层存储的架构。所以严格来说，镜像并非是像一个 `ISO` 那样的打包文件，镜像只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成。

镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层。比如，删除前一层文件的操作，实际不是真的删除前一层的文件，而是仅在当前层标记为该文件已删除。在最终容器运行的时候，虽然不会看到这个文件，但是实际上该文件会一直跟随镜像。因此，在构建镜像的时候，需要额外小心，每一层尽量只包含该层需要添加的东西，任何额外的东西应该在该层构建结束前清理掉。

分层存储的特征还使得镜像的复用、定制变的更为容易。甚至可以用之前构建好的镜像作为基础层，然后进一步添加新的层，以定制自己所需的内容，构建新的镜像。

### 容器（Container）

镜像（`Image`）和容器（`Container`）的关系，就像是面向对象程序设计中的 `类` 和 `实例` 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 [命名空间](https://en.wikipedia.org/wiki/Linux_namespaces)。因此容器可以拥有自己的 `root` 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。

### 仓库（Registry）

镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry] 就是这样的服务。

一个 **Docker Registry** 中可以包含多个 **仓库**（`Repository`）；每个仓库可以包含多个 **标签**（`Tag`）；每个标签对应一个镜像。

通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 `<仓库名>:<标签>` 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 `latest` 作为默认标签。

以 [Ubuntu 镜像](https://hub.docker.com/_/ubuntu) 为例，`ubuntu` 是仓库的名字，其内包含有不同的版本标签，如，`16.04`, `18.04`。我们可以通过 `ubuntu:16.04`，或者 `ubuntu:18.04` 来具体指定所需哪个版本的镜像。如果忽略了标签，比如 `ubuntu`，那将视为 `ubuntu:latest`。

仓库名经常以 *两段式路径* 形式出现，比如 `jwilder/nginx-proxy`，前者往往意味着 Docker Registry 多用户环境下的用户名，后者则往往是对应的软件名。但这并非绝对，取决于所使用的具体 Docker Registry 的软件或服务。

## 镜像操作

### 显示 Docker 信息

包含 Docker 版本、镜像数量、容器数量、占用硬盘空间等等

```bash
$ docker info
```

### 获取镜像

```bash
$ docker pull [选项] [Docker Registry 地址[:端口号]/]<仓库名>[:标签]
```

- Docker 镜像仓库地址：地址的格式一般是 `<域名/IP>[:端口号]`。默认地址是 Docker Hub(`docker.io`)。
- 仓库名：如之前所说，这里的仓库名是两段式名称，即 `<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 `library`，也就是官方镜像。

e.g.

```bash
$ docker pull ubuntu:18.04
```

### 列出镜像

列出已经下载好的镜像

```bash
$ docker image ls
```

e.g.

```bash
$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
redis                latest              5f515359c7f8        5 days ago          183 MB
nginx                latest              05a60462f8ba        5 days ago          181 MB
mongo                3.2                 fe9198c04d62        5 days ago          342 MB
<none>               <none>              00285df0df87        5 days ago          342 MB
ubuntu               18.04               329ed837d508        3 days ago          63.3MB
ubuntu               bionic              329ed837d508        3 days ago          63.3MB
```

列表包含了 `仓库名`、`标签`、`镜像 ID`、`创建时间` 以及 `所占用的空间`。

其中仓库名、标签在之前的基础概念章节已经介绍过了。**镜像 ID** 则是镜像的唯一标识，而一个镜像可以对应多个 **标签**。因此，在上面的例子中，我们可以看到 `ubuntu:18.04` 和 `ubuntu:bionic` 拥有相同的 ID，因为它们对应的是同一个镜像。

### 运行镜像

运行镜像的结果是产生并启动一个容器

```bash
$ docker run [选项] <image_name> [需要运行的指令]
```

e.g.

```
$ docker run -it --rm --name L ubuntu:18.04 bash
```

- `-it`：这是两个参数，一个是 `-i`：交互式操作，一个是 `-t` 终端。我们这里打算进入 `bash` 执行一些命令并查看返回结果，因此我们需要交互式终端。
- `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 `docker rm`。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 `--rm` 可以避免浪费空间。
- `ubuntu:18.04`：这是指用 `ubuntu:18.04` 镜像为基础来启动容器。
- `bash`：放在镜像名后的是 **命令**，这里我们希望有个交互式 Shell，因此用的是 `bash`。
- `--name L`: 为该 container 取名为 L。若使用该参数则系统会自动分配一个随机字符串。
- `-d`: --detach，后台运行

### 镜像体积

查看镜像、容器、数据卷所占用的空间。

```
docker system df
```

e.g.

```bash
$ docker system df

TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              24                  0                   1.992GB             1.992GB (100%)
Containers          1                   0                   62.82MB             62.82MB (100%)
Local Volumes       9                   0                   652.2MB             652.2MB (100%)
Build Cache                                                 0B                  0B
```

### 删除镜像

```bash
$ docker rmi <镜像1> [<镜像2> ...]
```

其中，`<镜像>` 可以是 `镜像短 ID`、`镜像长 ID`、`镜像名` 或者 `镜像摘要`。

我们可以用镜像的完整 ID（称为 `长 ID`）来删除镜像。也可以用 `短 ID` 来删除镜像。`docker image ls` 默认列出的就已经是短 ID 了，一般取前3个字符以上，只要足够区分于别的镜像就可以了。

### 修改镜像

`Commit` 命令。暂时不做介绍。

### 定制镜像

以后接触到再看。

## 容器操作

### 显示容器

```bash
docker container list
```

or

```bash
docker ps [-a]
```

- `-a`: 显示所有的容器，包括已经停止的

### 运行容器

```bash
docker start <container id>
```

### 停止容器

```bash
docker stop <container id>
```

### 后台运行

进入后台运行中的容器

```bash
docker exec -it <container id> /bin/bash
```

使用该命令，退出终端后不会导致容器停止

or

```bash
docker attach -it <container id> /bin/bash
```

使用该命令，退出终端后将会导致容器停止

### 删除容器

```dockerfile
docker rm <id>
```

## Volume 操作

### Volume

列出本机所有的 volume:

```bash
docker volume ls
```

创建一个volume:

```bash
docker volume create <volume_name>
```

删除一个volume:

```bash
docker volume rm <volume_name>
```

使用 volume

--mount 版本

```bash
docker run -it --rm --name test --mount source=<vol-name>,target=/my_data <image-name>
```

-v 版本

```bash
docker run -it -v /home/b320a/L:/my-dir -p 6321:8888 --gpus all --ipc=host nvcr.io/nvidia/pytorch:21.06-py3
```



## Pytorch & Docker

1. 更新驱动

2. 下载 [Pytorch 镜像（含 CUDA、CuDNN）](https://ngc.nvidia.com/catalog/containers/nvidia:pytorch)

3. 运行镜像：[参考链接](https://docs.nvidia.com/deeplearning/frameworks/user-guide/index.html#runcont)，Use `docker run --gpus` to run GPU-enabled containers.

   - Example using all GPUs:

     ```bash
     $ docker run --gpus all ...
     ```

   - Example using two GPUs:

     ```bash
     $ docker run --gpus 2 ...
     ```

   - Examples using specific GPUs:

     ```bash
     $ docker run --gpus "device=1,2" ...
     $ docker run --gpus "device=UUID-ABCDEF,1" ...
     ```

   - 我自己使用的例子

     ```bash
     $ docker run -it --rm --gpus 1 --ipc=host nvcr.io/nvidia/pytorch:21.06-py3 
     ```


## 生命周期 Lifecycle

![Docker lifecycle](https://miro.medium.com/max/2258/1*vca4e-SjpzSL5H401p4LCg.png)

彩色圆形 代表容器的五种状态：

- created: 初建状态
- running: 运行状态
- stopped: 停止状态
- paused: 暂停状态
- deleted: 删除状态

长方形 代表容器在执行某种命令后进入的状态：

- docker create: 创建容器后，不立即启动运行，容器进入初建状态；
- docker run: 创建容器，并立即启动运行，进入运行状态；
- docker start: 容器转为运行状态；
- docker stop: 容器将转入停止状态；
- docker kill: 容器在故障（死机）时，执行kill（断电），容器转入停止状态，这种操作容易丢失数据，除非必要，否则不建议使用；
- docker restart: 重启容器，容器转入运行状态；
- docker pause: 容器进入暂停状态；
- docker unpause: 取消暂停状态，容器进入运行状态；
- docker rm: 删除容器，容器转入删除状态（如果没有保存相应的数据库，则状态不可见）。
