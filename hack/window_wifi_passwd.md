> windows系统下,CMD命令获取已连接的WIFI信息(含密码)

```
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear
```
