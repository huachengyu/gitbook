### Nginx报错Primary script unknown
@Date 2017.05.21

> 现象

* Nginx + Php 服务,遇到访问网站出现file not found的错误,查看Nginx日志,发现以下错误日志:

```
2016/05/19 23:28:43 [error] 28239#0: *27 FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream, client: ..........., server: _, request: "GET / HTTP/1.1", upstream: "fastcgi://127.0.0.1:9000", host: "........"
```
   
> 基本环境

* CentOS release 6.5
* PHP 5.3
* Nginx/1.0.15

> 解决方案

1. Nginx配置文件问题

* Nginx配置文件会读取放置网站.php文件的目录,如果目录找不到,会出现此错误.
需要在配置文件的location内部配置php文件的root目录(如下),这样nginx才会找到.

```
location ~ \.php$  {
        root           /usr/share/nginx/html;
        try_files $uri =404;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
```

2. 目录权限问题

* 我在配置的过程中,以上文件都是对的,但是发现还是报错,最后调查发现是目录权限不对,需要给网站的php文件所在的目录给nginx用户权限.
  比如网站文件目录为/usr/share/nginx/html
  执行以下命令,分别给目录可读可执行,给文件读写权限.
  
```
find nginx -type d  -exec chmod 755 {} \;
find nginx -iname "*.php"  -exec chmod 644 {} \;
```

