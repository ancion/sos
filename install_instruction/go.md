# Go Development Environment Build

## Download golang 

[go语言中文社区](https://studygolang.com/dl) 

下载对应的平台的安装包即可

## Linux 下将下载好的安装包

+ 解压安装
```
    tar -C /usr/local -xzf go1.15.5.linux-amd64.tar.gz
```
+ 配置环境变量
```
    export PATH=$PATH:/usr/local/go/bin
```

+ 查看安装是否成功，查看成功后的各种golang 环境配置信息
```
    go env
```

## 在go中使用其他第三方的软件包，

+ 可能受限于外网速度需要对软件包的下载位置进行代理
```
    go env -w GO111MODULE=on
    go env -w GOPROXY="https://goproxy.io`irect"

```
+ 调试时使用
```
    go env -w GO111MODULE=off
```


### 其他版本安装信息
[其他平台版本安装](https://golang.org/doc/install) 

[golang学习官网](https://golang.org/doc/) 


