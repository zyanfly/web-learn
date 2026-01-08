# CSS 错题本

> 记录回答错误的问题，便于后续复习巩固

---

## Q: margin 塌陷问题

**难度**：⭐⭐
**首次错误日期**：2026-01-08
**错误次数**：1
**复习状态**：❌ 待复习

### 原题

```html
<div class="parent">
  <div class="child"></div>
</div>
```

```css
.parent {
  background: blue;
}
.child {
  margin-top: 50px;
  background: red;
  height: 100px;
}
```

**问：`.parent` 的顶部会和 `.child` 的顶部对齐吗？还是 `.child` 会在 `.parent` 内部向下偏移 50px？**

### 用户错误回答

不会对齐，因为 child 有 margin-top 50px

### 正确答案

会对齐！整个 parent 会向下移动 50px。这是 margin 塌陷现象——当父元素没有 padding-top、border-top 或 BFC 时，子元素的 margin-top 会穿透父元素。

### 关键知识点

- **margin 塌陷**：子元素 margin 穿透父元素
- **解决方案**：给父元素加 padding/border，或触发 BFC（overflow:hidden / display:flow-root）

---

## Q: Grid 布局 fr 单位计算

**难度**：⭐⭐⭐
**首次错误日期**：2026-01-08
**错误次数**：1
**复习状态**：❌ 待复习

### 原题

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 10px;
}
```

**问：`1fr 2fr 1fr` 是什么意思？如果容器宽度是 430px，三列各占多少宽度？**

### 用户错误回答

fr 是等分单位，比例 1:2:1，直接用 430 乘以对应比例

### 正确答案

- 概念正确：1fr + 2fr + 1fr = 4 份，比例 1:2:1
- 计算需要先减去 gap：430 - 10×2 = 410px
- 三列宽度：102.5px、205px、102.5px

### 关键知识点

- `fr` 分配的是**剩余空间**，要先减去固定宽度、gap 等
- 计算公式：(容器宽度 - gap 总宽度 - 固定宽度) ÷ 总份数 × 对应份数

---
