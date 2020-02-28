# Ways       
获取镜像：docker pull      
运行镜像：docker run     
列出镜像：docker images      
进入容器，修改其内容：docker exec      
查看具体的改动：docker diff      
叠加容器的存储层，构成新的镜像：docker commit （实际环境中慎用！！）     
定制镜像：Dockerfile 脚本     


## 获取镜像    
从 Docker Registry 获取镜像的命令是 docker pull 。    
其命令格式为：  
```
docker pull [选项] [Docker Registry地址]<仓库名>:<标签>      
```

i.e.   
```
$ docker pull ubuntu:14.04   
14.04: Pulling from library/ubuntu   
bf5d46315322: Pull complete
9f13e0ac480c: Pull complete
e8988b5b3097: Pull complete
40af181810e7: Pull complete
e6f7c7e5c03e: Pull complete
Digest: sha256:147913621d9cdea08853f6ba9116c2e27a3ceffecf3b49298
3ae97c3d643fbbe
Status: Downloaded newer image for ubuntu:14.04
```
上面的命令中没有给出 Docker Registry 地址，因此将会从 Docker Hub 获取镜像。而镜像名称是 ubuntu:14.04 ，因此将会获取官方镜像 library/ubuntu仓库中标签为 14.04 的镜像。     
从下载过程中可以看到我们之前提及的分层存储的概念，镜像是由多层存储所构成。下载也是一层层的去下载，并非单一文件。下载过程中给出了每一层的 ID 的前 12 位。并且下载结束后，给出该镜像完整的 sha256 的摘要，以确保下载一致性。     

## 运行    
有了镜像后，我们就可以以这个镜像为基础启动一个容器来运行。以上面的ubuntu:14.04 为例，如果我们打算启动里面的 bash 并且进行交互式操作的话，可以执行下面的命令。
```
$ docker run -it --rm ubuntu:14.04 bash
root@e7009c6ce357:/# cat /etc/os-release
NAME="Ubuntu"
VERSION="14.04.5 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.5 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
root@e7009c6ce357:/# exit
exit
$
```
-it ：这是两个参数，一个是 -i ：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终获取镜像端。     
--rm ：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm 。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 --rm 可以避免浪费空间。    
ubuntu:14.04 ：这是指用 ubuntu:14.04 镜像为基础来启动容器。      
bash ：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 bash 。       
最后我们通过 exit 退出了这个容器。      

## 列出镜像    
要想列出已经下载下来的镜像，可以使用 docker images 命令。     
```
$ docker images
REPOSITORY TAG IMAGE ID CRE
ATED SIZE
redis latest 5f515359c7f8 5 d
ays ago 183 MB
nginx latest 05a60462f8ba 5 d
ays ago 181 MB
mongo 3.2 fe9198c04d62 5 d
ays ago 342 MB
<none> <none> 00285df0df87 5 d
ays ago 342 MB
ubuntu 16.04 f753707788c5 4 w
eeks ago 127 MB
ubuntu latest f753707788c5 4 w
eeks ago 127 MB
ubuntu 14.04 1e0c3dd64ccd 4 w
eeks ago 188 MB
```
列表包含了仓库名、标签、镜像 ID、创建时间以及所占用的空间。    
镜像 ID 则是镜像的唯一标识，一个镜像可以对应多个标签。因此，在上面的例子中，我们可以看到ubuntu:16.04 和 ubuntu:latest 拥有相同的 ID，因为它们对应的是同一个镜像。        

### 列出部分镜像
不加任何参数的情况下， docker images 会列出所有顶级镜像，但是有时候我们只希望列出部分镜像。 docker images 有好几个参数可以帮助做到这个事情。根据仓库名列出镜像     
```
$ docker images ubuntu
```

一般来说，虚悬镜像(dangling image) <none> 已经失去了存在的价值，是可以随意删除的，可以用下面的命令删除。      
$ docker rmi $(docker images -q -f dangling=true)      
  
## 中间层镜像   
为了加速镜像构建、重复利用资源，Docker 会利用 中间层镜像。所以在使用一段时间后，可能会看到一些依赖的中间层镜像。默认的 docker images 列表中只会显示顶层镜像，如果希望显示包括中间层镜像在内的所有镜像的话，需要加 -a 参数。     
```
$ docker images -a
```
这样会看到很多无标签的镜像，与之前的虚悬镜像不同，这些无标签的镜像很多都是中间层镜像，是其它镜像所依赖的镜像。这些无标签镜像不应该删除，否则会导致上层镜像因为依赖丢失而出错。      

## 镜像构成    
我们可以使用 docker exec 命令进入容器，修改其内容。   
```
$ docker exec -it webserver bash
root@3729b97e8226:/# echo '<h1>Hello, Docker!</h1>' > /usr/share
/nginx/html/index.html
root@3729b97e8226:/# exit
exit
``` 
我们修改了容器的文件，也就是改动了容器的存储层。我们可以通过 docker diff 命令看到具体的改动。       

## 保存容器的存储层，称为新的镜像   
docker commit 的语法格式为：    
```
docker commit [选项] <容器ID或容器名> [<仓库名>[:<标签>]]      
```
使用 docker commit 命令虽然可以比较直观的帮助理解镜像分层存储的概念，但是实际环境中并不会这样使用。     
首先，如果仔细观察之前的 docker diff webserver 的结果，你会发现除了真正想要修改的 /usr/share/nginx/html/index.html 文件外，由于命令的执行，还有很多文件被改动或添加了。这还仅仅是最简单的操作，如果是安装软件包、编译构建，那会有大量的无关内容被添加进来，如果不小心清理，将会导致镜像极为臃肿。    
此外，使用 docker commit 意味着所有对镜像的操作都是黑箱操作，生成的镜像也被称为黑箱镜像，换句话说，就是除了制作镜像的人知道执行过什么命令、怎么生成的镜像，别人根本无从得知。而且，即使是这个制作镜像的人，过一段时间后也无法记清具体在操作的。      
如果使用 docker commit 制作镜像，以及后期修改的话，每一次修改都会让镜像更加臃肿一次，所删除的上一层的东西并不会丢失，会一直如影随形的跟着这个镜像，即使根本无法访问到。      
docker commit 命令除了学习之外，还有一些特殊的应用场合，比如被入侵后保存现场等。但是，不要使用 docker commit 定制镜像，定制行为应该使用Dockerfile 来完成      

## 定制镜像     
Dockerfile 脚本是一个文本文件，其内包含了一条条的指令(Instruction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。         

在一个空白目录中，建立一个文本文件，并命名为 Dockerfile ：     
```
$ mkdir mynginx
$ cd mynginx
$ touch Dockerfile
```
其内容为：    
FROM nginx    
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html     
这个 Dockerfile 很简单，一共就两行。涉及到了两条指令， FROM 和 RUN 。     

所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。     
而FROM 就是指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。     
```
FROM scratch
```
RUN 指令是用来执行命令行命令的。由于命令行的强大能力， RUN 指令在定制镜像时是最常用的指令之一。其格式有两种：     
1. shell 格式： RUN <命令> ，就像直接在命令行中输入的命令一样。刚才写的Dockrfile 中的 RUN 指令就是这种格式。   
```
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```
2. exec 格式： RUN ["可执行文件", "参数1", "参数2"] ，这更像是函数调用中的格式。     
```
FROM debian:jessie
RUN buildDeps='gcc libc6-dev make' \
&& apt-get update \
&& apt-get install -y $buildDeps \
&& wget -O redis.tar.gz "http://download.redis.io/releases/r
edis-3.2.5.tar.gz" \
&& mkdir -p /usr/src/redis \
&& tar -xzf redis.tar.gz -C /usr/src/redis --strip-component
s=1 \
&& make -C /usr/src/redis \
&& make -C /usr/src/redis install \
&& rm -rf /var/lib/apt/lists/* \
&& rm redis.tar.gz \
&& rm -r /usr/src/redis \
&& apt-get purge -y --auto-remove $buildDeps
```
首先，之前所有的命令只有一个目的，就是编译、安装 redis 可执行文件。因此没有必要建立很多层，这只是一层的事情。因此，这里没有使用很多个 RUN 对一一对应不同的命令，而是仅仅使用一个 RUN 指令，并使用 && 将各个所需命令串联起来。
在撰写 Dockerfile 的时候，要经常提醒自己，这并不是在写 Shell 脚本，而是在定义每一层该如何构建。    










