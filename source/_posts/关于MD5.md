---
title: 关于MD5
date: 2016-04-10 22:03:53
tags: iOS学习
---

- 什么是MD5?
 
    MD5全称是Message Digest Algorithm 5，译为“消息摘要算法第5版”

- 效果：对输入信息生成唯一的128位散列值（32个字符）

- MD5的特点：
 1. 输入两个不同的明文不会得到相同的输出值
 2. 根据输出值，不能得到原始的明文，即其过程不可逆

- MD5改进：
    - 加盐（Salt）：在明文的固定位置插入随机串，然后再进行MD5。

    - 先加密，后乱序：先对明文进行MD5，然后对加密得到的MD5串的字符进行乱序。

- 栗子：

    导入NSString+hash.h封装好的加密分类算法下面是封装好的demo：
    

```
#import "ViewController.h"
#import "NSString+Hash.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {

    [super viewDidLoad];

    [self MD5];

    [self MD5Salt:@"san"];

    [self doubleMD5];

    [self MD5Reorder];

}

//MD5加密

-(void)MD5{

    NSString *pwd = @"123456";

    NSString *pwdMD5 = [pwd md5String];

    NSLog(@"%@",pwdMD5);

}

/**
 * 撒盐 加密 MD5($pass.$salt)
 */

-(void)MD5Salt:(NSString *)salt{

    NSString *pwd = @"123456";

    pwd =[pwd stringByAppendingString:salt]; 撒盐：随机地往明文中插入任意字符串

    NSString *pwdMD5 = [pwd md5String];

    NSLog(@"%@",pwdMD5);
}

/**
 *  MD5(MD5($pass))
 */

- (void)doubleMD5{

    NSString *pwd = @"123456";

    NSString *pwdMD5MD5 = [[pwd md5String]md5String];

    NSLog(@"%@",pwdMD5MD5);

}

/**
 *  先加密，后乱序
 */

- (void)MD5Reorder{

    NSString *pwd = @"123456";

    NSString *pwdMD5 = [pwd md5String];
    NSLog(@"oldpwdMD5=%@",pwdMD5);

    NSString *prefix = [pwdMD5 substringFromIndex:3]; 从下标为3的开始截取（包含3）

    NSString *subfix = [pwdMD5 substringToIndex:3];   截取0到3的字符串（不包含3）

    pwdMD5 = [prefix stringByAppendingString:subfix];
    NSLog(@"newpwdMD5=%@",pwdMD5);
}

@end
```


