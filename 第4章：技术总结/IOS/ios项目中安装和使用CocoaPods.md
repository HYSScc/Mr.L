# ios项目中安装和使用CocoaPods

**注意：**
1. 苹果系统升级OS X EL Capitan后改为
```
$sudo gem install -n /usr/local/bin cocoapods
```
2. 因为使用的是ruby.taobao.com源，有些翻墙软件会更新不下来（无反应），需要退出翻墙软件即可。

1.移除现有Ruby默认源
```
$gem sources --remove https://rubygems.org/
```

2.使用新的源
```
$gem sources -a https://ruby.taobao.org/
```  
注意：亲测“ruby.taobao.com”源不能用，改用以下```https://gems.ruby-china.org/```源，需要关掉翻墙软件！

    $ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
    $ gem sources -l
    https://gems.ruby-china.org
    # 确保只有 gems.ruby-china.org



3.验证新源是否替换成功
```
$gem sources -l
```
4.安装CocoaPods

(1)```$sudo gem install cocoapods``` 备注：苹果系统升级OS X EL Capitan后改为```$sudo gem install -n /usr/local/bin cocoapods```
(2)```$pod setup```

5.更新gem
```
$sudo gem update --system
```
6.新建工程，并在终端用cd指令到文件夹内
```
$pod search 第三方
```
7.新建文件 vim “Podfile”，
``` 
$vim Podfile
 ```
写入以下内容并保存 小提示：（终端vim文件 按 i 可编辑 ，esc 退出编辑，：wq  可保存退出）
``` 
platform:ios, '6.0'   
pod 'AFNetworking', '~> 2.3.1'    <-------第三方
```
8.导入第三方库
```
$pod install
```
9.退出终端



## 错误解决办法：

1. “Could not find 'cocoapods' (>= 0) among 35 total gem(s) (Gem::LoadError)”

        1. Remove cocoapods using gem uninstall cocoapods
        2. Install rvm, to do this I followed this steps https://rvm.io/rvm/install
        3. After that reinstall cocoapods using gem install cocoapods
        4. do pod setup
        And after that everything works like a charm!.

        You could notice that I **didn't use sudo**.



解决办法： http://stackoverflow.com/questions/20042102/unable-to-load-gem-cocoa-pods-while-creating-repo



## 参考：
ios项目中安装和使用CocoaPods： http://blog.csdn.net/jjmm2009/article/details/41944959

iOS 最新版 CocoaPods 的安装流程： http://www.cnblogs.com/zxs-19920314/p/4985476.html?utm_source=tuicool&utm_medium=referral
