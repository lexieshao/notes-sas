## 二级标题notes-sas（  要有空格）

##### 四级标题 some notes about sas
>块注释   
block

*斜体*  
_突出，换行时空格空格回车_  
**粗体**
__粗体__

* 第一
* d第二无项列表
+ 第三
- 第四
1. 第一
2. dier  ，有序列表

### 连接  

This is an [百度](http://www.baidu.com/).    

http://google.com/        "Google" 

http://search.yahoo.com/  "Yahoo Search" 

http://search.msn.com/    "MSN Search"  


###图片插入
![美女](https://image.baidu.com/search/detail?ct=503316480&z=0&ipn=d&word=github&step_word=&hs=0&pn=0&spn=0&di=66016378010&pi=0&rn=1&tn=baiduimagedetail&is=0%2C0&istype=0&ie=utf-8&oe=utf-8&in=&cl=2&lm=-1&st=undefined&cs=1110612156%2C1998721484&os=1408536426%2C3805375213&simid=3470087785%2C407244213&adpicid=0&lpn=0&ln=1855&fr=&fmq=1494574551578_R&fm=&ic=undefined&s=undefined&se=&sme=&tab=0&width=undefined&height=undefined&face=undefined&ist=&jit=&cg=&bdtype=0&oriquery=&objurl=http%3A%2F%2Fcms.csdnimg.cn%2Farticle%2F201303%2F07%2F513845b040c30.jpg&fromurl=ippr_z2C%24qAzdH3FAzdH3Fooo_z%26e3Bvf1g_z%26e3BgjpAzdH3Fw6ptvsjAzdH3Fda8n-an-a0AzdH3Fdb89nl8-SwwS-GtpH7k%3Fs5vwpt5gN74%3D0&gsm=0&rpstart=0&rpnum=0)

![alt text][id] [id]: /path/to/img.jpg "Title"  

### 代码

`<blockquote>`  

```sas
proc sql;
   create table a as
   select * from tebl
   quit;
   
### 脚注

hello[^hello]  

[^hello]: hi


---
