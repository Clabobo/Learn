## 目标

* 构建覆盖更全面的 Android 面试知识体系，结合 3 年经验重点与源码原理。

* 增设 ART 虚拟机深入模块（dex、JIT/AOT、GC、类加载、启动流程等）。

* 为每个主题产出精炼总结、源码/图示、常见面试问答与实战示例。

## 输出规范

* 每个主题文档包含：核心概念、关键 API/源码位置、常见陷阱、面试答法框架、实战代码片段、延伸阅读。

* 统一采用中文，适配国内面试话术；必要处附英文术语。

* 建议每篇 800–1500 字，图示优先，代码片段可运行/可复用。

* 可以引用安卓官网的文章与知识

## 目录结构（拟）

* `docs/java/`、`docs/kotlin/`、`docs/android-core/`、`docs/ui-view/`、`docs/compose/`、`docs/jetpack/`、`docs/network-data/`、`docs/performance/`、`docs/testing/`、`docs/build-release/`、`docs/ndk-jni/`、`docs/framework/`、`docs/art-vm/`、`docs/tools/`、`docs/algorithms/`、`docs/interview-qa/`

* 每目录下分主题文件，如：`ui-view/touch-event-dispatch.md`、`art-vm/dex-format.md`

## 模块与主题

### Java（加强版）

* 集合与并发：HashMap/ConcurrentHashMap、线程池参数与拒绝策略、锁与原子类、阻塞队列。

* JVM：内存模型、类加载流程、双亲委派、逃逸分析、GC（G1 认知）。

* 设计模式与可维护性：常用模式在 Android 的落地、SOLID 原则、重构思路。

### Kotlin（加强版）

* 协程原理：调度器、挂起、结构化并发、异常与取消、Flow 背压与运算符。

* 语言特性：空安全、数据类、密封类、委托、内联、高阶函数与 DSL。

* 与 Java 互操作：SAM、注解处理、混编注意事项。

### Android 核心

* 四大组件：启动模式与任务栈、Service 前台/绑定、Broadcast 粘性/有序、ContentProvider 权限。

* 消息机制：Handler/Looper/MessageQueue 源码、线程切换与阻塞分析。

* IPC：Binder 基本原理、AIDL/Messenger、跨进程对象生命周期、稳定性与安全。

### UI 与 View

* 绘制流程：`measure/layout/draw`、`ViewRootImpl`、`Choreographer`、VSync 帧同步。

* 事件分发：`dispatchTouchEvent/onIntercept/onTouch`、`ViewGroup` 与嵌套滚动、`GestureDetector/VelocityTracker`、常见冲突解决模板。

* RecyclerView：缓存层级（scrap/pool）、DiffUtil、ItemAnimator 性能影响。

* 自定义 View：绘制优化、GPU 过度绘制、硬件加速注意事项。

### Jetpack 与架构

* MVVM/MVI 与状态管理：单向数据流、不可变状态、错误处理策略。

* Jetpack：ViewModel/Lifecycle、LiveData vs Flow、Room 并发、Paging3、WorkManager。

* 依赖注入：Hilt/Dagger 组件作用域、注入最佳实践与避免内存泄漏。

* 模块化与组件化：动态特性模块（DFM）、公共库拆分、依赖管理。

### Compose（加入）

* 组合与重组：`remember/derivedStateOf/LaunchedEffect`、稳定性与快照系统。

* 布局与绘制：`Modifier` 链、`SubcomposeLayout`、性能热点与测量成本。

* 状态提升与导航：`State Hoisting`、`Navigation Compose`、与 View 混用策略。

### 网络与数据

* OkHttp：连接复用、拦截器链、HTTP/2、TLS 配置、超时与重试策略。

* Retrofit：动态代理、协程适配器、错误模型设计、Mock/测试注入。

* 数据：Room/SQLite、DataStore vs SharedPreferences、缓存层（内存/磁盘/网络）。

### 性能优化

* 启动优化：冷/热启动分解、初始化推迟、Splash/预加载、AGP/App Startup。

* 渲染与卡顿：帧率监控、过度绘制、布局优化、图片优化、Recycler 滞后。

* 内存：泄漏定位（LeakCanary）、对象池、Bitmap 管理、OOM 防治。

* 体积：R8/D8、资源压缩、ABI/语言拆分、动态特性模块。

### 测试与质量

* 单元测试：JUnit/Mockito/Truth、协程测试、Repository 层隔离。

* UI 测试：Espresso/Compose Test、IdlingResource、截图对比。

* 静态检查：Lint、Detekt/Ktlint、CI 集成与质量门禁。

### 构建与发布

* Gradle/AGP：配置优化、构建缓存、并行与增量、变体与签名。

* 混淆与多 dex：R8 规则、反射保留、Multidex 原理与安装。

* 发布：渠道包、版本策略、Crash 收集与灰度。

### NDK/JNI（可选加强）

* JNI 基础：局部/全局引用、线程附着、字符串与数组、异常与泄漏。

* CMake/交叉编译、与 Kotlin/Java 交互、性能与稳定性权衡。

### 系统与 Framework

* Zygote/App 进程启动、AMS/WMS 简述、输入子系统与渲染管线（SurfaceFlinger）。

* ClassLoader/插件化/热修复基本原理与风险。

### ART 虚拟机深入（新增重点）

* dex 格式：指令集、字符串/类型/方法表、verifier 验证与 quickening。

* 编译模式：解释器、JIT（轮廓收集/Profiling）、AOT（`dex2oat`）、混合编译策略。

* OAT/ELF 与引导镜像：Boot Image、启动优化机制。

* GC：代际收集、并发/粘性标记清除（Sticky/CMS）策略与暂停时间优化。

* 内存管理：堆组织、分配策略、TLAB/对象对齐、逃逸分析影响。

* 类加载与反射：运行时布局、dex 缓存、反射开销与规避。

* 应用启动与方法热路径：Profile-guided 优化、Android N 以来的变化。

* 调试与分析：`perfetto/systrace`、`traceview`、ART 运行时参数。

### 工具与调试

* Profilers、ADB/`dumpsys`/`systrace`、`StrictMode`、网络抓包（Charles/mitmproxy）。

### 算法与数据结构

* 高频题：链表/二叉树/堆/滑窗/二分/并查集；复杂度与边界。

### 面试题库（贯穿）

* 按主题收录 5–10 道高频题：题目、思路、标准答法、追问路径、代码示例。

## 学习节奏（建议）

1. 第一阶段：Java/Kotlin 基础巩固 + Android 核心/事件分发 + 网络与数据。
2. 第二阶段：架构/Jetpack/Compose + 性能优化 + 构建发布。
3. 第三阶段：系统与 Framework 源码路径 + ART 虚拟机深入。
4. 贯穿：测试与质量、算法题库练习与面试答题模板。

## 首批交付（确认后执行）

* 建立 `docs` 目录与主题文件骨架。

* 完成《事件分发机制》与《ART 入门与编译模式》两篇详稿。

* 追加《OkHttp/Retrofit 原理》与《启动速度优化》概览文档。

## 需要您确认

* 是否接受上述更全面的模块划分与输出规范？

* ART 模块是否需要更多“源码级细节”（如具体类/方法路径）与实验脚本支持？

* 是否优先从“事件分发机制 + ART 入门”两篇开始实写？

