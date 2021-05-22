## ZSH安装配置记录

### 安装zsh终端
  - 配置环境变量，让终端配置文件生成在自己定义的位置，方便将文件放在gtihub上托管，防止丢失
### 切换为默认终端
   在终端运行如下命令可以切换终端默认使用zsh
   ```
   chsh -s /bin/zsh
   ```
### 安装Zim配置框架
  Zim has many module!Enable as many or as few as you'd like
  * config the `$ZDOTDIR` variable can generate default configuration in custom dictionary
    
  add follow code in `/etc/zsh/zshenv`

  ```
  export ZDOTDIR=$HOME/.config/zsh
  ```

#### Installation 
    
   * with curl

    curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
   
   * With wget
    
    wget -nv -O - https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh

### 安装常用模块
  安装模块只需要将下面代码块添加到.zimrc文件中

  * powerlevel10k
    
  ```
    zmodule romkatv/powerlevel10k
  ```
  * completions
  ```
    zmodule zsh-users/zsh-completions
  ```
  * autosuggestion
  ```
    zmodule zsh-users/zsh-autosuggestions
  ```
  * history-search
  ```
    zmodule zsh-users/zsh-history-substring-search
  ```
  * syntax-higthlight
  ```
    zmodule theniceboy/fast-syntax-highlighting
  ```
  
  * Install the module defined in .zimrc and build the initialization script
  ```
    source $ZDOTDIR/.zim/zimfw.zsh install   #zimfw install
  ```

