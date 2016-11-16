<head><meta charset="UTF-8"></head>

CTF全称Capture The Flag，中文叫做夺旗游戏。在一些关卡中通过各种各样的技术找到最终的flag，从而获得加分。
一般CTF比赛中，会涉及MISC,PPC,CRYPTO,PWN,REVERSE,WEB,STEGA这几种题目。

MISC     # 的全称是 miscellaneous 杂乱无章的意思，其中的题目可能涉及，流量分析，取证，人肉，大数据统计等等。
PPC      # 是 Professionally ProgramCoder 的意思，会涉及到一些简单的算法知识，不过比ACM要简单的多。
CRYPTO   # 是密码学的缩写，这一类的题目通常以破解密文为主，加密算法有可能是古典加密算法，也有可能是现代的加密算法，甚至有些是出题者杜撰的加密算法。
PWN      # 在黑客俚语中代表着，攻破，取得权限，在CTF中它代表着溢出类的题目。
REVERSE  # 就是逆向工程，这个大家应该很熟悉的。破解crackme.exe就是这类题的主旋律。
STEGA    # 是Steganography的缩写，隐写术的意思，这类题很有意思，flag被藏在一段视频，一张图片，一首音乐，或者任何你想想不到的载体中，也是CTF比赛中最有挑战和乐趣的一类题。
WEB      # 题型就是最大众化的WEB漏洞的考察了，他会涉及到注入，代码执行，文件包含等常见的WEB漏洞。

### 牛刀小试
#### ASCII码而已
``` python
\u5927\u5bb6\u597d\uff0c\u6211\u662f\u0040\u65e0\u6240\u4e0d\u80fd\u7684\u9b42\u5927\u4eba\uff01\u8bdd\u8bf4\u5fae\u535a\u7c89\u4e1d\u8fc7\u767e\u771f\u7684\u597d\u96be\u3002\u3002\u0077\u0063\u0074\u0066\u007b\u006d\u006f\u0072\u0065\u006d\u006f\u0072\u0065\u005f\u0077\u0065\u0069\u0062\u006f\u005f\u0066\u0061\u006e\u0073\u007d
str = u'上面那一串'
print str  # 一定要在交互模式下带print，这样才能转换成utf-8
```

#### 被改错的密码
从前有一个熊孩子入侵了一个网站的数据库，找到了管理员密码，手一抖在数据库中修改了一下，现在的密码变成了 cca9cc444e64c8116a30la00559c042b4，那个熊孩子其实就是我！肿么办求解！在线等，挺急的。。
可以直接去掉那个不是十六进制字符的l，或者暴力一下
``` python
In [1]: "cca9cc444e64c8116a30la00559c042b4"
In [2]: len(_)  # 结果是33，md5是32位的
In [3]: string="cca9cc444e64c8116a30la00559c042b4"
In [4]: for i in range(len(string)):
   ...:     print(string[:i]+string[i+1:])  # 会输出去掉一个字符的暴力结果
# 将结果复制
In [5]: len("""ca9cc444e64c8116a30la00559c042b4
   ...: ca9cc444e64c8116a30la00559c042b4
   ...: cc9cc444e64c8116a30la00559c042b4
   ...: ccacc444e64c8116a30la00559c042b4
   ...: cca9c444e64c8116a30la00559c042b4
   ...: cca9c444e64c8116a30la00559c042b4
   ...: cca9cc44e64c8116a30la00559c042b4
   ...: cca9cc44e64c8116a30la00559c042b4
   ...: cca9cc44e64c8116a30la00559c042b4
   ...: cca9cc44464c8116a30la00559c042b4
   ...: cca9cc444e4c8116a30la00559c042b4
   ...: cca9cc444e6c8116a30la00559c042b4
   ...: cca9cc444e648116a30la00559c042b4
   ...: cca9cc444e64c116a30la00559c042b4
   ...: cca9cc444e64c816a30la00559c042b4
   ...: cca9cc444e64c816a30la00559c042b4
   ...: cca9cc444e64c811a30la00559c042b4
   ...: cca9cc444e64c811630la00559c042b4
   ...: cca9cc444e64c8116a0la00559c042b4
   ...: cca9cc444e64c8116a3la00559c042b4
   ...: cca9cc444e64c8116a30a00559c042b4
   ...: cca9cc444e64c8116a30l00559c042b4
   ...: cca9cc444e64c8116a30la0559c042b4
   ...: cca9cc444e64c8116a30la0559c042b4
   ...: cca9cc444e64c8116a30la0059c042b4
   ...: cca9cc444e64c8116a30la0059c042b4
   ...: cca9cc444e64c8116a30la0055c042b4
   ...: cca9cc444e64c8116a30la00559042b4
   ...: cca9cc444e64c8116a30la00559c42b4
   ...: cca9cc444e64c8116a30la00559c02b4
   ...: cca9cc444e64c8116a30la00559c04b4
   ...: cca9cc444e64c8116a30la00559c0424
   ...: cca9cc444e64c8116a30la00559c042b
   ...: """.split('\n'))
Out[5]: 34
```

#### 啥?
直接用 vim 打开 jpg, /c*t*f, 看到答案

#### 摩斯密码
用在线工具转换一下, wctf{morsecode}, 卡大小写, 不爽

#### 聪明的小羊
见 CTF_加密解密

### 包罗万象 MISC
#### 图片里的英语 wctf{Mtfbwy}
小李的图片, 用 vim 十六进制打开，发现中间还有个文件头，用rar尝试打开，发现里面有张命名为 flag.jpg 的图片
:%!xxd && /jpg 可以看到 flag.jpg, 里面还藏着一个图片, 重命名为 test.zip, 鼠标双击打开
binwalk test.jpg  # RAR archive data, first volume type: MAIN_HEAD
图片里没有英文单词, 用百度搜图找原图

#### 抓到一只苍蝇
首先下载所给的文件misc_fly.pcapng，是一个抓包软件抓取的数据包, wireshark 打开
Filter(一定要按时间排序): http.request.method == "POST"
观察第一个报文最里面的传送的数据, HTML Form URL Encoded, 得到如下 json 信息
{"path":"fly.rar","appid":"","size":525701,"md5":"e023afa4f6579db5becda8fe7861c2d3","sha":"ecccba7aea1d482684374b22e2e7abad2ba86749","sha3":""}
观察上一层的 HTTP(Hypertext Transfer Protocol), 得到 Post 地址, 下面紧接着的 5 个相同地址的报文为传送的附件.
http://set2.mail.qq.com/cgi-bin/uploadunite?func=CreateFile&&inputf=json&outputf=json&&sid=x5O8ZuWvSp9yXFgM
观察倒数第二个报文, 这里面有邮件的内容, 可以确定一下信息
```
数据包的内容：一封带附件的邮件
发件人：81101652@qq.com
收件人：king@woldy.net
附件：fly.rar
附件大小：525701 Bytes
```
5 个附件报文的 Media Type 域大小为131436,131436,131436,131436,1777,合计527521，前面附件json里已知附件大小525701，多出来的部分应该是头部的信息之类
(527521−525701)/5=364Bytes 分析出头部多余部分为 346 字节, dd 命令除去
``` zsh
foremost -i -e misc_fly.pcapng  # 这个命令可以恢复数据, 大小对了, 但是 md5 错了, 以后研究
dd if=1 bs=1 skip=364 of=1.1 && dd if=2 bs=1 skip=364 of=2.1 && dd if=3 bs=1 skip=364 of=3.1 && dd if=4 bs=1 skip=364 of=4.1 && dd if=5 bs=1 skip=364 of=5.1
cat  1.1  2.1  3.1  4.1  5.1  >  fly.rar
md5sum fly.rar  # 结果: e023afa4f6579db5becda8fe7861c2d3, 和上面 json 中的 md5 相同
sha1sum fly.rar  # 同样一致, 至此, 由网络包还原出附件数据 fly.rar
```
解压失败 !!!!!  结果——伪加密, 即这是一个未加密过的rar文件，但是却将加密位置为了1
只需将文件开头处 0x74 位后面的 0x84 位置改为 0x80 即可, 修改后顺利解压，得到 flag.txt
打开 flag.txt 查看，这又是一个二进制文件, 继续二进制打开, 发现这是个win32的程序，修改为 .exe, 转到windows查看, 是个满屏幕跑苍蝇的程序……还挺逼真的…
``` zsh
binwalk flag.txt  # 发现最后有一个不一样大小的图片, 其它都是 60x60
# 991232        0xF2000         PNG image, 280 x 280, 1-bit colormap, non-interlaced
dd if=flag.txt bs=1 skip=991232 of=1.png  # 打开得到一个 二维码, 扫描得到结果
```

### 初探乾坤 PPC
#### 简单编程-字符统计
2 秒内统计页面随机字符数量, 需要用到带 cookie 的 post, 最简单就是用 session 了
``` python
import requests
url = "http://ctf.idf.cn/game/pro/37/"
s = requests.session()
res = s.get(url)
test = res.text.split('<hr />')
w, o, l, d, y = 0, 0, 0, 0, 0
for ch in test[1]:  # 两个 <hr /> 不是标签, 所以不好用 BeautifulSoup, 正好可以用 split
    if ch == "w": w += 1
    elif ch == "o": o += 1
    elif ch == "l": l += 1
    elif ch == "d": d += 1
    elif ch == "y": y += 1
answer = '%d%d%d%d%d' % (w, o, l, d, y)  # 也可以用 str(w) + str(o)
values = {'anwser': answer}
print type(values)  # dict 格式的
result = s.post(url, values)
```

#### 谁是卧底
``` zsh
wc whoiswoldy.txt  # 0 1 5586640 whoiswoldy.txt, 0行，1个词，55586640个字符，也就说文件里只有一个大字符串，字符数是千万级
```
用肉眼在5000多万字符里找一句话无疑是大海捞针，需要编程解决，接下来要考虑编程的思路了。 一个直接的想法是：
句子由单词组成，只要能够找到文件里所有的单词，其中单词密度最大的地方就是完整英文句子所在的地方。
于是问题分为两步：
1）统计汇总文件中所有单词的位置（单词首字母在字符串中的位置）
2）找到单词分布最为密集的位置
找一个 txt 格式的字典, 然后将 500w字符一行 的分割成每行 100字符 的文件, 然后试试 200字符一行, 万一被自己刚好断开了呢
首先是将一行换为很多行, vim 初始版的读还是很轻松的 ...
``` python
f1 = open('./res.txt')  # 31977 33
num, line, i, temp = 0, 0, 0, 0
for line1 in f1:
    #  print line1
    if(temp > num):
        line, num = i, temp
    i += 1
    temp = 0
    f2 = open('/home/coder352/github/jTemplate/src/3000_English.txt')
    for line2 in f2:
        line2 = line2.strip('\n').split()
        for line3 in line2:
            temp += line1.count(line3)
    f2.close()
print line, num
```
然后统计出单词最多的行
``` python
f1 = open('./res.txt')
num, line, i, temp = 0, 0, 0, 0
for line1 in f1:
    #  print line1
    if(temp > num):
        line, num = i, temp
    i += 1
    temp = 0
    f2 = open('/home/coder352/github/jTemplate/src/3000_English.txt')
    for line2 in f2:
        line2 = line2.strip('\n').split()
        for line3 in line2:
            temp += line1.count(line3)
    f2.close()
print line, num  # 31977 33
# hazbornndomhfchvlcwhatwillyouseeifyouthrowthebutteroutthewindowwzqmtwmyjutipvqetrsshyosypzydeveloppo
# whatwillyouseeifyouthrowthebutteroutthewindow -> butterfly
```

#### Fuck your brain
这是什么，你能理解它吗？
++++++++++++[>++++>+++++>++++++>+++++++>++++++++>+++++++++>++++++++++<<<<<<<-]>>>>+++.<-----.>---.<+++.>>>>+++.<<<<----.>>>++++++.<<<<<+++.--.>>>>>----.<<<++++.<<+++.>>>>+++.>---.>++.
brain fuck 语言, 可以找到 c++ 的解释器, gcc -o bf bf.cpp
将brain fuck语句保存在.bf文件文件，然后执行bf 1.bf，即可得出结果
WCTF{Br31nF4ck}
BrainFuck只有八条指令：
``` brainfuck
指令	含义	                                                      等价的C代码
>	    指针加一	                                                  ++ptr;
<	    指针减一	                                                  --ptr;
+	    指针指向的字节的值加一	                                      ++*ptr;
-	    指针指向的字节的值减一	                                      --*ptr;
.	    输出指针指向的单元内容（ASCII码）	                          putchar(*ptr);
,	    输入内容到指针指向的单元（ASCII码）	                          *ptr = getchar();
[	    如果指针指向的单元值为零，向后跳转到对应的]指令的次一指令处	  while (*ptr) {
]	    如果指针指向的单元值不为零，向前跳转到对应的[指令的次一指令处	}
```

### 倒行逆施
#### .NET 逆向第一题
不做 .NET 的题目, 附上反编译软件和答案 wctf{dotnet_crackme1}
.NET Decompile: [ILSpy](https://github.com/icsharpcode/ILSpy)

#### 简单的PE文件逆向
Windows IDA 逆向, 没好感, 慢慢学 IDA 去吧, 答案 wctf{Pe_cRackme1_1024}

#### 简单的ELF逆向
IDA 以后再学 && 答案 wctf{ElF_lnX_Ckm_0823}

### 百密一疏
#### 凯撒加密
这种简单的加密方式十分容易破解，因为英文中每个字母出现的频率是相对固定的，只要对于足够长的密文，统计其中字母出现的频度，与标准频度表进行比对，就可以轻易地发现原文字母与密文字母的对应关系
密文比较多和26个字母乱序时的统计方法:
``` python
stdFreq = {'A':8.2, 'N':6.7, 'B':1.5, 'O':7.5, 'C':2.8, 'P':1.9, 'D':4.3, 'Q':0.1, 'E':12.7, 'R':6.0, 'F':2.2, 'S':6.3, 'G':2.0, 'T':9.1, 'H':6.1, 'U':2.8, 'I':7.0, 'V':1.0, 'J':0.2, 'W':2.4, 'K':0.8, 'X':0.2, 'L':4.0, 'Y':2.0, 'M':2.4, 'Z':0.1}
```
密文比较少时的暴力法
``` python
s = '''U8Y]:8KdJHTXRI>XU#?!K_ecJH]kJG*bRH7YJH7YSH]*=93dVZ3^S8*$:8"&:9U]RH;g=8Y!U92'=j*$KH]ZSj&[S#!gU#*dK9\.'''
for i in range(127):
    s1 = ''
    for ch in s:
        tmp = chr((ord(ch) + i) % 127)
        if 32 < ord(tmp) < 127:  # ascii 0-32 是控制字符或通信专用字符
            s1 += tmp
            flag = 1
        else:
            flag = 0
            break
    if flag == 1:
        print s1  # 倒数第五行很明显是 base64 加密
```
``` python
s = '''dGhlIGZsYWcgaXMgd2N0ZntrYWlzYV9qaWFhYWFhbWl9LHBseiBmbG93IG15IHdlaWJvLGh0dHA6Ly93ZWliby5jb20vd29sZHk='''
import base64
print base64.b64decode(s)
```

#### 特殊的日子 CRC
点提示: CRC32
``` python
import binascii
real = 0x4D1FAE0B
for y in range(1000, 3000):
    for m in range(1, 12):
        for d in range(1, 31):
            al = ("%d%02d%02d") % (y, m, d)
            if real == binascii.crc32(al):
                print al
```

#### 伟人的名字
原文是英文，密文也全都是英文字母，很明显的古典密码
解析见这里, 膜拜一下, 我就不整理了...  [here](http://blog.csdn.net/ab748998806/article/details/46368337)

#### 笨笨的小猪
猪圈密码, 一个一个查就行了...

#### 孔子的学费
培根密码, 查表, 比较简单...

### 万里追踪
#### 图片里的秘密
用ps打开，直接显示 "此PNG包含用于Adobe Fireworks的其他数据，在储存是扔掉这些数据"，好吧，下载一个FW，直接打开,将图片拖动开, 赫然看见下面一句话
wctf{flag_woldy_s _email}, 空格不能省

#### 上帝也哭泣
一首歌的歌词,  office中，每一个字符都具有隐藏属性，而且office套件可以设置是否显示被隐藏得字符
文件 -> 选项 -> 勾选"隐藏字符", 发现每行的结尾多了一个字符, 拼起来就是 flag

#### 任务管理器中的秘密

#### 红与黑
(Windows)用图片查看器打开，调节曝光、阴影、亮度即可看到图片底部的一行小字
(Ubuntu)刚用 GIMP 打开, 底部就出现了答案...

### 天罗地网
#### 不难不易的 js 加密
``` javascript
eval(function (p, a, c, k, e, d) {
    e = function (c) {
        return (c < a ? "" : e(parseInt(c / a))) + ((c = c % a) > 35 ? String.fromCharCode(c + 29) : c.toString(36))
    };
    if (!''.replace(/^/, String)) {
        while (c--) d[e(c)] = k[c] || e(c);
        k = [function (e) {
                return d[e]
            }];
        e = function () {
            return '\\w+'
        };
        c = 1;
    };
    while (c--) if (k[c]) p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]);
    return p;
}(
    62, 77,
    .split('|'), 0, {}))
```

代码加密, 去掉eval保留括号，在chrome的查看元素的控制台里运行一下，就出来了
剩下的代码分析太麻烦了 [here](http://www.cnblogs.com/duanv/p/4542613.html)

#### Cookie 欺骗
url参数传递一般用base64编码, 和上面的伟人的名字一样, 常练练 Python 快速编程吧

#### 一种编码而已
``` javascript
[][(![]+[])[!![]+!![]+!![]]+({}+[])[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][({}+[])[!![]+!![]+!![]+!![]+!![]]+({}+[])[+!![]]+({}[[]]+[])[+!![]]+(![]+[])[!![]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+({}[[]]+[])[+[]]+({}+[])[!![]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+({}+[])[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+(!![]+[])[!![]+!![]+!![]]+(!![]+[])
```
一堆类似的编码, 其实就是 javascript, 直接放到 Chrome 的 Conslole 中去执行...

#### 古老的邮件编码 UUencode
``` UUencode
MR,O)^KNYU>;*Q[*[P_?#Q+"AHZS6Q\G,LKNYNZ.LR;;2LK*[N^&CK+/VN/;,
MXK:\TJJ]RKZAQ-36K:&CH:,*M/.XQ;3PL+B^S<K'U>+1^;#)=V-T9GMU=75U
*=65N8V]D95]??0``
```

#### 超简单的 js 题
``` javascript
<script>
var p1 = '%66%75%6e%63%74%69%6f%6e%20%63%68%65%63%6b%53%75%62%6d%69%74%28%29%7b%76%61%72%20%61%3d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%70%61%73%73%77%6f%72%64%22%29%3b%69%66%28%22%75%6e%64%65%66%69%6e%65%64%22%21%3d%74%79%70%65%6f%66%20%61%29%7b%69%66%28%22%61%39%35';
var p2 = '%66%64%30%36%39%36%33%30%37%62%65%37%64%39%22%3d%3d%61%2e%76%61%6c%75%65%29%72%65%74%75%72%6e%21%30%3b%61%6c%65%72%74%28%22%45%72%72%6f%72%22%29%3b%61%2e%66%6f%63%75%73%28%29%3b%72%65%74%75%72%6e%21%31%7d%7d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%6c%65%76%65%6c%51%75%65%73%74%22%29%2e%6f%6e%73%75%62%6d%69%74%3d%63%68%65%63%6b%53%75%62%6d%69%74%3b';
eval(unescape(p1) + unescape('%66%36%32%36%61%35%61%64%64%31%37%39%65%66%66' + p2));
</script>
// eval(unescape(p1) + unescape('%39%32%32%33%63%32%39%33%35%64%38%66%36%37' + p2)); 这个东西是动态生成的, 每次都不一样
// 代码的意思在p1后加 %39%32%32%33%63%32%39%33%35%64%38%66%36%37 再加p2, 加完以后进行解码
function checkSubmit() {
    var a = document.getElementById("password");
    if ("undefined" != typeof a) {
        if ("49223c2935d8f6777f1b21feefeee2ec" == a.value) return !0;
        alert("Error");
        a.focus();
        return !1
    }
}
document.getElementById("levelQuest").onsubmit = checkSubmit;
```

#### 简单的js解密
直接看代码，页面内有一段script
``` javascript
function pseudoHash(string, method) {
  // Default method is encryption
  if (!('ENCRYPT' == method || 'DECRYPT' == method)) {
    method = 'ENCRYPT';
  }
  // Run algorithm with the right method
  if ('ENCRYPT' == method) {
    // Variable for output string
    var output = '';
    // Algorithm to encrypt
    for (var x = 0, y = string.length, charCode, hexCode; x < y; ++x) {
      charCode = string.charCodeAt(x);
      if (128 > charCode) {
        charCode += 128;
      } else if (127 < charCode) {
        charCode -= 128;
      }
      charCode = 255 - charCode;
      hexCode = charCode.toString(16);
      if (2 > hexCode.length) {
        hexCode = '0' + hexCode;
      }

      output += hexCode;
    }
    // Return output
    return output;
  } else if ('DECRYPT' == method) {
    // DECODE MISS
    // Return ASCII value of character
    return string;
  }
}
document.getElementById('password').value = pseudoHash('4b481a4c4c471d4c1c4f474d464b4a1a19194f4b1b1a491b494a464e4a1e1b1a', 'DECRYPT');
```
最后得到的是将所有的16进制字符拼接起来的结果，由此不难编写解密算法，使用Python
``` python
s = '4b481a4c4c471d4c1c4f474d464b4a1a19194f4b1b1a491b494a464e4a1e1b1a'
def decode(s):
    charcode = []
    i = 0
    while i<len(s):
        charcode.append(int(s[i:i+2],16))
        i = i + 2
    charcode = [255-i for i in charcode]
    chars = []
    for i in charcode:
        if 127<i+128<255:
            chars.append(i+128)
        elif 0<i-128<128:
            chars.append(i-128)
        else: chars.append(i)
    result = [unichr(i) for i in chars]
    return ''.join(result)
t = decode(s)  # 47e338b3c082945eff04de6d65915ade  # 这个要再放到对话框里才能得到 flag
```

#### 你关注最新漏洞吗
wireshark，发现下面有个kerberos，并没有见过这玩意，于是百度了下kerberos，发现有点意思，然而百度了下kerberos 漏洞
好吧还真找到一个高危漏洞，时间就在出题一个月之前，于是提交公告的编号WCTF{MS14-068}
