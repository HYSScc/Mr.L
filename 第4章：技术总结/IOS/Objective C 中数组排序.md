# Objective C 中数组排序


```

NSArray *boxData = dic[@"data"];

//构建排序描述器
NSSortDescriptor *timeStampDesc = [NSSortDescriptor sortDescriptorWithKey:@"timestamp" ascending:NO];
NSSortDescriptor *contentDesc = [NSSortDescriptor sortDescriptorWithKey:@"content" ascending:YES];
//把排序描述器放进数组里，放入的顺序就是你想要排序的顺序
//我这里是：首先按照年龄排序，然后是车的名字，最后是按照人的名字
NSArray *descriptorArray = [NSArray arrayWithObjects:timeStampDesc,contentDesc, nil];
self.messageBox = [boxData sortedArrayUsingDescriptors: descriptorArray];
//                                                    self.messageBox = boxData;
DDLogVerbose(@"获取到的消息盒子数据:%@", self.messageBox);

```
注意：其中ascending:YES表示升序，ascending:NO表示降序


## 参考：

Objective C中数组排序几种情况的总结： http://my.oschina.net/pengloo53/blog/173810