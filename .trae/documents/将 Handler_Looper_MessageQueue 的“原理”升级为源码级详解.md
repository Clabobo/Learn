## 更深入的原理讲解（概览）

**架构分层**
- `Handler` 是消息的生产者/消费者接口；`MessageQueue` 是单线程的有序队列；`Looper` 提供事件循环，驱动队列出入与分发。
- 主线程在入口（ActivityThread）已调用 `Looper.prepareMainLooper()` 初始化 `Looper` 与 `MessageQueue`，随后在主线程一直 `Looper.loop()`。

**消息入队路径**
- `Handler.post(Runnable)`/`sendMessage(Message)` → `MessageQueue.enqueueMessage(Handler h, Message msg, long when)`：
  - `Message` 是单向链表；根据 `when` 时间戳插入保持有序（定时消息会延时出队）。
  - 若入队的时间点比当前队列头更早，需要通过 `nativeWake()` 唤醒阻塞的队列以重新调度。

**消息出队与分发**
- `Looper.loop()` 调用 `MessageQueue.next()`：
  - 当队列为空或下一条消息未到时间，会阻塞到 `nativePollOnce(ptr, timeout)`，其底层用 `epoll`/`eventfd` 等等待事件或超时。
  - 唤醒后取出可执行的消息，交还给 `Handler`：优先调用 `Callback`，否则执行 `handleMessage(msg)` 或 `Runnable#run()`。

**同步屏障与异步消息**
- `MessageQueue.postSyncBarrier()` 插入屏障标记，将同步消息暂时阻塞，优先调度标记为异步的消息（例如 UI 绘制相关）。
- 典型场景是 `Choreographer` 利用异步消息确保 VSync 驱动下的绘制优先级，防止被普通任务淹没；使用后必须 `removeSyncBarrier(token)` 及时移除，避免普通消息饥饿。

**IdleHandler 与空闲阶段**
- `MessageQueue.addIdleHandler(IdleHandler)`：当队列没有立即可执行的消息，进入空闲回调，可用于低优先级维护任务（日志上报、缓存清理）。
- Idle 回调要短、可中断，避免成为隐性阻塞。

**关键数据结构与语义**
- `Message`：`target`（目标 Handler）、`when`（执行时间）、`what/obj/arg1/arg2`（载荷）；链表按 `when` 有序。
- `MessageQueue`：维护头指针与若干特殊标记（如屏障）；对 `nativeWake()` 的唤醒机制与 `nativePollOnce()` 的阻塞/超时策略。
- `Looper`：持有当前线程的 `MessageQueue` 与 `Thread` 信息；循环直到 `quit()` 或异常。

**性能与风险**
- 主线程重活导致掉帧：拆分任务、使用子线程/后台调度，避免阻塞 `doFrame` 窗口。
- 屏障误用导致普通消息饥饿：必须成对管理 token 并及时移除。
- Handler 泄漏：匿名内部类持有外部引用，建议使用静态内部类 + `WeakReference`。
- 未清理的延时消息与循环消息引发逻辑异常或功耗问题：统一取消策略（`removeCallbacksAndMessages(null)`）。

## 文档改造计划

**目标**：将 `docs/android-core/handler-looper-messagequeue.md` 的“原理”板块升级到源码级详解，加入分层、入队/出队路径、同步屏障、Idle、数据结构与性能风险的完整说明。

**新增内容**
1. 架构分层与主线程初始化：`Looper.prepareMainLooper()`/`Looper.loop()` 的职责与时机
2. 入队路径详解：`enqueueMessage` 插入规则、`nativeWake()` 唤醒条件
3. 出队与阻塞：`next()` 与 `nativePollOnce()`（epoll/eventfd），超时与唤醒场景
4. 同步屏障与异步消息：屏障语义、`Choreographer` 优先级、饥饿风险与治理
5. IdleHandler 使用规范：空闲任务的约束与典型用途
6. 数据结构清单：`Message` 字段语义、链表有序、队列标记与状态
7. 风险与优化：主线程重活、泄漏、延时消息清理与度量

**配套补充**
- 源码路径与方法清单：`Looper#loop`、`MessageQueue#enqueueMessage/next/postSyncBarrier/removeSyncBarrier`、`Handler#dispatchMessage`、`nativePollOnce/nativeWake`
- 两张图：事件循环时序图（含屏障/唤醒）、类关系图（含队列/消息/Handler）
- 一个可运行的演示片段：插入屏障优先异步消息 + 成对移除验证
- 自测题（3–5 题）与追问路径（2–3 条）

**交付顺序**
- 先更新“原理”详解与图；再补示例与自测题；最后统一术语与验证。