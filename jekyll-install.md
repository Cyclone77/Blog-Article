---
layout: index
title: "本地搭建jekyll模版发布系统"
---

# 本地搭建jekyll模版发布系统

**前言**：这几天看了下jekyll这个静态网页发布系统，由于不依赖后台所以我选择用他了，当然用他只是生成自己的一个简单的博客，所以不需要什么大的后台做支撑，当然如果你需要后台交互的话那就另说了，我想写一个自己的一本书，（书籍的生成也有其他的工程包比如gitbook）那就先拿jekyll模版发布引擎先说了，好了废话不多说，开始进入正题。

**jekyl介绍**：jekyll只用 Markdown (或 Textile)、Liquid、HTML & CSS 就可以构建可部署的静态网站。

**配置说明**：我的当前系统环境是window7 64位操作系统，所以我只是在window7做了下jekyll的环境搭建

### 第一步安装ruby

由于jekyll是依赖ruby开发的所以我们先把ruby安装一下，[点击安装地址]("http://rubyinstaller.org/downloads/")如下图我选择了64位的（PS：根据你自己的系统，判断安装所需版本）

下载完之后，点击安装会出现如下界面 ,勾选第二个自动配置环境变量，要不然你用命令的时候会找不到ruby程序的。所以勾选上第二个噢噢 


当安装完毕的时候打开命令控制台，如下图：


如果如下命令查看ruby版本，因为我装ruby是2.0以上的版本所以我在这里就不在单独安装RubyGems了，ruby2.0一下的好像就不再包含rubygems模块了。
（PS:rubygems和node的npm差不多都是包管理）
	
	ruby -v

测试是否安装成功

### 第二步安装devkit

我先解释下为什么要安装devkit

在使用RubyGems安装其他类库的的同时。有一些是保持C语言编写的代码的形式来发布的，它们需要通过使用“gcc”编译器等结合环境一起来编译，但在Windows上是不能用gcc来编译的。满足我的win系统的需求所以我们必须的安装devkit来填补win7的缺陷。

好了现在我们就开始安装吧 [点击安装地址](http://rubyinstaller.org/downloads/)

如下图所示,我选择了DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe因为这个才符合我的64位操作系统要求


> 下载完之后直接点击安装, 安装完成之后。需要通过我们的命令窗口进入到我们安装的目录下，输入两个命令：
> 
	ruby dk.rb init
	ruby dk.rb install

通过输出我们可以看出我们dk.rb.init是成功的，可是第二个命令安装报错了，说我们的config.xml找不ruby程序的配置项.所以我们打开我们的config.xml,配置下我们的ruby安装目录,如下图所示

当我们配置好路径之后，再次运行安装dk.rb install命令会看到如下图所示：

到这里我们可以说这两个在我的window下安装成功了。哈哈

### 第三步安装jekyll

#### 1.修改gem安装库的源

(PS:这部分只是为了下载jekyll而做的一写特殊准备工作，如果你的电脑可以翻墙可以忽略过去，直接看  **2.gem命令安装jekyll**)
> 安装jekyll是通过gem命令安装的，由于我大天朝特殊的安全意识，所以我大天朝启动了长城牌安全防火墙。所以有部分的国外网站地址我们是访问不到的，以至于我们安装不上我们的jekyll。所以为了下载jekyll我们的需要修改安装源，gem默认安装的类库都是在http://rubygems.org/上存放的。
因为我只知道淘宝有个镜像，所以我想把gem安装类库的源改成淘宝镜像地址，这样我们安装jekyll就省事多了。

为了快点，替换gem安装的类库源我就不做截图了，直接写命令了

打开我们的命令窗口

* 先把gem安装类库的默认源http://rubygems.org/给移除掉

		gem sources --remove http://rubygems.org/

* 配置我们新的淘宝镜像地址源
		
		gem sources -a http://ruby.taobao.org/
		或
		gem sources -a https://gems.ruby-china.org/

* 如果想查看是否切换了可以用以下命令查看我们当前gem下载类库源是哪
		
		gem sources -l

#### 2.gem命令安装jekyll

为了照顾小白我也是算是墨迹够了又是截图又是命令的。好了我也不抱怨了，还是规规矩矩的写下去吧。

打开我们的命令窗口，执行一下命令安装我们的jekyll
		
	gem install jekyll

安装完成之后如下图所示 

查看jekyll安装的版本可以执行一下命令
	
	jekyll -v

查看jeyll的所有命令

	jekyll --help  
	jekyll -h

### 第四步用jekyll发布静态网站

其实生成静态网站直接看官网就行了，可是我为了保证小白一路顺风还是给你们生成一个exmple吧，毕竟都写到这里了，不妨完成一个demo。
我们通过命令**jekyll -h**命令知道他有一个new 命令可以为我们创建一个简单静态的网站demo，

打开我们的命令控制台输入以下命令：

	jekyll new projectName

原本通过这个就可以妥妥的生成一个静态的网站，谁知道不是我预想的那样访问到，噼里啪啦的报错,整的你恶心，我就把我报错的解决办法以及报错的内容展示你们，以后也少走弯路了。

当我输入**jekyll new heimao_blog**原本在我所在目录下生成一个**heimao_blog**的文件夹，可是命令控制台包错，如下图错：

通过报错的情况来看是我没有安装叫bundler的依赖类库，所以我要再次安装下这个类库。安装命令如下：

	gem install bundler

安装完事之后你再次启动命令**jekyll serve**结果又是报错了，报错内容如下：

通过这张图我们可以看到，我又缺少了一个叫**minima** 的类库。真“mini妈”啊小了就是藏的深啊。不一下子给我爆出来，我们继续安装这个**minima**的类库命令如下：
	
	gem install minima

到这里心里想终于解决完了，好了继续运行我的服务命令**jekyll serve** 又TMD报错，这下我可怒了玩一个自建博客有TMD这么费劲吗报错如下图

通过这个我们可以看到这4000端口被占用了，也就是没有权限我也是醉了。。。好吧我从新监听一个服务器端口号叫8000的命令如下
	
	jekyll serve --port 8000

我执行下完美运行，然后你可以访问http://localhost:8000网址效果如下

呵呵今天就写到这里了，，，，，，尽管报了一路子错，但是我相信肯定有解决办法的，俗话说的好办法总比错误多呵呵。。。

生成的**heimao_blog**文件至于里边的结构你可以参考[目录结构介绍](http://jekyll.com.cn/docs/structure/)进行你自己的网站设计修改


如果你嫌弃这样做的不知道new出来的一个什么项目。
手动创建文件分类或者自己的网站风格的话可以参看[官网教程](http://jekyll.com.cn/)
逐个新建我们的文档，布局，生成文件，和我们的_config.yml。

对了我这推荐一些jekyll的一些模版请笑纳呵呵。

[http://jekyllthemes.org/](http://jekyllthemes.org/)

[https://github.com/jekyll/jekyll/wiki/Sites](https://github.com/jekyll/jekyll/wiki/Sites)

[转自](https://heimiao.github.io/heimao_blog//2016/11/16/jekyll_down.html)
 