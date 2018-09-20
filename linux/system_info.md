### Linux命令(查看系统信息)
@Date 2017.07.10

> 查询命令

```
# head -n 1 /etc/issue   # 查看操作系统版本
# hostname               # 查看计算机名
# env                    # 查看环境变量
# lsmod                  # 列出加载的内核模块
# getconf LONG_BIT       # 查询系统64位还是32位
# uname -r               # 查询系统内核版本
# uname -a               # 查询系统所有版本信息
```

> 查询CPU详情
```bash
# 查看CPU所有信息
cat /proc/cpuinfo

# 查询CPU物理个数(本机器有几个CPU单元)
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l

# 查询逻辑CPU个数(物理CPU个数 * 单个CPU核数)
cat /proc/cpuinfo |grep "processor"|wc -l  

# 查询单个CPU核数
cat /proc/cpuinfo |grep "cores"|uniq  

# 查询HT(超线程)相关
cat /proc/cpuinfo |grep "sibling"|uniq
cat /proc/cpuinfo | grep "cpu cores"|uniq

# 查询CPU32位还是64位
getconf LONG_BIT
```