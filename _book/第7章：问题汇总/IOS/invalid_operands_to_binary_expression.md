# Invalid operands to binary expression ('double' and 'double *')
```
- (void)randomStr:(int)num {
    NSString *x= @"0123456789qwertyuioplkjhgfdsazxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";
    NSDate *timestamp = [NSDate date];
    NSString *tmp = @"";
    for(int i=0; i<num; i++)  {
        double midRandom = ceil((double)(1+arc4random()%99)/100 *100000000);
        int position = (int)(ceil(midRandom) % x.length);//报错”invalid operands to binary expression('double'and'double')“
    }
//    return timestamp.toString().substr(-10, 10) + tmp;
}
```
修改：用fmod(ceil(midRandom), x.length)替换%

```
- (NSString *)randomStr:(int)num
{
    NSString *feed= @"0123456789qwertyuioplkjhgfdsazxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";
    NSString *tmp = @"";
    for(int i=0; i<num; i++)  {
        double midRandom = ceil((double)(1+arc4random()%99)/100 *100000000);
        int position = fmod(ceil(midRandom), feed.length);
        [tmp stringByAppendingString:[NSString stringWithCharacters:[feed characterAtIndex:position] length:1]];
        DDLogVerbose(@"[MLMyDeviceTableViewController] 生成随机字符:%@", tmp);
    }
    NSString *timeSp = [NSString stringWithFormat:@"%ld", (long)[[NSDate date] timeIntervalSince1970]];
    DDLogVerbose(@"[MLMyDeviceTableViewController] 时间戳:%@", timeSp);
    return [NSString stringWithFormat:@"%@%@", [timeSp substringWithRange:NSMakeRange(-10,10)], tmp];
}
```