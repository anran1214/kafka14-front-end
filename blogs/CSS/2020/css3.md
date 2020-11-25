## 背景属性

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
                background:url(./01.jpg) no-repeat center top,url(./02.jpg)
                repeat center center; 
            }
</style>
```

## background-position

## background-size
css3之前 图片大小由图片的实际大小决定
|||
|contain|结合当前盒子的宽高|
|cover|根据当前浏览器的尺寸铺满|

## background-origin 指定背景图像的位置区域

|border-box|
|content-box|
|padding-box|

## background-clip 设置绘图区域背景

## 边框
## box-shadow 边框阴影
水平移动位置
垂直移动位置

## border-radius

## border-image

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

## 文本效果
## text-shadow 文本阴影


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


