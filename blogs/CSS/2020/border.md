---
title: CSS border属性
date: 2020-11-25
tags:
 - css基础
categories: 
 - CSS
---

## 边框
## border-width `css1`
`thin\medium\thick(粗)\length\inherit`
## border-style `css1`
`none\hidden\dotted\dashed\solid\double\`
hidden	与 "none" 相同。不过应用于表时除外，对于表，hidden 用于解决边框冲突。
## border-radius
`length\%`
## border-image
`border-image: source slice width outset repeat|initial|inherit;`
| 值        | 描述   |
| :--------  | :-----  | 
|border-image-source	|用于指定要用于绘制边框的图像的位置|
|border-image-slice|	图像边界向内偏移|
|border-image-width|	图像边界的宽度|
|border-image-outset|	用于指定在边框外部绘制 border-image-area 的量|
|border-image-repeat|	用于设置图像边界是否应重复（repeat）、拉伸（stretch）或铺满（round）|
```html
<style>
.box{
    width:300px;
    height:300px;
    margin:100px auto;
    border:20px solid red;
    border-image-source:url(./01.jpg);
    border-image-repeat:repeat;
    border-image-slice:20;/** 图像向内偏移 拉伸效果 */
}
```

