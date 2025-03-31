---
outline: deep
---

## 创建一个 Vue 实例
略
## 数据与方法
当一个 Vue 实例被创建时，它将 data 对象中的所有的 property(属性) 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```js
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的 property
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```
值得注意的是只有当实例被创建时就已经存在于 data 中的 property 才是响应式的。

这里唯一的例外是使用 Object.freeze()，这会阻止修改现有的 property，也意味着响应系统无法再追踪变化。

## 实例生命周期钩子
略
## 生命周期图示
略
