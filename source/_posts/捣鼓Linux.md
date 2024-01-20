---
title: 捣鼓Linux
---



## 捣鼓Linux

--也许会整理成比较系统的教程:)

---

### 在x86平台上安装Debian(HP-T520, )

#### 1. 启动盘制作

* 准备4GB以上的U盘
* 下载启动盘工具[rufus](https://github.com/pbatard/rufus/releases/tag/v4.1)

* 从镜像站下载debian镜像文件，注意选择x86平台，并选择喜欢的图形界面(gnome,standard...) standard不好入门...
* 选项基本默认，注意不要选择了系统盘(会导致系统盘格式化)

#### 2. 写入Linux系统

* 平台配置：
* 需要提前插入网线以便自动完成DHCP配置
* 分区如果选择了/home,/var,/tmp，需要手动调整，否则可能因为分区太小导致installation步骤失败；也可以不进行分区(recommanded)
* 为啥装不上boot?(分区时选用LVM就可以装上了，应该是因为没有为```/boot```分配逻辑卷，建议为```/boot```分配2GB的逻辑容量)

#### 3. 系统配置

##### 3.1 换源(Debian)

* 尽量先下载```ca-certificates```：

  ```sh
  sudo apt install ca-certificates
  ```

* 注意Debian各个发行版的名称：

  > Debian 12 bookworm
  >
  > Debian 11 bullseye
  >
  > Debian 10 buster

  不同的发行版的源也不同，可以根据后缀判断

* 备份/etc/apt/sources.list

* 修改/etc/apt/sources.list：

  阿里云：（阿里云很慢不知道为啥）

  清华源（Debian 12）：

  ```sh
  # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
  deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
  
  deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
  
  deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
  
  # deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
  # # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
  
  deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
  # deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
  ```

  

  中科大源（Debian 11）：

  ```sh
  deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
  deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
  deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
  deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
  deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
  deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
  deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
  deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
  ```

  华为云（Debian 11）：（华为云没有wget,tldr等包...）

  ```sh
  deb https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
  deb-src https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
  deb https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
  deb-src https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
  deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
  deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
  deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
  deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
  ```

##### 3.2 挂载U盘

* 查看磁盘分区信息:

  ```sh
  sudo fdisk -l
  ```

* 显示块设备（block device）的文件系统或卷标等信息：

  ```sh
  sudo blkid
  ```

* 显示挂载信息：

  ```sh
  bf
  ```

* 挂载设备：

  ```sh
  mkdir /mnt/xxx
  mount -t vfat /dev/sdb1 /mnt/xxx	# vfat为磁盘格式FAT32,具体格式可用blkid命令查看
  umount /mnt/xxx
  ```

##### 3.3 校园网认证

###### 3.3.1 尝试用curl进行认证(失败)

```sh
curl /cgi-bin/srun_portal?callback=jQuery112402779513334089566_1690089329252&action=login&username=200111133&password=%7BMD5%7D8321a8d374c6912ec77504cf49357ff1&ac_id=1&ip=10.250.251.198&chksum=88094686a51d3356aef095beed21f962a4e309a1&info=%7BSRBX1%7Dy2DLztn%2FKAYKcbZX5jBb4AJIE9CFnJPmw3%2B5hvbVXa8h8U2HHPK6cnq8%2B5DxSNcTMCXPLcsZjv8QAYilZ3Ik9Z7W9z3rPXoIPwa9NOuQzyWVZXkZ%2FxX3%2BBKHSzOM8ryYQ5k7%2B590er91rw9m&n=200&type=1&os=Windows+10&name=Windows&double_stack=0&_=1690089329255
```

###### 3.3.2 安装浏览器

* 在standard版本下失败(没有图形界面，装不来)

  * 解压tar.gz文件：

    ```sh
    tar -xvzf xxx.tar.gz
    ```

  * 解压tar.bz2文件：

    ```sh
    tar -xvjf xxx.tar.bz2
    ```

    > 每个选项的含义：
    >
    > - `-x`: 解压缩文件。
    > - `-v`: 显示详细解压缩过程。
    > - `-j`: 使用 `bzip2` 解压缩算法。
    > - `-z`: 使用 `gzip` 解压缩算法。
    > - `-f`: 指定要解压的文件。

* 在gnome版本下:

  * 内置firefox浏览器，可直接进行校园网认证

##### 3.4 给用户添加sudo权限

* 修改/etc/sudoers:

  添加```xxx ALL=(ALL:ALL) ALL```

##### 3.5 修改密码

* ```sh
  sudo passwd root
  sudo passwd username
  ```

##### 3.6 系统信息查询

* ```sh
  uname -a
  ```

#### 4. 代理配置

（1）基于[ikuuu](ikuuu.art)提供的免费代理

* 在用户目录下创建 clash 文件夹

  ```sh
  mkdir clash
  ```

* 下载适合的 Clash 二进制文件并解压重命名为 `clash`

  https://github.com/Dreamacro/clash/releases

* 在终端 `cd` 到 Clash 二进制文件所在的目录，执行 `wget -O config.yaml "https://api.sub-200.club/link/kcYFCcXJR8OTCo3H?clash=3"` 下载 Clash 配置文件

* 执行 `./clash -d .`  即可按当前目录下的配置文件启动 Clash，同时启动 HTTP 代理和 Socks5 代理。

* 配置系统代理：

  ```sh
  sudo nano /etc/environment
  # 在文件中添加：
  export https_proxy=http://127.0.0.1:7890
  export http_proxy=http://127.0.0.1:7890
  export all_proxy=socks5://127.0.0.1:7891
  ```

* ~~这一步似乎无效：打开系统设置，选择网络，选择手动，填写 HTTP 和 HTTPS 代理为 `127.0.0.1:7890`，填写 Socks 主机为 `127.0.0.1:7891`，即可启用系统代理（clash成功启动后会显示对应的ip地址和端口）~~

（2）使用go-graft

#### 5. git配置

* 下载git

* 查看配置

  ```sh
  git config -l
  ```

* 添加配置

  ```sh
  git config --global user.name "xxx"
  git config --global user.email xxx
  ```

* ssh-key配置

  ```sh
  ssh-keygen -t rsa -C "your_email@example.com"
  ssh -T git@ssh.github.com
  ```

* wsl使用ssh连接github.com出现问题：

  https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794

#### 6. zsh配置与美化

* 下载zsh：

  ```sh
  sudo apt install zsh
  ```

* 下载ohmyzsh：

  浏览器搜索ohmyzsh，进入官网选择curl或wget方式下载

* 选择nerd font字体：

  https://github.com/ryanoasis/nerd-fonts

  * 使用```wget```或```curl -OL```下载对应字体的压缩包

  * 创建文件夹：

    ```sh
    sudo mkdir /usr/share/fonts/NewFont
    ```

  * 将字体压缩包解压到/usr/share/NewFont

  * （修改系统字体）

* 下载zsh主题powerlevel10k：（[步骤](https://github.com/romkatv/powerlevel10k#oh-my-zsh)）

  * ```sh
    git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

  * Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`.

  * 输入```zsh```启动zsh

  * 进入主题设置界面

* 将zsh设为默认shell

  ```sh
  sudo chsh -s /bin/zsh
  ```

* 插件安装：

  * 将```.zshrc```中的```plugins=(git)```修改为：

    ```sh
    plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
    ```

  * 将自动补全插件```zsh-autosuggestions```和语法高光插件```zsh-syntax-highlighting```克隆到```~/.oh-my-zsh/plugins/```下：

    ```sh
    git clone https://gitee.com/zsh-users/zsh-autosuggestions.git
    git clone https://gitee.com/zsh-users/zsh-syntax-highlighting.git
    ```

  * ```sh
    source .zshrc
    ```

  * 配置vim式编辑方式：

    ```sh
    git clone https://github.com/jeffreytse/zsh-vi-mode \
      $ZSH_CUSTOM/plugins/zsh-vi-mode
    ```

    修改```.zshrc```

#### 7. 修改系统时间

* 查看系统时间：

  ```sh
  date
  ```

  查看时间配置：

  ```sh
  timedatectl
  ```

* 获取时区TZ值：

  ```sh
  tzselect    # 通过序号选择大洲、国家、城市
  ```

* 修改系统时间Local time：

  ```sh
  sudo rm -f /etc/localtime    # 如果Localtime与其他时间不一致，直接删除/etc/localtime就行
  sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
  ```

* 修改系统时区：

  ```sh
  sudo timedatectl set-timezone 'Asia/Shanghai'
  ```

#### 8. ssh配置

* 查看ip：

  ```sh
  ip addr show
  ```

* 安装openssh-server：

  ```sh
  sudo apt install openssh-server
  ```

* 启动sshd服务：

  ```sh
  systemctl start sshd
  systemctl enable sshd    # 设置sshd服务自动启动
  ```

* 修改ssh服务端口：

  ```sh
  sudo nano /etc/ssh/sshd_config
  # 将Port 22前的注释'#'去掉，可以修改为Port 1666
  systemctl restart sshd    # 修改端口后重启sshd
  ```

* 连接ssh：

  ```sh
  ssh -p 1666 shadowyee@hostname    # hostname可以为内网ip
  ```

* 如果某主机连接ssh时发生如下报错：

  ```sh
  WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
  ```

  则需要根据报错中提供的```.ssh/known_hosts```地址，在该主机上删除远程服务器ip地址对应的内容条目或将其修改为正确的host key即可

#### 9. Vim

* command模式下：

  ```sh
  :help :w      	# command模式下的w
  :help w       	# normal模式下的w
  
  # Vim有许多标签页tab，每个标签页有一些窗口window，每个窗口对应一个缓冲区buffer，每个缓冲区可以有多个窗口
  :sp				# 将同一文本分为两个窗口
  :q 				# 关闭一个窗口或标签
  :qa 			# 关闭所有窗口和标签
  
  ```

* normal模式下：

  ```sh
  w				# 向前移动一个单词 word
  b				# 向后移动一个单词 back
  e				# 跳到单词末尾 end of word 
  0				# 移动到行的开头 regex
  $				# 移动到行的末尾 regex
  ^				# 移动到行的第一个非空字符 regex
  ctrl-c			# 向上滚动 up
  ctrl-d			# 向下滚动 down
  G				# 移动到文本的底部
  gg				# 移动到文本的顶部
  L				# 将光标移动到屏幕上显示的底部 lowest
  M				# 将光标移动到屏幕上显示的中间 middle
  H				# 将光标移动到屏幕上显示的顶部 highest
  f-?				# 找到该行上光标后的第一个？字母 find
  F-?				# 找到该行上光标前的第一个？字母 find back
  t-?				# 找到该行上光标后的第一个？字母的前一个字母 to
  T-?				# 找到该行上光标前的第一个？字母的后一个字母 to back
  o				# 向下新建一行并进入insert模式
  O				# 向上新建一行并进入insert模式
  d				# 删除，需要与移动命令结合使用 delete
  dd				# 删除指定行
  u				# 撤销 undo
  ctrl-r			# 重做，与撤销相反 redo
  c				# 改变，在d的基础上进入insert模式 change
  c				# 删除指定行并进入insert模式
  x				# 删除一个字符
  r-?				# 将当前字符替换为？	replace
  y				# 复制，需要与移动命令结合使用 yank
  yy				# 复制当前行
  p				# 粘贴 paste
  ~				# 大小写转换
  num-cmd			# 计数符，执行cmd操作num次，4k则为向上移动4行，d4w删除4个单词
  i				# 修饰符，括号内，di[删除中括号内的所有内容 inside
  a				# 修饰符，括号及括号内，da[删除中括号内的所有内容以及中括号 around
  %				# 光标需要在括号上，实现两个括号之间的来回跳转
  /-*				# 在全文搜索*，按n查找下一个
  .				# 重复前一个编辑命令
  ```

* visual模式：

  ```sh
  # 如果要复制文字块，则需要进入visual模式
  移动操作：h,j,k,l; w,e,b...
  y				# 复制选中的文本块
  insert模式中的一些操作也可以用，比如删除，大小写转换等
  # 普通visual模式，按v进入
  # visual line模式，按V进入，每次选中一行
  # visual block模式，按ctrl-v进入，每次选中一矩形块文本
  ```

##### 9.1 .vimrc配置

* 一些好用的vim插件：

  

##### 9.2 Vim-adventure部署

* 下载并安装nodejs：

  ```sh
  sudo apt install nodejs
  node -v    # 查看nodejs版本
  ```

* 下载并安装npm：

  ```sh
  sudo apt install npm
  npm -v     # 查看npm版本
  ```

* 项目部署：

  ```sh
  git clone https://github.com/AkshayGupta8/Vim-adventure.git
  ```

* 将npm回退至6.x版本：

  否则下一步安装依赖模块时会报错导致无法安装

  ```sh
  npm install -g npm@6
  ```

* 下载依赖：

  在```~/Vim-adventure/vim-adventures```路径下：

  ```sh
  npm install
  ```

  启动项目：

  ```sh
  sudo node bin/www.js
  ```

  注意，需要修改```routes/index.js```中的```appDir```变量（添加一个绝对路径，该路径下包含```saved```文件夹），或者修改环境变量```APP_DIR```（修改环境变量目前不成功，似乎需要修改sudo模式下的环境变量），否则游戏进度无法正常保存



#### 10. systemd编程

* 新建service：

  ```sh
  sudo vim /etc/systemd/system/new.service    # systemd还存在于/usr/lib/systemd，/lib/systemd
  ```

  在```new.service```中写入：

  > [Unit]
  >
  > Description=vim-adventure
  >
  > [Service]
  >
  > ExecStart=node /home/shadowyee/Vim-adventures/vim-adventure/bin/www.js
  >
  > [Install]
  >
  > WantedBy=multi-user.target

  查询```.service```写法：

  ```sh
  man 5 systemd.service
  ```

  常见命令：

  ```sh
  systemctl status vim-adventure   # 查询状态
  systemctl start vim-adventure    # 启动
  systemctl enable vim-adventure   # 开机启动
  systemctl disable vim-adventure  # 取消开机启动
  
  systemctl list-units             # 查询已有单元
  systemctl list-unit-files        # 查询已有单元文件
  systemctl cat vim-adventure      # 查看单元文件
  
  journalctl  					 # 查询系统日志
  ```

  

#### 11. Bing自动搜索脚本

目的：获取Rewards

* 

#### 12. 校园网自动登录

#### 13. BitTorrent应用

* qbittorrent-nox:

  ```sh
  sudo apt install qbittorrent-nox
  qbittorrent-nox -d --webui-port=8081
  # 默认账户：admin
  # 默认密码：adminadmin
  ```

* transmission:

#### 14. Python

* 虚拟环境```virtualenv```的使用：

  ```sh
  sudo apt install virtualenv
  virtualenv xxx		# 在当前目录下创建虚拟环境xxx
  # Win用python -m virtualenv xxx来创建
  
  # 创建指定版本的虚拟环境：
  virtualenv -p /usr/bin/python2.7 名称
  virtualenv -p /usr/bin/python3.5 名称
  
  source bin/activate # 启动虚拟环境（不要在bin目录下启动虚拟环境）
  deactivate          # 退出虚拟环境
  rm -rf xxx          # 删除虚拟环境
  ```

* ```requirements.txt```:

  ```sh
  pip3 freeze > requirements.txt   # 导出当前虚拟环境的依赖requirements.txt
  pip3 install -r requirements.txt # 安装依赖
  ```



#### 15. Go-graft

* [go-graft](https://github.com/mzz2017/gg)

* Q: When I use `oh-my-zsh`, it reports `git: 'gui' is not a git command. See 'git --help'.`, ho can I fix it?

  A: It is a problem of `oh-my-zsh`, it added an alias from gg to `git gui`. **Append** following content to `~/.zshrc`:

  ```sh
  unalias gg
  ```

#### 16. Hexo

* [Hexo Blog](https://hexo.io/)

* 下载nodejs和npm

  ```sh
  sudo apt install nodejs npm
  ```

* 安装hexo package

  ```sh
  npm install hexo
  ```

* 初始化hexo

  ```sh
  hexo init myblog-hexo
  ```

* 使用systemd[A systemD unit file for Hexo](https://chrisbergeron.com/2017/10/07/hexo_systemd_unit_file/)

  ```sh
  [Service]
  WorkingDirectory=/home/[yourdirectory]/blog
  ExecStart=/bin/hexo server -p80
  Restart=always
  StandardOutput=syslog
  StandardError=syslog
  SyslogIdentifier=hexo
  User=root
  Group=root
  Environment=NODE_ENV=production
  
  [Install]
  WantedBy=multi-user.target
  ```

#### 17. Cron



---

### WSL

#### 1. 安装

* [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

* ```sh
  wsl --list --online
  wsl --install -d xxx
  ```

#### 2.配置代理

* [为 WSL2 一键设置代理](https://zhuanlan.zhihu.com/p/153124468)

  ```sh
  export ALL_PROXY="127.0.0.1:7890"
  ```

#### 3. linux清空命令行快捷键

>  在 Linux 中，可以使用以下快捷键清空命令行：
>
>  1. Ctrl + L：清空当前命令行窗口。
>  2. Ctrl + U：清空当前命令行中光标所在位置及之前的所有内容。
>  3. Ctrl + K：清空当前命令行中光标所在位置及之后的所有内容。
>  4. Ctrl + W：删除当前命令行中光标所在位置之前的一个单词。
>  5. Ctrl + C：终止当前正在运行的命令。

### Linux一些好用的包

* ripgrep
* Crtl+R反向搜索命令，建议使用fzf

* tree, broot, nnn



---

### The Missing Semester

--learn from the MIT 2020: The Missing Semester of Your CS Education

建议根据官方文档来完善

---

#### 1. Data Wrangling

* 在服务器上运行命令：

  ```sh
  ssh servername 'journalctl | grep ssh | grep "Disconnected from"' | less # 单引号内命令在服务器上运行，less在本地运行
  ```

* sed

  一种流编辑器，是运行在流上的完整编程语言

  ```sh
  cat ssh.log | sed 's/.*Disconnected from //'	# s代表替换，第一个'/'后是被替换的内容；第二个'/'是替换后的内容，此处为空
  echo 'abc' | sed 's/[ab]//g'	# sed默认只匹配一次，添加参数g可以匹配多次
  echo 'abcaba' | sed -E 's/(ab)*//g'		# sed是很古老的编辑器，只支持旧的正则表达式，所以需要加上参数-E，否则这里的括号需要加上转义符\
  cat ssh.log | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'		# 任意一个()都是一个捕获组，可以在替换时引用它们，使用\2引用第二个捕获组
  ```

* awk

  一中列数据流处理器

* Regular Expression

  一种强大的匹配文本的方法，一般默认按行匹配而不跨行匹配

  ```sh
  .     		# 任意单个字符
  *  			# 跟在某个字符后面，表示0个或多个该字符，是贪婪的，会尽可能匹配多的字符
  + 			# 跟在某个字符后面，表示1个或多个该字符，是贪婪的，会尽可能匹配多的字符
  []			# 匹配括号内任意一个字符
  ()			# 匹配括号内的字符串
  0-9			# 从0到9
  ^ 			# 匹配行的开头
  $			# 匹配行的结尾
  | 			# 或
  ?			# 使贪婪匹配变为首次匹配
  ```

* wc

  word count计数

  ```sh
  wc -l		# 计算行数
  ```

* sort

  排序，默认按升序排列

  ```sh
  sort -nk1,1	# -n表示按数字排序，-k表示选择输入中以空格为分隔的列来执行排序，1,1表示从第一列开始到第一列结束，即按第一列排序
  ```

* uniq

  从有序列表中去掉重复的行

  ```sh
  uniq -c 	# 去掉重复的行并计算重复的次数
  ```

* head

  获取前几行数据

  ```sh
  head -n10	# 获取前10行数据
  ```

* tail

  获取末尾几行数据

  ```sh
  tail -n10	# 获取末尾10行数据
  ```

* paste

  将很多行合并为一行

  ```sh
  paste -sd,	# 将不同的行以,为分隔符并合并为一行
  ```

* bc

  计算器

  ```sh
  echo "1 + 2" | bc -l
  ```

* R

  专门用于统计分析的工具，也是一种编程语言

* gnuplot

  从标准输出中获取内容的绘图工具

* xargs

  将输入列表转换为参数

#### 2. Job Control

* ```sh
  man signal 			# 查看信号列表，通过kill命令发送信号
  ```

* ```sh
  ctrl-z				# 暂停进程
  xxx &				# 使进程在后台运行
  jobs				# 查看当前工作（包括每个工作的状态）
  bg %num				# 继续某个暂停的工作，num在使用jobs命令的第一列会显示
  fg %num				# 继续某个暂停的工作，并将其恢复到前台并重新连接到标准输出
  kill %num			# 直接停止某个工作
  kill -STOP %num		# 向某个工作发送暂停信号
  kill -HUP %num		# 向某个工作发送挂起信号
  kill -KILL %num		# 向某个工作发送停止信号
  nohup cmd			# 会将执行的命令cmd封装起来，忽略任何挂起信号，并使其继续执行，即使关闭终端窗口也会继续执行
  ```

#### 3. Terminal Multiplexes

* sessions

  * windows
    * panes

* tmux

  运行tmux时，启动一个会话，相当于在原本的shell中启动tmux进程，然后tmux又打开一个新的shell，tmux进程和原本的shell进程是分开的；

  在远程连接时，关闭连接，发送挂起信号，已启动的tmux不会关闭

  ```sh
  # session - 每个会话都是一个独立的工作区，其中包含一个或多个窗口
  tmux 开始一个新的会话
  tmux new -s NAME 以指定名称开始一个新的会话
  tmux ls 列出当前所有会话
  在 tmux 中输入 ctrl-b d ，将当前会话分离
  tmux a 重新连接最后一个会话。您也可以通过 -t 来指定具体的会话
  # windows - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分
  ctrl-b c 创建一个新的窗口，使用ctrl-d关闭 create
  ctrl-b N 跳转到第 N 个窗口，注意每个窗口都是有编号的
  ctrl-b p 切换到前一个窗口
  ctrl-b n 切换到下一个窗口
  ctrl-b , 重命名当前窗口
  ctrl-b w 列出当前所有窗口
  # panes - 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell
  ctrl-b " 水平分割
  ctrl-b % 垂直分割
  ctrl-b <方向> 切换到指定方向的面板，<方向> 指的是键盘上的方向键
  ctrl-b z 切换当前面板的缩放 zoom
  ctrl-b [ 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
  ctrl-b <空格> 在不同的面板排布间切换
  ```

#### 4. Alias

* 默认情况下 shell 并不会保存别名。为了让别名持续生效，需要将配置放进 shell 的启动文件里，像是`.bashrc` 或 `.zshrc`

  ```sh
  # 创建常用命令的缩写
  alias ll="ls -lh"
  
  # 能够少输入很多
  alias gs="git status"
  alias gc="git commit"
  alias v="vim"
  
  # 手误打错命令也没关系
  alias sl=ls
  
  # 重新定义一些命令行的默认行为
  alias mv="mv -i"           # -i prompts before overwrite
  alias mkdir="mkdir -p"     # -p make parent dirs as needed
  alias df="df -h"           # -h prints human readable format
  
  # 别名可以组合使用
  alias la="ls -A"
  alias lla="la -l"
  
  # 在忽略某个别名
  \ls
  # 或者禁用别名
  unalias la
  
  # 获取别名的定义
  alias ll
  # 会打印 ll='ls -lh'
  ```

#### 5. Dotfiles

* ```sh
  bash - ~/.bashrc, ~/.bash_profile
  git - ~/.gitconfig
  vim - ~/.vimrc 和 ~/.vim 目录
  ssh - ~/.ssh/config
  tmux - ~/.tmux.conf
  ```

* dotfiles的相关资源：

  github上的dotfiles仓库：https://github.com/mathiasbynens/dotfiles

  https://dotfiles.github.io/

* 可移植性：

  ```sh
  if [[ "$(uname)" == "Linux" ]]; then {do_something}; fi
  
  # 使用和 shell 相关的配置时先检查当前 shell 类型
  if [[ "$SHELL" == "zsh" ]]; then {do_something}; fi
  
  # 您也可以针对特定的设备进行配置
  if [[ "$(hostname)" == "myServer" ]]; then {do_something}; fi
  ```

* stow的使用：

  https://brandon.invergo.net/news/2012-05-26-using-gnu-stow-to-manage-your-dotfiles.html

#### 6. Remote Machine

