### Sublime功能总结
@Date 2016.11.13

---

> 安装Package Control

* 命令安装sublime的包管理器

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

* 包管理器快捷键

```
ctrl + shift + P -> install
```

* 预览MarkDown快捷键

```
ctrl + shift + P -> preivew
```