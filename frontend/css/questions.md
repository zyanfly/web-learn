# CSS 问答记录

> 记录所有已回答的 CSS 相关问题

---

## Q1: box-sizing 盒模型计算

**难度**：⭐⭐
**日期**：2026-01-08

### 原题

```css
.box {
  width: 100px;
  padding: 20px;
  border: 5px solid black;
  box-sizing: content-box;
}
```

**问：这个 `.box` 元素实际占据的总宽度是多少像素？改成 `border-box` 又是多少？**

### 回答

实际占据 150px，改成 border-box 就是 100px。content-box 是元素自身宽度不包含 padding/border，border-box 将 width 作为整个盒模型的宽度。

### 评价

✅ 正确（满分回答，概念解释清晰）

### 知识点补充

实际开发中推荐全局设置 `* { box-sizing: border-box; }`，这样设置 width 就是最终宽度，不用手动计算。

---

## Q2: Flexbox 主轴与交叉轴

**难度**：⭐⭐⭐
**日期**：2026-01-08

### 原题

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

**问：`justify-content` 和 `align-items` 分别控制哪个方向？`flex-direction: column` 时会怎样？**

### 回答

justify-content 控制水平方向（x 轴），align-items 控制垂直方向（y 轴）。flex-direction 改为 column 后两者反过来。

### 评价

✅ 正确（结论正确，建议用主轴/交叉轴描述更专业）

### 知识点补充

- `justify-content` 始终控制**主轴**，`align-items` 始终控制**交叉轴**
- `flex-direction: row` 时主轴水平；`column` 时主轴垂直
- 用主轴/交叉轴理解，逻辑始终一致

---

## Q3: z-index 层叠顺序

**难度**：⭐⭐⭐
**日期**：2026-01-08

### 原题

```html
<div class="a">A</div>
<div class="b">B</div>
<div class="c">C</div>
```

```css
.a {
  position: relative;
  z-index: 1;
}
.b {
  position: absolute;
  z-index: 999;
}
.c {
  position: relative;
  z-index: 2;
}
```

**问：从视觉上看，A、B、C 三个元素的层叠顺序是怎样的（从下到上）？**

### 回答

A 和 C 是同一层级，B 在他们上面

### 评价

⚠️ 部分正确（B 在最上面正确，但没说明 A 和 C 之间 C 在 A 上面）

### 知识点补充

- 同级兄弟元素直接比较 z-index：A(1) → C(2) → B(999)
- 只有在**嵌套层叠上下文**时，子元素 z-index 再大也会被限制在父元素的层叠上下文内
- 触发层叠上下文的方式：position+z-index、opacity<1、transform、filter 等

---
