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
有的时候用一个事件来抛出一个特定的值是非常有用的。例如我们可能想让 \<blog-post> 组件决定它的文本要放大多少。这时可以使用 $emit 的第二个参数来提供这个值：
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
自定义事件也可以用于创建支持 v-model 的自定义输入组件。记住：
```html
<input v-model="searchText">
```
等价于：
```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```
当用在组件上时，v-model 则会这样：
```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```
为了让它正常工作，这个组件内的 \<input> 必须：

将其 value attribute 绑定到一个名叫 value 的 prop 上
在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出
写成代码之后是这样的：
```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```
现在 v-model 就应该可以在这个组件上完美地工作起来了：
```html
<custom-input v-model="searchText"></custom-input>
```
到目前为止，关于组件自定义事件你需要了解的大概就这些了，如果你阅读完本页内容并掌握了它的内容，我们会推荐你再回来把自定义事件读完。
## 通过插槽分发内容
略
## 动态组件
有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：
![4](/4.png)
上述内容可以通过 Vue 的 \<component> 元素加一个特殊的 is attribute 来实现：
```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```
在上述示例中，currentTabComponent 可以包括

* 已注册组件的名字，或
* 一个组件的选项对象

你可以在这里查阅并体验完整的代码，或在这个版本了解绑定组件选项对象，而不是已注册组件名的示例。

请留意，这个 attribute 可以用于常规 HTML 元素，但这些元素将被视为组件，这意味着所有的 attribute 都会作为 DOM attribute 被绑定。对于像 value 这样的 property，若想让其如预期般工作，你需要使用 .prop 修饰器。

到目前为止，关于动态组件你需要了解的大概就这些了，如果你阅读完本页内容并掌握了它的内容，我们会推荐你再回来把动态和异步组件读完。
## 解析 DOM 模板时的注意事项
解析 DOM 模板时的注意事项
有些 HTML 元素，诸如 \<ul>、\<ol>、\<table> 和 \<select>，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 \<li>、\<tr> 和 \<option>，只能出现在其它某些特定的元素内部。

这会导致我们使用这些有约束条件的元素时遇到一些问题。例如：
```html
<table>
  <blog-post-row></blog-post-row>
</table>
```
这个自定义组件 \<blog-post-row> 会被作为无效的内容提升到外部，并导致最终渲染结果出错。幸好这个特殊的 is attribute 给了我们一个变通的办法：
```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```
需要注意的是如果我们从以下来源使用模板的话，这条限制是不存在的：

* 字符串 (例如：template: '...')
* 单文件组件 (.vue)
* \<script type="text/x-template">