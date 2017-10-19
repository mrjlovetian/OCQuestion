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


