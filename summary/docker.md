#### Docker操作总结
@Date 2018.08.24

> 基本操作

* 登录远程镜像仓库
```bash
docker login --username=hua registry.cn-beijing.aliyuncs.com
```

> 镜像操作

* 根据指定路径的Dockerfile打镜像, 指定本地Tag名
```bash
docker build -t tag -f docker-config/Dockerfile .
```

* 修改本地tag改为远程专有地址仓库tag
```bash
docker tag local_tag registry.cn-beijing.aliyuncs.com/[命名空间]/[仓库名称]:[版本]
```

* PUSH本地镜像到远程仓库
```bash
docker push registry.cn-beijing.aliyuncs.com/[命名空间]/[仓库名称]:[版本]
```

* 删除无容器运行的镜像
```bash
docker rmi [imageID]
```

> 容器操作

* 启动容器,支持传入参数
```bash
docker run -it -d -e spring.profiles.active="dev" [REPOSITORY]:[TAG]
```

* 停止容器
```bash
docker stop [容器ID]
```

* 删除已停止的容器
```bash
docker container rm [容器ID]
```