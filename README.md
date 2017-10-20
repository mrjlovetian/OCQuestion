# OCQuestion
# 拷贝通讯录里的手机号码问题
最近有个需求，就是在录入客户号码的时候，许多用户喜欢用copy通讯录的方式拷贝到文本输入框，但是文本输入框又有长度显示，中国用户手机号码的长度为11位。
* 问题1 我从铜须录拷过来的的号码明明是11位的为啥，有13位？
原来复制的时候系统，手机号码的前后两个位置会有两个你看不见也打印不出来的特殊符号，经过debug环境下找到原有字符串，拷贝到转码工具里发现特殊字符在ASCII里原形毕露。
![ALT](/question1.png)
  
  
* 问题2 我怎么找到这两字符呢
怎么解决呢，百度了就是找不到，于是想到既然我拿不到这两个奇怪的字符，不如我直接去拿我看的到数字字符。
![ALT](/question2.png)
  
  
* 解决办法
```
NSString *phoneStr = [NSString stringWithFormat:@"%@", textFile.text];
        phoneStr = [phoneStr stringByReplacingOccurrencesOfString:@"-" withString:@""];
        phoneStr = [phoneStr stringByReplacingOccurrencesOfString:@" " withString:@""];
        NSString *num = phoneStr;
        if (textFile.text.length > 1) {
            NSInteger number;
            NSScanner *scanner = [NSScanner scannerWithString:phoneStr];
            [scanner scanUpToCharactersFromSet:[NSCharacterSet decimalDigitCharacterSet] intoString:nil];
            [scanner scanInteger:&number];
            num = [NSString stringWithFormat:@"%ld",number];
        }
```


# 通过airdrop分享文件给ios设备
首先需要知道Uniform Type Identifiers (UTIs)统一标示符（UTIs）  
当你把图片分享之其他iOS设备，接收方会自动打开拍照类app并加载图片。如果你传递的是PDF文件，接收方设备可能会提示你选择一个app来打开文件，或者直接在iBooks中打开。iOS是如何知道哪个app适合什么样的数据类型呢？
 
在系统中，苹果用UTIs来处理数据类型的标示。简单的说，一个uti是用来标示特定类型的数据或文件。例如，com.adobe.pdf标示一个pdf文件，而public.png代表一个PNG图片。在[这里](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html)可以查看已经在系统中注册了的完整的UTIs清单。（love cc cat）应用程序可以打开在iOS系统中已经注册了的UTI。因此无论文件是否被打开，iOS都会用特定的程序打开这个文件。
 
系统允许多个程序注册相同的UTI。在这个教程中，iOS将通过app列表打开文件。比如，当你分享PDF文档时，你可以在接收端设备上看到如下屏幕：  

![ALT](/share.jpg)


