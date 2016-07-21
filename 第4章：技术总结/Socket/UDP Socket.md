# UDP socket

## 0. 广播地址不建议使用“255.255.255.255”
“255.255.255.255” 是一种受限的广播地址，常用于在计算机不知道自己IP地址的时候发送，比如设备启动时向DHCP服务器索要地址等等，一般情况下，路由器不会转发目标为受限广播地址的广播包。

而且，有些路由器/Wi-Fi热点不支持该广播地址（例如：用Android手机做Wi-Fi热点的时候），因此在程序中会出现“ENETUNREACH (Network is unreachable)”的异常，因此，为了保证程序成功发送广播包，建议使用直接广播地址，例如：当前IP地址是 192.168.1.100，子网掩码是 255.255.255.0 的情况下，广播地址为：192.168.1.255，（具体的推算方法这里就不展开了，可以参考计算机网络相关书籍）

--------------------------------------------------------------
    由于路由器厂商或者手机厂商设定，路由器或热点AP的广播地址不一定是本机ip(前3位)+.255。
    所以，如果直接用 ip(前3位)+.255 发广播包的话，有可能对方收不到广播包。此时应该按照以下方法获取路由器 或手机热点(AP)的广播地址。   
                                                --by Mr.L
--------------------------------------------------------------


## 一. IOS 获取 AP(路由器/热点) 的广播IP

```
// 获取手机IP
- (NSString *)myBroadcastIPAddress {
    
    NSString *address = @"ERROR";
    struct ifaddrs *interfaces = NULL;
    struct ifaddrs *temp_addr = NULL;
    int success = 0;
    // retrieve the current interfaces - returns 0 on success
    success = getifaddrs(&interfaces);
    if (success == 0) {
        // Loop through linked list of interfaces
        temp_addr = interfaces;
        while(temp_addr != NULL) {
            if(temp_addr->ifa_addr->sa_family == AF_INET) {
                //~NSLog(@"INTERFACES: %@", [NSString stringWithUTF8String:temp_addr->ifa_name]);
                // Check if interface is en0 which is the wifi connection on the iPhone
                if (
                    ([[NSString stringWithUTF8String:temp_addr->ifa_name] isEqualToString:@"en0"])
                    || ([[NSString stringWithUTF8String:temp_addr->ifa_name] hasPrefix:@"bridge"])
                    )
                {
                    // Get NSString from C String
                    // IP地址
                    // address = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_addr)->sin_addr)];
//                    DDLogVerbose(@"IP地址:%@",[NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_addr)->sin_addr)]);
                    
                    // address = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_netmask)->sin_addr)];
//                    DDLogVerbose(@"子网掩码:%@",[NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_netmask)->sin_addr)]);
                    
                    address = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_dstaddr)->sin_addr)];
                    DDLogVerbose(@"广播地址:%@",[NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_dstaddr)->sin_addr)]);
                    
                }
                
            }
            
            temp_addr = temp_addr->ifa_next;
        }
    }
    // Free memory
    freeifaddrs(interfaces);
    return address;
    
}
```


## 二. Android 获取 AP(路由器/热点) 的广播IP
如何得到本网段的直接广播地址呢，下面是stackoverflow上面有位大牛分享的代码：
```
public static InetAddress getBroadcastAddress(Context context) throws UnknownHostException {
    WifiManager wifi = (WifiManager)context.getSystemService(Context.WIFI_SERVICE);
    DhcpInfo dhcp = wifi.getDhcpInfo();
    if(dhcp==null) {
        return InetAddress.getByName("255.255.255.255");
    }
    int broadcast = (dhcp.ipAddress & dhcp.netmask) | ~dhcp.netmask;
    byte[] quads = new byte[4];
    for (int k = 0; k < 4; k++)
        quads[k] = (byte) ((broadcast >> k * 8) & 0xFF);
    return InetAddress.getByAddress(quads);
}
```
直接使用该函数即可得到正确的“广播地址”，通过setAddress函数设置到DatagramPacket对象中即可。


### Android设置为Wi-Fi热点时的广播地址
这是个比较大的坑，当Android设备被设置为Wi-Fi热点的时候，上面的函数得到的地址是"0.0.0.0"，因此，我们需要探究当Android设备被设置为Wi-Fi热点的时候，它的IP地址究竟是多少？

有人研究了Android底层源码发现，当Android设备被设置为Wi-Fi热点的时候，其IP地址是hardcode写死在源码中的，地址是：“192.168.43.1”，对应的广播地址是："192.168.43.255"

为此，我们需要写个函数来判断一下当前Android手机是否处于Wi-Fi热点模式下，如果是，则应该使用上面给出的这个广播地址，这里给出代码示例：
```
protected static Boolean isWifiApEnabled(Context context) {
    try {
        WifiManager manager = (WifiManager)context.getSystemService(Context.WIFI_SERVICE);  
        Method method = manager.getClass().getMethod("isWifiApEnabled");
        return (Boolean)method.invoke(manager);
    }
    catch (NoSuchMethodException e) {
        e.printStackTrace();
    }
    catch (IllegalAccessException | IllegalArgumentException | InvocationTargetException e)  {
        e.printStackTrace();
}
    return false;
}
```



## 参考：

Android Socket 发送广播包的那些坑:
http://ticktick.blog.51cto.com/823160/1707858

https://github.com/Jhuster/Android/blob/master/Socket/Broadcaster.java