<head><meta charset="UTF-8"></head>

# 隐写术
## 图片隐写术
１．图种：bless(2进制查看器); binwalk; foremost
２．数据改动、lsb(png):stegsolve
３．bmp图先使用stegsolve查看lsb，再用binwalk查看文件末尾。jpg图片小心exif信息和文件起始位的一些信息，使用winhex或010editer最好，最后看看是否有附加。png图同jpg图，但是不要忘记有时候可以使用adobe firework查看图层。

# Web
1. sql注入：地址栏 ' ; sqlmap
2. 查看、更改报文：burpsuite；request、response
3. 查看源代码

# 安全杂项
1. 截断上传漏洞，0x00，%00，/00之类，chr(0)后面的数据会被截断，后面的数据直接忽略，导致漏洞产生。[小谈截断上传漏洞](http://www.cnblogs.com/hack0ne/p/4603144.html)


# CTF-MISC之编码转换
1.如何来判断密文的加密方式呢？
 1如果密文是十进制，字符范围为“0-9”，可以猜测是ASCII编码；
 2如果密文由“a-z”“A-Z”和“=”构成，特别是末尾有“=”，那么可以判断为Base64编码；
 3如果密文含有“%” ，形式为 “%xx” 和 “%uxxxx”，字符范围又是十六进制“0-F”，判断是escape() 函数编码，用unescape()解码；(　浏览器console-unescape("密文")，即可解密　)
    ```
    URL字符转义
    +    URL 中+号表示空格                      %2B
    空格 URL中的空格可以用+号或者编码           %20
    /   分隔目录和子目录                        %2F
    ?    分隔实际的URL和参数                    %3F
    %    指定特殊字符                           %25
    #    表示书签                               %23
    &    URL 中指定的参数间的分隔符             %26
    =    URL 中指定参数的值                     %3D
    ```
 4若密文由“[]，()，{}，+,！”字符组成的编码通常就通过Jother解密。(直接在浏览器console中输入解密)
