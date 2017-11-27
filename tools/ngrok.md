### 内网穿透神器(ngrok)
@Date 2016.11.13

---

> 介绍

* 介绍一个内网穿透神器,可以绑定域名和端口.对外提供此域名和端口,允许从外部访问到本机服务

> 下载地址

* [官网地址](https://ngrok.com/download)
* [百度网盘(Windows)](http://pan.baidu.com/s/1b7rW7s) 密码：ep5w
* [百度网盘(Linux)](http://pan.baidu.com/s/1cyeJ4E) 密码：kvt3
* [百度网盘(Mac)](http://pan.baidu.com/s/1bpNLdhl) 密码：nbvs

> 使用命令

* 启动HTTP映射端口命令：

```
ngrok http 8001
```

* 访问：http://127.0.0.1:4040/ 控制
  
* 当我们的机器绑定了多个IP时，通过指定IP来转发映射：

```
ngrok http 192.168.1.101:8006
```  
  
* TCP端口转发：

```
ngrok tcp 22
```