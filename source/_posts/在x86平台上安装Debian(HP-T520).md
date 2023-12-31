---
title: 捣鼓Linux
---
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

#### 4. 代理配置

基于[ikuuu](ikuuu.art)提供的免费代理

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

#### 9. Vim-adventures部署

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



#### 15.Go-graft

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
  hexo init hexo
  ```

* 