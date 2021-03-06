# Flex 布局

- 设计
- 原理
- 应用

## 设计

**Flex 排版的核心**是 display:flex 和 flex 属性，它们配合使用。具有 display:flex 的元素我们称为**flex 容器**，它的子元素或者盒被称作 **flex 项**。

### 设计思路

flex 项如果有 flex 属性，会根据 flex 方向代替宽 / 高属性，形成“填补剩余尺寸”的特性。

## 原理

1. 轴：Flex 延伸的方向称为**“主轴”**，把跟它垂直的方向称为**“交叉轴”**。这样，flex 项中的 width 和 height 就会称为**交叉轴尺寸**或者**主轴尺寸**。
2. **轴点**：Flex 又支持反向排布，这样，我们又需要抽象出交叉轴起点、交叉轴终点、主轴起点、主轴终
   点，它们可能是 top、left、bottom、right。
3. flex 排版
   1. flex 项分行，有 Flex 属性的 flex 项可以暂且认为主轴尺寸为 0，所以，它可以一定放进当前行。
      1. 不允许换行，则 flex 项在一行，允许换行，flex 项尽可能在同一行，剩余空间不够就换行
      1. 主轴剩余空间 = flex 容器 - 该行 flex 项尺寸
      1. 交叉轴尺寸 = 该行所有 flex 项的最大值
   2. 来计算每个 flex 项主轴尺寸和位置。
      1. 计算 flex 项主轴尺寸
         - 只有单行： 如果主轴尺寸超出容器，**等比缩放**
         - 允许换行：找出本行所有带有 `flex` 属性项，将剩余空间按比例分
      2. 计算 flex 项位置
   3. 来计算每个 flex 项交叉轴尺寸和位置。
      1. 根据 `align-content` 计算每行位置
      2. 根据`align-items` 和 `align-self`计算每个元素在行内的位置

## 应用

### 三大经典布局

1. 垂直居中：思路是创建一个只有一行的 flexbox，然后用 `align-items:center;` 和 `align-content:center;` 来
   保证行位于容器中，元素位于行中。
2. 两列等高：思路是创建一个只有一行的 flexbox，然后用 `stretch` 属性让每个元素高度都等于行高。
3. 自适应宽：一个子元素设置固定宽，另一个设置 flex 属性。
