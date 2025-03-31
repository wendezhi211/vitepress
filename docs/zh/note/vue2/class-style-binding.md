---
outline: deep
---

## 绑定 HTML Class
### 对象语法
略
### 数组语法
略
### 用在组件上
略

## 绑定内联样式
### 对象语法
略
### 数组语法
v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上：
```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
### 自动添加前缀
略
### 多重值
从 2.3.0 起你可以为 style 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。