# shadowsocks使用教程

标签： 翻墙 shadowsocks

---
本文介绍shadowsocks的基本和高级用法，高级用法主要是配合chrome的SwitchyOmega扩展使用，也介绍了如何让处在同一局域网的伙伴利用shadowsocks翻墙。 *（本文以windows系统为例）*

###一. 准备工作：

1. 从[shadowsocks官网](http://www.shadowsocks.org)下载一个适合你电脑版本的文件。[备用下载地址](https://raw.githubusercontent.com/ojawo/shadowsocks-/master/Shadowsocks-win-2.5.2.zip)
2. 下载之后解压，打开shadowsocks客户端。
3. 在如图所示的shadowsocks客户端中填入你的服务器IP、端口、密码、加密方式后点击 `添加` 按钮。*（免费的shadowsocks账号可从网上搜索得到，也可自己买一个vps后自己搭建shadowsocks服务器端，详见[史上最详尽Shadowsocks从零开始一站式翻墙教程](http://shadowsocks.blogspot.com/2015/01/shadowsocks.html)）*
![此处输入图片的描述][1]

###二. 基本用法：

最简单的用法就是直接勾选 `启用系统代理`，在 `系统代理模式` 中有 `pac模式` 和 `全局模式` 两种。`pac模式` 是代理自动配置模式，凡是在pac文件中的网址都会自动代理；`全局模式` 是不管你访问国内网站还是国外网站都使用代理。
![此处输入图片的描述][2]

启用系统代理后，不管你是使用chrome还是IE浏览器，都可以翻墙了，不需要进行额外的设置，即“傻瓜式翻墙”。
若其他软件如qq、百度云管家也要使用代理的话，设置代理服务器为 **127.0.0.1**，端口为**1080**（默认代理端口为1080，可更改为自己想要的端口，后文采用默认代理端口）
![此处输入图片的描述][3]

###三. 高级用法：

高级用法是在不勾选 `启用系统代理` 的基础上进行的。
shadowsocks在打开之时就已经开了一个本地代理端口 (http://127.0.0.1:1080)， 接下来就配合chrome扩展 [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif) 使用shadowsocks。
SwitchyOmega中有四种情景模式，本文介绍前三种模式，第四种“虚情景模式”笔者也不知道如何使用。
![此处输入图片的描述][4]

在设置好每一个情景模式后注意点击 `应用选项` 保存你的设置。

![此处输入图片的描述][5]

####1. 代理服务器模式
情景模式名称为“**proxy**”,代理协议为“**SOCKS5**”，代理服务器为“**127.0.0.1**”，端口为“**1080**”。设置完毕后如图所示。

![此处输入图片的描述][6]

####2. 自动切换模式
情景模式名称为“**SS-Autoproxy**”,规则列表规则对应的情景模式为“**proxy**”（即刚才建立的代理服务器模式），默认情景模式为“**直接连接**”。
规则列表格式选择 “**AutoProxy**”,规则列表网址为https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt，点击 `立即更新情景模式`，将规则列表下载到SwitchyOmega中。
**解释**：规则列表中的网址是网友收集的需要翻墙才能访问的网址，当SwitchyOmega发现你访问的网址在规则列表中时，就自动调用“**proxy模式**”（即使用shadowsocks的本地端口翻墙），否则访问一般国内的网址时（不在规则列表上）就采用默认情景模式“**直接连接**”，这样达到了自动切换的目的。
![此处输入图片的描述][7]
####3. PAC情景模式

shadowsocks客户端中选择“**使用本地PAC**”（推荐做法，方便个人定制，pac的编写请自己搜索），本地pac文件可从GFWList中更新，如图所示。

![此处输入图片的描述][8]

SwitchyOmega新建情景模式名称为“**SS-pac**”，PAC网址为“**http://127.0.0.1:1080/pac.txt**” (此处设置为使用本地pac文件，pac文件是shadowsocks已经自动建立好的，不需要额外设置)，点击 `立即更新情景模式`，将SwitchyOmega中的pac脚本更新。
![此处输入图片的描述][9]

**解释**：pac文件中的网址是需要翻墙访问的，SwitchyOmega选择“**SS-pac**”情景模式后，SwitchyOmega会对你将要访问的网址进行判断，在pac文件中的网址就自动代理，否则就直接连接，即达到上述2.自动切换模式的效果。

至此三种情景模式已经介绍完毕，使用时选择一个情景模式，就来到了久违的“世界网”！
![此处输入图片的描述][10]

###四. 共享翻墙网络

举个例子，现在你的笔记本电脑 **A** 已经开了shadowsocks客户端，如果你想让处在同一局域网的其他小伙伴也一起翻墙，那么该怎么做呢？

1. 在shadowsocks右键菜单中勾选 `允许来自局域网的连接`，不勾选 `启用系统代理`
2. 打开电脑的命令行输入端，输入“ipconfig”（不包括引号）查到笔记本电脑的ip地址。如图所示，笔者使用笔记本电脑ip地址为 http://192.168.1.101
![此处输入图片的描述][11]

3. * 共享给安卓手机：选择好wifi后点击详情，设置如图所示

![此处输入图片的描述][12]

  **解释：**手机选择和笔记本电脑同一个wifi后，在手机上wifi详情设置中，代理选择“**手动**”，主机名为**“192.168.1.101”**（即刚才查到的笔记本的ip地址，这里的ip是笔者笔记本所使用的ip，读者根据自己查到的ip来设置），端口选择**1080**。shadowsocks在笔记本电脑上开了本地翻墙端口1080，手机代理设置好后就把笔记本电脑当成代理服务器了。大功告成！
  * 共享给另一台电脑 **B**：
 在另一台电脑 **B** 和笔记本电脑 **A** 使用chrome或者IE浏览网页时,通过设置IE选项即可翻墙（chrome代理默认采用IE设置）。

![此处输入图片的描述][13]
![此处输入图片的描述][14]

在IE选项中的“**连接**”选项卡选择“**局域网设置**”，代理服务器地址填写查到的笔记本**A**的ip（也即电脑**B**通过笔记本**A**来翻墙），端口为**1080**,不使用自动配置。若使用自动配置，则不勾选下方的手动配置，
在自动配置脚本中填写**192.168.1.101:1080/pac.txt**，可实现智能翻墙。


**本教程到此结束，希望能帮助到各位！**


  [1]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/1.jpg
  [2]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/2.jpg
  [3]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/12.jpg
  [4]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/3.jpg
  [5]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/8.jpg
  [6]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/4.jpg
  [7]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/5.jpg
  [8]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/7.jpg
  [9]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/6.jpg
  [10]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/9.1.jpg
  [11]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/10.jpg
  [12]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/11.1.png
  [13]: https://raw.githubusercontent.com/ojawo/shadowsocks-/master/13.jpg
  [14]:https://raw.githubusercontent.com/ojawo/shadowsocks-/master/14.jpg