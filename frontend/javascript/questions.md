# JavaScript 问答记录

> 记录所有已回答的 JavaScript 相关问题

---

## Q1: typeof null 的输出结果

**难度**：⭐⭐
**日期**：2026-01-07

### 原题

```javascript
console.log(typeof null)
```

**问：这行代码的输出结果是什么？为什么会是这个结果？**

### 回答

结果是 Object

### 评价

✅ 正确

### 知识点补充

这是 JS 历史遗留 Bug。早期实现中，值用类型标签+数据表示，object 标签是 000，而 null 是空指针（全 0），所以被错误判断为 object。此 bug 已无法修复，会破坏现有代码。

---

## Q2: var 变量提升问题

**难度**：⭐⭐
**日期**：2026-01-07

### 原题

```javascript
var a = 1
function foo() {
  console.log(a)
  var a = 2
}
foo()
```

**问：控制台会输出什么？为什么？**

### 回答

输出 undefined，因为在 foo 函数内部声明了 a 变量，导致 console 读取到的是提升后的变量 a，这时候还未赋值所以是 undefined

### 评价

✅ 正确（满分回答，准确描述了变量提升机制）

### 知识点补充

var 声明会被提升到函数顶部，但赋值留在原地。若用 let/const，会因暂时性死区(TDZ)直接报 ReferenceError。

---

## Q3: 引用赋值 vs 展开运算符

**难度**：⭐⭐⭐
**日期**：2026-01-07

### 原题

```javascript
const obj1 = { a: 1, b: 2 }
const obj2 = obj1
obj2.a = 10
console.log(obj1.a)

const obj3 = { ...obj1 }
obj3.b = 20
console.log(obj1.b)
```

**问：两个 console.log 分别输出什么？简单说说为什么。**

### 回答

obj1.a 输出 10（引用同一对象），obj1.b 输出 2（展开运算符创建新对象，修改不影响原对象）

### 评价

✅ 正确（核心概念理解到位）

### 知识点补充

用户说的是"解构赋值"，实际应为"展开运算符(Spread)"。另外展开运算符只是浅拷贝，嵌套对象仍共享引用。深拷贝可用 structuredClone() 或 JSON.parse(JSON.stringify())。

---

## Q4: for 循环 + setTimeout 经典闭包问题

**难度**：⭐⭐⭐
**日期**：2026-01-07

### 原题

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i)
  }, 100)
}
```

**问：控制台会输出什么？如果想让它输出 0, 1, 2，有哪些方法可以实现？**

### 回答

输出三个 3。因为 var 是函数作用域，异步执行时循环已结束，i 共享同一份。修复方案：1)用 let 替代 var（块级作用域）2)用 IIFE 创建闭包保存副本

### 评价

✅ 正确（满分回答，问题本质和解决方案都讲透了）

### 知识点补充

还有第三种方案：setTimeout 第三个参数可传值给回调函数，如 `setTimeout((j) => console.log(j), 100, i)`

---

## Q5: Promise 微任务队列执行顺序

**难度**：⭐⭐⭐
**日期**：2026-01-07

### 原题

```javascript
Promise.resolve()
  .then(() => {
    console.log(1)
    return Promise.resolve(2)
  })
  .then((res) => {
    console.log(res)
  })

Promise.resolve()
  .then(() => {
    console.log(3)
  })
  .then(() => {
    console.log(4)
  })
  .then(() => {
    console.log(5)
  })
```

**问：控制台的输出顺序是什么？**

### 回答

1, 3, 4, 5, 2

### 评价

✅ 正确

### 知识点补充

当 then 回调返回 Promise 时，需要额外的微任务来"拆包"。`return Promise.resolve(x)` 比 `return x` 多消耗两个微任务周期，导致后续 then 被延迟执行。

---
