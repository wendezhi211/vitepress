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

>在遍历对象时，会按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致。

## 维护状态
略

## 数组更新检测
### 变更方法
略
### 替换数组
变更方法，顾名思义，会变更调用了这些方法的原始数组。相比之下，也有非变更方法，例如 filter()、concat() 和 slice()。它们不会变更原始数组，而总是返回一个新数组。当使用非变更方法时，可以用新数组替换旧数组：
```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的启发式方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

### 注意事项
由于 JavaScript 的限制，Vue 不能检测数组和对象的变化。深入响应式原理中有相关的讨论。

## 显示过滤/排序后的结果
略

## 在 v-for 里使用范围值
v-for 也可以接受整数。在这种情况下，它会把模板重复对应次数。
```html
<div>
  <span v-for="n in 7">{{ n }} </span>
</div>
```
结果：
1 2 3 4 5 6 7

## 在 \<template> 上使用 v-for
略

## v-for 与 v-if 一同使用
当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你只想为部分项渲染节点时，这种优先级的机制会十分有用，如下：
```html
<li v-for="todo in todos" v-if="!todo.isComplete">
    {{ todo }}
</li>
```
上面的代码将只渲染未完成的 todo。

而如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或 \<template>) 上。如：

```html
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```
## 在组件上使用 v-for
在自定义组件上，你可以像在任何普通元素上一样使用 v-for。
```html
<my-component v-for="item in items" :key="item.id"></my-component>
```
>2.2.0+ 的版本里，当在组件上使用 v-for 时，key 现在是必须的。
然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 prop：
```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```
不自动将 item 注入到组件里的原因是，这会使得组件与 v-for 的运作紧密耦合。明确组件数据的来源能够使组件在其他场合重复使用。

下面是一个简单的 todo 列表的完整例子：
```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```

>注意这里的 is="todo-item" attribute。这种做法在使用 DOM 模板时是十分必要的，因为在 \<ul> 元素内只有 \<li> 元素会被看作有效内容。这样做实现的效果与 \<todo-item> 相同，但是可以避开一些潜在的浏览器解析错误。查看 DOM 模板解析说明 来了解更多信息。