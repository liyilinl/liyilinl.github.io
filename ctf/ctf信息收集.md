# Ctfshow(信息收集1-20)


 信息收集（1-17）

 web1（源码）

 web2（源码）

 web3（抓包）

 web4 （robots）

 web5（phps）

 web6（解压源码泄露）

 web7（git泄露）

 web8（svn泄露）

 web9（vim缓存）

 web10（cookie）

 web11（域名）

 web12（公开信息）

 web13（公开文档）

 web14（源码泄露editor）

 web15（公开邮箱）

 web16（PHP探针）

 web17（sql备份泄露）

 web18（web小游戏）

 web19（php代码信息）

 web20 （mdb文件）

_ **总结：（①** __**https://www.52pojie.cn/thread-1560243-1-1.html**__ **）** _

_ **（②** __**https://blog.csdn.net/weixin\_57210723/article/details/121596694**__ **）** _

### Web1：

解题思路：直接右键查看源码

Flag：ctfshow{d7a0b9bc-ebe5-42d4-a167-e778cbec1bdb}

![image.png](https://pic4.58cdn.com.cn/nowater/webim/big/n_v22a4b0204b30a43ed80163b94e47a3e29.png)
### web2: 前台js绕过

解题思路：

无法使用右键，也无法使用f12，但是查看网页源码的方法不只有这两种方法。

查看网页源码的方法：①鼠标右键查看网页源码②按f12③CTRL+u④网页地址栏前面加上入view-source:。

### Web3: 响应头：

![image.png](https://pic9.58cdn.com.cn/nowater/webim/big/n_v28c33e52d68e44d7fa92cf0fa83be748c.png)

第一步尝试查看源码：并没有发现flag。

![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v24ccfb1e904d94c388b198c98232bdb07.png)

第二步运用bp进行抓包查看响应：发现flag。

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v2e164470d60974855a82eadff32525b8c.png)

### Web4:（robots协议）

![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v2b2224914cc984bf0b525e34ac23f0b7e.png)

解题思路：

进入后发现查看源码并没有找到flag，所以我们可以去网上搜索robots协议。进入给出的网址，在网址后面加上\robots.txt 之后会跳转的另一个页面，然后找Disallow：将该处后面的东西复制，替换掉\robots.txt 即可得到flag。
![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v2d6906fb963574627ae17d450d94696e9.png)

robots协议（又叫伪君子协议）

用于网站中，防止网站一些铭感目录被爬虫爬取，所以特地建了一个文本文档用来表明那些目录是攻击者不能爬取的。

robots.txt文件的格式

User-agent：\_\_\_\_\_ 空白处为定义搜索引擎的类型；

Crawl-delay：\_\_\_\_\_ 空白处为定义抓取延迟；

Disallow：\_\_\_\_\_ 空白处为定义禁止搜索引擎收录的地址；

Allow：\_\_\_\_\_ 空白处为定义允许搜索引擎收录的地址；

文件用法

例1. 禁止所有搜索引擎访问网站的任何部分

User-agent: \*

Disallow: /

例2. 允许所有的robot访问 (或者也可以建一个空文件 "/robots.txt" file)

User-agent: \*

Allow:　/

例3. 禁止某个搜索引擎的访问

User-agent: BadBot

Disallow: /

例4. 允许某个搜索引擎的访问

User-agent: Baiduspider

allow:/

### Web5：phps源码泄露

![image.png](https://pic9.58cdn.com.cn/nowater/webim/big/n_v244623cb870d34260a5c43ef460c4f308.png)

解题思路：由题目可以知道其为phps源码泄露。

程序员使用vim编辑器编写一个index.php文件时，会有一个`.index.php.swp`文件，如果文件正常退出，则该文件被删除，如果异常退出，该文件则会保存下来，该文件可以用来恢复异常退出的index.php，同时多次意外退出并不会覆盖旧的swp文件，而是会生成一个新的。

所以我们可以在网址后面加上/index.phps

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v24ce8647a833c49159e2db367fb00e667.png)

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v29a2d36e17c144d28a1907dc104c4ae3d.png)

### Web6：源码压缩包泄露

![](RackMultipart20230116-1-u3a5bc_html_be790e380b5b7a6b.png)

解题思路：解压源码到当前目录，测试正常，收工，那么我们输入常见的源码包名字www.zip，即可下载压缩包。网站备份压缩文件：管理员将网站源代码备份在Web目录下，攻击者通过猜解文件路径，下载备份文件，导致源代码泄露。

![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v2fa9086dba81340ef953d5d55e4917260.png)

![image.png](https://pic4.58cdn.com.cn/nowater/webim/big/n_v2b20bee0a08954f01be21a9142459552b.png)

![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v27616e70336d54d1291d810450a60b366.png)

常见备份文件后缀：

![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v27616e70336d54d1291d810450a60b366.png)

### Web7 ：git泄露

解题思路：版本控制器，不是git泄露就是svn泄露；此题通过访问/.git/即可获得flag。

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v2fdcf0722498649959da2b1c6d4fe8f87.png)

Ctf常见的版本控制器：

1.git

git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。

Svn

SVN是subversion的缩写，是一个开放源代码的版本控制系统，通过采用分支管理系统的高效管理，简而言之就是用于多个人共同开发同一个项目，实现共享资源，实现最终集中式的管理。

### Web8：svn泄露

解题思路：版本控制器，不是git泄露就是svn泄露；此题通过访问/.svn/即可获得flag。

[image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v2cb0647424ce64bc597e7950e9ca855a2.png)

Flag：ctfshow{c917ad97-58af-4a86-98bf-233246d37a57}

### Web9：vim临时文件泄露

解题思路：由提示可以知道其为vim文件泄露，所以需要知道vim编辑器原理：程序员使用vim编辑器编写一个index.php文件时，会有一个.index.php.swp文件，如果文件正常退出，则该文件被删除，如果异常退出，该文件则会保存下来，该文件可以用来恢复异常退出的index.php，同时多次意外退出并不会覆盖旧的.swp文件，而是会生成一个新的，例如.swo文件。所以由提示可知，生成了,swp文件。

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v201e65bb8eaac4ccc94616888eaf62be7.png)

下载打开即可得到flag：ctfshow{e779952e-dcca-4971-9edd-008a2b337778}

Index.php备份文件名：常见的备份文件后缀名有：".git" 、".[svn](https://so.csdn.net/so/search?q=svn&spm=1001.2101.3001.7020)"、" .swp"".~"、".bak"、".bash\_history"、".bkf" 尝试在URL（index.php）后面，依次输入常见的文件备份扩展名。

### Web10：找cookie

解题思路：题目说和cookie有关所以找到cookie，即可得到flag

![image.png](https://pic8.58cdn.com.cn/nowater/webim/big/n_v232f89502b32c42b4924fd55ea9a5768d.png)

Flag：ctfshow{%7B4e25aa79-4d3f-4e2c-8f8f-e031fd0deef7%7D}

### Web11 ：dns域名查询

解题思路：DNS域名查询（http://dbcha.com/?t=1671459243）获得flag

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v253828d53694947adb617cd54502cc2af.png)

### Web12：公开信息

解题思路：看到这种网站，一般都是先找到后台，访问/admin/。

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v29656f394098f4ccebf3732c861fb0e94.png)

输入后发现是需要密码和账号，回到原网页里面寻找

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v2ebb6ad7ba1cb4dcb91d5509c4ebe3e92.png)

找到密码

![image.png](https://pic9.58cdn.com.cn/nowater/webim/big/n_v24db0c98d55624557863b5e5bec5ae0c0.png)

寻找账号：输入robots.txt

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v22dd1b7d06415400aa25a827b63f86e23.png)

### Web13：

解题思路：根据提示在生产环境页面即打开后的网页寻找答案，通过寻找发现最下面的只有document可以点击，点击后可以进入到一个pdf页面。

![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v25c9b81596a6d43cd9d318e3ce2e9f67e.png)

点开后进入到另一个页面，查看一下/sytem1103/login.php然后输入用户名和密码就得到了flag。

![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v2b3ffe33c07c4445a9f4733cc8ec08b27.png)

### Web14：

解题思路：网址后面加上/editor/,进入到文件编辑器，找上传文件点击文件空间找到最下面哪一个根目录点开找到www然后点开找到html找到nothinghere最后找到fl000.txt，然后在网址后加上nothinghere/flooo.txt即可得到flag。

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v2fc4b1ff7ea5b46c28eebfdfc98fbf3ee.png)

### Web15 ：公开信息

解题思路：直接在网址后面加上/admin/

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v2a275609f50a5473398d6ea5508bab496.png)

点击忘记密码

![image.png](https://pic9.58cdn.com.cn/nowater/webim/big/n_v2c3e774440e66422390c667fb20e488ef.png)

通过刚刚那个页面最下方的qq邮箱可以通过qq号查询找到所在地：西安

然后即可得到密码，输入密码得到flag。

Flag：ctfshow{9edd35d8-3c57-4694-a150-acd1f574552f}

### Web16 ：php探针

解题思路：考察PHP探针php探针是用来探测空间、服务器运行状况和PHP信息用的，探针可以实时查看服务器硬盘资源、内存占用、网卡 流量、系统负载、服务器时间等信息。 url后缀名添加/tz.php 版本是雅黑PHP探针，然后查看phpinfo搜索flag 。

打开tz.php，再打开PHPINFO ，然后Ctrl+f 搜索 flag

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v24c0d9e3f6b0e41f78438a8efa5e3800f.png)

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/5e3674b3-815c-4cee-87c4-e38c0e4ef95c.png)

点进去后，搜索falg。

Flag：CTFSHOW{B1aff185-FAA7-4B70-9A6C-C63FB2746712}

### Web17 ： sql备份泄露

解题思路：通过dirsearch扫描，找到是sql备份泄露。

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/317de209-30df-4a5a-b3c1-fce4847f8732.png)

然后在url后面加上/backup.sql即可得到一个备份文件然后打开得到flag。

Flag：ctfshow{4e5e627a-7a34-4bc8-8d0d-fea312d15b3c}

### Web18 ：web小游戏（查看js源码）

解题思路：查看源码，像这种小游戏一般都会有源码大于多少会给出不同的结果，本题找到大于100，看到一个Unicode编码，将其转换为中文（http://www.msxindl.com/tools/unicode16.asp），然后得到 你赢了，去幺幺零点皮爱吃皮看看，在url后面输入110.php即可得到flag。

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/a5bafc9b-c602-4ac2-a59a-8dd3d97755db.png)

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/88b4e32c-55e5-4430-a196-a3f113f926e5.png)

Falg：ctfshow{6e1422cc-a184-4815-8b31-d21062522ac8}

### Web19：

解题思路：根据题目可知前端有我们想要的东西，打开源码，可以看到用户名和密码，但是密码需要解密，为AES加密①aes解密②可以通过hackbar获取密码③可以通过bp抓包。

①：aes解密

[img]https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/e4f50bc4-a564-4962-9a0f-c4624220b29e.png[/img]

②：hackbar

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/7a5d26fa-ff1f-4292-8848-d4e3a3620cca.png)
### Web20：

解题思路：mdb文件是早期asp+access构架的数据库文件 直接查看url路径添加/db/db.mdb 下载文件通过txt打开.

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/7bd55363-61d2-48b0-93ec-9e88d91df968.png)

Flag：flag={ctfshow\_old\_database}