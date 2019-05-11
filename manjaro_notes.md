# manjaro notes
## 1,修改hosts,重启网络使github可以连接 
```bash
#查询域名global-ssl.fastly.Net和 github.com 公网地址
#https://www.ipaddress.com/。
#或者http://tool.chinaz.com/dns/?type=1&host=github.global.ssl.fastly.net&ip=      查找检测列表里的TTL值最小的IP
#分别查找
#github.global.ssl.fastly.net
#github.com
--------------------- 

sudo nano /etc/hosts
##备注在本机上装双系统git不需要设置，网很快不知道原因
```
#编辑文本

--------------------------------------------------------------
>127.0.0.1       localhost
> 127.0.1.1       fydman
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
#add new
192.30.253.113    github.com
192.30.252.131 github.com
185.31.16.185 github.global.ssl.fastly.net
74.125.237.1 dl-ssl.google.com
173.194.127.200 groups.google.com
192.30.252.131 github.com
185.31.16.185 github.global.ssl.fastly.net
74.125.128.95 ajax.googleapis.com

``` bash
sudo systemctl stop NetworkManager.service
sudo systemctl restart NetworkManager.service
```

## 2,添加pacman源,安装AUR软件
### 更换软件源镜像
#### 自动选择
```bash
#search nearest mirrot in China
sudo pacman-mirrors -i -c China -m rank
```
#### 手动选择
编辑文件：/etc/pacman.d/mirrorlist,在最上面添加：
```
# 清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
# 阿里云
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```
### 添加软件源 添加archlinuxcn软件源，AUR源
编辑文件/etc/pacman.conf，在最下面添加：
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
更新软件源并导入公钥
```bash
sudo pacman -Syy
sudo pacman -S archlinuxcn-keyring
```
#### 配置pacman多线程多链接加速 

sudo pacman -Sy aria2c
编辑pacman配置文件/etc/pacman.conf，找到Xfercommand修改成如下：

```shell
......
# aria2c 多线程多链接
XferCommand = /usr/bin/aria2c --allow-overwrite=true --log-level=error -l aria2c-error.log -c -m2 -x 8 -s 8 -j 8 -d $(dirname %o) -o $(basename %o) %u
......
```

#### 配置yaourt多线程多链接加速 

编辑makepkg配置文件/etc/makepkg.conf，找到DLAGENTS修改成如下

```nano
...... 
#DLAGENTS=('file::/usr/bin/curl -gqC - -o %o %u' 
#          'ftp::/usr/bin/curl -gqfC - --ftp-pasv --retry 3 --retry-delay 3 -o %o %u' #          'http::/usr/bin/curl -gqb "" -fLC - --retry 3 --retry-delay 3 -o %o %u' #          'https::/usr/bin/curl -gqb "" -fLC - --retry 3 --retry-delay 3 -o %o %u' #          'rsync::/usr/bin/rsync --no-motd -z %u %o' 8#          'scp::/usr/bin/scp -C %u %o') 
#aria2c 多线程多链接
DLAGENTS=('file::/usr/bin/aria2c --allow-overwrite=true --log-level=error -l aria2c-error.log -c -m2 -x 8 -s 8 -j 8 %u -o %o'
	'ftp::/usr/bin/aria2c --allow-overwrite=true --log-level=error -l aria2c-error.log -c -m2 -x 8 -s 8 -j 8 %u -o %o' 
	'http::/usr/bin/aria2c --allow-overwrite=true --log-level=error -l aria2c-error.log -c -m2 -x 8 -s 8 -j 8 %u -o %o' 
	'https::/usr/bin/aria2c --allow-overwrite=true --log-level=error -l aria2c-error.log -c -m2 -x 8 -s 8 -j 8 %u -o %o'         
	'rsync::/usr/bin/rsync --no-motd -z %u %o'
	'scp::/usr/bin/scp -C %u %o')
......

```

## 安装AUR安装器
yaourt yay
```bash
sudo pacman -Syy
sudo pacman -S yourt
yaourt -S yay
```
## 安装搜狗输入法,设置输入法
```bash
sudo pacman -S fcitx fcitx-configtool fcitx-qt4 fcitx-qt5
#配置~/.Xprofile，重启
sudo nano ~/.Xprofile
_________________________________
      export LC_ALL=zh_CN.UTF-8
       export GTK_IM_MODULE=fcitx
       export QT_IM_MODULE=fcitx
       export XMODIFIERS="@im=fcitx"
reboot
```
## 设置时间
解决双系统不一致问题
sudo timedatectl set-local-rtc 1

## 安装字体
### 

```
sudo pacman -S ttf-dejavu wqy-zenhei wqy-microhei  ttf-monaco ttf-wps-fonts adobe-source-code-pro-fonts adobe-source-sans-pro-fonts adobe-source-serif-pro-fonts adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
```

要使用新安装的字体，需要再设置里自行选择。

设置——》外观——》字体——》选择默认字体和默认等宽字体

### 安装ms-fonts
<p><a href="https://wiki.archlinux.org/index.php/Microsoft_fonts_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)" title=" Microsoft fonts (简体中文)"></a></p>

[Microsoft fonts (简体中文)]: https://wiki.archlinux.org/index.php/Microsoft_fonts_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

将Windows的字体复制到`/usr/share/fonts`: 

```shell
# mkdir /usr/share/fonts/WindowsFonts
# cp /windows/Windows/Fonts/* /usr/share/fonts/WindowsFonts
# chmod 755 /usr/share/fonts/WindowsFonts/*
```

然后重新生成字体缓存： 

```shell
# fc-cache -f 
```

##  安装markdown编辑器
```shell
yay -S typora
```


主题下载后将css放到主题文件夹里
https://theme.typora.io/

## 安装vim,zsh及配置
### vim
```shell 
yay -S vim
```

#### 超强vim配置 
项目地址：https://github.com/elinuxboy/vim-deprecated
使用下面的命令自动安装配置：

```bash
wget -qO- https://raw.githubusercontent.com/elinuxboy/vim-deprecated/master/setup.sh | sh -x
```
或者用另一种方式自动安装：
```shell
wget https://raw.githubusercontent.com/elinuxboy/vim-deprecated/master/setup.sh
chmod +x setup.sh
./setup.sh
```
###  zsh
```bash
yay -S zsh
#安装oh-my-zsh
wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh
chmod +x install.sh
sh install.sh
chsh -s /bin/zsh
```
#### 配置oh my zsh 

- 安装autojump自动跳转插件
```shell
sudo pacman -S autojump
echo ". /usr/share/autojump/autojump.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source .zshrc
```
- 安装zsh-syntax-highlighting语法高亮插件
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
echo "source $ZSH_CUSTOM/plugins/zsh-syting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source ${ZDOTDIR:-$HOME}/.zshrc
```
- 安装zsh-autosuggestions语法历史记录插件 #下面的两个插件不要装，反应慢
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
echo "source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source ${ZDOTDIR:-$HOME}/.zshrc
```
- 安装自动补全插件incr
```shell
cd $ZSH_CUSTOM/plugins
mkdir incr
cd incr
wget http://mimosa-pudica.net/src/incr-0.2.zsh
echo "source $ZSH_CUSTOM/plugins/incr/incr*.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source ${ZDOTDIR:-$HOME}/.zshrc
```
![img](https://img2018.cnblogs.com/blog/1548353/201812/1548353-20181215111451681-1556416322.png)

详细的.zshrc请参见附录IV。
- 修改主题
修改.zshrc文件
```shell
vim ~.zshrc
```
找到ZSH_THEME=“robbyrussell”，修改为：ZSH_THEME=“ys”；
```shell
ZSH_THEME="ys"
```
更新配置
```shell
source .zshrc
```

## 安装编辑软件（Java,Rstudio,VScode,Python)
### java

```shell
java -version
#卸载openJDK,安装ocral java
yay -Rs
yay -S java
```

### 安装R和Rstudio

參考wiki arch

   ```bash
  yay -S  rstudio-desktop-bin
   #会自动安装R R.x.x.1-git
   #但R的单独运行，需要安装install.packages() requires tk to be installed for selecting mirrors.
   yay -S tk
   #配置R的环境，全部sudo权限下运行R，所以包的安装都在/usr/lib/R/library里，故配置Renviron文件
   sudo nano /etc/R/Renviron
   ```
> ---------
> ```nano
> #修改其中的到指定的文件下，添加
> R_HOME_USER = #/path/to/your/r/directory
> R_PROFILE_USER = ${HOME}/.config/r/.Rprofile ##最好定义好HOME
> R_LIBS_USER=${R_LIBS_USER-'/usr/lib/R/library'}
> #R_LIBS_USER=${R_LIBS_USER-'/lib/R//library'} #/path/to/your/r/library 主要是这个文件有使用系统package的权利
> R_HISTFILE = /path/to/your/filename.Rhistory     ## Do not forget to append the .Rhistory                    MYSQL_HOME = /var/lib/mysql
> ```
>
> ```bash
> mrdir  ${HOME}/.config/r
> touch ${HOME}/.config/r/.Rprofile
> nano ${HOME}/.config/r/.Rprofile
> ```
> ```bash
> #The .First function is called after everything else in .Rprofile is executed
> .First <- function() {
> # Print a welcome message
> message("Welcome back ", Sys.getenv("USER"),"!\n","working directory is:", getwd())
> }
> options(digits = 12)                                          # number of digits to print. Default is 7, max is 15
> options(stringsAsFactors = FALSE)                             # Disable default conversion of character strings to factors
> options(show.signif.stars = FALSE)                            # Don't show stars indicating statistical significance in model outputs
> local({
> 	r <- getOption("repos")            
> 	r["CRAN"] <- "https://mirrors.tongji.edu.cn/CRAN/"			 # hard code the CN repo for CRAN
> 	options(repos = r)
> })	
> 		
> ```
>
> ```R
> #设置Bioconductor镜像
> #Rstudio里运行
> options(BioC_mirror="http://mirrors.ustc.edu.cn/bioc/")
> #修改字体改变光标错位的问题
> ```

```shell
#更改启动方式，以管理员的身份运行
sudo /usr/bin/rstudio-bin  --handless --no-sandbox
```

```R
#安装基础的包,设置好Rstudio的镜像源
R必须包：reshape2,ggplot2,export，XML,
bioconductor
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()
library("BiocManager")
BiocManager::install("Biobase")
BiocManager::install("GEOquery")
BiocManager::install("org.Hs.eg.db",version="3.8")
BiocManager::install("clusterProfiler")
options( download.file.method.GEOquery = 'libcurl' )
R语言里的
function定义 例子download_GEO<- function(ID) {}
 if (!file.exists( GSE_file )) {
    download_GEO <- function(ID) {
      options( download.file.method.GEOquery = 'libcurl' )
      gset <- getGEO( ID, getGPL = T, destdir = "./raw_data" ) 
      #使用getGEO(GEO = NULL, destdir = tempdir(), GSElimits = NULL, GSEMatrix = TRUE, AnnotGPL = FALSE#注释平台文件, getGPL = TRUE#平台文件, parseCharacteristics = TRUE)
      save( gset, file = GSE_file )	#保存到本地
    }
    download_GEO( ID)
  }

```

#### 更换Rstudio的主题

```R
rstudioapi::addTheme("https://raw.githubusercontent.com/jnolis/synthwave85/master/Synthwave85.rstheme", TRUE, TRUE, FALSE)
```



## 虚拟机安装vm-tools,arch linux 参见wiki需要安装open-vm-tools

```bash
sudo pacman -S open-vm-tools open-vm-tools-dkms 
#Try to install gtkmm3 manually if it does not work properly. To enable copy and paste between host and guest gtkmm3 is required. 
#Shared Folders with vmhgfs-fuse utility 共享文件
vmware-hgfsclient
sudo mkdir <shared folders root directory>
sudo vmhgfs-fuse -o allow_other -o auto_unmount .host:/<shared_folder> <shared folders root directory>
#Create the following .service
sudo touch /etc/systemd/system/<shared folders root directory>-<shared_folder>.service

## sudo nano /etc/systemd/system/<shared folders root directory>-<shared_folder>.service

[Unit]
Description=Load VMware shared folders
Requires=vmware-vmblock-fuse.service
After=vmware-vmblock-fuse.service
ConditionPathExists=.host:/<shared_folder>
ConditionVirtualization=vmware
wenti
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/vmhgfs-fuse -o allow_other -o auto_unmount .host:/<shared_folder> <shared folders root directory>
[Install]
## WantedBy=multi-user.target
Enable the <shared folders root directory>-<shared_folder>.service mount target.
## Enable the <shared folders root directory>-<shared_folder>.service mount target.
sudo enable /etc/systemd/system/<shared folders root directory>-<shared_folder>.service
```

## 安装部分软件

```bash
#安装google
yay -S google-chrome
```
--------------------- --------------------- ---------------------
QQ
安装QQ或TIM的任意一种：pacman -S deepin.com.qq.office或pacman -S deepin.com.qq.im 

注：==KDE==里无法正常使用wine QQ方案

```
yaourt -S deepin-wine-tim
```



--------------------- --------------------- ---------------------
Linux微信electronic-wechat:

```shell
yay -S electronic-wechat
```

--------------------- --------------------- ---------------------
网易云音乐
```shell
yay -S netease-cloud-music
```
--------------------- --------------------- ---------------------
Foxit
强大的PDF阅读器Foxit: 
```shell
yay -S  foxit
```
--------------------- --------------------- ---------------------
护眼软件红移redshift
```shell
yay -S redshift
```
--------------------- --------------------- ---------------------
#办公软件WPS
安装软件和缺失字体：

```shell
sudo pacman -S wps-office
sudo pacman -S ttf-wps-fonts
```
解决无法输入中文问题：
sudo vim /usr/bin/wps，在第一行（#!/bin/bash）下面添加：

```
sudo vim /usr/bin/wps        # 编辑wps配置文件
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
--------------------- --------------------- ---------------------
安装截图软件
```shell
yaourt -S deepin-screenshot
```
--------------------- --------------------- ---------------------
安装aria2c
```shell
sudo pacman -Sy aria2c
```
--------------------- --------------------- ---------------------

安装ps,AI GIMP, Inkscape
```shell
yay -S gimp
yay -S inkscape
```

安装epub书阅读软件

```shell
yay -S bookworm
#https://babluboy.github.io/bookworm/
```

安装torrent软件

```shell
yay -S deluge
```

安装cairo dock栏

```shell
yay -S cairo-dock
```

配置firefox下载器

```shell
#uget+aria2
yay -S uget
#打开Uget的设置（点击屏幕右上角‘Uget’小图标），打开设置界面如图，切换到“插件”界面，选择“启用aria2插件”启用aria2；
#选择Uget主界面左边的分类栏鼠标右击，选择属性，切换到默认一般设置，这里可以设置默认的下载路径和调整最大连接数（建议5个左右）
#配置Firefox火狐浏览器：安装flashgot
https://flashgot.net/
https://addons.thunderbird.net/en-US/seamonkey/addon/flashgot/
```

![img](https://upload-images.jianshu.io/upload_images/3970488-00eb5fcd42cc65cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000)

![img](https://upload-images.jianshu.io/upload_images/3970488-d21eb594920de5fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000)

### 卸载不必要的软件

```shell
#卸载libreoffice
sudo pacman -Rs libreoffice-still libreoffice-still-zh-cn
```
### 解决下载GitHub项目速度慢的问题
方法一：使用ladder（需要费用）
方法二：修改hosts文件（稳定，免费）

方法二具体操作：
step1 在我的电脑路径中输入：C:\Windows\System32\drivers\etc
step2 点击hosts文件，右键属性，修改本机用户的权限，给予全部权限，保存设置
step3 双击hosts文件，以记事本形式打开，在原始文本下一行粘贴上以下地址：
219.76.4.4 github-cloud.s3.amazonaws.com
也可以添加其他的地址：
地址来源：在http://tool.chinaz.com/dns中搜索 github.global.ssl.fastly.net 和 github.com
分别找到延迟小的地址，加入到hosts文件中即可
保存
--------------------- 

### 安装docker
sudo pacman -S docker
开启Docker
Docker 会创建一个系统服务，用下面命令来启动 Docker：
$ sudo systemctl start docker
设置开机启动：
$ sudo systemctl enable docker
```nano
 为了永久性保留更改，您可以修改 /etc/docker/daemon.json 文件并添加上 registry-mirrors 键值。

{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
###Docker 中科大的镜像。中国官方镜像加速可通过 registry.docker-cn.com 访问。该镜像库只包含流行的公有镜像。私有镜像仍需要从美国镜像库中拉取。 
sudo systemctl restart docker
docker info
```

## npm切换为国内淘宝镜像



```shell
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install xxxx
#使用cnpm安装
```

### 安装lantern

```shell
yay -S lantern-bin

version :lantern-bin-5.3.8-1
```



https://github.com/getlantern/lantern
设置git的hosts没代理可怜

> 哈哈，编译了半天没成功，之后直接库里有啊
>
> ！！！
>
> yay -S lantern-bin
>
> version :lantern-bin-5.3.8-1

```
### Prerequisites

- [Custom fork of Go](https://github.com/getlantern/go/tree/lantern) is currently required. We'll eventually switch to Go 1.7 which supports what we need due to [this](https://github.com/golang/go/issues/13998). 
- An OSX or Linux host. Building on Windows is only partially supported with the help of [Cygwin](https://www.cygwin.com/).
- [Git](https://git-scm.com/downloads) - `brew install git`, `apt-get install git`, etc
- [GNU Make](https://www.gnu.org/software/make/)
- [Nodejs & NPM](https://nodejs.org/en/download/package-manager/)
- GNU C Library (linux only) - `apt-get install libc6-dev-i386`, etc
- [Gulp](http://gulpjs.com/) - `npm i gulp-cli -g`

```
```bash
sudo pacman -S go
yay -S npm
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
sudo npm i gulp-cli -g
yay -Sgnome-shell-extension-appindicator-git
git clone https://github.com/getlantern/lantern.git
cd lantern
make lantern
./lanternremove

#During development, you'll likely want to do a clean build like this:
make clean-desktop lantern && ./lantern
```

```bash
sudo rstudio-bin  --handless --no-sandbox
```

##  安全

[ClamAV](https://link.jianshu.com?t=https://www.clamav.net/)- Clam 防病毒

[GuFW](https://link.jianshu.com?t=http://gufw.org/)- Linux 世界中最简单的防火墙之一

[Bleach bit](https://link.jianshu.com?t=https://www.bleachbit.org/)- BleachBit 快速释放磁盘空间并不知疲倦地守卫你的隐私。释放缓存，删除 cookie，清除互联网浏览历史，清理临时文件，删除日志，以及更多功能...

## 美化

## konsole终端半透明

konsole终端设置背景图片或者设置背景透明设置如下：

打开终端--设置---管理配置方案---编辑配置方案----外观-----编辑------- 背景透明度/壁纸   分别设置

