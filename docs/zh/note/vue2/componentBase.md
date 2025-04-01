---
outline: deep
---

## 基本示例
略
## 组件的复用
略
### data 必须是一个函数
略
## 组件的组织
略
## 通过 Prop 向子组件传递数据
略
## 单个根元素
略
## 监听子组件事件
在我们开发 \<blog-post> 组件时，它的一些功能可能要求我们和父级组件进行沟通。例如我们可能会引入一个辅助功能来放大博文的字号，同时让页面的其它部分保持默认的字号。

在其父组件中，我们可以通过添加一个 postFontSize 数据 property 来支持这个功能：
```JS
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```
它可以在模板中用来控制所有博文的字号：
```HTML
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```
现在我们在每篇博文正文之前添加一个按钮来放大字号：
```JS
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button>
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```
问题是这个按钮不会做任何事：
```HTML
<button>
  Enlarge text
</button>
```
当点击这个按钮时，我们需要告诉父级组件放大所有博文的文本。幸好 Vue 实例提供了一个自定义事件的系统来解决这个问题。父级组件可以像处理 native DOM 事件一样通过 v-on 监听子组件实例的任意事件：
```HTML
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```
同时子组件可以通过调用内建的 $emit 方法并传入事件名称来触发一个事件：
```HTML
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```
有了这个 v-on:enlarge-text="postFontSize += 0.1" 监听器，父级组件就会接收该事件并更新 postFontSize 的值。
### 使用事件抛出一个值
有的时候用一个事件来抛出一个特定的值是非常有用的。例如我们可能想让 <blog-post> 组件决定它的文本要放大多少。这时可以使用 $emit 的第二个参数来提供这个值：
```HTML
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```
然后当在父级组件监听这个事件的时候，我们可以通过 $event 访问到被抛出的这个值：
```HTML
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```
或者，如果这个事件处理函数是一个方法：
```HTML
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```
那么这个值将会作为第一个参数传入这个方法：
```JS
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```
### 在组件上使用 v-model
## 通过插槽分发内容
## 动态组件
## 解析 DOM 模板时的注意事项