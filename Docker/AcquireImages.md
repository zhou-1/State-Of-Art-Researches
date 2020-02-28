# Ways    

从 Docker Registry 获取镜像的命令是 docker pull 。    
其命令格式为：    
docker pull [选项] [Docker Registry地址]<仓库名>:<标签>      

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



