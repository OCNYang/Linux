# Linux 之行

> 在几经思量后，将电脑的 Window 10 + Ubuntu 16.04 双系统改成 Manjaro 单系统。在此记录下使用过程中遇到的坑坑洼洼...

## 1. 将本地数据包与远程数据包同步$  

```
sudo pacman -Syy
```

默认manjaro是没有同步数据包的，也就是说，这个时候你执行pacman -S pack_name 会报数据包找不到的错误(warning: database file for 'core' does not exist ...)

## 2. 安装vim

vim 无疑是linux下最好用的编辑器之一，为了方便我们待会修改配置文件，可以先将这个软件装上

```
sudo pacman -S vim
```

如果我们没有执行第一步操作，这个时候直接安装是会报错的

## 3. 添加 archlinxCN 源 $  
用vim编辑 `/etc/pacman.conf 文件(sudo vim /etc/pacman.conf)`，在文件底部添加下面几行:

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server =https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

修改配置文件后，执行下面的命令：

```
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

## 4. 安装ZSH

ZSH可以说是shell中的极品，它的优点太多了，我就不一一写出来，有兴趣的同学可以看这篇文章知乎-为什么说zsh是shell中的极品

我们首先将git安装好，因为我们带回配置的时候需要用到git

```
sudo pacman -S git
```

接着安装zsh
```
  sudo pacman -S zsh
```
然后配置oh-my-zsh


    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"


更换默认的shell

```
  chsh -s /bin/zsha
```

好了，zsh已经安装完了

## 5. 安装搜狗输入法$

我们安装基于fcitx的搜狗输入法

```
sudo pacman -S fcitx-sogoupinyin
sudo pacman -S fcitx-im         # 全部安装
sudo pacman -S fcitx-configtool # 图形化配置工具
```

设置中文输入法环境变量，编辑 ~/.xprofile 文件，增加下面几行(如果文件不存在，则新建)

```
exportGTK_IM_MODULE=fcitx
exportQT_IM_MODULE=fcitx
exportXMODIFIERS="@im=fcitx"
```

保存成功后，在终端输入fcitx启动服务后，会增加一个键盘的托盘图标，右击这个图标，打开配置工具，在输入法栏目中增加sogoupinyin输入法。

重启后就能正常使用了。

## 6. 常用软件安装

```
//Chrome $
sudo pacman -S google-chrome
```

```
//Atom $
sudo pacman -S atom
```

```
//网易云音乐 $
sudo pacman -S netease-cloud-music
```

```
//WPS $
sudo pacman -S wps-office
```

> WPS 缺少字体的解决方法
解决方法：
从http://bbs.wps.cn/thread-22355435-1-1.html下载字体库，离线版本：（链接: https://pan.baidu.com/s/1i5dzB9r 密码: pwe1）

提示：以下方式任选一个
1、解压

```
sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office
```

解压完成后再次打开WPS就不会看到以上错误。

2、注意：一定要以wps-office的文件夹进行保存，如果没有以这样命名，那么可以按照以下方法（也可以直接将解压出来的字体文件移动到上面文件夹下）进行：

```
#生成字体的索引信息
sudo mkfontscale
sudo mkfontdir
#运行fc-cache命令更新字体缓存
sudo fc-cache
```

重启WPS即可。

3、这种方式是直接双击字体进行安装，进入到解压出的文件，双击即可。

4、我在Octopi中有WPS的字体程序，可以直接安装（未测试不知道行不行，应该可以），命令安装如下：

```
sudo pacman -S ttf-wps-fonts
```

其他的软件就不多说了，大家可以自行去AUR上查找.

## 7、Git $
(没有安装，好像系统自带的)

```
$ git config --global user.name "w3c"
$ git config --global user.email w3c@w3cschool.cn
$ git config --list
```

## 8、Java $
(没有安装，系统自带，不过好像是openJdk8)

## 9、Foxit Reader $

## 10、更新软件包签名秘钥

```
sudo pacman -Sy archlinux-keyring manjaro-keyring
sudo pacman-key --populate archlinux manjaro
sudo pacman-key --refresh-keys
```

出现Keys错误，签名失败之类的  
依次运行以下命令  
```
# 移除旧的keys
sudo rm -rf /etc/pacman.d/gnupg
# 初始化pacman的keys
sudo pacman-key --init
# 加载签名的keys
sudo pacman-key --populates archlinux manjaro
# 刷新升级已经签名的keys
sudo pacman-key -refresh-keys
# 清空并且下载新数据
sudo pacman -Sc
# 更新
sudo pacman -Syu
```

## 11、安装 Java
```
0.卸载自带的openJDK
sudo pacman -R jdk8-openjdk
sudo pacman -R jre8-openjdk
sudo pacman -R jre8-openjdk-headless

1.可选择使用 yaourt进行安装，其会自动配置。
    yaourt jdk
选择列出来的 OracleJDK8

2.手动安装， 下载tar.gz包  [下载地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
# 解压
tar -zxvf xxx.tar.gz
# 移动到 `/opt`目录下
sudo mv xxx /opt/

# 配置jdk环境变量  修改配置文件`/etc/profile`
# setting for jdk-oracle
JAVA_HOME=/opt/jdk1.8.0_131
CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
# end

# 启用配置
source /etc/profile
```

查看安装目录（同时可以用到其他软件的才看上）
```
whereis java
which java （java执行路径）
echo $JAVA_HOME
echo $PATH
````

## 12、VirtualBox $  

### 安装前需要知道   
你需要知道你当前的内核版本  
```
uname -r //比如输出了 4.14.20-2-MANJARO 那么你的内核版本为414
```

### 安装VirtualBox
1. `sudo pacman -S virtualbox`
这里需要选择与当前内核相同的内核模块比如笔者正在使用的内核版本为 414，则需要安装 linux414-virtualbox-host-modules  

2. `sudo pacman -Ss virtualbox-ext-oracle` #安装扩展包  
你也可以去官网下载扩展包

3. `sudo gpasswd -a $USER vboxusers` #添加当前用户到 vboxusers  
这里需要将 `$USER` 替换为你的用户名，如果不需要使用USB外设，可以不执行此操作。

4. 重新启动系统或执行 `sudo modprobe vboxdrv`

## 13安装 Android studio $

## 14安装有道翻译

## 15、安装 digiKam (Adobe Lightroom) $

## 16、命令行打印出 manjaro 图标及系统信息

`screenfetch`

## 17、安装 shadowsocks-qt5 $

## 18、卸载软件

删除单个软件包，保留其全部已经安装的依赖关系
`pacman -R package_name`   

删除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系：
`pacman -Rs package_name`  



要删除软件包和所有依赖这个软件包的程序:
`pacman -Rsc package_name`  

> 警告: 此操作是递归的，请小心检查，可能会一次删除大量的软件包。


要删除软件包，但是不删除依赖这个软件包的其他程序：  
`pacman -Rdd package_name`  

pacman 删除某些程序时会备份重要配置文件，在其后面加上*.pacsave扩展名。-n 选项可以删除这些文件：
`pacman -Rn package_name`  
`pacman -Rsn package_name`

用pacman应该就可以了

## 19、manjaro android studio 神坑

> 找了一两天的方法，一条一条尝试Google出来的方法，最终解决
```
What went wrong: A problem occurred configuring project ':app'.  
executing external native build for cmake 包路径/app/CMakeLists.txt
```

[解决方法地址](https://stackoverflow.com/questions/40696961/error-on-build-cpp-project-in-android-studio-in-linux)

> I experienced the same issue. Quite many of the other distributions often have outdated packages. They still use ncurses 5 instead of ncurses 6 (libtinfo seems to belong to ncurses). Assuming, that the android ndk's version of clang was built on such a system, it was worth a try to use ncurses 5. From the Arch User Repositories I was able to install the latest version of [ncurses5-compat-libs](https://aur.archlinux.org/packages/ncurses5-compat-libs/) (and I also installed the 32bit version: lib32-ncurses5-compat-libs). This solved the problem for me. If you have it installed already, try reinstalling or updating it, if it is outdated.

manjaro(archlinux) 使用的是 ncurses,需要下载ncurses5-compat-libs

## 20、octopi 0.8.12-2 无法连接网络 错误

是无法连接 www.google.com 测试网络的。（已在github上提交Issues证实，Fixed at upstream.）

目前解决方法：add 127.0.0.1 www.google.com to the /etc/hosts（我使用的127.0.0.2）

## 21、KGet 不能加载插件

插件加载器无法加载插件：`/usr/lib/qt/plugins/kget/kget_bittorrent.so。`  

又是依赖包冲突没有解决好。  
安装 libktorrent 2.1.1

## 22、ubuntu 下的 IDEA 如何实现全局菜单？

链接：[知乎](https://www.zhihu.com/question/45535175/answer/104979742)

### 步骤 1  
安装 Jayatana package :  

  sudo add-apt-repository ppa:danjaredg/jayatana
  sudo apt-get update
  sudo apt-get install jayatana

### 步骤 2（以下方法选其一）

* 方法１  
一次修改支持所有 jetbrains 系列开启全局菜单。  

  sudo gedit /etc/profile

添加 `export _JAVA_OPTIONS="-javaagent:/usr/share/java/jayatanaag.jar"`  

更新修改的配置文件  

  source /etc/profile  

* 方法２  
单独修改某软件目录下的. vmoptions 配置文件，只对该款软件有效。  
打开软件目录下的 `bin/idea64.vmoptions` 文件，添加以下内容 :  
`-javaagent:/usr/share/java/jayatanaag.jar`

* 移除安装的包  

1. Remove previously appended line from `bin/idea64.vmoptions`.
2. Remove Jayatana package:  

  sudo apt-get --purge remove jayatana libjayatana libjayatanaag libjayatana-java libjayatanaag-java
