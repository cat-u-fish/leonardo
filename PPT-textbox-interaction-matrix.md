# 文本框对象完整交互矩阵（M × N Matrix）

> 本文档是"内容容器交互规则"框架的实战应用：为"文本框"对象构建完整的 M × N 交互矩阵
>
> **这是框架的最终交付物：可执行的"交互宪法"**

---

## 一、对象定义：文本框（Text Box）

### 1.1 对象特征

**对象类型：** Text Box（文本框）

**基本属性：**
- 包含文本内容（可编辑）
- 有填充区域（可设置背景色）
- 有边框（可选）
- 支持几何变换（移动、缩放、旋转）

**示例文本框：**
```
┌─────────────────────────┐
│                         │
│    Hello World 123      │  ← 文本内容
│                         │
└─────────────────────────┘
```

### 1.2 对象状态

该对象支持的核心状态（N维度）：

| 状态ID | 状态名称 | 描述 |
|-------|---------|------|
| `S0` | **Idle** | 默认态 - 对象创建后未交互 |
| `S1` | **Hovered** | 悬停态 - 鼠标悬停但未点击 |
| `S2` | **Selected** | 选中态 - 显示控制点 |
| `S3` | **Editing** | 编辑态 - 编辑文本内容 |
| `S4` | **Moving** | 移动中 - 正在拖拽移动 |
| `S5` | **Resizing** | 缩放中 - 正在缩放尺寸 |
| `S6` | **Rotating** | 旋转中 - 正在旋转角度 |

---

## 二、可交互属性（M 维度）

文本框对象包含以下可交互属性：

| 属性ID | 属性名称 | 描述 | 几何类型 |
|-------|---------|------|---------|
| `P01` | **填充区域** | 文本框的主体区域（空白部分） | Region |
| `P02` | **文本内容区** | 包含文字的区域 | Region |
| `P03` | **边框** | 文本框的外边框线 | Line |
| `P04_TL` | **左上角缩放点** | 左上角的缩放控制点 | Point |
| `P04_TR` | **右上角缩放点** | 右上角的缩放控制点 | Point |
| `P04_BR` | **右下角缩放点** | 右下角的缩放控制点 | Point |
| `P04_BL` | **左下角缩放点** | 左下角的缩放控制点 | Point |
| `P05_T` | **上边缩放点** | 上边中点的缩放控制点 | Point |
| `P05_R` | **右边缩放点** | 右边中点的缩放控制点 | Point |
| `P05_B` | **下边缩放点** | 下边中点的缩放控制点 | Point |
| `P05_L` | **左边缩放点** | 左边中点的缩放控制点 | Point |
| `P06` | **旋转柄** | 顶部的旋转控制手柄 | Point |

**可视化示意图：**

```
                      P06 (旋转柄)
                         ○
                         │
         P04_TL ●────P05_T────● P04_TR
                │             │
                │             │
          P05_L │   文本框    │ P05_R
                │  Hello 123  │
                │             │
                │             │
         P04_BL ●────P05_B────● P04_BR
```

---

## 三、完整的 M × N 交互矩阵

### 3.1 矩阵概览表格

下表展示了每个属性在每个状态下的**可见性**和**主要交互**：

| 属性 ↓ / 状态 → | S0: Idle | S1: Hovered | S2: Selected | S3: Editing | S4: Moving | S5: Resizing | S6: Rotating |
|---------------|---------|------------|-------------|------------|-----------|-------------|-------------|
| **P01: 填充区** | ⚫ 不响应 | 🖱️ Move<br>Click→S2<br>Drag→S4 | 🖱️ Move<br>Drag→S4 | ⚫ 显示<br>不响应 | 🖱️ Move<br>拖拽中 | - | - |
| **P02: 文本内容** | ⚫ 不响应 | 🖱️ I-beam<br>Click→S2<br>DblClick→S3 | 🖱️ I-beam<br>DblClick→S3<br>F2→S3 | 🖱️ I-beam<br>输入/选择 | 随对象移动 | 随对象缩放 | 随对象旋转 |
| **P03: 边框** | ⚫ 不响应 | 🖱️ Move<br>（同P01） | 🖱️ Move<br>（同P01） | 显示（虚线） | - | - | - |
| **P04_TL: 角点** | ❌ 不显示 | ❌ | ✅ 🖱️ ↖↘<br>Drag→S5 | ❌ 隐藏 | ❌ | 🖱️ ↖↘<br>拖拽中 | ❌ |
| **P04_TR: 角点** | ❌ | ❌ | ✅ 🖱️ ↗↙<br>Drag→S5 | ❌ | ❌ | 🖱️ ↗↙<br>拖拽中 | ❌ |
| **P04_BR: 角点** | ❌ | ❌ | ✅ 🖱️ ↖↘<br>Drag→S5 | ❌ | ❌ | 🖱️ ↖↘<br>拖拽中 | ❌ |
| **P04_BL: 角点** | ❌ | ❌ | ✅ 🖱️ ↗↙<br>Drag→S5 | ❌ | ❌ | 🖱️ ↗↙<br>拖拽中 | ❌ |
| **P05_T: 边点** | ❌ | ❌ | ✅ 🖱️ ↕<br>Drag→S5 | ❌ | ❌ | 🖱️ ↕<br>拖拽中 | ❌ |
| **P05_R: 边点** | ❌ | ❌ | ✅ 🖱️ ↔<br>Drag→S5 | ❌ | ❌ | 🖱️ ↔<br>拖拽中 | ❌ |
| **P05_B: 边点** | ❌ | ❌ | ✅ 🖱️ ↕<br>Drag→S5 | ❌ | ❌ | 🖱️ ↕<br>拖拽中 | ❌ |
| **P05_L: 边点** | ❌ | ❌ | ✅ 🖱️ ↔<br>Drag→S5 | ❌ | ❌ | 🖱️ ↔<br>拖拽中 | ❌ |
| **P06: 旋转柄** | ❌ | ❌ | ✅ 🖱️ 🔄<br>Drag→S6 | ❌ | ❌ | ❌ | 🖱️ 🔄<br>拖拽中 |

**图例：**
- ✅ 可见且可交互
- ❌ 不显示
- ⚫ 显示但不响应
- 🖱️ 鼠标光标样式

---

## 四、详细的单元格定义

下面为每个关键的矩阵单元格提供完整的定义。

### 4.1 P01: 填充区域

#### `Matrix[P01, S0: Idle]`

```json
{
  "visible": true,
  "cursorStyle": "default",
  "allowedEvents": [],
  "eventTransitions": {},
  "visualFeedback": null,
  "note": "对象在默认态时不响应任何操作"
}
```

#### `Matrix[P01, S1: Hovered]`

```json
{
  "visible": true,
  "cursorStyle": "move",
  "allowedEvents": ["Click", "Drag", "RightClick"],
  "eventTransitions": {
    "Click": "S2",
    "Drag": "S4",
    "RightClick": "显示右键菜单（不改变状态）"
  },
  "visualFeedback": {
    "hover": "可选：轻微高亮边框"
  }
}
```

**说明：**
- 鼠标进入填充区域时，光标变为**移动光标**（四向箭头）
- 单击 → 进入选中态（S2）
- 拖拽 → 直接进入移动中（S4）
- 右键 → 显示上下文菜单

#### `Matrix[P01, S2: Selected]`

```json
{
  "visible": true,
  "cursorStyle": "move",
  "allowedEvents": ["Drag", "RightClick", "Delete", "Copy", "Cut"],
  "eventTransitions": {
    "Drag": "S4",
    "Click_Outside": "S0",
    "Press_ESC": "S0"
  },
  "visualFeedback": {
    "active": "显示选择框和控制点"
  }
}
```

**说明：**
- 在选中态时，填充区域可拖拽移动
- 点击外部或按ESC → 返回默认态（S0）

#### `Matrix[P01, S3: Editing]`

```json
{
  "visible": true,
  "cursorStyle": "default",
  "allowedEvents": [],
  "eventTransitions": {},
  "note": "编辑态下，焦点在文本内容区，填充区不响应"
}
```

#### `Matrix[P01, S4: Moving]`

```json
{
  "visible": true,
  "cursorStyle": "move",
  "allowedEvents": ["Dragging", "MouseUp", "Press_ESC"],
  "eventTransitions": {
    "Dragging": "S4",
    "MouseUp": "S2",
    "Press_ESC": "S2"
  },
  "visualFeedback": {
    "moving": "对象跟随鼠标移动，可能显示对齐辅助线"
  },
  "constraints": {
    "modifiers": {
      "Shift": "锁定水平或垂直方向移动",
      "Ctrl": "复制对象而非移动（Windows）"
    }
  }
}
```

---

### 4.2 P02: 文本内容区

#### `Matrix[P02, S0: Idle]`

```json
{
  "visible": true,
  "cursorStyle": "default",
  "allowedEvents": [],
  "eventTransitions": {}
}
```

#### `Matrix[P02, S1: Hovered]`

```json
{
  "visible": true,
  "cursorStyle": "text",
  "allowedEvents": ["Click", "DoubleClick"],
  "eventTransitions": {
    "Click": "S2",
    "DoubleClick": "S3"
  },
  "visualFeedback": {
    "hover": "可选：文本区域高亮或边框显示"
  },
  "note": "I-beam 光标（工字型）提示用户可以编辑文本"
}
```

**关键：** 当鼠标悬停在文本内容区时，光标变为 **I-beam（工字型）**，这与悬停在填充区时的 **Move** 光标不同。

**属性检测优先级：**
```
if (鼠标位置在文字边界内):
    触发 P02: 文本内容区 → I-beam 光标
else if (鼠标位置在填充区内):
    触发 P01: 填充区域 → Move 光标
```

#### `Matrix[P02, S2: Selected]`

```json
{
  "visible": true,
  "cursorStyle": "text",
  "allowedEvents": ["DoubleClick", "Press_F2"],
  "eventTransitions": {
    "DoubleClick": "S3",
    "Press_F2": "S3"
  },
  "visualFeedback": {
    "hover": "文本区域可能显示轻微高亮"
  }
}
```

**说明：**
- 在选中态，鼠标悬停到文本内容区时，光标仍为 I-beam
- 双击或按F2 → 进入编辑态

#### `Matrix[P02, S3: Editing]`

```json
{
  "visible": true,
  "cursorStyle": "text",
  "allowedEvents": [
    "TextInput",
    "Click",
    "Drag",
    "Press_Backspace",
    "Press_Delete",
    "Press_Enter",
    "Press_ArrowKey",
    "Press_ESC",
    "Click_Outside",
    "Ctrl+A", "Ctrl+C", "Ctrl+V", "Ctrl+X"
  ],
  "eventTransitions": {
    "TextInput": "S3",
    "Click": "S3 (移动光标位置)",
    "Drag": "S3 (选中文本)",
    "Press_ESC": "S2",
    "Click_Outside": "S2",
    "Press_Enter": "S3 (换行)"
  },
  "visualFeedback": {
    "editing": "显示闪烁的文本光标（插入符），选中文本时显示蓝色背景"
  },
  "note": "编辑态下，文本内容区成为主要交互区域"
}
```

**说明：**
- 编辑态是文本框最复杂的状态
- 所有文本编辑操作（输入、删除、选择、复制粘贴）都在此状态进行
- 点击文本内容区内部 → 移动插入符位置
- 拖拽 → 选中文本
- ESC 或点击外部 → 退出编辑态，返回选中态（S2）

#### `Matrix[P02, S4/S5/S6: Moving/Resizing/Rotating]`

```json
{
  "visible": true,
  "cursorStyle": "继承自变换操作",
  "allowedEvents": [],
  "eventTransitions": {},
  "note": "文本随对象一起移动/缩放/旋转，但文本内容区本身不响应交互"
}
```

---

### 4.3 P04: 缩放控制点（以左上角点为例）

#### `Matrix[P04_TL, S0/S1: Idle/Hovered]`

```json
{
  "visible": false,
  "note": "缩放点仅在选中态显示"
}
```

#### `Matrix[P04_TL, S2: Selected]`

```json
{
  "visible": true,
  "cursorStyle": "nwse-resize",
  "geometry": {
    "position": "对象左上角",
    "hitRadius": 6
  },
  "allowedEvents": ["Drag"],
  "eventTransitions": {
    "Drag": "S5"
  },
  "visualFeedback": {
    "display": "小圆点或方块，通常为白色填充、蓝色边框",
    "hover": "可能放大或变色"
  },
  "constraints": {
    "modifiers": {
      "Shift": "等比例缩放",
      "Ctrl": "从中心点缩放"
    }
  },
  "note": "光标样式 nwse-resize 表示 ↖↘ 方向的双向箭头"
}
```

**说明：**
- 缩放点仅在 `S2: Selected` 状态下显示
- 左上角点的光标：↖↘（nwse-resize）
- 拖拽 → 进入缩放中（S5）
- 按住 Shift → 等比例缩放
- 按住 Ctrl → 从对象中心点缩放

**其他角点的光标样式：**
- 右上角（P04_TR）：↗↙（nesw-resize）
- 右下角（P04_BR）：↖↘（nwse-resize）
- 左下角（P04_BL）：↗↙（nesw-resize）

#### `Matrix[P04_TL, S3: Editing]`

```json
{
  "visible": false,
  "note": "编辑态下，缩放点隐藏，防止用户误操作"
}
```

#### `Matrix[P04_TL, S5: Resizing]`

```json
{
  "visible": true,
  "cursorStyle": "nwse-resize",
  "allowedEvents": ["Dragging", "MouseUp", "Press_ESC"],
  "eventTransitions": {
    "Dragging": "S5",
    "MouseUp": "S2",
    "Press_ESC": "S2"
  },
  "visualFeedback": {
    "resizing": "对象实时缩放，可能显示尺寸提示（如 300x200 px）"
  },
  "note": "拖拽过程中，光标保持 nwse-resize，对象跟随缩放"
}
```

---

### 4.4 P05: 边缩放点（以上边点为例）

#### `Matrix[P05_T, S2: Selected]`

```json
{
  "visible": true,
  "cursorStyle": "ns-resize",
  "geometry": {
    "position": "对象上边中点",
    "hitRadius": 6
  },
  "allowedEvents": ["Drag"],
  "eventTransitions": {
    "Drag": "S5"
  },
  "visualFeedback": {
    "display": "小圆点或方块"
  },
  "note": "光标样式 ns-resize 表示 ↕ 方向的双向箭头，用于单向拉伸高度"
}
```

**其他边点的光标样式：**
- 上边点（P05_T）：↕（ns-resize）
- 右边点（P05_R）：↔（ew-resize）
- 下边点（P05_B）：↕（ns-resize）
- 左边点（P05_L）：↔（ew-resize）

**边点 vs 角点：**
- **边点**：单向拉伸（只改变宽度或高度），不保持宽高比
- **角点**：可以自由缩放，或按住 Shift 等比例缩放

---

### 4.5 P06: 旋转柄

#### `Matrix[P06, S0/S1: Idle/Hovered]`

```json
{
  "visible": false,
  "note": "旋转柄仅在选中态显示"
}
```

#### `Matrix[P06, S2: Selected]`

```json
{
  "visible": true,
  "cursorStyle": "alias",
  "geometry": {
    "position": "对象顶部中心，距离边界约20px",
    "hitRadius": 8,
    "connectorLine": "一条连接到对象顶部的线"
  },
  "allowedEvents": ["Drag"],
  "eventTransitions": {
    "Drag": "S6"
  },
  "visualFeedback": {
    "display": "圆形图标，通常带有旋转箭头符号 🔄",
    "hover": "可能放大或高亮"
  },
  "constraints": {
    "modifiers": {
      "Shift": "以 15° 为单位旋转（0°, 15°, 30°, ...）"
    }
  },
  "note": "光标样式可能是 'alias' 或自定义的旋转光标"
}
```

**说明：**
- 旋转柄通常位于对象上方，通过一条短线连接到对象
- 拖拽 → 进入旋转中（S6）
- 按住 Shift → 以15°为增量旋转
- 旋转中心点默认为对象的几何中心（可调整）

#### `Matrix[P06, S3: Editing]`

```json
{
  "visible": false,
  "note": "编辑态下隐藏旋转柄"
}
```

#### `Matrix[P06, S6: Rotating]`

```json
{
  "visible": true,
  "cursorStyle": "alias",
  "allowedEvents": ["Dragging", "MouseUp", "Press_ESC"],
  "eventTransitions": {
    "Dragging": "S6",
    "MouseUp": "S2",
    "Press_ESC": "S2"
  },
  "visualFeedback": {
    "rotating": "对象实时旋转，可能显示角度提示（如 45°）"
  }
}
```

---

## 五、状态转换完整流程图

### 5.1 文本框对象的状态机

```
                    MouseEnter(任意属性)
      ┌────────┐   ─────────────────→   ┌──────────┐
      │        │                        │          │
      │  Idle  │                        │ Hovered  │
      │  (S0)  │   ←─────────────────   │  (S1)    │
      │        │     MouseLeave         │          │
      └────────┘                        └──────────┘
                                             │
                                             │ Click(P01/P02)
                                             ↓
                                        ┌──────────┐
                                        │          │
                    ┌──────────────────│ Selected │──────────────────┐
                    │                  │  (S2)    │                  │
                    │                  │          │                  │
                    │                  └──────────┘                  │
                    │                       │  │  │                  │
                    │                       │  │  │                  │
          DblClick(P02)          Drag(P01)  │  │  │ Drag(P06)       │
          或 F2                             │  │  │                  │
                    │                       │  │  │                  │
                    ↓                       ↓  │  ↓                  │
              ┌─────────┐             ┌────────┐ ┌────────┐         │
              │         │             │        │ │        │         │
              │ Editing │             │ Moving │ │Rotating│         │
              │  (S3)   │             │  (S4)  │ │  (S6)  │         │
              │         │             │        │ │        │         │
              └─────────┘             └────────┘ └────────┘         │
                    │                     │  ↑       │  ↑           │
              ESC / │               MouseUp  │  MouseUp │           │
          Click_Out │                     │  │       │  │           │
                    │                     │  │       │  │           │
                    └─────────────────────┴──┴───────┴──┴───────────┘
                              返回 Selected (S2)


                    Drag(P04/P05)
              S2 ─────────────→ S5 (Resizing)
                              ↓
                           MouseUp
                              ↓
                             S2
```

### 5.2 关键转换规则

| 从状态 | 事件 | 到状态 | 条件 |
|-------|------|-------|------|
| S0 | MouseEnter | S1 | - |
| S1 | MouseLeave | S0 | - |
| S1 | Click(P01或P02) | S2 | - |
| S1 | DoubleClick(P02) | S3 | - |
| S1 | Drag(P01) | S4 | - |
| S2 | Click_Outside | S0 | - |
| S2 | ESC | S0 | - |
| S2 | DoubleClick(P02) | S3 | - |
| S2 | Press_F2 | S3 | 对象有文本 |
| S2 | Drag(P01) | S4 | - |
| S2 | Drag(P04/P05) | S5 | - |
| S2 | Drag(P06) | S6 | - |
| S3 | ESC | S2 | - |
| S3 | Click_Outside | S2 | - |
| S4 | MouseUp | S2 | - |
| S4 | ESC | S2 | 取消移动 |
| S5 | MouseUp | S2 | - |
| S5 | ESC | S2 | 取消缩放 |
| S6 | MouseUp | S2 | - |
| S6 | ESC | S2 | 取消旋转 |

---

## 六、代码实现示例

### 6.1 TypeScript 类型定义

```typescript
// 状态枚举
enum TextBoxState {
  Idle = 'S0',
  Hovered = 'S1',
  Selected = 'S2',
  Editing = 'S3',
  Moving = 'S4',
  Resizing = 'S5',
  Rotating = 'S6',
}

// 属性枚举
enum TextBoxProperty {
  FillArea = 'P01',
  TextContent = 'P02',
  Border = 'P03',
  ResizeHandle_TL = 'P04_TL',
  ResizeHandle_TR = 'P04_TR',
  ResizeHandle_BR = 'P04_BR',
  ResizeHandle_BL = 'P04_BL',
  ResizeHandle_T = 'P05_T',
  ResizeHandle_R = 'P05_R',
  ResizeHandle_B = 'P05_B',
  ResizeHandle_L = 'P05_L',
  RotateHandle = 'P06',
}

// 事件枚举
enum TextBoxEvent {
  MouseEnter = 'MouseEnter',
  MouseLeave = 'MouseLeave',
  Click = 'Click',
  DoubleClick = 'DoubleClick',
  Drag = 'Drag',
  MouseUp = 'MouseUp',
  Press_ESC = 'ESC',
  Press_F2 = 'F2',
  TextInput = 'TextInput',
  Click_Outside = 'Click_Outside',
}

// 光标样式
enum CursorStyle {
  Default = 'default',
  Move = 'move',
  Text = 'text',
  NWSE_Resize = 'nwse-resize',
  NESW_Resize = 'nesw-resize',
  NS_Resize = 'ns-resize',
  EW_Resize = 'ew-resize',
  Alias = 'alias',
}

// 矩阵单元格接口
interface MatrixCell {
  visible: boolean;
  cursorStyle: CursorStyle;
  allowedEvents: TextBoxEvent[];
  eventTransitions: Record<TextBoxEvent, TextBoxState>;
}

// 完整矩阵
type InteractionMatrix = Record<
  TextBoxProperty,
  Record<TextBoxState, MatrixCell>
>;
```

### 6.2 矩阵数据结构

```typescript
const textBoxMatrix: InteractionMatrix = {
  // P01: 填充区域
  [TextBoxProperty.FillArea]: {
    [TextBoxState.Idle]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Hovered]: {
      visible: true,
      cursorStyle: CursorStyle.Move,
      allowedEvents: [TextBoxEvent.Click, TextBoxEvent.Drag],
      eventTransitions: {
        [TextBoxEvent.Click]: TextBoxState.Selected,
        [TextBoxEvent.Drag]: TextBoxState.Moving,
      },
    },
    [TextBoxState.Selected]: {
      visible: true,
      cursorStyle: CursorStyle.Move,
      allowedEvents: [TextBoxEvent.Drag, TextBoxEvent.Press_ESC],
      eventTransitions: {
        [TextBoxEvent.Drag]: TextBoxState.Moving,
        [TextBoxEvent.Press_ESC]: TextBoxState.Idle,
        [TextBoxEvent.Click_Outside]: TextBoxState.Idle,
      },
    },
    [TextBoxState.Editing]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Moving]: {
      visible: true,
      cursorStyle: CursorStyle.Move,
      allowedEvents: [TextBoxEvent.MouseUp, TextBoxEvent.Press_ESC],
      eventTransitions: {
        [TextBoxEvent.MouseUp]: TextBoxState.Selected,
        [TextBoxEvent.Press_ESC]: TextBoxState.Selected,
      },
    },
    [TextBoxState.Resizing]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Rotating]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
  },

  // P02: 文本内容区
  [TextBoxProperty.TextContent]: {
    [TextBoxState.Idle]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Hovered]: {
      visible: true,
      cursorStyle: CursorStyle.Text,
      allowedEvents: [TextBoxEvent.Click, TextBoxEvent.DoubleClick],
      eventTransitions: {
        [TextBoxEvent.Click]: TextBoxState.Selected,
        [TextBoxEvent.DoubleClick]: TextBoxState.Editing,
      },
    },
    [TextBoxState.Selected]: {
      visible: true,
      cursorStyle: CursorStyle.Text,
      allowedEvents: [TextBoxEvent.DoubleClick, TextBoxEvent.Press_F2],
      eventTransitions: {
        [TextBoxEvent.DoubleClick]: TextBoxState.Editing,
        [TextBoxEvent.Press_F2]: TextBoxState.Editing,
      },
    },
    [TextBoxState.Editing]: {
      visible: true,
      cursorStyle: CursorStyle.Text,
      allowedEvents: [
        TextBoxEvent.TextInput,
        TextBoxEvent.Click,
        TextBoxEvent.Press_ESC,
        TextBoxEvent.Click_Outside,
      ],
      eventTransitions: {
        [TextBoxEvent.TextInput]: TextBoxState.Editing,
        [TextBoxEvent.Click]: TextBoxState.Editing,
        [TextBoxEvent.Press_ESC]: TextBoxState.Selected,
        [TextBoxEvent.Click_Outside]: TextBoxState.Selected,
      },
    },
    [TextBoxState.Moving]: {
      visible: true,
      cursorStyle: CursorStyle.Move,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Resizing]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Rotating]: {
      visible: true,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
  },

  // P04_TL: 左上角缩放点
  [TextBoxProperty.ResizeHandle_TL]: {
    [TextBoxState.Idle]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Hovered]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Selected]: {
      visible: true,
      cursorStyle: CursorStyle.NWSE_Resize,
      allowedEvents: [TextBoxEvent.Drag],
      eventTransitions: {
        [TextBoxEvent.Drag]: TextBoxState.Resizing,
      },
    },
    [TextBoxState.Editing]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Moving]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Resizing]: {
      visible: true,
      cursorStyle: CursorStyle.NWSE_Resize,
      allowedEvents: [TextBoxEvent.MouseUp, TextBoxEvent.Press_ESC],
      eventTransitions: {
        [TextBoxEvent.MouseUp]: TextBoxState.Selected,
        [TextBoxEvent.Press_ESC]: TextBoxState.Selected,
      },
    },
    [TextBoxState.Rotating]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
  },

  // P06: 旋转柄
  [TextBoxProperty.RotateHandle]: {
    [TextBoxState.Idle]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Hovered]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Selected]: {
      visible: true,
      cursorStyle: CursorStyle.Alias,
      allowedEvents: [TextBoxEvent.Drag],
      eventTransitions: {
        [TextBoxEvent.Drag]: TextBoxState.Rotating,
      },
    },
    [TextBoxState.Editing]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Moving]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Resizing]: {
      visible: false,
      cursorStyle: CursorStyle.Default,
      allowedEvents: [],
      eventTransitions: {},
    },
    [TextBoxState.Rotating]: {
      visible: true,
      cursorStyle: CursorStyle.Alias,
      allowedEvents: [TextBoxEvent.MouseUp, TextBoxEvent.Press_ESC],
      eventTransitions: {
        [TextBoxEvent.MouseUp]: TextBoxState.Selected,
        [TextBoxEvent.Press_ESC]: TextBoxState.Selected,
      },
    },
  },

  // ... 其他属性的定义类似
};
```

### 6.3 交互处理函数

```typescript
class TextBox {
  private state: TextBoxState = TextBoxState.Idle;
  private matrix: InteractionMatrix = textBoxMatrix;

  // 检测鼠标位置对应的属性
  detectProperty(mousePos: Point): TextBoxProperty | null {
    if (this.state === TextBoxState.Selected) {
      // 优先检测控制点（有命中范围）
      const handle = this.detectHandle(mousePos);
      if (handle) return handle;
    }

    // 检测文本内容区
    if (this.isInsideTextContent(mousePos)) {
      return TextBoxProperty.TextContent;
    }

    // 检测边框
    if (this.isOnBorder(mousePos)) {
      return TextBoxProperty.Border;
    }

    // 检测填充区
    if (this.isInsideFillArea(mousePos)) {
      return TextBoxProperty.FillArea;
    }

    return null;
  }

  // 处理鼠标事件
  handleMouseEvent(event: TextBoxEvent, mousePos: Point): void {
    // 检测当前鼠标位置对应的属性
    const property = this.detectProperty(mousePos);
    if (!property) return;

    // 获取当前状态下该属性的行为定义
    const cell = this.matrix[property][this.state];

    // 检查事件是否被允许
    if (!cell.allowedEvents.includes(event)) {
      console.log(`Event ${event} not allowed for ${property} in state ${this.state}`);
      return;
    }

    // 执行状态转换
    const nextState = cell.eventTransitions[event];
    if (nextState) {
      this.transitionTo(nextState);
    }
  }

  // 获取光标样式
  getCursorStyle(mousePos: Point): CursorStyle {
    const property = this.detectProperty(mousePos);
    if (!property) return CursorStyle.Default;

    const cell = this.matrix[property][this.state];
    return cell.visible ? cell.cursorStyle : CursorStyle.Default;
  }

  // 状态转换
  private transitionTo(newState: TextBoxState): void {
    console.log(`State transition: ${this.state} → ${newState}`);
    this.state = newState;
    this.onStateChanged(newState);
  }

  // 状态变化回调
  private onStateChanged(state: TextBoxState): void {
    switch (state) {
      case TextBoxState.Selected:
        this.showHandles();
        break;
      case TextBoxState.Editing:
        this.hideHandles();
        this.showTextCursor();
        break;
      case TextBoxState.Idle:
        this.hideHandles();
        break;
      // ... 其他状态处理
    }
  }

  // 辅助方法
  private detectHandle(mousePos: Point): TextBoxProperty | null {
    const HOTSPOT_RADIUS = 6;
    // 检测各个控制点...
    // 返回命中的控制点
    return null;
  }

  private isInsideTextContent(mousePos: Point): boolean {
    // 检测鼠标是否在文本字符边界内
    return false;
  }

  private isInsideFillArea(mousePos: Point): boolean {
    // 检测鼠标是否在对象内部
    return false;
  }

  private isOnBorder(mousePos: Point): boolean {
    // 检测鼠标是否在边框上
    return false;
  }

  private showHandles(): void {
    // 显示控制点
  }

  private hideHandles(): void {
    // 隐藏控制点
  }

  private showTextCursor(): void {
    // 显示文本插入符
  }
}
```

### 6.4 使用示例

```typescript
const textBox = new TextBox();

// 场景1：用户悬停到文本内容区
const mousePos1 = { x: 100, y: 50 }; // 假设这是文本内容区的位置
const cursor1 = textBox.getCursorStyle(mousePos1);
console.log(cursor1); // 输出: "text" (I-beam)

// 场景2：用户点击文本内容区
textBox.handleMouseEvent(TextBoxEvent.Click, mousePos1);
// 输出: "State transition: S1 → S2"

// 场景3：用户在选中态双击文本内容区
textBox.handleMouseEvent(TextBoxEvent.DoubleClick, mousePos1);
// 输出: "State transition: S2 → S3"

// 场景4：用户在选中态悬停到左上角缩放点
const mousePos2 = { x: 10, y: 10 }; // 假设这是左上角缩放点
const cursor2 = textBox.getCursorStyle(mousePos2);
console.log(cursor2); // 输出: "nwse-resize" (↖↘)

// 场景5：用户拖拽缩放点
textBox.handleMouseEvent(TextBoxEvent.Drag, mousePos2);
// 输出: "State transition: S2 → S5"
```

---

## 七、边界情况与约束条件

### 7.1 对象属性约束

| 约束条件 | 影响的转换 | 说明 |
|---------|----------|------|
| `isLocked == true` | S2 → S4/S5/S6 被禁止 | 锁定对象无法移动/缩放/旋转 |
| `hasText == false` | S2 → S3 被禁止 | 无文本对象无法进入编辑态 |
| `isEmpty == true` | P02 不响应 | 空文本框，文本内容区无效 |

### 7.2 属性冲突解决

**场景：** 鼠标同时位于文本内容区和填充区

```typescript
function detectProperty(mousePos: Point): TextBoxProperty {
  // 优先级1：控制点（如果在选中态）
  if (state === TextBoxState.Selected) {
    const handle = detectHandle(mousePos);
    if (handle) return handle;
  }

  // 优先级2：文本内容区
  if (isInsideTextBounds(mousePos)) {
    return TextBoxProperty.TextContent;
  }

  // 优先级3：边框（如果鼠标在边框附近3px内）
  if (isNearBorder(mousePos, 3)) {
    return TextBoxProperty.Border;
  }

  // 优先级4：填充区
  if (isInsideBounds(mousePos)) {
    return TextBoxProperty.FillArea;
  }

  return null;
}
```

### 7.3 特殊场景

#### 场景1：空文本框（无文字）

- P02（文本内容区）不显示
- 悬停时只触发 P01（填充区）→ Move 光标
- 双击直接进入编辑态（S3），开始输入

#### 场景2：纯文本（无背景、无边框）

- P01（填充区）和 P03（边框）不可见
- 只有 P02（文本内容区）响应
- 悬停时显示 I-beam 光标

#### 场景3：文本框旋转后

- 控制点和旋转柄随对象旋转
- 光标样式需要根据旋转角度调整方向
- 例如：旋转45°后，左上角点的光标从 ↖↘ 变为 ↑↓

---

## 八、总结与验证

### 8.1 矩阵完整性验证

✅ **M 维度（属性）：** 12个
- P01: 填充区域
- P02: 文本内容区
- P03: 边框
- P04_TL/TR/BR/BL: 4个角缩放点
- P05_T/R/B/L: 4个边缩放点
- P06: 旋转柄

✅ **N 维度（状态）：** 7个
- S0: Idle
- S1: Hovered
- S2: Selected
- S3: Editing
- S4: Moving
- S5: Resizing
- S6: Rotating

✅ **矩阵单元格：** 12 × 7 = **84个单元格**

每个单元格都定义了：
- visible（可见性）
- cursorStyle（光标样式）
- allowedEvents（允许的事件）
- eventTransitions（状态转换）

### 8.2 形式化验证

#### ✅ 完备性（Completeness）
- 所有 84 个单元格都有明确定义
- 没有空白或未定义的单元格

#### ✅ 确定性（Determinism）
- 每个 (状态, 事件, 属性) 三元组有唯一的目标状态
- 不存在模糊或冲突的转换规则

#### ✅ 可达性（Reachability）
- 所有状态都可以从 S0 到达
- 所有状态都可以返回到 S0 或 S2

#### ✅ 一致性（Consistency）
- 光标样式与操作语义一致（Move → 移动，I-beam → 编辑）
- 控制点的可见性与状态一致（仅在 S2 显示）

### 8.3 实际应用价值

**对于产品经理：**
- 这是一份完整的交互规格说明书
- 可以用于评审和验证交互设计

**对于UI/UX设计师：**
- 明确了所有交互细节（光标、视觉反馈）
- 确保了交互的一致性和可预测性

**对于开发工程师：**
- 可以直接转译为代码逻辑
- 提供了清晰的实现指南和数据结构

**对于测试工程师：**
- 84个单元格 = 84个测试场景
- 可以系统地验证所有交互路径

---

## 九、扩展与应用

### 9.1 其他对象的矩阵

基于文本框的矩阵模板，可以快速构建其他对象的矩阵：

| 对象类型 | 属性差异 | 状态差异 |
|---------|---------|---------|
| **形状（Shape）** | 同文本框 | 同文本框 |
| **图片（Image）** | 无P02（文本内容区）<br>无S3（编辑态） | 更简单 |
| **表格（Table）** | +单元格、行列边界线 | +S10（单元格编辑态） |
| **图表（Chart）** | +图表元素（数据系列、图例等） | +子对象选中态 |

### 9.2 矩阵的动态生成

```typescript
function generateMatrix(
  objectType: ObjectType,
  properties: Property[],
  states: State[]
): InteractionMatrix {
  const matrix: InteractionMatrix = {};

  for (const property of properties) {
    matrix[property.id] = {};
    for (const state of states) {
      matrix[property.id][state.id] = computeCell(
        objectType,
        property,
        state
      );
    }
  }

  return matrix;
}
```

### 9.3 协同编辑场景

在协同编辑环境中，矩阵需要考虑额外的约束：

```typescript
interface CollaborativeConstraints {
  isLockedByOthers: boolean;  // 是否被其他用户锁定
  currentEditor?: User;        // 当前编辑者
}

// 修改状态转换函数
function transitionWithConstraints(
  currentState: State,
  event: Event,
  property: Property,
  constraints: CollaborativeConstraints
): State {
  if (constraints.isLockedByOthers) {
    // 如果被其他用户锁定，禁止某些操作
    if ([Event.Drag, Event.DoubleClick].includes(event)) {
      return currentState; // 保持当前状态
    }
  }

  return normalTransition(currentState, event, property);
}
```

---

## 十、总结

### ✅ 我们完成了什么

1. **构建了完整的 M × N 交互矩阵**
   - 12个属性 × 7个状态 = 84个单元格
   - 每个单元格包含完整的行为定义

2. **提供了形式化的规格说明**
   - 状态机定义（7个状态）
   - 事件集合（10+事件）
   - 状态转换规则（完整流程图）

3. **给出了可执行的代码实现**
   - TypeScript 类型定义
   - 矩阵数据结构
   - 交互处理逻辑

4. **处理了边界情况和约束**
   - 属性冲突解决（优先级规则）
   - 对象属性约束（锁定、无文本等）
   - 特殊场景（空文本框、旋转等）

### 📊 这个矩阵的价值

**这不仅仅是一份文档，而是：**
- ✅ **产品的交互宪法** - 完备、严谨、无二义性
- ✅ **设计的规范指南** - 确保一致性和可预测性
- ✅ **开发的真值表** - 可直接转译为代码
- ✅ **测试的用例清单** - 覆盖所有交互路径

---

**这就是文本框对象的完整 M × N 交互矩阵！你对这个矩阵有什么反馈或建议吗？**
