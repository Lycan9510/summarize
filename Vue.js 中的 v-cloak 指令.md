
> 本文主要文案cv于 小人物的秘密花园 的[文章](https://www.jianshu.com/p/9ab97749f0f3)，主要用于个人记录。

当网络较慢，网页还在加载 Vue.js ，而导致 Vue 来不及渲染，这时页面就会显示出 Vue 源代码。我们可以使用 v-cloak 指令来解决这一问题。

使用 v-cloak 指令设置样式，这些样式会在 Vue 实例编译结束时，从绑定的 HTML 元素上被移除。
html：
```
<div id="app">
    {{context}}
</div>
```

js：
```
<script>
    var app = new Vue({
        el: '#app',
        data: {
            context:'互联网头部玩家钟爱的健身项目'
        }
    });
</script>
```

效果：

![nrm](img/3386108-cc0cda1d980b9586.webp)


我们使用 v-cloak 指令来解决屏幕闪动的问题吧O(∩_∩)O~

js 不变，在 div 中加入 v-cloak 指令。

html：
```
<div id="app" v-cloak>
    {{context}}
</div>`
```

css：
```
[v-cloak]{
    display: none;
}
```
使用 v-cloak 指令之后的效果（demo）：

![nrm](img/3386108-5ebe5bb452a25298.webp)

在简单项目中，使用 v-cloak 指令是解决屏幕闪动的好方法。但在大型、工程化的项目中（webpack、vue-router）只有一个空的 div 元素，元素中的内容是通过路由挂载来实现的，这时我们就不需要用到 v-cloak 指令咯。