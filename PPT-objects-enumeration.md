# PPT 对象类型完整枚举

> 本文档是"内容容器交互规则"框架的第一步：穷举所有实体对象（Entities）

---

## 一、基础内容容器

### 1.1 文本类
- **文本框** (Text Box)
  - 普通文本框
  - 垂直文本框
- **艺术字** (WordArt)
- **标题占位符** (Title Placeholder)
- **副标题占位符** (Subtitle Placeholder)
- **内容占位符** (Content Placeholder)

### 1.2 形状类 (Shapes)
- **基本形状**
  - 矩形 (Rectangle)
  - 圆角矩形 (Rounded Rectangle)
  - 椭圆/圆形 (Oval/Circle)
  - 三角形 (Triangle)
  - 菱形 (Diamond)
  - 平行四边形 (Parallelogram)
  - 梯形 (Trapezoid)
  - 五边形、六边形等多边形

- **线条与连接线**
  - 直线 (Line)
  - 箭头 (Arrow)
  - 双向箭头 (Double Arrow)
  - 肘形连接线 (Elbow Connector)
  - 曲线连接线 (Curved Connector)
  - 自由曲线 (Curve)
  - 自由绘制形状 (Freeform)

- **箭头形状**
  - 块状箭头 (Block Arrow)
  - 左箭头、右箭头、上箭头、下箭头
  - V形箭头、U形箭头等

- **流程图形状**
  - 流程 (Process)
  - 判断 (Decision)
  - 数据 (Data)
  - 预定义流程 (Predefined Process)
  - 文档 (Document)
  - 多文档 (Multi-Document)
  - 终止 (Terminator)
  - 准备 (Preparation)
  - 手动输入 (Manual Input)
  - 等等...

- **标注形状** (Callouts)
  - 矩形标注
  - 圆形标注
  - 云形标注
  - 线形标注

- **星形与旗帜**
  - 星形 (Star)
  - 爆炸形 (Explosion)
  - 缎带 (Ribbon)
  - 卷轴 (Scroll)
  - 旗帜 (Banner)

### 1.3 图片与媒体
- **图片** (Picture/Image)
  - 插入的图片文件 (JPG, PNG, GIF等)
  - 剪贴板粘贴的图片
  - 截图
- **图标** (Icons) - Office 365功能
- **3D模型** (3D Models) - Office 365功能
- **矢量图形** (SVG)
- **GIF动图**

### 1.4 媒体对象
- **视频** (Video)
  - 本地视频文件
  - 在线视频（YouTube等）
  - 录屏
- **音频** (Audio)
  - 音频文件
  - 录音

---

## 二、数据与图表类

### 2.1 表格
- **表格** (Table)
  - 单元格 (Cell)
  - 行 (Row)
  - 列 (Column)

### 2.2 图表
- **图表** (Chart)
  - 柱形图 (Column Chart)
  - 条形图 (Bar Chart)
  - 折线图 (Line Chart)
  - 饼图 (Pie Chart)
  - 散点图 (Scatter Chart)
  - 面积图 (Area Chart)
  - 组合图 (Combo Chart)
  - 等等...

### 2.3 智能图形
- **SmartArt**
  - 列表 (List)
  - 流程 (Process)
  - 循环 (Cycle)
  - 层次结构 (Hierarchy)
  - 关系 (Relationship)
  - 矩阵 (Matrix)
  - 棱锥图 (Pyramid)

---

## 三、嵌入与链接对象

### 3.1 Office 嵌入对象
- **Excel 工作表/图表嵌入**
- **Word 文档嵌入**
- **Visio 图表嵌入**

### 3.2 其他嵌入对象
- **PDF 嵌入**
- **OLE 对象** (Object Linking and Embedding)
  - 任意支持 OLE 的程序对象

### 3.3 Web 对象
- **屏幕录制** (Screen Recording)
- **在线图片**
- **股票信息** (Stock) - Office 365
- **地理位置** (Geography) - Office 365

---

## 四、交互与特殊对象

### 4.1 交互元素
- **超链接** (Hyperlink)
- **动作按钮** (Action Button)
  - 自定义按钮
  - 前进、后退、开始、结束等预设按钮

### 4.2 公式与符号
- **公式** (Equation)
- **符号** (Symbol)

### 4.3 墨迹与绘图
- **墨迹/涂鸦** (Ink Drawing) - 触屏/手写笔
- **荧光笔标注**
- **激光笔效果**

---

## 五、容器与组织类

### 5.1 组合对象
- **组合** (Group)
  - 多个对象的组合
  - 嵌套组合

### 5.2 占位符系统
- **图片占位符** (Picture Placeholder)
- **图表占位符** (Chart Placeholder)
- **表格占位符** (Table Placeholder)
- **SmartArt占位符**
- **媒体占位符** (Media Placeholder)
- **在线图片占位符**

### 5.3 节与容器
- **节** (Section) - 幻灯片的分组
- **缩放定位** (Zoom) - Office 365
  - 幻灯片缩放
  - 节缩放
  - 摘要缩放

---

## 六、页面级元素

### 6.1 幻灯片元素
- **背景** (Background)
  - 纯色背景
  - 渐变背景
  - 图片背景
  - 纹理背景

### 6.2 页眉页脚
- **日期和时间** (Date & Time)
- **幻灯片编号** (Slide Number)
- **页脚文本** (Footer)

### 6.3 母版相关
- **母版占位符** (Master Placeholder)
- **版式占位符** (Layout Placeholder)

---

## 七、特殊对象

### 7.1 批注与协作
- **批注** (Comments)
- **批注回复**
- **墨迹批注**

### 7.2 文档级对象
- **备注** (Notes/Speaker Notes)
- **讲义母版元素**
- **备注母版元素**

---

## 八、动画与过渡（非实体对象，但影响交互）

虽然动画和过渡不是"内容容器"，但它们会影响对象的交互状态：
- **进入动画**
- **退出动画**
- **强调动画**
- **路径动画**
- **幻灯片过渡效果**

---

## 九、对象属性的共性

所有上述对象通常都具有以下共同属性维度：
- **几何属性**: 位置(X,Y)、大小(W,H)、旋转角度、层级(Z-order)
- **视觉属性**: 填充、边框、阴影、反射、发光、柔化边缘、3D效果
- **文本属性**: 字体、大小、颜色、对齐、行距等（如果包含文本）
- **交互属性**: 超链接、动作设置
- **动画属性**: 动画效果、触发器
- **可见性**: 显示/隐藏
- **锁定状态**: 锁定/解锁
- **组合状态**: 是否属于某个组

---

## 十、对象分类视角总结

可以从不同维度对对象进行分类：

### 按内容类型
- 文本类、图形类、媒体类、数据类

### 按创建方式
- 直接插入、占位符、母版继承、嵌入对象

### 按可编辑性
- 矢量对象（可编辑形状）、位图对象（图片）、嵌入对象

### 按层次结构
- 原子对象、容器对象（组合、表格、SmartArt）

### 按交互复杂度
- 简单对象（图片）、中等对象（形状）、复杂对象（表格、SmartArt）

---

**下一步建议**：
1. 选择核心对象类型进行深入分析（如：文本框、形状、图片、表格）
2. 为选定对象枚举"可交互属性"（M维度）
3. 定义完整的"状态机"（N维度）
4. 构建 M × N 交互矩阵
