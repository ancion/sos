# jvm 内存分析

## Linux 系统

- top 命令可以查看具体的进程名称与资源的占用情况（物理内存占用）
- Jps 可以使用查看java进程

## JVM 层面的内存分析过程，
- [1.0] 使用java层面的工具进行具体的内存区域的定位
（堆内内存，Code区域或者使用unsafe.allocateMemory和DirectByteBuffer申请的堆外内存）

* 在项目启动的时候添加启动参数
 ```
    --XX:NativeMemoryTracking=detail
 ```
* 随后在终端中使用如下命令查看具体的各区域的内存使用情况
```
    jcmd pid VM.native_memory detail 
```
  这个命令可以查看具体各区域申请与占用的内存的情况与内存区域的映射,可以比较在此处commited 
  的总内存与物理内存占用是否一致.

  jcmd显示的内存包含的具体内容是（堆内内存，Code区域内存，unsafe.allocateMemory和DirectByteBuffer申请的堆外内存）
  但是并不包含其他的 Native Code (C代码)申请的堆外内存。

- java中常用的堆栈分析工具
```
    jstack pid #查看线程栈
    btrace     #追踪栈
    
```



##  OS层面内存分析

- 该命令可以查看系统中的内存空间，可以与jcmd查看的内存空间进行匹配，发现异常的地址空间分配。
```
    pmap -x pid | sort -k -n -r   # Linux 系统级别内存分析工具

```

- 如果出现系统层面的内存问题，可以使用`gperftools` 工具进行分析，
(`gperftools` 工具的分析原理就是通过动态链接的方式替换了操作系统默认的内存分配器（glibc)）

    * malloc 内存申请
    * mmap/brk 内存申请

```
    # Linux追踪向OS系统申请内存的请求，查看是否可疑内存申请,在系统启动的时候追踪(可能会发现异常)
    strace -f -e "brk,mmap,munmap" -p pid 
```
- GDB 调试
```
    gdb -pid pid  # 进入GDB

    # dump想要查看的内存 => startAddress and endAddress can be find /proc/pid/smaps
    dump memory mem.bin startAddress endAddress 

    # 查看内存
    string men.bin 
```

### 知识点普及

- mmap内存分配
mmap分配内存时按需向上取整到整数个页，所以存在着巨大的空间浪费，操作系统在分配内存的时候
是按照延迟分配的方式，通过mmap向系统申请内存地址的时候，系统仅仅返回内存地址并没有分配真实
的物理内存，只有到使用的时候，系统产生一个缺页中断，然后再分配实际的物理Page

- 文章来源
  * [Spring-boot-memory-leak-stace](https://tech.meituan.com/2019/01/03/spring-boot-native-memory-leak.html) 
- 参考文献
  * [GUN C library](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/6.0_release_notes/compiler)  
  * [Native Memory Tracking](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr007.html) 
  * [Spring Boot](https://github.com/spring-projects/spring-boot) 
  * [gperftools](https://github.com/gperftools/gperftools) 
  * [Btrace](https://github.com/btraceio/btrace) 
