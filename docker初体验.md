### 安装并启动
```
sudo pacman -S docker
sudo systemctl start docker
sudo docker info  #查看信息
```

### 修改源
/etc/default/docker
```
DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"
```

### 基础命令
```
# 列出本地主机上的镜像
docker images
# 搜索镜像
sudo docker search [image]
# 下载容器
sudo docker image pull [image]
# 新建并运行容器
sudo docker container run [image]
# 开始容器
sudo docker container start [name]
# 列出正在运行的容器
sudo docker container ls
# 列出所有容器
sudo docker container ls --all
# 删除一个容器
sudo docker container rm [id]
# 杀死一个正在运行的容器
sudo docker container kill [id]
# 进入一个正在运行的 docker 容器
docker container exec -it [id] /bin/bash
# 将文件拷贝到本机
docker container cp [containID]:[/path/to/file] .
```

### 制作容器
新建.dockerignore用于排除不放进容器的文件
```

```
新建Dockerfile
