# NSDictionary 判断 key 存在，且 value 非空


```
/**
 *  NSDictionary 判断 key 存在，且 value 非空
 *
 *  @param key <#key description#>
 *
 *  @return YES-存在key且非空， NO-不存在key或key值为空
 */
- (BOOL)judgeDictionary:dict ContainKey:(NSString *)key {
    // judge nil
    if(![dict objectForKey:key]){
        return NO;
    }
    
    id obj = [dict objectForKey:key];// judge NSNull
    
    return ![obj isEqual:[NSNull null]];
}
```



## 参考:
NSDictionary判断key存在，且value非空: http://blog.csdn.net/kyfxbl/article/details/44538803