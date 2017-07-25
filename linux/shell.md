### Linux常用命令指南
@Date 2017.05.23

> tail

```
# 实时监听文件写入模式
tail -f jvm.log
```

```
# 查看文件倒数100行内容
tail -100 jvm.log
```

```
# 查看文件倒数100行内容 并 实时监听文件写入
tail -100f jvm.log
```

> awk

* awk ' pattern {action} '
* 默认以空格切分

```
# 在jvm.log日志中搜索匹配com.test.java的行
awk '/com.test.java/' jvm.log
```

```
# 在jvm.log日志中搜索不匹配com.test.java的行
awk '!/com.test.java/' jvm.log
```

```
# 在jvm.log日志中搜索全匹配com.test.java 和 com.test2.java的行
awk '/com.test.java/ && /com.test2.java/' jvm.log
```

```
# 在jvm.log日志中,指定列(第一列)匹配com.test.java的行
awk '$1 ~ /com.test.java/' jvm.log
```

```
# NR(Number of Record) 输出行的行数(默认是换行符,所以是行数)
# NF(Number of Field) 当前行记录里域个数(横向切分的个数)
# 输出本行第一个和最后一个数据
awk '{print NR,NF,$1,$NF}' jvm.log     
```

```
# 输出的时候没有逗号间隔,则输出结果没有分割,是紧挨着的
awk '{print $1 $2}' jvm.log     
```

```
# -F 自定义指定分隔符
awk -F: jvm.log     
```

```
# -F 自定义指定分隔符
awk -F: jvm.log     
```

```
# RS 输入记录分隔符
# BEGIN 表示在处理任意行之前进行的操作
# END 表示在所有输入行处理完后进行处理
# NF(Number of Field) 当前行记录里域个数(横向切分的个数)
cat jvm.log  | grep -v "result" | awk  'BEGIN{RS="2017/05/23"} { split($NF,a,"m"); if(a[1]>1000) {print $0;} } END{}'
```

> grep 

```
# 文件查找
grep 'test' jvm.log
```

```
# 多文件查找
grep 'test' jvm.log gc.log 
```

```
# 在指定目录下查找所有符合关键字的文件
grep 'test' /home/admin/ -r -n
```
```
# -i 不区分大小写查找(默认区分大小写grep)
grep -i 'pattern1' jvm.log
```

```
# 匹配行向上匹配x
grep 'test' -A x

# 匹配行向下匹配x
grep 'test' -B x

# 匹配行上下匹配x
grep 'test' -C x
```

```
# grep 中OR 匹配
# \| : 注意转义
grep 'pattern1\|pattern2' jvm.log

# -E 选项可以用来扩展选项为正则表达式
# 使用-E时,可以直接使用 | 表达OR 
# egrep = grep -E
grep -E 'pattern1|pattern2' jvm
egrep 'pattern1|pattern2' jvm
```

```
# -v 可以实现 NOT 操作
grep -v 'pattern1' jvm.log
```

> find

```
# 在多个目录下查找文件
find /home/admin/ /temp /etc -name \*.log
```

```
# 查找指定大小的文件
# 大于100k的文件
find /home/admin -size +100k

# 小于100k的文件
find /home/admin -size -100k
```

> uniq

```
# 默认去重
uniq -c
```

> netstat

```
# 查看网络链接并输出状态
netstat -nat|awk  '{print $6}'|sort|uniq -c|sort -rn 
```

> top

```
# 显示PID对应的所有线程占用资源
top -Hp pid
```

> printf

```
# 输出TID(线程ID)对应的十六进制表示
printf "%x" TID
```

> date

```
# UNIX的时间戳
date +%s
```

```
# 标准时间 -> UNIX时间戳
date -d "2017-06-19 00:00:00" +%s 
date -d "2017/06/19 00:00:00" +%s
```

```
# UNIX时间戳 -> 标准时间
date -d @1497801600  "+%Y-%m-%d"
date -d @1497801600  "+%Y/%m/%d"
```

> sed

```
# 统计一段时间内的日志
sed -n '/2017-07-16 12:30:00/,/2017-07-16 16:00:00/p' info.log
```

