# Android 面试知识体系

**目标读者**: Android 开发工程师 (3年经验)
**技术栈**: Java (主要), Kotlin

## 目录

### 1. Java 基础
- [x] 集合框架 (HashMap, ArrayList, LinkedList 等源码分析)
  - 文档: [Java 集合源码与性能](docs/java/collections-internals.md)
- [x] 并发编程 (线程, 线程池, 锁机制, volatile, synchronized)
  - 文档: [Java 并发与线程池](docs/java/concurrency-threads-pools-locks.md)
- [x] JVM (内存模型, 垃圾回收机制, 类加载)
  - 文档: [JVM 内存/GC/类加载](docs/java/jvm-memory-gc-classloading.md)
- [x] 面向对象原则与设计模式 (单例, 工厂, 观察者, 代理等)
  - 文档: [OOP 原则与设计模式](docs/java/oop-design-patterns.md)

### 2. Kotlin 基础
- [x] Kotlin 与 Java 的对比 (优缺点, 互操作)
  - 文档: [Kotlin vs Java 互操作](docs/kotlin/kotlin-vs-java-interoperability.md)
- [x] 协程 (Coroutines) 与 Flow (原理与实战)
  - 文档: [协程调度与取消](docs/kotlin/coroutines-scheduling-cancellation.md)
- [x] 扩展函数与高阶函数
  - 文档: [扩展函数与高阶函数](docs/kotlin/extensions-higher-order-functions.md)
- [x] 空安全机制 (Null Safety)
  - 文档: [Kotlin 空安全](docs/kotlin/null-safety.md)

### 3. Android 核心组件
- [x] 四大组件深入 (Activity 启动模式, Service 类型, Broadcast, ContentProvider)
  - 文档: [Activity 启动模式与任务栈](docs/android-core/activity_launch_modes_guide-优秀.md) · [Activity 启动流程源码解析](docs/android-core/activity-startup-process-deep-dive.md) · [Service & Broadcast 深度解析](docs/android-core/service-broadcast-receiver-deep-dive.md) · [ContentProvider 与 IPC](docs/android-core/contentprovider-permissions-ipc.md)
- [x] 生命周期管理 (Activity/Fragment 状态保存与恢复)
  - 文档: [生命周期与状态恢复](docs/android-core/lifecycle-owners-state-restoration.md)
- [x] 进程间通信 IPC (AIDL, Messenger, Binder 原理)
  - 文档: [ContentProvider 与 IPC](docs/android-core/contentprovider-permissions-ipc.md) · [Binder 原理](docs/framework/binder-overview.md)
- [x] 消息机制 (Handler, Looper, MessageQueue 源码分析)
  - 文档: [Handler 机制详解](docs/android-core/handler机制详解.md)

### 4. UI 与 View 体系
- [x] View 绘制流程 (Measure, Layout, Draw)
  - 文档: [View 绘制管线](docs/ui-view/view-drawing-pipeline.md)
- [x] 事件分发机制 (Touch Event Dispatch)
  - 文文档: [事件分发机制](docs/ui-view/touch-event-dispatch.md)
- [ ] 自定义 View 实战与优化
- [x] RecyclerView 缓存机制与性能优化
  - 文档: [RecyclerView 缓存优化](docs/ui-view/recyclerview-caching-optimization.md)

### 5. 架构与 Jetpack
- [ ] 架构模式 (MVC / MVP / MVVM / MVI)
- [x] Jetpack 核心组件 (ViewModel, LiveData, Room, Navigation, WorkManager)
  - 文档: [ViewModel/Lifecycle 深入](docs/jetpack/viewmodel-lifecycle-internals.md) · [WorkManager 调度与重试](docs/jetpack/workmanager-scheduling-retries.md)
- [ ] 依赖注入 (Dagger2 / Hilt)

### 6. 网络与数据持久化
- [x] 网络框架原理 (OkHttp 拦截器与连接池, Retrofit 动态代理)
  - 文档: [OkHttp/Retrofit 原理](docs/network-data/okhttp-retrofit-overview.md)
- [x] 数据存储 (SQLite, Room 封装, SharedPreferences vs DataStore)
  - 文档: [Room 并发与事务](docs/network-data/room-concurrency-transactions.md) · [DataStore vs SharedPreferences](docs/network-data/datastore-vs-sharedpreferences.md)

### 7. 性能优化
- [ ] 内存优化 (内存泄漏检测 LeakCanary, 内存抖动, Profiler 工具)
- [ ] UI 渲染与卡顿优化 (布局层级, 过度绘制)
- [x] App 启动速度优化 (冷启动, 热启动)
  - 文档: [启动速度优化](docs/performance/startup-optimization-overview.md)
- [x] APK 体积瘦身
  - 文档: [Gradle 构建优化](docs/build-release/gradle-agp-build-optimization.md) · [R8 混淆与瘦身](docs/build-release/r8-rules-reflection-keep.md)
- [x] 性能工具
  - 文档: [性能诊断工具](docs/tools/profilers-tracing-systrace.md)

### 8. Framework 源码与进阶
- [x] Binder 驱动与通信原理
  - 文档: [Binder 原理](docs/framework/binder-overview.md)
- [ ] 系统服务 (AMS, WMS 启动流程简述)
- [x] ClassLoader 类加载机制 (热修复插件化基础)
  - 文档: [ART 入门与编译模式](docs/art-vm/art-intro-and-compilation.md) · [ART GC 策略](docs/art-vm/gc-strategies-and-pause-optimization.md) · [Boot Image/OAT/ELF](docs/art-vm/boot-image-oat-elf-details.md) · [PGC 实战](docs/art-vm/profile-guided-compilation-practice.md)

### 9. 算法与数据结构
- [x] 常见面试算法题 (链表, 树, 排序, 动态规划)
  - 文档: [算法题库](docs/algorithms/interview-problems-collection.md)

---
*本项目用于总结面试核心知识点、代码片段及重点笔记，方便复习与记忆。*

## 建议学习路径

- 基础巩固: Java/Kotlin → Android 核心（Handler/事件分发/绘制）
- 架构与Jetpack: ViewModel/WorkManager/Room → Compose 状态
- 工程与性能: 启动优化/诊断工具/Gradle & R8
- Framework 与 ART: Binder/GC/Boot Image/PGC
