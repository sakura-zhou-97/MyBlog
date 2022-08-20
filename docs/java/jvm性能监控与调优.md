

# 1.概述篇

<img src="picture/jvm性能监控与调优.assets/image-20210414105855331.png" alt="image-20210414105855331" style="zoom: 50%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414110032668.png" alt="image-20210414110032668" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414110154647.png" alt="image-20210414110154647" style="zoom:67%;" />![image-20210414110253927](picture/jvm性能监控与调优.assets/image-20210414110253927.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414110321582.png" alt="image-20210414110321582" style="zoom:67%;" />

![image-20210414110459058](picture/jvm性能监控与调优.assets/image-20210414110459058.png)

![image-20210414110730207](picture/jvm性能监控与调优.assets/image-20210414110730207.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414110825831.png" alt="image-20210414110825831" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414111104549.png" alt="image-20210414111104549" style="zoom:67%;" />

# 2.JVM监控和诊断工具-命令行工具

<img src="picture/jvm性能监控与调优.assets/image-20210414111350443.png" alt="image-20210414111350443" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414111933151.png" alt="image-20210414111933151" style="zoom:67%;" />

## jps

<img src="picture/jvm性能监控与调优.assets/image-20210414111410839.png" alt="image-20210414111410839" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414111623029.png" alt="image-20210414111623029" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414111735135.png" alt="image-20210414111735135" style="zoom:67%;" />

## jstat

<img src="picture/jvm性能监控与调优.assets/image-20210414112043215.png" alt="image-20210414112043215" style="zoom:67%;" />![image-20210414112427777](picture/jvm性能监控与调优.assets/image-20210414112427777.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414112440601.png" alt="image-20210414112440601" style="zoom:67%;" />

![image-20210414112133994](picture/jvm性能监控与调优.assets/image-20210414112133994.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414112211638.png" alt="image-20210414112211638" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112354195.png" alt="image-20210414112354195" style="zoom: 50%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112538662.png" alt="image-20210414112538662" style="zoom:50%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112658679.png" alt="image-20210414112658679" style="zoom:50%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112747614.png" alt="image-20210414112747614" style="zoom:50%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112853735.png" alt="image-20210414112853735" style="zoom: 67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414112924364.png" alt="image-20210414112924364" style="zoom: 67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414113557201.png" alt="image-20210414113557201" style="zoom:67%;" />

## jinfo

<img src="picture/jvm性能监控与调优.assets/image-20210414113638529.png" alt="image-20210414113638529" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414113854277.png" alt="image-20210414113854277" style="zoom: 67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414114002109.png" alt="image-20210414114002109" style="zoom:80%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414114330575.png" alt="image-20210414114330575" style="zoom:67%;" />

![image-20210414114523382](picture/jvm性能监控与调优.assets/image-20210414114523382.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414114637263.png" alt="image-20210414114637263" style="zoom: 80%;" />

## jmap

<img src="picture/jvm性能监控与调优.assets/image-20210414114908568.png" alt="image-20210414114908568" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414115426880.png" alt="image-20210414115426880" style="zoom:67%;" />

![image-20210414115047673](picture/jvm性能监控与调优.assets/image-20210414115047673.png)

![image-20210414115605578](picture/jvm性能监控与调优.assets/image-20210414115605578.png)

```
format=b也就是说生成的文件是标准文件
```

<img src="picture/jvm性能监控与调优.assets/image-20210414115938690.png" alt="image-20210414115938690" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210414120147287.png" alt="image-20210414120147287" style="zoom: 80%;" />

heap没有jinfo好用，jinfo可以近似实时的展示。jmap只能展示一瞬间

<img src="picture/jvm性能监控与调优.assets/image-20210414120509224.png" alt="image-20210414120509224" style="zoom:67%;" />

## jhat

<img src="picture/jvm性能监控与调优.assets/image-20210414120716349.png" alt="image-20210414120716349" style="zoom:80%;" />

![image-20210414120923614](picture/jvm性能监控与调优.assets/image-20210414120923614.png)

![image-20210414120953568](picture/jvm性能监控与调优.assets/image-20210414120953568.png)

## jstack

**打印线程快照**

<img src="picture/jvm性能监控与调优.assets/image-20210414121040509.png" alt="image-20210414121040509" style="zoom:80%;" />

![image-20210414121222712](picture/jvm性能监控与调优.assets/image-20210414121222712.png)

死锁

![image-20210414121258835](picture/jvm性能监控与调优.assets/image-20210414121258835.png)

死锁位置

![image-20210414121314617](picture/jvm性能监控与调优.assets/image-20210414121314617.png)

线程sleep的状态和在sycnronized状态

![image-20210414121411868](picture/jvm性能监控与调优.assets/image-20210414121411868.png)

## jcmd

<img src="picture/jvm性能监控与调优.assets/image-20210414121700640.png" alt="image-20210414121700640" style="zoom:80%;" />

![image-20210414121810915](picture/jvm性能监控与调优.assets/image-20210414121810915.png)

![image-20210414121851062](picture/jvm性能监控与调优.assets/image-20210414121851062.png)

![image-20210414121907217](picture/jvm性能监控与调优.assets/image-20210414121907217.png)

![image-20210414121941377](picture/jvm性能监控与调优.assets/image-20210414121941377.png)

# 3.JVM监控和诊断工具-GUI

![image-20210414122142372](picture/jvm性能监控与调优.assets/image-20210414122142372.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414122237732.png" alt="image-20210414122237732" style="zoom:80%;" />

## jconsole

![image-20210414125421707](picture/jvm性能监控与调优.assets/image-20210414125421707.png)

![image-20210414125703790](picture/jvm性能监控与调优.assets/image-20210414125703790.png)

![image-20210414125824049](picture/jvm性能监控与调优.assets/image-20210414125824049.png)

![image-20210414125844149](picture/jvm性能监控与调优.assets/image-20210414125844149.png)

![image-20210414130031403](picture/jvm性能监控与调优.assets/image-20210414130031403.png)

![image-20210414130053764](picture/jvm性能监控与调优.assets/image-20210414130053764.png)

![image-20210414130109224](picture/jvm性能监控与调优.assets/image-20210414130109224.png)

## visual vm

![image-20210414130304178](picture/jvm性能监控与调优.assets/image-20210414130304178.png)

![image-20210414131314147](picture/jvm性能监控与调优.assets/image-20210414131314147.png)

![image-20210414131344416](picture/jvm性能监控与调优.assets/image-20210414131344416.png)

![image-20210414131620296](picture/jvm性能监控与调优.assets/image-20210414131620296.png)

[**实时**]()

![image-20210414131633175](picture/jvm性能监控与调优.assets/image-20210414131633175.png)

[**线程的状态明确**]()

![image-20210414131730951](picture/jvm性能监控与调优.assets/image-20210414131730951.png)

![image-20210414131746894](picture/jvm性能监控与调优.assets/image-20210414131746894.png)

[**dump**]()

<img src="picture/jvm性能监控与调优.assets/image-20210414131854308.png" alt="image-20210414131854308" style="zoom: 50%;" />

**生成之后，另存为，保存**

![image-20210414132055795](picture/jvm性能监控与调优.assets/image-20210414132055795.png)

[**线程dump**]()

![image-20210414132323932](picture/jvm性能监控与调优.assets/image-20210414132323932.png)

[**抽样器**]()

![image-20210414132456520](picture/jvm性能监控与调优.assets/image-20210414132456520.png)

![image-20210414132532440](picture/jvm性能监控与调优.assets/image-20210414132532440.png)

## JProfiler

![image-20210414144417178](picture/jvm性能监控与调优.assets/image-20210414144417178.png)

<img src="picture/jvm性能监控与调优.assets/image-20210414144538151.png" alt="image-20210414144538151" style="zoom:67%;" />

![image-20210414144715193](picture/jvm性能监控与调优.assets/image-20210414144715193.png)

![image-20210414145938437](picture/jvm性能监控与调优.assets/image-20210414145938437.png)

![image-20210414145952582](picture/jvm性能监控与调优.assets/image-20210414145952582.png)

![image-20210414150813111](picture/jvm性能监控与调优.assets/image-20210414150813111.png)

![image-20210414151650058](picture/jvm性能监控与调优.assets/image-20210414151650058.png)

## Arthas

![image-20210414152411152](picture/jvm性能监控与调优.assets/image-20210414152411152.png)

![image-20210414152615830](picture/jvm性能监控与调优.assets/image-20210414152615830.png)

![image-20210414152645041](picture/jvm性能监控与调优.assets/image-20210414152645041.png)

![image-20210414154000711](picture/jvm性能监控与调优.assets/image-20210414154000711.png)

![image-20210414154501895](picture/jvm性能监控与调优.assets/image-20210414154501895.png)

![image-20210414154646302](picture/jvm性能监控与调优.assets/image-20210414154646302.png)

![image-20210414154735978](picture/jvm性能监控与调优.assets/image-20210414154735978.png)

![image-20210414154840031](picture/jvm性能监控与调优.assets/image-20210414154840031.png)

![image-20210414154913616](picture/jvm性能监控与调优.assets/image-20210414154913616.png)

![image-20210414154943811](picture/jvm性能监控与调优.assets/image-20210414154943811.png)

![image-20210414155040634](picture/jvm性能监控与调优.assets/image-20210414155040634.png)

![image-20210414155105065](picture/jvm性能监控与调优.assets/image-20210414155105065.png)![image-20210414155118087](picture/jvm性能监控与调优.assets/image-20210414155118087.png)

![image-20210414155226835](picture/jvm性能监控与调优.assets/image-20210414155226835.png)

![image-20210414155326381](picture/jvm性能监控与调优.assets/image-20210414155326381.png)

![image-20210414155359959](picture/jvm性能监控与调优.assets/image-20210414155359959.png)

**火焰图**

![image-20210414155546700](picture/jvm性能监控与调优.assets/image-20210414155546700.png)

# 4.常用的JVM参数

![image-20210415191934229](picture/jvm性能监控与调优.assets/image-20210415191934229.png)

## jvm参数选项类型

### 标准

![image-20210415192100558](picture/jvm性能监控与调优.assets/image-20210415192100558.png)

### -X

<img src="picture/jvm性能监控与调优.assets/image-20210415192249125.png" alt="image-20210415192249125" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415192242685.png" alt="image-20210415192242685" style="zoom:67%;" />

![image-20210415192548431](picture/jvm性能监控与调优.assets/image-20210415192548431.png)

<img src="picture/jvm性能监控与调优.assets/image-20210415192520128.png" alt="image-20210415192520128" style="zoom:67%;" />

### -XX

![image-20210415192629369](picture/jvm性能监控与调优.assets/image-20210415192629369.png)

#### PrintFlagsFinal

![image-20210415193128385](picture/jvm性能监控与调优.assets/image-20210415193128385.png)

## 常用的jvm参数选项

![image-20210415193411406](picture/jvm性能监控与调优.assets/image-20210415193411406.png)

![image-20210415193457747](picture/jvm性能监控与调优.assets/image-20210415193457747.png)

### 堆栈方法区等内存大小设置

![image-20210415193741798](picture/jvm性能监控与调优.assets/image-20210415193741798.png)

```
-XX:+UseAdaptivePolicy  默认是开启的，虽然你的默认比例是8:1:1，但是自动调整就会导致比例是6:1:1
而当你显式地设置了survivorratio=8，比例就会是8:1:1
```

![image-20210415194531437](picture/jvm性能监控与调优.assets/image-20210415194531437.png)

![image-20210415194612259](picture/jvm性能监控与调优.assets/image-20210415194612259.png)

![image-20210415194635132](picture/jvm性能监控与调优.assets/image-20210415194635132.png)

### OutofMemory相关

![image-20210415194805135](picture/jvm性能监控与调优.assets/image-20210415194805135.png)

<img src="picture/jvm性能监控与调优.assets/image-20210415195213146.png" alt="image-20210415195213146" style="zoom:67%;" />

### 垃圾回收器相关选项

<img src="picture/jvm性能监控与调优.assets/image-20210415195318771.png" alt="image-20210415195318771" style="zoom:67%;" />

![image-20210415195404813](picture/jvm性能监控与调优.assets/image-20210415195404813.png)

<img src="picture/jvm性能监控与调优.assets/image-20210415195420460.png" alt="image-20210415195420460" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415195539383.png" alt="image-20210415195539383" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415195612785.png" alt="image-20210415195612785" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415195737874.png" alt="image-20210415195737874" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415195923566.png" alt="image-20210415195923566" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415200124415.png" alt="image-20210415200124415" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415200304523.png" alt="image-20210415200304523" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415200435867.png" alt="image-20210415200435867" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415200335757.png" alt="image-20210415200335757" style="zoom:67%;" />

<img src="picture/jvm性能监控与调优.assets/image-20210415200555960.png" alt="image-20210415200555960" style="zoom:67%;" />

### GC日志相关

![image-20210415200644928](picture/jvm性能监控与调优.assets/image-20210415200644928.png)

不常用

![image-20210415201444568](picture/jvm性能监控与调优.assets/image-20210415201444568.png)

### 其他参数

![image-20210415201555202](picture/jvm性能监控与调优.assets/image-20210415201555202.png)

# 5.分析GC日志

## GC日志分析工具

### GCEasy

各种GC的信息，吞吐量啥的

![image-20210415202421489](picture/jvm性能监控与调优.assets/image-20210415202421489.png)

![image-20210415202449012](picture/jvm性能监控与调优.assets/image-20210415202449012.png)