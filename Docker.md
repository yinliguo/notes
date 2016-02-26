# Docker

### 镜像操作
- 获取镜像
```
docker pull 镜像名
```
- 删除镜像
```
docker rmi 镜像名
```
- 持久化镜像
```
docker save 镜像名 > /tmp/myimage.tar
```
- 加载镜像
```
docker load < /tmp/myimage.tar
```

### 容器操作
- 创建并运行容器
```
docker run --privileged -ti -v /host/mydocker:/guest/mydocker -p 80:80 镜像名:版本 /bin/bash
```
- 创建容器
```
docker create -i -t 镜像名:版本 /bin/bash
```
- 启动容器
```
docker start 容器id
```
- 停止容器
```
docker stop 容器id
```
- 进入容器
```
docker exec -ti 容器id /bin/bash
```
- 删除容器
```
docker rm 容器id
```
- 创建镜像
```
docker commit 容器id 自定义镜像名
```
- 持久化容器
```
docker export 容器id > /tmp/mycontainer.tar
```