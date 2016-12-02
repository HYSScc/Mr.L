# iOS 视图控制对象生命周期

    iOS视图控制对象生命周期:
    init、viewDidLoad、viewWillAppear、viewDidAppear、viewWillDisappear、viewDidDisappear的区别及用途
    init－初始化程序
    viewDidLoad－加载视图
    viewWillAppear－UIViewController对象的视图即将加入窗口时调用；
    viewDidApper－UIViewController对象的视图已经加入到窗口时调用；
    viewWillDisappear－UIViewController对象的视图即将消失、被覆盖或是隐藏时调用；
    viewDidDisappear－UIViewController对象的视图已经消失、被覆盖或是隐藏时调用；
    viewVillUnload－当内存过低时，需要释放一些不需要使用的视图时，即将释放时调用；
    viewDidUnload－当内存过低，释放一些不需要的视图时调用。