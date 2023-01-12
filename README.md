# Headline
# **搭建自己的知识库并且发布到公网上**
**使用的系统：**** Windows**

**使用到的工具：①**** docsify ****（需要电脑有**** npm ****工具，如果没有可以下载一个**** node.js ****），**** Git bash**

_ **步奏：** _

①使用npm安装docsify本地命令行工具npm install -g docsify-cli

②建立一个文件夹，之后使用docsify命令对新建文件夹进行初始化，docsify init，然后输入y。

③启动网站，输入docsify serve，到现在为止该网站已经搭建好了。

④文件夹里有index.html是网站的主入口文件，readme.md是网站的内容文件，docsify可以把markdown渲染成网页。

⑤如果想要别人也可以访问那么你需要将网站部署到公网上，这里推荐使用github的GitHub pages功能不需要服务器就可以直接部署，对新手也是十分友好。

⑥登录github创建仓库

起个名比如我的就叫Blogs.github.io（注意这里后面必须要有github.io）

除了名字，别的什么都不需要，直接点击Create New repository就行了。

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v2effeee1a52d447fbb231543c0d494f60.png)

然后右键点击你建好的文件夹，找到Git Bash Here，打开Git命令行，然后输入以下命令行。

git init #初始化一个仓库

git add -A #添加所有文件到暂存区（先交给git管着）

git commit -m "fuck" #提交到git仓库，-m后面是注释

git remote add origin https://github.com/A1walker/Blogs.git #网址要根据情况变化

git push -u origin master #推送到远程Blogs仓库

这时，本地MyBlogs应该就同步到GitHub上面了。

_ **用** __ **GitHub Pages** _ _ **建立站点** _

相当简单，令人发指的简单！在GitHub上你的仓库那里，选中Settings选项，向下滑动到GitHub Pages页签，在Source下面选择master branch/docs folder。

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v2f68ea0f6c69d47dba1f28cbaf900e8de.png)

到这里就结束了，去访问你的网站吧（xxx.github.io）

_ **！如果需要从新部署，以下为操作过程（新手必踩的坑已经帮你们填好了）** _。

操作和上面一样不过使用git 添加远程[github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020)仓库的时候提示错误：fatal: remote origin already exists.因为你之前已经部署所以这里会说仓库已经存在。

![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v22040e21aa788470db4997d0f6966f17f.png)

1、先删除远程 Git 仓库

$ git remote rm origin

再添加远程 Git 仓库

git remote add origin https://github.com/A1walker/Blogs.git #网址要根据情况变化

3、后面还按照上面部署步奏走，这里就是多了一个删除远程git仓库。
