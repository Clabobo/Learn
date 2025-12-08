# 生命周期管理与状态恢复深度指南：原理与源码

本文将深入探讨 Android 开发中最容易出错的领域：**生命周期 (Lifecycle)** 与 **状态恢复 (State Restoration)**。我们将超越基础 API，深入 `FragmentManager` 事务、`ViewModelStore` 存储原理以及 Jetpack Compose 的状态机制。

## 1. Fragment 生命周期与事务内部机制

Fragment 的生命周期不仅依赖 Activity，还受 **FragmentTransaction** 操作的影响。

### 1.1 `add` vs `replace` 的生命周期差异

```mermaid
graph TD
    subgraph Add["Transaction.add()"]
        A1[Fragment A (Existing)] --> A2[Fragment A (Resumed)]
        B1[Fragment B (New)] --> B2[onAttach -> onCreate -> ... -> onResume]
        Note1[Fragment A 不受影响，两者重叠]
    end

    subgraph Replace["Transaction.replace()"]
        C1[Fragment A (Existing)] --> C2[onPause -> onStop -> onDestroyView]
        D1[Fragment B (New)] --> D2[onAttach -> onCreate -> ... -> onResume]
        Note2[Fragment A 视图被销毁，实例保留(若入栈)]
    end
```

-   **add**: 仅仅是将 Fragment 的 View 添加到容器中。原有的 Fragment **不会走任何生命周期回调**（依然是 Resumed 状态）。
-   **replace**: 相当于 `remove(Old)` + `add(New)`。
    -   旧 Fragment 走 `onDestroyView`（如果加入 BackStack，实例保留，不走 `onDestroy`）。
    -   新 Fragment 走完整生命周期。

### 1.2 ViewLifecycleOwner 的由来

为什么 Fragment 有两个生命周期？(`Fragment` 实例 vs `Fragment View`)

当 Fragment 被放入 BackStack 时：
1.  **View 被销毁**: `onDestroyView` 被调用。
2.  **实例仍存活**: `onDestroy` **未**被调用。
3.  **出栈恢复**: 当用户按返回键，Fragment 从 BackStack 恢复，重新走 `onCreateView`。

**陷阱**: 如果你在 `onCreate` 中观察 LiveData，当 Fragment 出栈恢复时，`onCreate` 不会再次调用，但 `onCreateView` 会。如果你在 `onCreateView` 中观察 LiveData 传入 `this` (Fragment)，每次 View 重建都会注册一个新的 Observer，导致 **重复回调**。

**解法**: 永远使用 `viewLifecycleOwner`。

---

## 2. ViewModel 内部原理：它是如何幸存的？

`ViewModel` 能在配置变更（如旋转屏幕）中存活，靠的是 `Activity` 的 `NonConfigurationInstances` 机制。

### 2.1 源码链路

1.  **Activity 重建**: 当旋转屏幕时，系统销毁 Activity，但会调用 `onRetainNonConfigurationInstance()`。
2.  **保存 ViewModelStore**: `ComponentActivity` 重写了这个方法，将 `mViewModelStore` 对象返回给系统。
3.  **恢复 ViewModelStore**: 新 Activity 启动时，在 `onCreate` 中通过 `getLastNonConfigurationInstance()` 拿回了旧的 `mViewModelStore`。
4.  **关联**: 新 Activity 的 `ViewModelProvider` 从这个复用的 Store 中获取 ViewModel 实例。

```java
// ComponentActivity.java (简化)
public Object onRetainNonConfigurationInstance() {
    // 核心：将 ViewModelStore 作为一个普通对象返回给系统托管
    Object viewModelStore = mViewModelStore;
    return viewModelStore;
}
```

### 2.2 SavedStateRegistry 原理

`SavedStateHandle` 是如何跨进程保存数据的？

-   它利用了 `onSaveInstanceState(Bundle)` 机制。
-   **SavedStateRegistry**: 一个钩子，允许组件（如 ViewModel）注册自己的状态保存回调。
-   **序列化**: 当 Activity 停止时，Registry 收集所有注册者的 Bundle，合并成一个大 Bundle，通过 Binder 传递给 AMS。
-   **反序列化**: Activity 重建时，Registry 从 Bundle 中拆分数据，还原给 `SavedStateHandle`。

---

## 3. Jetpack Compose 生命周期与状态

Compose 没有 Fragment 那样复杂的生命周期回调，它只有 **进入组合 (Enter)**、**重组 (Recompose)** 和 **离开组合 (Leave)**。

### 3.1 `remember` vs `rememberSaveable`

| API | 存活范围 | 存储位置 | 适用场景 |
| :--- | :--- | :--- | :--- |
| `remember` | **重组 (Recomposition)** | SlotTable (内存) | 动画状态、临时 UI 逻辑 |
| `rememberSaveable` | **配置变更 + 进程被杀** | Bundle (SavedStateRegistry) | 用户输入、滚动位置、导航状态 |

### 3.2 源码：`rememberSaveable` 如何工作？

它内部使用了 `DisposableEffect` 向 `LocalSavedStateRegistryOwner` 注册了一个 Provider。

```kotlin
@Composable
fun <T> rememberSaveable(
    inputs: Array<out Any?>,
    saver: Saver<T, out Any>,
    key: String? = null,
    init: () -> T
): T {
    // 1. 获取 Registry
    val registry = LocalSavedStateRegistryOwner.current.savedStateRegistry
    
    // 2. 注册保存回调
    DisposableEffect(registry, key) {
        val provider = { saver.save(value) }
        val entry = registry.registerProvider(finalKey, provider)
        onDispose { entry.unregister() }
    }
    
    // 3. 恢复数据
    val restored = registry.consumeRestored(finalKey)
    return remember { restored ?: init() }
}
```

---

## 4. 实战：进程被杀 (Process Death) 模拟与验证

这是开发者最容易忽略的场景。

### 4.1 模拟步骤 (ADB)
这是最真实的模拟方式，比“开发者选项-不保留活动”更靠谱。

1.  打开 App，进入二级页面，输入一些文本。
2.  按 Home 键回到桌面（App 进入后台）。
3.  终端执行：`adb shell am kill <your.package.name>`
4.  从“最近任务”切回 App。

### 4.2 验证清单
-   [ ] **ViewModel**: 数据是否丢失？(如果没用 `SavedStateHandle` 就会丢)
-   [ ] **Compose**: `TextField` 内容还在吗？(如果只用 `remember` 就会丢，必须 `rememberSaveable`)
-   [ ] **Navigation**: 返回栈是否正常？按 Back 键能否回到上一页？

## 5. 总结

-   **Fragment**: 理解 `ViewLifecycleOwner` 是避免内存泄漏的关键。
-   **ViewModel**: 本质是利用 `onRetainNonConfigurationInstance` 在 Activity 重建间隙“偷渡”对象。
-   **Compose**: `rememberSaveable` 是 View 系统 `onSaveInstanceState` 的声明式替代品，底层机制完全一致。
