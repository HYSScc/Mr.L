# IOS概念之KVO（键值监听）

知识点介绍:

    Key-Value Observing (简写为KVO)：当指定的对象的属性被修改了，允许对象接受到通知的机制。每次指定的被观察对象的属性被修改的时候，KVO都会自动的去通知相应的观察者。
    
    

## 1，注册与解除注册
如果我们已经有了包含可供键值观察属性的类，那么就可以通过在该类的对象（被观察对象）上调用名为 NSKeyValueObserverRegistration 的 category 方法将观察者对象与被观察者对象注册与解除注册：
```
- (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(void *)context;
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;

```
这两个方法的定义在 Foundation/NSKeyValueObserving.h 中，NSObject，NSArray，NSSet均实现了以上方法，因此我们不仅可以观察普通对象，还可以观察数组或结合类对象。在该头文件中，我们还可以看到 NSObject 还实现了 NSKeyValueObserverNotification 的 category 方法（更多类似方法，请查看该头文件）：
```
- (void)willChangeValueForKey:(NSString *)key;
- (void)didChangeValueForKey:(NSString *)key;
```
这两个方法在手动实现键值观察时会用到，暂且不提。

值得注意的是：不要忘记解除注册，否则会导致资源泄露。


## 2，设置属性

将观察者与被观察者注册好之后，就可以对观察者对象的属性进行操作，这些变更操作就会被通知给观察者对象。注意，只有遵循 KVO 方式来设置属性，观察者对象才会获取通知，也就是说遵循使用属性的 setter 方法，或通过 key-path 来设置：
```
[target setAge:30];
[target setValue:[NSNumber numberWithInt:30] forKey:@"age"];

```
## 3，处理变更通知

观察者需要实现名为 NSKeyValueObserving 的 category 方法来处理收到的变更通知：
```
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context;
```




## 参考：

[深入浅出Cocoa]详解键值观察（KVO）及其实现机理： http://www.cppblog.com/kesalin/archive/2012/11/17/kvo.html

KVC 和 KVO: http://objccn.io/issue-7-3/

通讯录示例 里的 DetailViewController 和 Contact 类详解了这个用法: https://github.com/objcio/issue-7-contact-editor

Key-Value Observing机制: http://www.cnblogs.com/pengyingh/articles/2383629.html

