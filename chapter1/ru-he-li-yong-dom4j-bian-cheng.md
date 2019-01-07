相信你对Dom4j的功能已经有了一个比较好的理解了，如果还没有建议先去看看我所推荐的有关XML和XML DOM的相关教程再继续下面的内容。

下面，我们就开始下载安装Dom4j吧！

#### STEP1：下载Dom4j类包

* 打开网页：[https://dom4j.github.io/ ](https://dom4j.github.io/)

你将会看到如下界面：

![](/assets/loaddom4j1.png)

* 选择你需要的Dom4j版本，点击Download，我这里选择了v2.1.1版本，只下载dom4j-2.1.1.jar包就可以使用了，其中dom4j-2.1.1-sources.jar是相应的Dom4j源代码，dom4j-2.1.1-javadoc.jar是配合dom4j-2.1.1.jar使用的一个代码与注释包（建议下载下来，以便对照后面的内容学习）。

* 下载好后是这个样子：

![](/assets/loaddom4j2.png)

#### STEP2：在Eclipse中导入Dom4j的jar包

* 先创建一个普通的控制台Java项目（project），我取名为test\_dom4j
* 在Project Manager里选中项目test\_dom4j文件夹然后右键选择Build Path，可以看到如下界面：

![](/assets/loaddom4j3.png)

点击Add External JARs进入文件夹浏览找到刚才下载好的jar包导入，然后点击Apply=&gt;Apply and Close即可。

#### STEP3:检查Dom4j的jar包是否导入成功

* 在Project manager里的Referenced Libraries里如果看到Dom4j的jar包，那么恭喜你，jar包导入成功，接下来你可以尽情使用它了！

![](/assets/loaddom4j5.png)



