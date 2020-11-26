---
title: 字体文本常用样式
date: 2020-11-24
tags:
 - css基础
categories: 
 - CSS
---

## white-space 设置如何处理元素中的空白符 `css1`

| 值        | 描述   |
| :--------:   | :-----  |
| normal      |默认。空白会被浏览器忽略|  
| pre        |空白会被浏览器保留   |
| nowrap        |文本不会换行，会在同一行上继续，直到遇到`<br>`标签为止|
|pre-wrap |保留空白符序列，但是正常地进行换行|
|pre-line | 合并空白符序列，但是保留换行符|
|inherit| 规定从父元素继承该属性的值|

::: warning
示例：

```html
<p>
This is some text. This is some text.     This is some text.
This is some text. This is some text. This is some text.
</p>
nowarp：
This is some text. This is some text. This is some text. This is some text. This is some text. This is some text.
pre /pre-wrap:
This is some text. This is some text.     This is some text.
This is some text. This is some text. This is some text.
pre-line：
This is some text. This is some text.This is some text.
This is some text. This is some text. This is some text.
:::

## word-spacing 增加或减少字与字之间的空白 `css1`
| 值        | 描述   |
| :--------:   | :-----  |
| normal      |默认，0。定义单词间的标准空间|  
| length        |定义单词间的固定空间   |
| inherit        |规定从父元素继承该属性的值|
::: warning
示例：
```html
<p>
This is some text.
</p>
word-spacing:30px / 5em
This           is          some          text.
:::
## overflow 规定当内容溢出元素框时发生的事情 `css2`
| 值        | 描述   |
| :--------:   | :-----  |
| visible      |默认。内容不会被修剪，会呈现在元素框之外|  
| hidden      |内容会被修剪，并且其余内容不可见   |
| scroll        |内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容|
|auto |如果内容被修剪，则浏览器会显示滚动条|
|inherit| 规定从父元素继承该属性的值|
::: warning 如何修改滚动条默认的背景色、宽高

```html
<script text="css">
    /** 滚动条整体样式 */
    ::-webkit-scrollbar{
        height:28px;
        width:28px;
    }
    /** 滚动条滑块的样式 */
    ::webkit-scrollbar-thumb{
        border-radius:0;
        border-style:dashed;
        background-color:
        border-color:
        background-clip:
    }
    /** 定义轨道的样式 */
    ::webkit-scrollbar-track{
        /** 滚动条里的轨道 */
        -webkit-box-shadow:
        ...
    }
    ::webkit-scrollbar-track:hover{

    }
</script>
```

:::

## text-overflow 规定当文本溢出包含元素时发生的事情 `css3`

| 值        | 描述   |
| :--------:   | :-----  |
| clip      |修剪文本|  
| ellipsis        |显示省略符号来代表被修剪的文本   |
| string        |使用给定的字符串来代表被修剪的文本|

:::warning 如何使用css或js实现多行文本溢出省略效果，考虑兼容性
单行文本溢出省略

```html
<script text="css">
/** 单行文本溢出显示省略号的情况 */
.demo{
    width:100px;
    white-space:nowarp; /** 设置文字在一行显示，不能换行 */
    overflow:hidden;/** 文字长度超出限定宽度，则隐藏超出内容 */
    text-overflow:ellipsis;/** 规定当文本溢出时，显示省略符号来代表被修剪的文本 */
}
/**
 * 优点：无兼容问题，响应式截断 短板：只支持单行文本截断
*/
</script>
```

多行文本溢出省略（按行数）

```html
<script text="css">
/** 多适用于移动端页面，因为移动设备浏览器更多是基于 WebKit 内核 */
.demo{
    display:-webkit-box;
    overflow:hidden;/** 文字长度超出限定宽度，则隐藏超出内容 */
    -webkit-line-clamp:2;/** 用来限制在一个块元素显示的文本行数，2表示最多显示2行 */
    -webkit-box-orient:vertical;/** 设置或检索伸缩盒对象的子元素的排列方式 */
}
/**  优点：响应式截断 短板：兼容性问题 */
</script>
```

多行文本溢出省略（按高度）

```html
<script text="css">
/** 多适用于移动端页面，因为移动设备浏览器更多是基于 WebKit 内核 */
.demo{
    overflow:hidden;/** 文字长度超出限定宽度，则隐藏超出内容 */
    max-height:40px;/** 设定当前元素最大高度 */
    line-height:20px;/** 结合元素高度，高度固定情况下，设定行高，控制行数 */
}

/** 优点：无兼容问题，响应式截断 短板:生硬，适用于文本溢出不需要显示省略号的情况 */
</script>
```

鼠标悬浮展示全部信息，用到`title`属性

#### title属性规定关于元素的额外信息。通常会在鼠标移动到元素上时显示一段工具提示文本

`<eliment title="text">`

:::

## text-align 规定元素中的文本的水平对齐方式 `css3`

| 值        | 描述   |
| :--------:   | :-----  |
| left      |默认。把文本排列到左边|  
| right      |把文本排列到右边   |
| center        |文本居中|
|justify |实现两端对齐文本效果|
|start| 内容对其开始边界|
|end| 内容对其结束边界|

## cursor 规定要显示的光标类型 `css2`

| 值        | 描述   |
| :--------:   | :-----  |
| url      |需使用的自定义光标的urkl  
| default      |默认光标(通常是一个箭头)   |
| auto        |默认。浏览器设置的光标|
|pointer |一只手|
|move|指示某对象可被移动|
|text| 指示文本|
|wait|指示程序在忙|

## 文本效果

## text-shadow 文本阴影 `css3`

语法：
`text-shadow: h-shadow v-shadow blur color;`
| 值        | 描述   |
| :--------:   | :-----  |
|h-shadow|必需。水平阴影的位置。允许负值|
|v-shadow|必需。垂直阴影的位置。允许负值|
|blur|可选。模糊的距离|
|color|可选。阴影的颜色。|

## text-transform 控制文本的大小写 `css1`

| 值        | 描述   |
| :--------:   | :-----  |
|none|默认。定义带有小写字母和大写字母的标准的文本。|
|capitalize|文本中的每个单词以大写字母开头。|
|uppercase|定义仅有大写字母。|
|lowercase|定义无大写字母，仅有小写字母|
