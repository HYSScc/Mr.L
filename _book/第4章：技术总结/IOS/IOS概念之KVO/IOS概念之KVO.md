# IOS概念之KVO
在一个复杂的，有状态的系统中，当一个对象的状态发生改变，如何通知系统，并对状态改变做出相应的行为是必需考虑的一个问题，在iOS中为这类问题提供了4种解决方法： 

    1. NSNotifiactaion和NSNotificationCenter：通知中心 
    2. Delegates：代理， 
    3. Callback：回调， 
    4. KVO（Key-Value Observing）：键值观察 

这篇文章就来说说通过KVO通知系统状态发生改变的用法。 

**KVO是什么？** 

KVO是Object-C中定义的一个通知机制，其定义了一种对象间监控对方状态的改变，并做出反应的机制。对象可以为自己的属性注册观察者，当这个属性的值发生了改变，系统会对这些注册的观察者做出通知。其用途十分广泛，比方说，你的下载进度条是根据下载百分比决定的，那么，可以通过观察下载百分比的改变，刷新进度条的样式，来直观的反应下载进度等等。 

**KVO的用法 **

根据上面的描述，一个KVO的用法主要就涉及3个部分： 

**1.**为对象的属性注册观察者：对象通过调用下面这个方法为属性添加观察者 

```
- (void)addObserver:(NSObject *)observer  
         forKeyPath:(NSString *)keyPath  
            options:(NSKeyValueObservingOptions)options  
            context:(void *)context 
```
            
* observer: 观察者对象. 其必须实现方法observeValueForKeyPath:ofObject:change:context:.
* keyPath: 被观察的属性，其不能为nil.
* options: 设定通知观察者时传递的属性值，是传改变前的呢，还是改变后的，具体的设定可以这儿：https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Protocols/NSKeyValueObserving_Protocol/Reference/Reference.html#//apple_ref/doc/c_ref/NSKeyValueObservingOptions，通常设置为NSKeyValueObservingOptionNew。
* context: 一些其他的需要传递给观察者的上下文信息，通常设置为nil

观察者接收通知，并做出处理:观察者通过实现下面的方法，完成对属性改变的响应： 

**2.**为对象的属性注册观察者：对象通过调用下面这个方法为属性添加观察者 

```
- (void)observeValueForKeyPath:(NSString *)keyPath  
                      ofObject:(id)object  
                        change:(NSDictionary *)change  
                       context:(void *)context 
```


* keyPath: 被观察的属性，其不能为nil.
* object: 被观察者的对象.
* change: 属性值，根据上面提到的Options设置，给出对应的属性值
* context: 上面传递的context对象。
                  
**3.**清除观察者:对象通过下面这个方法移除观察者： 
```
- (void)removeObserver:(NSObject *)anObserver forKeyPath:(NSString *)keyPath  
```

* 注意事项： 

使用KVO消息传递机制有两个要求：

    （1)观察者必须知道被观察对象，即在同一作用域。
    
    （2）观察者还需要知道被观察对象的生命周期，因为在销毁发送者对象之前，需要取消观察者的注册。 
    
另外：如果计划在Core Data对象上使用KVO，需要知道这跟一般的KVO使用方法有点不同。还必须结合Core Data的故障机制(faulting mechanism)，一旦core data出现了故障，它将会触发其属性对应的观察者(即使这些属性值没有发生改变)。 

* 一些好的实践 

 1. 当一个观察者观察多个对象的相同属性（即不同Object，但是KeyPath相同），可通过设定静态的Context变量来区分不同的通知。
 
 2. 使用NSStringFromSelector(@selector(method))来获取KeyPath，而不是直接通过NSString写属性名，这样编译器可以帮助发现属性名中的Typo。

 3. 通过方法：+ (NSSet *)keyPathsForValuesAffectingValueForKey:(NSString *)key，通过一个Key观察多个属性值的改变
。



