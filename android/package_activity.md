#### 获取APK的包名(package和activity)
@Date 2017.04.20

> adb方式

1. 安装此APK到一安卓手机上,并通过USB调试模式连接电脑
2. 查询手机是否连接成功
```
   adb devices
```
3. 监听logcat日志并输出到指定目录文件
```
    adb logcat>C:/log.log
```
4. 此时直接打开待测APP
5. ctrl + c 关闭日志输出
6. 查看输出的日志文件,在其中搜索以下例子(以淘宝举例),Displayed后面以/切分的就是package/activity
```
04-18 19:15:12.443  1813  1922 I ActivityManager: [AppLaunch] Displayed Displayed com.alibaba.android.rimet/.biz.SlideActivity: +4s455ms (total +6s273ms)
04-18 19:15:12.443  1813  1922 D ActivityManager: AP_PROF:AppLaunch_LaunchTime:com.alibaba.android.rimet/.biz.SlideActivity:4455:7479393
```