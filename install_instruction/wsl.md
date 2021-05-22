# wsl（ubuntu20.04）安装笔记

* 安装wsl主要是因为个人是做java的后端开发，很多时候需要与linux后台进行交互
* 也是我个人由于受到lunix系统的吸引，对一些高效脚本技术感兴趣

对于怎样在windows高级功能中开启linux子系统，然后在商店中安装对应的linux 系统，本人不多做赘述，
可参考以下几篇文章：

[安装wsl](https://www.jianshu.com/p/3e627ff45ccb)

## 安装我自己常用的软件
* nvim (默认仓库中没有,只有vim)需要自行到官网下载压缩包进行解压、安装
  
  - 安装nvim 之后需要安装我自己需要的插件[nvim-plug](https://github.com/ancion/nvim) 
  - 安装nodejs并安装  `npm install neovim -g`
  - 安装ruby并安装  `gem install neovim`
  - 安装 `python2 python-pip python-dev python-pynvim`
  - 安装 `python3 python3-pip python3-dev python3-pynvim`
  - 安装ranger
  - 安装fzf文件查找工具
* zsh终端模拟器
  - oh-my-zsh主题(安装oh-my-zsh之后已经安装很多的主题，可以根据需要自行切换)
   
   Via curl:
    ```
    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    ```
    Via wget:
    ```
   $ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
    ```
    * 没有`wget` 或者`curl` 工具自行安装即可
    * 我自己使用的主题是powerlevel10k(显示样式很丰富)
  - 安装两个常用的插件
    * zsh-autosuggestions
    ```
    git clone https://github.com/zsh-users/zsh-autosuggestions 
    ```
    * zsh-syntax-highlighting
    ```
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
    ```
* 默认已经安装了tmux
  - 需要自己去安装选定的主题，配置自己的 `.tmux.conf`
  - 安装需要的主题

## 将fzf与ranger、tmux 、nvim 整合


## 将windows的浏览器连接到linux中
  
  1、在linux系统中的`/usr/bin/`目录中软连接浏览器地址
  
  ```
    sudo ln /mnt/c/Program\ Files\ \(x86\)/Microsoft/Edge/Application/msedge.exe chromium -s
  ```
  2、还可以将浏览器设置为默认浏览器

 - 在`/etc/alternatives` 下面创建软连接连接到之前的软连接
    
    ```
    sudo ln /usr/bin/chromium gnome-www-browser -s
    ```
 - 删除原来默认浏览器
    ```
    sudo rm x-www-browser
    ```
 - 将chromium作为默认浏览器
    ```
    sudo ln /usr/bin/chromium x-www-browser -s 
    ```
    
