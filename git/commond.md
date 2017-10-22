### Git命令
@Date 2016.05.18

> Git配置

---

##### 1.设置Git的user name和email
```
$ git config --global user.name "登陆用户名"
$ git config --global user.email "邮箱"
```

##### 2.生成SSH KEY
```
$ ssh-keygen -t rsa -C "邮箱"
```

##### 3.测试连接
```
$ ssh git@github.com
```

> 基本命令

---

##### 1.获取远程代码到本地
```
$ git clone url
```

##### 2.本地仓库初始化
```
$ git init
```

##### 3.添加到本地仓库
```
$ git add
```

##### 4.把修改的文件放到暂存区
```
$ git stage
```

##### 5.把文件从暂存区撤出(例如commit时不想提交)
```
$ git reset
```

##### 6.添加到远程仓库
```
$ git remote add origin
```

##### 7.提交变更到暂存区
```
$ git commit -m "本次提交log"
```

##### 8.提交整个git所有变更（尽量少用）
```
$ git commit -a -m "本次提交log"  
```

##### 9.把本地更新到远程
```
$ git push 
```

##### 10.把远程代码merge到本地
```
$ git pull origin master
```

##### 11.把远程代码最新版本获取到本地
```
$ git fetch
```

##### 12.删除远程仓库固定文件
```
$ git rm
```

##### 13.回滚固定head版本
```
$ git reset --hard HEAD
```

> 进阶命令

---

##### 1.创建分支
```
$ git branch 分支名
```

##### 2.切换到某一个分支
```
$ git checkout 分支名
```

##### 3.上面两个命令合一
```
$ git checkout -b 分支名
```

##### 4.合并分支到master上(要回到master上实施此命令)
```
$ git merge 分支名
```

##### 5.删除分支
```
$ git branch -d 分支名
```
