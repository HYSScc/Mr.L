# MQTT-Client-Framework - OC 实现的 MQTT 库

    github: https://github.com/ckrey/MQTT-Client-Framework
    
### 一. MQTT over TCP:
参考：http://www.jianshu.com/p/b093fe6c3f10

1.建立链接
```
MQTTCFSocketTransport *transport = [[MQTTCFSocketTransport alloc] init];//初始化对象
transport.host = @"localhost";//设置MQTT服务器的地址
transport.port = 1883;//设置MQTT服务器的端口（默认是1883，当然，你也可以和你的后台好基友协商~）
self.mySession = [[MQTTSession alloc] init];//初始化MQTTSession对象
self.mySession.transport = transport;//给mySession对象设置基本信息
self.mySession.delegate = self;//设置mySession的代理为APPDelegate，同时不要忘记遵守协议~
[self.mySession connectAndWaitTimeout:30];//设定超时时长，如果超时则认为是连接失败，如果设为0则是一直连接。
```
2.订阅主题

```
//Topic则表示要订阅的主题，Level（qosLevel）表示消息等级。
[self.mySession subscribeToTopic:@"example/#" 
                         atLevel:2 
                subscribeHandler:^(NSError *error, NSArray*gQoss) {
  if (error) {
      NSLog(@"Subscription failed %@", error.localizedDescription);
  } else {
      NSLog(@"Subscription sucessfull! Granted Qos: %@", gQoss);
  }
}];

```
3.收到消息


```Objective-C
- (void)newMessage:(MQTTSession *)session
              data:(NSData *)data
           onTopic:(NSString *)topic
               qos:(MQTTQosLevel)qos
          retained:(BOOL)retained
               mid:(unsigned int)mid{
    DDLogInfo(@"MQTT 订阅到消息:%@", data);
}
```

4.发布消息
```
[self.mySession publishAndWaitData:data
                            onTopic:@"topic"
                             retain:NO
                                qos:MQTTQosLevelAtLeastOnce];
```


### 二. MQTT over websocket:

1.建立链接
  ```Objective-C
    MQTTWebsocketTransport *transport = [[MQTTWebsocketTransport alloc] init];//初始化对象
    self.wsTransport = [[MQTTWebsocketTransport alloc] init];//初始化对象
    self.wsTransport.host = MQTT_Base_URL;//设置MQTT服务器的地址
    self.wsTransport.port = MQTT_Base_PORT;//设置MQTT服务器的端口（默认是11883，当然，你也可以和你的后台好基友协商~）
    self.mqttSession = [[MQTTSession alloc] init];//初始化MQTTSession对象
    self.mqttSession.transport = self.wsTransport;//给mqttSession对象设置基本信息
    [self.mqttSession setClientId:@"lianghuiyuan-id"];
    [self.mqttSession setKeepAliveInterval:60];
    [self.mqttSession setUserName:self.userModel.token];
    [self.mqttSession setPassword:self.userModel.uid];
    [self.mqttSession setCleanSessionFlag:true];
    self.mqttSession.delegate = self;//设置mqttSession的代理，同时不要忘记遵守协议~
    [self.mqttSession connectAndWaitTimeout:30];//设定超时时长，如果超时则认为是连接失败，如果设为0则是一直连接。
  ```

2.订阅主题

```
//Topic则表示要订阅的主题，Level（qosLevel）表示消息等级。
[self.mySession subscribeToTopic:@"example/#" 
                         atLevel:2 
                subscribeHandler:^(NSError *error, NSArray*gQoss) {
  if (error) {
      NSLog(@"Subscription failed %@", error.localizedDescription);
  } else {
      NSLog(@"Subscription sucessfull! Granted Qos: %@", gQoss);
  }
}];

```
3.收到消息


```Objective-C
- (void)newMessage:(MQTTSession *)session
              data:(NSData *)data
           onTopic:(NSString *)topic
               qos:(MQTTQosLevel)qos
          retained:(BOOL)retained
               mid:(unsigned int)mid{
    DDLogInfo(@"MQTT 订阅到消息:%@", data);
}
```

4.发布消息
```
[self.mySession publishAndWaitData:data
                            onTopic:@"topic"
                             retain:NO
                                qos:MQTTQosLevelAtLeastOnce];
```

### 三. make MQTT by MQTTSessionManager:
1.建立链接
```
if (!self.manager) {
  self.manager = [[MQTTSessionManager alloc] init];
  self.manager.delegate = self;
  self.manager.subscriptions = [[NSMutableDictionary alloc] init];
  NSMutableDictionary *mySubscript = [[NSMutableDictionary alloc]init];
  [mySubscript setValue:@(MQTTQosLevelAtLeastOnce) forKey:Topic_System_Time];
  self.manager.subscriptions = mySubscript;
  [self.manager connectTo:MQTT_Base_URL
                     port:MQTT_Base_PORT
                      tls:NO
                keepalive:60
                    clean:true
                     auth:true
                     user:self.userModel.token
                     pass:self.userModel.uid
                     will:false
                willTopic:nil
                  willMsg:nil
                  willQos:MQTTQosLevelAtMostOnce
           willRetainFlag:false
             withClientId:[self randomStr:13]];
} else {
  [self.manager connectToLast];
}

- (NSString *)randomStr:(int)num
{
    NSString *feed= @"0123456789qwertyuioplkjhgfdsazxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";
    NSMutableString * tmp =  [[NSMutableString alloc]initWithString:@""];
    double midRandom;
    int position;
    for(int i=0; i<num; i++)  {
        midRandom = ceil((double)(1+arc4random()%99)/100 *100000000);
        position = fmod(midRandom, feed.length);
        [tmp appendString:[NSString stringWithFormat:@"%@", [feed substringWithRange:NSMakeRange(position, 1)]]];
    }
    NSString *timeSp = [NSString stringWithFormat:@"%ld", (long)[[NSDate date] timeIntervalSince1970]];
    return [NSString stringWithFormat:@"%@%@", [timeSp substringWithRange:NSMakeRange(timeSp.length-10,10)], tmp];
}

```

2.订阅主题

参考：https://github.com/ckrey/MQTT-Client-Framework/issues/136
```
manager.subscriptions = [@{TOPIC: @(0),@"$SYS/#": @(0)} mutableCopy];
```

3.收到消息

```
/**
 * MQTTSessionManagerDelegate
 */
- (void)handleMessage:(NSData *)data onTopic:(NSString *)topic retained:(BOOL)retained
{
    /*
     * MQTTClient: process received message
     */
     DDLogInfo(@"收到订阅消息(%@).....data=%@", topic, data);
}
```

4.发布消息
```
/** publishes data on a given topic at a specified QoS level and retain flag

 @param data the data to be sent. length may range from 0 to 268,435,455 - 4 - _lengthof-topic_ bytes. Defaults to length 0.
 @param topic the Topic to identify the data
 @param retainFlag if YES, data is stored on the MQTT broker until overwritten by the next publish with retainFlag = YES
 @param qos specifies the Quality of Service for the publish
 qos can be 0, 1, or 2.
 @return the Message Identifier of the PUBLISH message. Zero if qos 0. If qos 1 or 2, zero if message was dropped
 @note returns immediately.
 */
- (UInt16)sendData:(NSData *)data topic:(NSString *)topic qos:(MQTTQosLevel)qos retain:(BOOL)retainFlag;
```

## 四.Q&A：
1.在Block内部建立MQTT链接成功，但是subscribe topic失败。

    解决办法：MQTT建立链接和subscribe topic都放在block外部即可。
        
2.使用MQTTSessionManager建立MQTT链接的时候，提示“eventCode: connection closed (2) (null)“，“eventCode: connection closed by broker (5) (null)”

    解决办法：[MQTT-3.1.2-13] If the Will Flag is set to 0, then the Will QoS MUST be set to 0 (0x00).
        
    参考：https://github.com/ckrey/MQTT-Client-Framework/issues/91
             
3.CFNetwork internal error (0xc01a:/SourceCache/CFNetwork/CFNetwork-711.4.6/Foundation/NSURLProtocol.mm:182)

    解决办法:
        