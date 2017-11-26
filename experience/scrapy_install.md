### Linux下安装Scrapy
@Date 2017.05.18


> 基本环境

* Ubuntu
* Python 2.7 (安装教程详见:5.4)
   
> 安装命令

```
$ sudo apt-get update

$ sudo apt-get install python-dev
$ sudo apt-get install libevent-dev
$ sudo apt-get install libxml2 libxml2-dev
$ sudo apt-get install python-lxml
$ sudo apt-get install python-twisted python-libxml2 python-simplejson

$ wget http://pypi.python.org/packages/source/p/pyOpenSSL/pyOpenSSL-0.13.tar.gz
$ tar -zxvf pyOpenSSL-0.13.tar.gz
$ cd pyOpenSSL-0.13
$ sudo python setup.py install

$ wget http://pypi.python.org/packages/source/p/pycrypto/pycrypto-2.5.tar.gz
$ cd pycrypto-2.5
$ sudo python setup.py install

$ wget http://peak.telecommunity.com/dist/ez_setup.py
$ sudo python ez_setup.py
$ sudo easy_install -U w3lib
$ sudo apt-get install python-pip
$ sudo pip install pyasn1 --upgrade
$ sudo easy_install Scrapy
```

> 注意事项

* 在安装以上包之前，执行一次apt-get update,之前自己弄的时候,就是因为有的版本不够新,安装包总是失败或者编译失败
* 如果其中有报错pyasn1版本过低,可以执行上面的upgrade升级
* 在安装libxml2的时候卡了好久,就是因为其他包版本过低,看别人的博客解决办法好少
