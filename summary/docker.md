#### Docker操作总结
@Date 2018.08.24

> 基本操作

* 登录远程镜像仓库
```bash
docker login --username=hua registry.cn-beijing.aliyuncs.com
```
------
> 镜像操作

* 根据指定路径的Dockerfile打镜像, 指定本地Tag名
```bash
docker build -t tag -f docker-config/Dockerfile .
```
---

* 修改本地tag改为远程专有地址仓库tag
```bash
docker tag local_tag registry.cn-beijing.aliyuncs.com/[命名空间]/[仓库名称]:[版本]
```
---

* PUSH本地镜像到远程仓库
```bash
docker push registry.cn-beijing.aliyuncs.com/[命名空间]/[仓库名称]:[版本]
```
---

* 删除无容器运行的镜像
```bash
docker rmi [imageID]
```
---

* 利用全拼删除有重复镜像ID的镜像
```
docker rmi [REPOSITORY]:TAG
```
---

* 删除所有镜像
```
docker rmi $(docker images -q)
```

------

> 容器操作

* 启动容器,支持传入参数
```bash
docker run -it -d -p 7001:7001 -e spring.profiles.active="dev" [REPOSITORY]:[TAG]
```
---

* 停止容器
```bash
docker stop [容器ID]
```
---
* 停止所有容器
```bash
docker stop $(docker ps -a -q)
```
---

* 删除已停止的容器
```bash
docker container rm [容器ID]
```
---

* 删除所有容器
```bash
docker rm $(docker ps -a -q)
```
------

* 使用docker-compose编排模板
```
# 使用目录下默认的docker-compose.yaml创建容器
docker-compose up -d 

# 使用指定的yaml文件创建容器
docker-compose -f xxx.yaml up
```
