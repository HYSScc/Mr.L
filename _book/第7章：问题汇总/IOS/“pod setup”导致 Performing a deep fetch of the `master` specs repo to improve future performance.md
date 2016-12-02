# “pod setup”导致 Performing a deep fetch of the `master` specs repo to improve future performance


更新cocoapod的时候会出现 “Performing a deep fetch of the `master` specs repo to improve future performance” 的错误。

纠结半天，是因为pod steup的时候创建master这个库，没成功，之后就算移除镜像重新安装的话 默认是从matser库里获取，导致安装不成功。

**解决办法**是移除master库，重新创建。
```
pod repo upadte --verbose

rm -rf ~/.cocoapods/repos/master
```
再重新setup 
```
pod setup
```


## 参考：

 Performing a deep fetch of the `master` specs repo to improve future performance： http://blog.csdn.net/a_ellisa/article/details/51556685