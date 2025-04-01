---
outline: deep
---

## 基础用法
略
### 文本
略
### 多行文本
略
### 复选框
略
### 单选按钮
略
### 选择框
略
## 值绑定
略
### 复选框
略
### 单选按钮
略
### 选择框的选项
略
## 修饰符
### .lazy
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转为在 change 事件之后进行同步：
```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```
### .number
如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：
```html
<input v-model.number="age" type="number">
```
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。
### .trim
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：
```html
<input v-model.trim="msg">
```
## 在组件上使用 v-model
HTML 原生的输入元素类型并不总能满足需求。幸好，Vue 的组件系统允许你创建具有完全自定义行为且可复用的输入组件。这些输入组件甚至可以和 v-model 一起使用！

> 要了解更多，请参阅组件指南中的自定义输入组件。