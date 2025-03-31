---
outline: deep
---

## 用 v-for 把一个数组对应为一组元素
略
## 在 v-for 里使用对象
你也可以用 v-for 来遍历一个对象的 property。
```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```
```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```
结果：
![1](/1.png)

你也可以提供第二个的参数为 property 名称 (也就是键名)：

<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>

![2](/2.png)

还可以用第三个参数作为索引：
```html
<div v-for="(value, name, index) in object">
    {{ index }}. {{ name }}: {{ value }}
</div>
```

![3](/3.png)

>>在遍历对象时，会按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致。

## 维护状态
