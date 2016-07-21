# 单例模式Singleton

单例模式的意思就是只有一个实例。单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。

**1.单例模式的要点：**

　　显然单例模式的要点有三个；一是某个类只能有一个实例；二是它必须自行创建这个实例；三是它必须自行向整个系统提供这个实例。

**2.单例模式的优点：**

　　1.实例控制：Singleton 会阻止其他对象实例化其自己的 Singleton 对象的副本，从而确保所有对象都访问唯一实例。
　　2.灵活性：因为类控制了实例化过程，所以类可以更加灵活修改实例化过程

**单例模式需要达到的目的**
1. 封装一个共享的资源
2. 提供一个固定的实例创建方法
3. 提供一个标准的实例访问接口

**IOS中的单例模式**

在objective-c中要实现一个单例类，至少需要做以下四个步骤： 
1. 为单例对象实现一个静态实例，并初始化，然后设置成nil
2. 实现一个实例构造方法检查上面声明的静态实例是否为nil，如果是则新建并返回一个本类的实例
3. 重写allocWithZone方法，用来保证其他人直接使用alloc和init试图获得一个新实力的时候不产生一个新实例
4. 适当实现allocWitheZone，copyWithZone，release和autorelease



**Singleton.h:**

```
// 帮助实现单例设计模式

// .h文件的实现
#define SingletonH(methodName) + (instancetype)shared##methodName;

// .m文件的实现
#if __has_feature(objc_arc) // 是ARC
#define SingletonM(methodName) \
static id _instace = nil; \
+ (id)allocWithZone:(struct _NSZone *)zone \
{ \
if (_instace == nil) { \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instace = [super allocWithZone:zone]; \
}); \
} \
return _instace; \
} \
\
- (id)init \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instace = [super init]; \
}); \
return _instace; \
} \
\
+ (instancetype)shared##methodName \
{ \
return [[self alloc] init]; \
} \
+ (id)copyWithZone:(struct _NSZone *)zone \
{ \
return _instace; \
} \
\
+ (id)mutableCopyWithZone:(struct _NSZone *)zone \
{ \
return _instace; \
}

#else // 不是ARC

#define SingletonM(methodName) \
static id _instace = nil; \
+ (id)allocWithZone:(struct _NSZone *)zone \
{ \
if (_instace == nil) { \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instace = [super allocWithZone:zone]; \
}); \
} \
return _instace; \
} \
\
- (id)init \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instace = [super init]; \
}); \
return _instace; \
} \
\
+ (instancetype)shared##methodName \
{ \
return [[self alloc] init]; \
} \
\
- (oneway void)release \
{ \
\
} \
\
- (id)retain \
{ \
return self; \
} \
\
- (NSUInteger)retainCount \
{ \
return 1; \
} \
+ (id)copyWithZone:(struct _NSZone *)zone \
{ \
    return _instace; \
} \
 \
+ (id)mutableCopyWithZone:(struct _NSZone *)zone \
{ \
    return _instace; \
}

#endif
```
使用Singleton.h我们在需要实现单例模式的类中轻易就可以实现单例模式了，假设有个工具类MJSoundTool。因此就可以这么做实现单例模式。

**实现头文件MJSoundTool.h**
```
#import <Foundation/Foundation.h>

#import "Singleton.h"

@interface MJSoundTool : NSObject

SingletonH(SoundTool)

@end
```

**实现文件MJSoundTool.m**
```
#import "MJSoundTool.h"

@implementation MJSoundTool

SingletonM(SoundTool)

@end
```


## 参考:

1. iOS设计模式——单例模式 http://blog.csdn.net/lovefqing/article/details/8516536
2. IOS单例模式(Singleton) http://www.cnblogs.com/lyanet/archive/2013/01/11/2856468.html
3. IOS开发之单例设计模式（完整正确版本）: http://www.cnblogs.com/JackieHoo/p/5050010.html