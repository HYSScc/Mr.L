# DDLog-让xCode控制台输出文本有颜色
    CocoaLumberjack: https://github.com/CocoaLumberjack/CocoaLumberjack
    XcodeColors: https://github.com/robbiehanson/XcodeColors


### 1. 安装XcodeColors插件
下载地址：https://github.com/robbiehanson/XcodeColors

**安装方法：**

* 下载并解压缩XcodeColors-master.zip
打开XcodeColors项目，编译项目可以自动将插件安装至~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/XcodeColors.xcplugin
重新启动Xcode
再次打开XcodeColors项目
运行TestXcodeColors测试插件是否安装成功


### 2. 下载CocoaLumberjack开源框架
下载地址：https://github.com/CocoaLumberjack/CocoaLumberjack


### 3. 新建项目，将CocoaLumberjack拖入项目中
    Obj-C version via CocoaPods，通过Pods安装
    
    platform :ios, '7.0'
    pod 'CocoaLumberjack'
### 4. 创建Common.h

```
#ifdef DEBUG
static const int ddLogLevel = LOG_LEVEL_VERBOSE;
#else
static const int ddLogLevel = LOG_LEVEL_OFF;
#endif
```
或

```
#ifdef LUMBERJACK
    #define LOG_LEVEL_DEF ddLogLevel
    #import <CocoaLumberjack/CocoaLumberjack.h>
    #ifndef myLogLevel
        #ifdef DEBUG
            static const DDLogLevel ddLogLevel = DDLogLevelWarning;
        #else
            static const DDLogLevel ddLogLevel = DDLogLevelWarning;
        #endif
    #else
        static const DDLogLevel ddLogLevel = myLogLevel;
    #endif
#else
    #ifdef DEBUG
        #define DDLogVerbose NSLog
        #define DDLogWarn NSLog
        #define DDLogInfo NSLog
        #define DDLogError NSLog
    #else
        #define DDLogVerbose(...)
        #define DDLogWarn(...)
        #define DDLogInfo(...)
        #define DDLogError(...)
        #endif
#endif
```

### 5. 在xxx-Prefix.pch中添加Common.h的引入
```
#import "Common.h"
```


### 6. 实例化DDLog

在```- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions```方法中设置DDLog
```
// 实例化 lumberjack
[DDLog addLogger:[DDTTYLogger sharedInstance]];
// 允许颜色
[[DDTTYLogger sharedInstance] setColorsEnabled:YES];
```
**使用方法**
lumberjack提供了四种Log方法

```
DDLogError(@"错误信息"); // 红色
DDLogWarn(@"警告"); // 橙色
DDLogInfo(@"提示信息"); // 默认是黑色
DDLogVerbose(@"详细信息"); // 默认是黑色
```
其他
如果要修改Log输出的颜色可以使用如下代码：
```
[[DDTTYLogger sharedInstance] setForegroundColor:[UIColor blueColor] backgroundColor:nil forFlag:LOG_FLAG_INFO];
```


### 参考：
    iOS第三方库-CocoaLumberjack-DDLog: http://blog.sina.com.cn/s/blog_7b9d64af0101kkiy.html
    iOS开发中善用日志记录工具: http://blog.devzeng.com/blog/using-logging-tools-in-ios.html
    CocoaLumberjack——带颜色的Log: http://www.cnblogs.com/liufan9/p/3552832.html

