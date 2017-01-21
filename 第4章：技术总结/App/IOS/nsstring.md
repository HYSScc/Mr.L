# NSString
NSString超全总结: http://www.jianshu.com/p/7994b0ad6b88


## 一、NSString : 不可变字符串对象



### 1.字符串的创建
```
// 创建一个新的空字符串
NSString *string1 = [NSString string];
// C语言字符串
char *name = "hello word";
// 初始化一个字符串，在赋值
NSString *string2 = [[NSString alloc] init];
string2 = @"wangchong";
NSString *string3 = @"hello";
// 一下两个方法是把字符串做一次拷贝,返回拷贝后的字符串
NSString *string4 = [NSString stringWithString:@"hello"];
NSString *string5 = [[NSString alloc] initWithString:string3];
NSLog(@"%p,%p,%p",&string3,&string4,&string5);
NSString *string6 = [string5 stringByAppendingString:@"\nHi"];
// 把C的字符串转化为OC的字符串
NSString *str = [[NSString alloc] initWithUTF8String:"hello world"];
NSString *str2 = [NSString stringWithUTF8String:"hello world"];

// 用格式化字符串初始化  可完成字符串的拼接以及完成C的字符串与OC的字符串的转化
int a = 123;
NSString *str3 = [[NSString alloc]initWithFormat:@"a = %d %s%@", a, "aaaa", @"bbbb"];
NSString *str4 = [NSString stringWithFormat:@"%s","hello world"];

```


### 2.字符串的类型转换

```
NSString * str = @"hello";
const char *p = [str UTF8String]; // 将OC字符串转化为C的字符串

str = @"123";
int a = [str intValue]; // 将数字串转化成整型数据
[str floatValue]; // 将数字串转化成float型
[str doubleValue]; // 将数字串转化成double型

// 结构体 输出
// 结构体 [点] 转换字符串
NSString *str1 = NSStringFromCGPoint(point);

// 结构体 [尺寸] 转换字符串
NSString *str2 = NSStringFromCGSize(size);

// 结构体 [矩形] 转换字符串
NSString *str3 = NSStringFromCGRect(rect);
```


### 3.字符串大小写转换
```
<pre name="code" class="objc"> // 全部转为大写
// 结果 ABC
[@"abc" uppercaseString];
// 全部转为小写
// 结果 bcd
[@"BCD" lowercaseString];
// 首字母大写
// 结果 Acb
[@"acb" capitalizedString];

```


### 4.字符串比较
```
// 比较两个字符串内容是否相同
// 相等返回 yes 不相等返回 no
BOOL b =[str isEqualToString:str2];

//判断str2中是否包含str1
[str2 containsString:str1];
// 忽略大小写进行比较
NSComparisonResult  result1 = [string caseInsensitiveCompare:str];
NSComparisonResult  result2 = [string localizedCaseInsensitiveCompare:str];

// 两个字符串内容比较
// NSComparisonResult result = {NSOrderedAscending, NSOrderedSame，NSOrderedDescending}
// NSOrderedAscending    右边 > 左边  == -1
// NSOrderedSame         内容相同     ==  0
// NSOrderedDescending   左边 > 右边  ==  1

NSComparisonResult result3 = [str compare:str2];
NSComparisonResult result4 = [string compare:@"taojian" options:NSCaseInsensitiveSearch];
NSComparisonResult result5 = [string compare:@"taojian" options:NSCaseInsensitiveSearch range:NSMakeRange(0, string.length)];
NSComparisonResult result6 = [string compare:@"taojian" options:NSCaseInsensitiveSearch range:NSMakeRange(0, string.length) locale:nil];
NSComparisonResult result7 = [string localizedCompare:str];

options: // 枚举参数
enum{
    NSCaseInsensitiveSearch = 1, // 不区分大小写比较
    NSLiteralSearch = 2, // 区分大小写比较
    NSBackwardsSearch = 4, // 从字符串末尾开始搜索
    NSAnchoredSearch = 8, // 搜索限制范围的字符串
    NSNumbericSearch = 64 // 按照字符串里的数字为依据，算出顺序。例如 Foo2.txt < Foo7.txt < Foo25.txt

    // 以下定义高于 mac os 10.5 或者高于 iphone 2.0 可用    ,
    NSDiacriticInsensitiveSearch = 128, // 忽略 "-" 符号的比较
    NSWidthInsensitiveSearch = 256, // 忽略字符串的长度，比较出结果
    NSForcedOrderingSearch = 512 // 忽略不区分大小写比较的选项，并强制返回 NSOrderedAscending 或者 NSOrderedDescending

    // 以下定义高于 iphone 3.2 可用    ,
    /// 只能应用于 rangeOfString:..., stringByReplacingOccurrencesOfString:...和 replaceOccurrencesOfString:... 方法。
    /// 使用通用兼容的比较方法，如果设置此项，可以去掉 NSCaseInsensitiveSearch 和 NSAnchoredSearc
    NSRegularExpressionSearch = 1024
```


### 5.字符串搜索
```
// 判断字符串是否以abc开头
[@"abcdfa" hasPrefix:@"abc"];
// 判断字符串是否bcd结尾
[@"adbcd" hasSuffix:@"bcd"];
// 判断字符串是否包含指定字符串，返回位置和长度
NSRange range = [@"123456" rangeOfString:@"456"];
// 搜索字符串所在的范围
NSRange range = [@"123456456qweasasd456" rangeOfString:@"456" options:NSBackwardsSearch];  // 输出{17, 3}
// 指定范围进行搜索
NSRange range = NSMakeRange(0, 9);
range = [@"123456456qweasasd456" rangeOfString:@"456" options:NSBackwardsSearch range:range];
// 找与之开头相同的字符 返回相同开头的字符串
  NSString *string = @"fg   s  abcdefg hijklmn s     d \n fdsgf";
NSString *str = [string commonPrefixWithString:@"fgsdfgrg" options:NSLiteralSearch]; // 输出fg

```

### 6.字符串截取
```
<pre name="code" class="objc"><pre name="code" class="objc">
NSString * str5 = @"helloworld";
NSString * ptr1 = [str5 substringToIndex:2];// 字符串抽取 从头开始抽取2个字母,返回he
NSString * ptr2 = [str5 substringFromIndex:4];// 从第4个字母开始抽取到字符串结束,返回oworld
NSRange range1 = {6,2};// 结构体初始化
NSString * ptr3 = [str5 substringWithRange:range1];// 在range指定范围内抽取,返回or
NSString * ptr4 = [str5 substringWithRange:NSMakeRange(6,2)];// NSMakeRange可以生成一个结构体

// 取出字符串"123-456-789-000"中的数字部分,组成一个新的字符串输出
NSMutableString *strm = [NSMutableString stringWithString:@"123-456-789-000"]; //只有可变字符串有这个方法
[strm replaceOccurrencesOfString:@"-"
                      withString:@""
                         options:NSLiteralSearch
                           range:NSMakeRange(0, strm.length)];
NSLog(@"%@",strm);  //输出:123456789000

```
### 7.字符串的遍历
```
// 根据\n遍历
NSString *string = @"   s  abcdefg hijklmn s     d \n fdsf";
// 根据\n一行一行的打印
[string enumerateLinesUsingBlock:^(NSString *line, BOOL *stop){NSLog(@"\n%@",line);}];

// 根据 条件options 遍历
NSString *string = @"   s  abcdefg hijklmn s     d ";
NSMutableString * mutableString = [NSMutableString string];
// NSStringEnumerationByWords：将string按空格分开，并且会自动清理首尾的空格
// 这个方法会把中间多余的空格也清理掉，比如上面的字符串，s和d之间有两个空格，会变成一个空格
[string enumerateSubstringsInRange:NSMakeRange(0, string.length)         
                           options:NSStringEnumerationByWords 
                        usingBlock:^(NSString *substring, NSRange substringRange, NSRange enclosingRange, BOOL *stop) {
    [mutableString appendFormat:@"%@ ",substring];
}];

// 删除我们添加的末尾的一个空格
[mutableString deleteCharactersInRange:NSMakeRange(outputString.length-1, 1)];

```
### 8.路径操作与数组操作
```
// 用指定字符串分割字符串，返回一个数组   
NSArray *array = [@"1,2,3,4,5,6" componentsSeparatedByString:@","];  

// 根据空格拆分
NSArray *array = [string componentsSeparatedByCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]]; 

// 将数组中的字符串组合成一个文件路径   
NSMutableArray *components = [NSMutableArray array];  
[components addObject:@"Users"];  
[components addObject:@"CentralPerk"];  
[components addObject:@"Desktop"];  
NSString *path = [NSString pathWithComponents:components];  
NSLog(@"%@",path);  //Users/CentralPerk/Desktop   

// 将一个路径分割成一个数组   
NSArray *array1 = [path pathComponents];  
NSLog(@"%@",array1);

// 判断是否为绝对路径(依据：是否以'/'开始)   
path = @"/Users/CentralPerk/Desktop";  
NSLog(@"%i",[path isAbsolutePath]);  

// 获取最后一个目录   
NSLog(@"%@",[path lastPathComponent]);  

// 删除最后一个目录   
NSLog(@"%@",[path stringByDeletingLastPathComponent]);  

// 拼接一个目录   
NSLog(@"%@",[path stringByAppendingPathComponent:@"aaa"]);   //Users/CentralPerk/Desktop/aaa   
NSLog(@"%@",[path stringByAppendingString:@"aaa"]);      //Users/CentralPerk/Desktopaaa   
NSLog(@"%@",[path stringByAppendingFormat:@"%@%@",@"b",@"c"]);  //Users/CentralPerk/Desktopbc

```
### 9.文件扩展名
```
// 拓展名出来   
// 获取拓展名,不带.   
NSString *str2 = @"Users/CentralPerk/Desktop/test.txt";  
NSLog(@"%@",[str2 pathExtension]);  
// 添加拓展名,不需要带.   
NSLog(@"%@",[str2 stringByAppendingPathExtension:@"mp3"]);  
// 删除拓展名,带.一块删除   
NSLog(@"%@",[str2 stringByDeletingPathExtension]);

```
### 10.文件操作
```
// 将字符串设置为path制定的文件的内容 
-(id) initWithContentsOfFile:path encoding:enc error:err  
// 创建一个新字符串并将其设置为path指定的文件的内容，使用字符编码enc，如果非零，则返回err中错误  
+(id) stringWithContentsOfFile:path encoding:enc error:err  


// 将字符串设置为url(NSURL *)url的内容，使用字符编码enc，如果非零，则返回err中的错误 
-(id) initWithContentsOfURL:url encoding:enc error:err   
// 创建一个新的字符串，并将其设置为url的内容，使用字符编码enc，如果非零，则返回err中的错误 
+(id) stringWithContentsOfURL:url encoding:enc error:err

```
### 11.补充
```
// 求字符串长度 
[str1 length]   
// 获取字符串中的字符 
[str1 characterAtIndex:1]
// 清除左右两段的空格
NSString *str = [string stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];
// 在字符串后面补0
NSString *str = [string stringByPaddingToLength:12 withString:@"0" startingAtIndex:0];

```

## 二、NSMutableString: 可变字符串对象


```
// 将不可变的字符串转换为可变的字符串 
NSMutableString * str =  [[NSMutableString alloc]initWithString:@"hello world"];

// 在指定下标为2的(不要越界)位置插入013字符串
[str insertString:@"013" atIndex:2]; 

// 在字符串末尾追加字符串taotao 
[str appendString:@"taotao"];

// 从0位置删除2个字符 
[str deleteCharactersInRange:NSMakeRange(0, 2)];

// 给字符串重新赋值 
[str setString:@"yintian"];

// 将3位置后1个字符替换成ios字符串
[str replaceCharactersInRange:NSMakeRange(3, 1) withString:@"ios"];

// 根据选项 opts ，使用指定 range 中的nsstring2 替换所有的 nsstring
-(void)replaceOccurrencesOfString:nsstring withString:nsstring2 options:opts range:range

```


## Q&A
问题1：NSString到底是不是字符串？

    NSString 是 OC中专门处理字符串的对象！提供了转换大小写，拼接字符串，lastPathComponent等方法！

