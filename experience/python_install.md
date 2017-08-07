### Linux通过Anaconda安装管理python
@Date 2017.05.17

> [Anaconda官网下载地址](https://www.continuum.io/downloads)

1. 根据不同系统下载不同环境sh
2. Linux如下: wget https://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh
3. 安装命令: bash Anaconda3-4.3.1-Linux-x86_64.sh
4. 注意: 安装完成之后不会改变系统默认的自带的python版本(一般系统自带为2.7)
    1. 上面下载的是Anaconda3版本的包,期望使用python3,那么需要如下操作
    2. /bin/conda create --name python3 python=3.3  # conda会按照3.3版本去搜索对应的python包安装
    3. source activate python3 # 生效
   
> 使用Anaconda包管理(和pip类似)

```
# 安装xxx
conda install xxx

# 查看已经安装的packages
conda list

# 查看某个指定环境的已安装包
conda list -n python3

# 向指定环境安装xxx
conda install -n python3 xxx

# 更新指定环境package
conda update -n python3 xxx
 
# 删除指定环境package
conda remove -n python3 xxx
```

