[Tomcat学习之ClassLoader](https://blog.csdn.net/aesop_wubo/article/details/7582266)

[Tomcat 实现原理与源码解析系统 —— 精品合集](http://www.iocoder.cn/Tomcat/Tomcat-collection/)

* 《Tomcat 7.0.42 源代码运行环境搭建》
* 《Tomcat 7 启动分析（一）启动脚本》
* 《Tomcat 7 启动分析（二）Bootstrap 类中的 main 方法》
* 《Tomcat 7 启动分析（三）Digester 的使用》
* 《Tomcat 7 启动分析（四）各组件 init、start 方法调用》
* 《Tomcat 7 启动分析（五）Lifecycle 机制和实现原理》
* 《Tomcat 7 的一次请求分析（一）处理线程的产生》
* 《Tomcat 7 服务器关闭原理》
* 《Tomcat 7 的一次请求分析（一）处理线程的产生》
* 《Tomcat 7 的一次请求分析（二）Socket 转换成内部请求对象》
* 《Tomcat 7 的一次请求分析（三）请求与容器中具体组件的匹配》
* 《Tomcat 7 的一次请求分析（四）Tomcat 7 阀机制原理》
* 《Tomcat 7 中 web 应用加载原理（一）Context 构建》
* 《Tomcat 7 中 web 应用加载原理（二）web.xml 解析》
* 《Tomcat 7 中 web 应用加载原理（三）Listener、Filter、Servlet 的加载和调用》
* 《Tomcat 7 自动加载类及检测文件变动原理》


## 主流程

1、通过脚本执行 org.apache.catalina.startup.Bootstrap 的 main方法

Lifecycle的状态流转图(摘自Lifecycle类注释):

```java
/*
 *            start()
 *  -----------------------------
 *  |                           |
 *  | init()                    |
 * NEW -»-- INITIALIZING        |
 * | |           |              |     ------------------«-----------------------
 * | |           |auto          |     |                                        |
 * | |          \|/    start() \|/   \|/     auto          auto         stop() |
 * | |      INITIALIZED --»-- STARTING_PREP --»- STARTING --»- STARTED --»---  |
 * | |         |                                                            |  |
 * | |destroy()|                                                            |  |
 * | --»-----«--    ------------------------«--------------------------------  ^
 * |     |          |                                                          |
 * |     |         \|/          auto                 auto              start() |
 * |     |     STOPPING_PREP ----»---- STOPPING ------»----- STOPPED -----»-----
 * |    \|/                               ^                     |  ^
 * |     |               stop()           |                     |  |
 * |     |       --------------------------                     |  |
 * |     |       |                                              |  |
 * |     |       |    destroy()                       destroy() |  |
 * |     |    FAILED ----»------ DESTROYING ---«-----------------  |
 * |     |                        ^     |                          |
 * |     |     destroy()          |     |auto                      |
 * |     --------»-----------------    \|/                         |
 * |                                 DESTROYED                     |
 * |                                                               |
 * |                            stop()                             |
 * ----»-----------------------------»------------------------------
 */
```

初始化组件过程: 链式调用


阅读源码: 时序图、类图、寻找执行主路径