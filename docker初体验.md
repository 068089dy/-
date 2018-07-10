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
  -d:让容器在后台运行。
  -P:将容器内部使用的网络端口映射到我们使用的主机上。
# 查看容器端口的映射情况

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
docker container cp [containID]:[/path/to/file] 
# 重命名容器
sudo docker rename oldname newname.
```

### 导入导出
```
# 导出
docker export [id] > arch.tar
# 导入
sudo docker load < arch.tar
```

### 制作容器
新建.dockerignore用于排除不放进容器的文件
```

```
新建Dockerfile
### 提交容器
1.先在[https://hub.docker.com/](https://hub.docker.com/)注册
2.在本机登录
```
docker login
```
3.为本地的 image 标注用户名和版本（这里我只是为了修改base/archlinux，没有自己建镜像）
```
sudo docker image tag base/archlinux 068089dy/koa_arch:0.0.1
```
4.提交
```
sudo docker image push 068089dy/koa_arch:0.0.1
```
5.修改
```
docker commit 698 learn/ping
```
