## 目标
- 立即编写 6 篇高质量深度文档，采用“原理→源码→示例→对比→图表→性能与瓶颈→面试框架”结构。
- 每篇覆盖方法级细节、关键数据结构与设计权衡，示例完整可运行（Kotlin/Java），配套 Mermaid 时序/类图。

## 文档与路径
1. docs/framework/binder-overview.md（Binder 原理与用户态/内核态协同）
2. docs/ui-view/recyclerview-caching-optimization.md（RecyclerView 缓存机制与性能）
3. docs/kotlin/coroutines-scheduling-cancellation.md（协程调度/取消/结构化并发）
4. docs/art-vm/gc-strategies-and-pause-optimization.md（ART GC 策略与暂停优化）
5. docs/art-vm/boot-image-oat-elf-details.md（Boot Image 与 OAT/ELF 产物细节）
6. docs/art-vm/profile-guided-compilation-practice.md（Profile-Guided Compilation 实战）

## 章节模板（每篇均包含）
- 原理：组件与流程、设计决策、权衡取舍
- 源码：类/方法路径，关键调用链与数据结构
- 示例：可运行片段（Kotlin/Java），工程化落地
- 对比：不同实现方案优劣与适用场景
- 图表：时序图/类图/组件关系图（Mermaid）
- 性能与瓶颈：指标、优化点、常见误区
- 面试答题框架：3–5 点结构化回答与追问路径

## 每篇大纲（摘要）

### 1) Binder 原理（framework/binder-overview.md）
- 原理：用户态 BinderProxy/Stub，Binder 驱动（/dev/binder），ServiceManager/Handle，内核队列，事务与引用计数
- 源码：frameworks/base/core/java、system/libbinder、drivers/android/binder
- 示例：AIDL 与 Messenger 对比实现；安全与稳定性策略（超时、异常）
- 图表：跨进程调用时序、类关系图（Binder、Parcel、IBinder）

### 2) RecyclerView 缓存（ui-view/recyclerview-caching-optimization.md）
- 原理：四层缓存（Attached/Changed/Scrap/ViewCacheExtension/RecyclerPool），测量与绑定成本，DiffUtil/动画影响
- 源码：RecyclerView、Recycler、RecycledViewPool、AdapterHelper、ItemAnimator
- 示例：大列表优化模板（稳定 id、预取、池大小调优、禁用不必要动画）
- 图表：复用时序、类图（Recycler/Pool/ViewHolder）

### 3) 协程调度与取消（kotlin/coroutines-scheduling-cancellation.md）
- 原理：调度器（Main/IO/Default/Unconfined）、结构化并发、取消传播、异常处理、背压与 Flow
- 源码：kotlinx-coroutines（Job/Deferred/Dispatcher/CoroutineScope/Channel/Flow）
- 示例：并发请求聚合、超时取消、Supervisor 降级、Flow 背压
- 图表：协程生命周期/取消传播时序、类图（Job/Scope/Dispatcher）

### 4) ART GC 策略（art-vm/gc-strategies-and-pause-optimization.md）
- 原理：CMS、Sticky、Young/Generational、并发标记清除、写屏障与根集扫描
- 源码：art/runtime/gc/collector/*、gc/heap、thread-list
- 示例：内存压力模拟与 GC 观察、对象池/Bitmap 管理策略
- 图表：GC 时序、堆结构图

### 5) Boot Image 与 OAT/ELF（art-vm/boot-image-oat-elf-details.md）
- 原理：Boot Image 组成、加载流程、OAT/ELF 文件结构与关联、dex2oat 生成路径
- 源码：image_space.cc、oat_file.cc、dex2oat、class_linker.cc
- 示例：Baseline Profiles 与启动路径联动案例
- 图表：加载时序与组件关系

### 6) PGC 实战（art-vm/profile-guided-compilation-practice.md）
- 原理：运行期 Profile 收集、AOT 生成与应用、冷/热/常用方法分类
- 源码：profile.cc、jit/jit_code_cache.cc、dex2oat 关联
- 示例：Gradle/AndroidX Baseline Profiles 配置与验证
- 图表：Profile 采集与应用流程时序

## 质量与规范
- 术语统一；示例完整可运行；图表标注规范；源码路径准确到类/方法
- 示例尽量不依赖复杂外部库，使用标准 Android/Kotlin API

## 交付节奏（本轮）
- 依次完成 6 篇文档的全文撰写与图表补充；每篇约 1500–2500 字（含图表/代码）

## 確认事项
- 若后续希望补充更多源码段落（整段贴出），可逐篇指定具体类与方法，我将增补原文片段与注释版解析
