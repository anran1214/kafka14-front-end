---
title: background 背景属性
date: 2020-11-25
tags:
 - css基础
categories: 
 - CSS
---

| 值        | 说明   |
| :--------  | :-----  |
|background-color|指定要使用的背景颜色|
|background-position|指定背景图像的位置|
|background-size|指定背景图片的大小|
|background-repeat|指定如何重复背景图像|
background-origin|指定背景图像的定位区域|
|background-clip|指定背景图像的绘画区域|
|background-attachment|设置背景图像是否固定或者随页面的其余部分滚动|
|background-image|指定要使用一个或多个背景图像|

```html
<style>
*{
    padding: 0;
    margin: 0;
}
body{
    width:100%;
    height:100%;
    /** 可以设置多个背景图，用逗号隔开，第一个设置的永远在上面 */
    /** 单一属性 */
    background-image: url(./01.jpg),url(./02.jpg);
    background-repeat: no-repeat,repeat;
    background-position: center top,center center;
    /** 综合属性 */
    background:url(./01.jpg) no-repeat center top,url(./02.jpg) repeat center center; 
}
</style>

```

## background-color设置一个元素的背景颜色。`css1`

元素的背景是元素的总大小，包括填充和边界（但不包括边距）。
| 值        | 描述   |
| :--------  | :-----  |
|color|指定背景颜色|
|transparent|指定背景颜色为透明|
|inherit|继承|

## background-position 设置图像的起始位置 `css1`

| 值        | 描述   |
| :--------  | :-----  |
|center top、left bottom、...|如果仅指定一个关键字，其他值将会是center|
|x% y%|第一个值是水平位置，第二个值是垂直 如100% 100%|
|xpx ypx|第一个值是水平位置，第二个值是垂直 如 10px 10px|

## background-size 指定图片大小 `css3`

css3之前 图片大小由图片的实际大小决定
| 值        | 描述   |
| :--------  | :-----  |
|contain|结合当前盒子的宽高,缩放成适合背景定位区域的最大大小|
|cover|根据当前浏览器的尺寸铺满|
|length|宽高。如果只给出一个值，第二个是设置为 `auto`(自动)|
|percentage|相对背景定位区域的百分比。如果只给出一个值，第二个是设置为 `auto`(自动)|

## background-origin 指定背景图像的位置区域 `css3`

| 值        | 描述   |
| :--------  | :-----  |
|border-box| 边界框|
|content-box|内容框|
|padding-box|填充框|

## background-clip 设置绘图区域背景 `css3`

| 值        | 描述   |
| :--------  | :-----  |
|border-box| 背景绘制在边框方框内|
|content-box|背景绘制在内容方框内|
|padding-box|背景绘制在padding框内|

## background-repeat 设置如何平铺背景图片 `css1`

| 值        | 描述   |
| :--------  | :-----  |
|repeat\repeat-x\repeat-y|默认，水平垂直方向重复\水平位置重复\垂直位置重复|
|no-reptea|不会重复|

## background-attachment 设置背景图片是否固定或随页面的其余部分滚动

| 值        | 描述   |
| :--------  | :-----  |
|scroll| 随着页面滚动而滚动|
|fixed|固定，不会随着页面滚动|
|local|随着元素的滚动而滚动|