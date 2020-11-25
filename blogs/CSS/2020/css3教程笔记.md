---
title: CSS3 教程未归类笔记
date: 2020-11-25
tags:
 - 未归类笔记
categories: 
 - CSS
---

## linear-gradient() 线性渐变
```html
<style>
.box{
    bacground:linear-gradient(red,blue)/** 默认从上往下*/
    bacground:-webkit-linear-gradient(left,red,blue)/** 从左往右 */
    bacground:linear-gradient(to right,red,blue)/** 从左往右 */
    bacground:linear-gradient(to bottom right,red,blue)/** 从上往右下角 */
    
} 
/** 
-ms- ie
-webkit- google safari
-moz- firefox
-o opera
 */
</style>
```
角度 水平线到渐变线之间的角度，逆时针方向计算，0deg从下到上 90deg从左到右
```html
<style>
.box{
    bacground:linear-gradient(180deg,red,blue)/** 从下往上 */
} 
 */
</style>
```
重复的线性渐变
```html
<style>
.box{
    bacground:repeating-linear-gradient(180deg,red 10%,blue 10%,yellow)/** 从下往上 */
} 
 */
</style>
```
径向的线性渐变
```html
<style>
.box{
    bacground:radial-linear-gradient(center,形状(circle,ellipse(椭圆是默认值)), 大小,开始的颜色,结束的颜色)/** 从下往上 */
} 
 */
</style>
```


## box-shadow 边框阴影
水平移动位置
垂直移动位置