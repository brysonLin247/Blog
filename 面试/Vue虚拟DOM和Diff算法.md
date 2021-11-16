## Vue虚拟DOM和Diff算法

JS执行

数据改变 -> 虚拟DOM（计算变更） ->  操作真实的DOM -> 视图更新

什么是虚拟DOM？

用JS模拟DOM结构

```html
<div id="app" class="container">
  <h1>虚拟DOM</h1>
  <ul style="color: orange">
  	<li>第一项</li>
    <li>第二项</li>
    <li>第三项</li>
  </ul>
</div>
```

用JS表达DOM结构：

```javascript
{
  tag:'div',
  props:{
    id:'app',
    className:'container'
  },
  children:[
    {
      tag:'h1',
      children:'虚拟DOM'
    },
    {
      tag:'ul',
      props:{ style:'color: orange'},
      children:[
      	{
      		tag:'li',
      		children:'第一项'
    		},
      	{
          tag:'li',
          children:'第二项'
    		},
      	{
          tag:'li',
          children:'第三项'
    		},
      ]
    }
  ]
}
```

