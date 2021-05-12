1. 查看GC 情况

----------------------------------------------------------------------------
### 场景

> 一个高QPS的 web 服务, 单机的QPS 一直维持再1.5k
> 配置的堆区8G, Yong区是4G,垃圾回收使用的策略是parNew+CMS



```shell
jstat -gcutil ${pid} 1000  # 每隔1s 输出一次GC统计信息

```

加入JVM启动参数，查看 gclog 

``` shell

  -XX:+PrintGCDateStamps  # GC发生时的时间戳

  -XX:+PrintTenuringDistribution  # 打印GC时的分代信息

  -XX:+PrintGCApplicationStoppedTime # 打印GC停顿时长

  -XX:+PrintGCApplicationConcurrentTime # 打印 GC间隔的服务运行时长

  -XX:+PrintGCDetails # 打印GC详情信息

  -Xloggc:../gclogs/gc.log.date # 指定 gc log 的路径

```

JVM 启动参数

```

-Xmx500m                  # 最大堆空间占用
-Xms100m                  # 最小堆空间占用
-Xmn300m                  # 年轻代空间占用
-Xss1m                    # 设置每个线程的堆栈大小
-----------------------------------------------------------------
堆空间大小 = 年轻代 + 老年代 + 持久代


-XX:NewRatio=4             # 设置年轻代(Eden+Survivor)与老年代的比例(除去老年代)为 1：4 年轻代占用1/5

-XX:SurvivorRatio=4        # 设置年轻代中Eden 与 Survivor 区的大小比值。两个Survivor 与 Eden 区 2:4 
                           # 一个survivor 区占用1/6

-XX:MaxTenuringThreshold=0 # 设置幸存区的最大年龄，设置为零的话则年轻代对象不经过survivor 直接进入
                           # 老年代

-XX:MaxPermSize64m         # 设置持久代最大值大小
-XX:PermSize64m            # 设置持久代最大值大小

--推荐设置，年轻代的大小应该为堆区的 3/8 根据情况不同可以做适当的调整
--持久代一般设置为64m

```
