# PPT 对象交互的严格有限状态机（FSM）定义

> 本文档基于形式化的有限状态机理论，定义 PPT 对象的交互状态机
>
> **核心原则："一动一态"** - 每个键鼠操作（事件）触发一次状态转换

---

## 一、有限状态机的形式化定义

一个有限状态机 **M** 可以表示为五元组：

```
M = (S, E, δ, s₀, F)
```

其中：
- **S**：有限的状态集合（States）
- **E**：有限的事件集合（Events）- 键鼠操作
- **δ**：状态转换函数 `δ: S × E → S`（当前状态 × 事件 → 下一个状态）
- **s₀**：初始状态（Initial State）
- **F**：终止状态集合（Final States）- 在交互系统中可选

---

## 二、事件集合 E（Events）- 键鼠操作

所有状态转换都由用户的键鼠操作触发。以下是完整的事件枚举：

### 2.1 鼠标事件（Mouse Events）

#### 基础鼠标事件
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E1` | **MouseEnter** | 鼠标进入对象区域 |
| `E2` | **MouseLeave** | 鼠标离开对象区域 |
| `E3` | **MouseDown** | 鼠标按下（在对象上） |
| `E4` | **MouseUp** | 鼠标释放 |
| `E5` | **Click** | 单击（MouseDown + MouseUp） |
| `E6` | **DoubleClick** | 双击 |
| `E7` | **RightClick** | 右键单击 |

#### 拖拽事件
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E8` | **DragStart** | 开始拖拽（MouseDown + MouseMove） |
| `E9` | **Dragging** | 拖拽中（持续 MouseMove） |
| `E10` | **DragEnd** | 结束拖拽（MouseUp） |

#### 特定区域事件（基于"可交互属性"）
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E11` | **Click_Body** | 单击对象主体区域 |
| `E12` | **Click_Text** | 单击文本内容区域 |
| `E13` | **DoubleClick_Body** | 双击对象主体 |
| `E14` | **DoubleClick_Text** | 双击文本区域 |
| `E15` | **Drag_Body** | 拖拽对象主体 |
| `E16` | **Drag_ResizeHandle** | 拖拽缩放控制点 |
| `E17` | **Drag_RotateHandle** | 拖拽旋转手柄 |
| `E18` | **Click_Outside** | 点击对象外部（画布空白） |

### 2.2 键盘事件（Keyboard Events）

#### 功能键
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E20` | **Press_ESC** | 按下 ESC 键 |
| `E21` | **Press_Enter** | 按下 Enter 键 |
| `E22` | **Press_Delete** | 按下 Delete 键 |
| `E23` | **Press_F2** | 按下 F2 键 |
| `E24` | **Press_Tab** | 按下 Tab 键 |
| `E25` | **Press_ArrowKey** | 按下方向键 |

#### 修饰键组合（Modifier Keys）
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E30` | **Ctrl+Click** | Ctrl + 单击 |
| `E31` | **Shift+Click** | Shift + 单击 |
| `E32` | **Ctrl+A** | 全选 |
| `E33` | **Ctrl+C** | 复制 |
| `E34` | **Ctrl+V** | 粘贴 |
| `E35` | **Ctrl+G** | 组合 |
| `E36` | **Ctrl+Shift+G** | 解组 |
| `E37` | **Shift+Drag** | Shift + 拖拽（约束操作） |
| `E38` | **Ctrl+Drag** | Ctrl + 拖拽（复制拖拽） |

### 2.3 文本输入事件
| 事件ID | 事件名称 | 描述 |
|-------|---------|------|
| `E40` | **TextInput** | 输入文字（在编辑态） |
| `E41` | **TextSelect** | 选中文本（拖拽或 Shift+方向键） |
| `E42` | **TextDelete** | 删除文字（Backspace/Delete） |

---

## 三、状态集合 S（States）

**核心原则：状态是互斥的，同一时刻对象只能处于一种状态**

### 3.1 核心状态定义

| 状态ID | 状态名称 | 描述 | 进入条件示例 |
|-------|---------|------|------------|
| `S0` | **Idle** | 默认态/空闲态 | 对象创建后的初始状态 |
| `S1` | **Hovered** | 悬停态 | 鼠标进入对象区域 |
| `S2` | **Selected** | 选中态 | 单击对象 |
| `S3` | **Editing** | 编辑态 | 双击文本框/按F2 |
| `S4` | **Moving** | 移动中 | 拖拽对象主体 |
| `S5` | **Resizing** | 缩放中 | 拖拽缩放控制点 |
| `S6` | **Rotating** | 旋转中 | 拖拽旋转手柄 |

**说明：**
- `S0 (Idle)` 是初始状态 `s₀`
- 这7个状态是**互斥的**
- 不包含"变换中(Transforming)"父状态 - `Moving/Resizing/Rotating` 是独立状态

### 3.2 可选扩展状态

根据对象类型，可能有额外状态：

| 状态ID | 状态名称 | 适用对象 | 描述 |
|-------|---------|---------|------|
| `S7` | **MultiSelected** | 所有 | 多个对象被选中 |
| `S8` | **PreSelection** | 表格/组合 | 子元素预选状态 |
| `S9` | **PathEditing** | 曲线/形状 | 路径编辑模式 |
| `S10` | **CellEditing** | 表格 | 单元格编辑 |
| `S11` | **Playing** | 视频/音频 | 播放中（放映模式） |

---

## 四、状态转换函数 δ（Transition Function）

### 4.1 状态转换表（核心）

**格式：`δ(当前状态, 事件) = 下一个状态`**

| 当前状态 ↓ / 事件 → | MouseEnter | MouseLeave | Click_Body | DoubleClick_Text | Drag_Body | Drag_ResizeHandle | Click_Outside | ESC |
|----------|-----------|-----------|-----------|----------------|----------|-----------------|--------------|-----|
| **S0: Idle** | S1 | - | S2 | S3 | - | - | S0 | - |
| **S1: Hovered** | - | S0 | S2 | S3 | S4 | - | S0 | S0 |
| **S2: Selected** | - | - | S2 | S3 | S4 | S5 | S0 | S0 |
| **S3: Editing** | - | - | - | - | - | - | S2 | S2 |
| **S4: Moving** | - | - | - | - | S4 | - | - | S2 |
| **S5: Resizing** | - | - | - | - | - | S5 | - | S2 |
| **S6: Rotating** | - | - | - | - | - | - | - | S2 |

**说明：**
- `-` 表示该事件不触发状态转换（保持当前状态或无效操作）
- `S4/S5/S6` 的 `MouseUp` 事件（即 `DragEnd`）统一返回 `S2 (Selected)`

### 4.2 详细状态转换规则

#### 从 S0 (Idle) 出发
```
δ(S0, MouseEnter)      = S1  // 鼠标进入 → 悬停态
δ(S0, Click_Body)      = S2  // 单击 → 选中态
δ(S0, DoubleClick_Text)= S3  // 双击文本 → 编辑态
```

#### 从 S1 (Hovered) 出发
```
δ(S1, MouseLeave)      = S0  // 鼠标离开 → 默认态
δ(S1, Click_Body)      = S2  // 单击 → 选中态
δ(S1, DoubleClick_Text)= S3  // 双击文本 → 编辑态
δ(S1, Drag_Body)       = S4  // 拖拽主体 → 移动中
```

#### 从 S2 (Selected) 出发
```
δ(S2, Click_Outside)   = S0  // 点击外部 → 默认态
δ(S2, ESC)             = S0  // 按ESC → 默认态
δ(S2, DoubleClick_Text)= S3  // 双击文本 → 编辑态
δ(S2, Press_F2)        = S3  // 按F2 → 编辑态
δ(S2, Drag_Body)       = S4  // 拖拽主体 → 移动中
δ(S2, Drag_ResizeHandle)=S5  // 拖拽缩放点 → 缩放中
δ(S2, Drag_RotateHandle)=S6  // 拖拽旋转柄 → 旋转中
```

#### 从 S3 (Editing) 出发
```
δ(S3, Click_Outside)   = S2  // 点击外部 → 选中态（或S0）
δ(S3, ESC)             = S2  // 按ESC → 选中态
δ(S3, TextInput)       = S3  // 输入文字 → 保持编辑态
```

#### 从 S4/S5/S6 (Moving/Resizing/Rotating) 出发
```
δ(S4, MouseUp)         = S2  // 释放鼠标 → 选中态
δ(S4, ESC)             = S2  // 按ESC → 选中态（取消操作）
δ(S4, Dragging)        = S4  // 持续拖拽 → 保持移动中

δ(S5, MouseUp)         = S2  // 释放鼠标 → 选中态
δ(S5, ESC)             = S2  // 按ESC → 选中态
δ(S5, Dragging)        = S5  // 持续拖拽 → 保持缩放中

δ(S6, MouseUp)         = S2  // 释放鼠标 → 选中态
δ(S6, ESC)             = S2  // 按ESC → 选中态
δ(S6, Dragging)        = S6  // 持续拖拽 → 保持旋转中
```

---

## 五、对象属性作为约束条件（不是状态）

之前定义的"修饰状态"（如锁定、成组、隐藏）实际上是**对象的持久化属性**，它们**不是状态机的状态**，而是**约束条件**，影响状态转换规则。

### 5.1 对象属性枚举

| 属性名 | 类型 | 描述 |
|-------|------|------|
| `isLocked` | boolean | 对象是否被锁定 |
| `isHidden` | boolean | 对象是否隐藏 |
| `isGrouped` | boolean | 对象是否属于组合 |
| `isMasterObject` | boolean | 对象是否来自母版 |
| `hasText` | boolean | 对象是否包含可编辑文本 |
| `hasAnimation` | boolean | 对象是否绑定动画 |

### 5.2 约束条件对状态转换的影响

**条件转换函数：** `δ'(S, E, P) = S'`
- `S`：当前状态
- `E`：事件
- `P`：对象属性集合（约束条件）
- `S'`：下一个状态

#### 示例：锁定对象的约束

```
if (isLocked == true):
    δ(S2, Drag_Body)       = S2  // 无法移动，保持选中态
    δ(S2, Drag_ResizeHandle)=S2  // 无法缩放，保持选中态
    δ(S2, DoubleClick_Text) = S2  // 无法编辑，保持选中态
else:
    δ(S2, Drag_Body)       = S4  // 正常转换到移动中
    δ(S2, Drag_ResizeHandle)=S5  // 正常转换到缩放中
    δ(S2, DoubleClick_Text) = S3  // 正常转换到编辑态
```

#### 示例：无文本对象的约束

```
if (hasText == false):
    δ(S2, DoubleClick_Body) = S2  // 双击无效，保持选中态
    δ(S2, Press_F2)         = S2  // F2无效，保持选中态
else:
    δ(S2, DoubleClick_Body) = S3  // 进入编辑态
    δ(S2, Press_F2)         = S3  // 进入编辑态
```

#### 示例：成组对象的约束

```
if (isGrouped == true):
    δ(S0, Click_Body)      = S2_Group  // 选中整个组
    δ(S2_Group, DoubleClick_Body) = S2_InGroup  // 进入组内编辑模式
```

---

## 六、状态转换图（可视化）

```
                MouseEnter        Click_Body
    ┌──────┐  ────────────→  ┌──────────┐  ─────────→  ┌──────────┐
    │      │                 │          │              │          │
    │ Idle │                 │ Hovered  │              │ Selected │
    │ (S0) │                 │  (S1)    │              │  (S2)    │
    │      │  ←────────────  │          │  ←──────────  │          │
    └──────┘   MouseLeave    └──────────┘  Click_Outside└──────────┘
                                  │  ↑                      │  │  │
                                  │  │                      │  │  │
                        Drag_Body │  │ MouseUp    F2/       │  │  │
                                  │  │            DblClick  │  │  │
                                  ↓  │                      ↓  │  │
                              ┌──────────┐              ┌─────────┐
                              │          │              │         │
                              │  Moving  │              │ Editing │
                              │   (S4)   │              │  (S3)   │
                              │          │              │         │
                              └──────────┘              └─────────┘
                                                            │  ↑
                                                      ESC / │  │ TextInput
                                                  Click_Out │  │
                                                            ↓  │
                                                        (return to S2)

    (S2) Selected ──[Drag_ResizeHandle]──→ (S5) Resizing ──[MouseUp]──→ (S2)
    (S2) Selected ──[Drag_RotateHandle]──→ (S6) Rotating ──[MouseUp]──→ (S2)
```

---

## 七、形式化验证清单

一个严格的 FSM 必须满足：

### ✅ 7.1 完备性（Completeness）
- 每个状态对于每个可能的事件，必须有明确的转换规则（可以是"保持当前状态"）
- **状态转换表**必须没有空白单元格

### ✅ 7.2 确定性（Determinism）
- 对于任意 `(状态, 事件)` 对，下一个状态是**唯一确定的**
- 不存在一个事件导致多个可能的目标状态（除非有明确的约束条件区分）

### ✅ 7.3 互斥性（Mutual Exclusion）
- 核心状态是**互斥的**，对象在任意时刻只能处于一个核心状态
- `S0, S1, S2, S3, S4, S5, S6` 不可能同时存在

### ✅ 7.4 可达性（Reachability）
- 所有状态都可以从初始状态 `S0` 经过有限步骤到达
- 没有"孤立状态"

### ✅ 7.5 可恢复性（Recoverability）
- 从任意状态，用户可以通过操作返回到 `S0` 或 `S2`
- 没有"死锁状态"

---

## 八、与 M × N 矩阵的关系

现在我们有了：
- **N 维度（状态）**：7个核心状态（S0-S6）
- **E（事件）**：40+ 键鼠操作

**但是，M 维度（可交互属性）与事件的关系：**

**关键洞察：事件不是直接的键鼠操作，而是"在特定属性区域的键鼠操作"**

例如：
- `Click_Body` ≠ `Click_Text` ≠ `Click_ResizeHandle`
- 虽然都是"Click"事件，但作用在不同的**可交互属性（M）**上

**因此，M × N 矩阵实际上是：**

```
(可交互属性 M) × (状态 S) = (光标样式, 允许的事件集合, 事件触发的状态转换)
```

**示例：**

| 属性 ↓ / 状态 → | S0: Idle | S1: Hovered | S2: Selected | S3: Editing |
|---------------|---------|------------|-------------|------------|
| **文本内容** | - | 🖱️ I-beam<br>Click→S2<br>DblClick→S3 | 🖱️ I-beam<br>DblClick→S3 | 🖱️ I-beam<br>TextInput→S3 |
| **填充区域** | - | 🖱️ Move cursor<br>Click→S2<br>Drag→S4 | 🖱️ Move cursor<br>Drag→S4 | ❌ 不可见 |
| **缩放点** | - | 🖱️ Resize cursor<br>Drag→S5 | 🖱️ Resize cursor<br>Drag→S5 | ❌ 隐藏 |

---

## 九、总结与下一步

### ✅ 已完成：严格的 FSM 定义

1. **事件集合 E**：40+ 键鼠操作
2. **状态集合 S**：7个核心互斥状态
3. **转换函数 δ**：完整的状态转换表
4. **约束条件**：对象属性如何影响转换

### 🎯 下一步：连接 FSM 与 M × N 矩阵

**核心问题：**
> 不同的"可交互属性（M）"在不同的"状态（S）"下，接收哪些事件（E），并触发什么状态转换？

**需要做的：**
1. **枚举可交互属性（M）** - 如：文本内容、填充区域、缩放点、旋转柄、边框等
2. **构建 M × S × E 三维映射** - 属性 × 状态 × 事件 → (光标, 转换规则)
3. **形成最终的"交互宪法"**

---

## 附录：形式化表示（伪代码）

```typescript
// 状态枚举
enum State {
  Idle = 'S0',
  Hovered = 'S1',
  Selected = 'S2',
  Editing = 'S3',
  Moving = 'S4',
  Resizing = 'S5',
  Rotating = 'S6',
}

// 事件枚举
enum Event {
  MouseEnter = 'E1',
  MouseLeave = 'E2',
  Click_Body = 'E11',
  DoubleClick_Text = 'E14',
  Drag_Body = 'E15',
  Drag_ResizeHandle = 'E16',
  Press_ESC = 'E20',
  // ... 更多事件
}

// 对象属性
interface ObjectProperties {
  isLocked: boolean;
  isHidden: boolean;
  isGrouped: boolean;
  hasText: boolean;
}

// 状态转换函数
function transition(
  currentState: State,
  event: Event,
  properties: ObjectProperties
): State {
  // 基础转换
  const nextState = transitionTable[currentState][event];

  // 应用约束条件
  if (properties.isLocked) {
    if (event === Event.Drag_Body || event === Event.Drag_ResizeHandle) {
      return currentState; // 锁定对象无法移动/缩放
    }
  }

  if (!properties.hasText) {
    if (event === Event.DoubleClick_Text) {
      return currentState; // 无文本对象无法进入编辑态
    }
  }

  return nextState;
}

// 状态转换表
const transitionTable: Record<State, Partial<Record<Event, State>>> = {
  [State.Idle]: {
    [Event.MouseEnter]: State.Hovered,
    [Event.Click_Body]: State.Selected,
    [Event.DoubleClick_Text]: State.Editing,
  },
  [State.Hovered]: {
    [Event.MouseLeave]: State.Idle,
    [Event.Click_Body]: State.Selected,
    [Event.Drag_Body]: State.Moving,
  },
  [State.Selected]: {
    [Event.Click_Outside]: State.Idle,
    [Event.Press_ESC]: State.Idle,
    [Event.DoubleClick_Text]: State.Editing,
    [Event.Drag_Body]: State.Moving,
    [Event.Drag_ResizeHandle]: State.Resizing,
  },
  [State.Editing]: {
    [Event.Click_Outside]: State.Selected,
    [Event.Press_ESC]: State.Selected,
  },
  [State.Moving]: {
    [Event.MouseUp]: State.Selected,
    [Event.Press_ESC]: State.Selected,
  },
  [State.Resizing]: {
    [Event.MouseUp]: State.Selected,
    [Event.Press_ESC]: State.Selected,
  },
  [State.Rotating]: {
    [Event.MouseUp]: State.Selected,
    [Event.Press_ESC]: State.Selected,
  },
};
```

---

**这个定义是否符合你说的"一动一态"原则？有没有需要调整的地方？**
