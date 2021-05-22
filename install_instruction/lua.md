## Lua install instruction 


### install lua support 

1. download the intepreter of lua 

> Linux 系统安装说明

- [官方下载安装地址](https://www.lua.org/) 

```shell 
    curl -R -O https://www.lua.org/ftp/lua-5.3.0.tar.gz

    tar -xvf lua-5.3.0.tar.gz

    cd lua-5.3.0

    make linux test

    make install 

```
- 默认安装的位置是在 `/usr/local/bin/lua` 自定义安装位置根据源码中 MAKEFILE 中要求添加前缀等


2. install lua package manager `luarocks`

- [官网地址](https://luarocks.org/) 

```shell

    wget https://luarocks.org/releases/luarocks-3.7.0.tar.gz

    tar zxpf luarocks-3.7.0.tar.gz 

    cd luarocks-3.7.0

    # 先将文件放在自己想要安装的位置，并将所属者改为当前用户

    ./configure  && make sudo make install  

```
- 默认的安装位置应该是在 `/usr/local/bin/luarocks 3.7.0` 自定义安装编译期加上相关的前缀

3. 为neovim 添加 lua 的 `language-server` support 

```shell 
    
    luarocks install --server=http://luarocks.org/dev lua-lsp 

```
This will install lua-lsp command. Language clients can then commmunicate with this process
using stdio as a transport.

- if you use `coc.nvim`, add following to you `coc-settings.json`

```json
"languageserver": {  
    "lua": {    
        "command": "lua-lsp",    
        "filetypes": ["lua"]   
    } 
}
```
-------------------------------------------------------------------------------------------
- if you don't use `coc.nvim`,add following to your `init.vim`

```vim
    let g:LanguageClient_serverCommands = {
        \ 'lua': ['lua-lsp'],
        \ }
    let g:LanguageClient_autoStart = 1
    
```
