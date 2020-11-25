---
title: CSS 新增选择器
date: 2020-11-24
tags:
 - 伪类伪元素
categories: 
 - CSS
---
## 属性选择器
```html
<input type="text">
<style>
    input[type=text]{

    }
    input[type="password"]{

    }
    a[title=""]{

    }
    a[href=""]{

    }
</style>
```

## 伪类
```html
<style>
a:link{
    //没被访问过
}
a:visited{
    //访问过后的
}
a:hover{
    //鼠标悬停
}
a:active{
    //按住
}
</style>
```
## 伪元素
```html
<style>
p::after{
    //p标签的后面
    content:''
}
p::before{
    //p标签的前面
    content:''
}
</style>
```

```html
<style>
ul li:first-child{
    //第一个li标签
}
ul li:last-child{
    //最后一个li标签
}
ul li:nth-child(1~n){
    //指定第几个li标签
    //匹配属于其父元素的第几个子元素，
    //不论元素的类型
}
/** 奇偶数 */
ul li:nth-child(odd/even){
    
}
</style>
```
## nth-of-type与nth-child的区别
nth-of-type是取当前元素的兄弟元素的第n个
nth-child是当前元素的第n个节点的当前元素，//当第n个元素不是li元素，则取不到
```html
<body>
    <p>我是第一个</p>
    <p>我是第二个</p>
        <h1>我是中间插入的</h1>
    <p>我是第三个</p>
    <p>我是第四个</p>
</body>
<style>
    p:nth-child(4){
        //选择的是  <p>我是第三个</p>
    }
    p:nth-of-type(4){
        //选择的是  <p>我是第四个</p>
    }
</style>
```
```html
<body>
    <p>我是第一个</p>
    <p>我是第二个</p>
    <p>我是第三个</p>
        <h1>我是中间插入的</h1>
    <p>我是第四个</p>
</body>
<style>
    p:nth-child(4){
        //取不到
    }
    p:nth-of-type(4){
        //选择的是  <p>我是第四个</p>
    }
</style>
```



