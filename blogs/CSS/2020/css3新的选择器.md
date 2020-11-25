---
title: CSS 新增选择器
date: 2020-11-25
tags:
 - 伪类伪元素
categories: 
 - CSS
---
## 新的选择器
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
}
/** 奇偶数 */
ul li:nth-child(odd/even){
    
}
</style>
```

