### CentOS下安装WordPress
@Date 2016.05.15

---

> 1.创建一个启动用户

```
useradd apache
passwd apache
```

> 2.安装流程

> 2.1执行命令

```
# 配置防火墙，80端口可用
sudo vi /etc/sysconfig/iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
sudo service iptables restart

# 安装Apache
yum install httpd httpd-devel
/etc/init.d/httpd start

# 安装PHP
yum install php php-devel
/etc/init.d/httpd restart

# 安装PHP扩展
yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc

# 安装完重启Apache
/etc/init.d/httpd restart

# 把WordPress压缩包解压后文件发到www目录下
cp -rf WordPress/* /var/www/html/
chown -R apache:apache /var/www/html

# 允许WordPress访问服务器,安装FTP
yum -y install vsftpd
service vsftpd start

```

> 2.2安装mysql环境

* [见以下链接](http://www.cnblogs.com/xiaoluo501395377/archive/2013/04/07/3003278.html)

> 2.3启动WordPress

* 访问服务器localhost/index.php，如果出现成功页面，则代表WordPress环境已经OK，之后可以按照WordPress提示的教程设置各种配置。