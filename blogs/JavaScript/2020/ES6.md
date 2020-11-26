---
title: ES6教程笔记
date: 2020-11-20
tags:
 - ES6
categories: 
 - JavaScript
---

## 1. let const 声明的变量在块级作用域内

## 2. 函数

（1）参数默认值

```javascript
function add(a,b=20){
  return a+b
}
```

（2）默认的表达式也可以是函数

```javascript
function add(a,b=getVal(5)){
    return a+b
}
function getVal(val){
  return val+5
}
```

（3）剩余参数 由...和一个具名参数指定

```javascript
function pick(obj,...keys) {
  let result=Object.create(null);
  for (let i=0;i<keys.length;i++){
    result[keys[i]] = obj[keys[i]]
  }
  return result
}
function pick(obj,...keys){
  let result=Object.create(null)
}
let book ={
    title:'es6-tutorial',
  author:'someone',
  year:'2020',
}
let bookData=pick(book,'year','author')

function checkArgs(...args){
  console.log(args) //["a","b","c"]
  console.log(arguments) // Arguments(3)["a","b","c"]
}
```

（4）扩展运算符
 * 
    * 剩余运算符： 把多个独立的参数合并到一个数组中
    * 扩展运算符：将一个数组分割，并将各项作为分离的参数传给函数

（5）箭头函数
  *
    * 使用=>来定义   function( ){ } ===   ( )=>{ }
    * 没有this指向，只能通过查找作用域链来决定this的值

  * 内部没有arguments
    * 不能使用new关键字来实例化对象（function函数也是一个对象；而箭头函数相当于函数的语法糖）
      箭头函数不能使用new关键字来实例化对象
  

## 4.解构赋值

  (一) 对象的解构
   * 
    * 可以用剩余运算符
    * 可以用默认值 let {a, b=30 }={a=20}

（二）数组的结构

（三）可嵌套 let {a,[b],c}={1,[2],3}

## 5.扩展的对象功能

```javascript
const name='a'
const obj={
  isShow:true,
  [name+'bc']:123,
  ['f'+name](){
    console.log(this)
  }
}
//console.log(obj)  // {isShow:true,abc:123,fa:f}
```

### 对象的方法

（1）is( )  比较两个值是否严格相等

```javascript
console.log(NaN === NaN) // false
console.log(Object.is(NaN,NaN)) // true
```

（2）assign( ) 浅拷贝（全部复制过来），对象的合并，返回合并后的新对象

```javascript
Object.assign ( target, obj1, obj2 )
```

## 6.Symbol类型

#### 原始数据类型；表示独一无二的值（内存地址不相等）

```javascript
//原始数据类型Symbol 表示是独一无二的值
//最大的用途：用来定义对象的私有变量
const name=Symbol('name')
const nae2=Symbol('name')
console.log(name === name2) // false
```

## 7. Map 和 Set

（1)Set 集合：表示无重复值的有序列表
 add delete has(//校验某个值是否存在)

```javascript
将set转换成数组
let set=new Set( [ 1,2,3,3,4, ] )
let arr=[ ...set ] // [1,2,3,4]
(set中对象的引用无法被释放）
```

（2）Map类型是键值对对有序列表，键和值是任意类型

## 8.数组的扩展功能

（1）from 将伪数组转换成真正对数组

```javascript
Array.from( )  / 使用扩展运算符[ ...arr ]
还可以接受第二个参数，用来对每个元素进行处理
```

（2) of() 将任意对数据类型，转换成数组

```javascript
Array.of( 3,11, [1,2,3] ,{ id:1 }) ／／[3,11,Array[1,2,3],{id:1}]
```

（3）copywithin 数组内部指定位置对元素复制到其他位置，返回当前数组

```javascript
console.log([1,2,3,8,9,0].copywithin(0,3)) // [8,9,10,8,9,10]
```

（4）find() findIndex()
find() 找出第一个符合条件到数组成员
findIndex()找出第一个符合条件到数组成员的索引

（5）entires() keys() values()
返回一个遍历器 可以使用for...of循环进行
keys() 键名 values() 值 entries() 键值

（6）includes() 返回一个布尔值，表示某个数组是否包含给定的值

之前用indexOf

## 9.迭代器 Iterator

新的遍历机制
使用迭代

## 10.生成器 Generator

Generator函数 可以通过yield关键字 将函数挂起 ，改变执行流

* 与普通函数的区别：1. function后面，函数名之前有一个
* 2.只能在函数内部使用yield表达式，让函数挂起

```javascript
function* func(){
        yield 2 (阻止执行，next（）继续执行)
}
//返回一个遍历器对象 可以调用next()
let fn = func()
console.log(fn.next()) //{value: 2, done: false}
console.log(fn.next()) //{value: undefined, done: true}
console.log(fn.next()) //{value: undefined, done: true}
```

### 总结：Generator函数是分段执行的，yield语句是暂停执行的，而next()恢复执行

* 使用场景1：为不具备迭代器Interator接口的对象提供了遍历操作

```javascript
const obj={
  name:'xxxx',
  age:18,
}
function* objectEntries(obj){
  // 获取对象的所以key [name,age]
    const propKeys=Object.keys(obj)
  for(const propkey of propkeys){
    yield [propkey,obj[propkey]]
  }
}
obj[Symbol.interator] = objectEntries
//接下去就可以进行遍历
for( let [key,value] of objectEntries(obj)){
    console.log(`${key}${value}`)
}
```

* 使用场景2：异步编程
痛点：回调地狱

```javascript
function* main(){
  let res = yield request('http:...')
  //执行后面的操作
  console.log('数据请求完成，可以继续操作')
}
const ite=main()
ite.next()
function request(url){
    $.ajax({
        url,
      method:'get',
      success(res){
      }
    })
}
```

// 加载loading页面
// 数据加载完成（异步）
// loading关闭

```javascript
function* load(){
  loadrUI()
  yield showData()
  hideUI()
}

let isLoad=load()
isLoad.next()
function loadUI(){
  consoloe.log('加载loading页面')
}
function showData(){
  //模拟异步操作
  setTimeout(()=>{
    console.log('数据加载完成')
    isLoad.next()
  },1000)
}
```

## 11. Promise

特点：

* 1.对象的状态不受外界影响

* 处理异步操作三个状态：pending（进行） resolved（成功） rejected（失败）

```javascript
let pro = new Promise( 接收一个回调函数，两个参数（resolved,rejected）){
  //执行异步操作
    返回promise对象
    then: 成功返回的结果
    catch: 捕获异常

    if(){resolved(res.data)}
    else{rejected(res.error)}
}
pro.then((val)=>{
    val就是resolved(res.data)返回的值
},(err)=>{
    err
})

const getJSON=function(url){
  return new Promise(resolve,reject)=>{
    const xht=new XMLHttpRequest()
    xhr.open('GET',url)
    xhr.onreadystatechange=handler
    xhr.responseType='json'
    xhr.setRequestHeader('Accept','application/json')
    //发送
    xhr.send()
    function handler(){
      if(this.readyState ===4){
        if(this.status === 200){
          resolve(this.response.xxx)
          else{

          }
        }
      }
    }
  }
}
getJSON('http://xxx')
.then((data)=>{
  console.log(data)
},(error)=>{
  console.log(error)
})
```

* `then( ) 方法`

第一个参数是resolve回调函数，第二个参数可选，是reject状态回调的函数
resolve( )

能将现有的任何对象转换成promise对象
· 以下两个表达式等价：

```javascript
const p=Promise.resolve('foo')
//一般使用这种：
const p=new Promise(resolve=>resolve('foo'))
```

* `all( )方法`

并行执行多个异步
比如一些游戏类的素材比较多，等待图片，flash，静态资源文件都加载完成才进行页面初始化

```javascript
let promise1=new Promise((resolve,reject)=>{})
let promise2=new Promise((resolve,reject)=>{})
let promise3=new Promise((resolve,reject)=>{})
let p4=Prommise.all([promise1,promise2,promise3])
p4.then(()=>{
  //三个都成功才成功
}).catch(err=>{
  //如果有一个失败则失败
})
```

* `race( )`

给某个异步请求设置超时时间，并且在超时后执行相应的操作
接收一个数组

* 1.请求图片资源

```javascript
function requestImg(imgSrc){
  return new Promise(resolve,reject)=>{
    const img=new Image()
    img.onload=function(){
      resolve(img)
    }
    img.src=imgSrc
  }
}
function timeout(){
  return new Promise((resolve,reject)=>{
    setTimeout(()=>{
      reject('图片请求超时')
    }，3000)
  })
}
Promise.race([requestImg('images/01.png'),timeout()]).then(data=>{
  document.body.appendChild(data)
}).catch(err=>{
    console.log(err)
}).done(不管成功与否)
.finally()
```

* 2. async异步操作

会返回一个Promise对象。相当于Generator的一个语法糖

```javascript
async function f(){
    return await 'hello'
  //如果async中有多个await then函数会等待所有await指令执行完成才会执行
  //如果有一个出错，则不会往下执行
}
f().then(v=>v).catch(err=>err)
```

异步发展：Generator Promise async
解决回调地狱
使得异步调用更加方便

## 13. class关键字 类

 es5造类，用构造函数 function Person(name,age){ } let p1=Person('xxx',18)

 es6:

```javascript
class Person{
  //构造函数实例化的时候会被立即调用,是一个默认的方法
  constructor(name,age){
    this.name=name;
    this.age=age;
  }
  sayName(){
    return this.name
  }//p1.sayName()
}
//通过Object.assign()方法一次性向类中添加多个方法
Object.assign(Person.prototype,{
  sayName(){
    return this.name
  }
    sayAge(){
    return this.age
  }
})
let p1=new Person('xxx',18)
```

## 14.类的继承

`extends`

```javascript
class Xiao extends Person{
    constructor(name,age,color){
    //继承，超类
    super(name,age);//相当于Animal.call(this,name,age)
    this.color=color
  }
  sayColor(){
    return ``
  }
  //重写父类方法
    sayName(){
    return this.name + super.sayAge()+this.color
  }
let x=new Xiao('xiao',24,'red)
    x.sayAge()
```

## 15.模块化的实现

* 两个命令
  * export 规定模块的对外接口
  * import 输入其他模块提供的功能
  * 一个模块就是独立的文件，可以抛出多个变量/一个对象
* 命名： export default obj 同一个文件里 (只能default一次)/ export可以多个

```html
<script type='module'>
    import {name} from '../index.js';
    import * as f from '';
</script>
```
