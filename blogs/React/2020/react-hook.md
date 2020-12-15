---
title: react-hooks基础-学习笔记
date: 2020-12-13
tags:
 - react-hook
categories: 
 - React
---

## 背景

### 单向数据流

React采用自上而下单向数据流的方式，管理自身的数据和状态。
单向数据流：数据只能从父组件触发，向下传递到子组件。
::: warning
React中，state和props的改变，都会引发组件重新渲染
对于class组件，重新渲染是执行render方法；对于函数式组件，是整个函数重新执行
:::



### 函数式组件
函数执行完毕式，返回的是一个jsx结构
1.接收props作为自己的参数

```javascript
interface Props {
    name:string,
    age:number
}
function Demo({ name, age }: Props){
    return [
        <div>名字: {name}</div>
        <div>年龄: {age}</div>
    ]
}
```

2.props的每次变动，组件都会重新渲染一次，函数重新执行
3.没有this

## Hooks 有状态的函数式组件
特性：
## useState
利用闭包，在函数内部创建一个当前函数组件的状态。并提供一个修改该状态的方法。

```javaScript
//利用数组解构的方式接收状态名及其设置方法
//传入0作为状态counter的初始值
const [counter,setCounter]=useState(0);
//onClick={() => setCounter(counter + 1)}
每次赋值，counter得到的，都是新传入setCounter中的值
引用类型 setCounter(...counter,b:4)
```

如果初始值需要通过较为复杂的计算得出，则可以传入一个函数作为参数，函数返回值为初始值。该函数也只会在组件首次渲染时执行一次。

```javaScript
const a = 10;
const b = 20
// 初始值为a、b计算之和
const [counter, setCounter] = useState(() => {
  return a + b;
})
```

在TS中使用：

```javaScript
// 能根据 false 推导为 boolean 类型
const [visible, setVisible] = useState(false);

// 能根据 [] 推导为 any[] 类型，因此此时还需要专门声明any为何物
const [arr, setArr] = useState<number[]>([]);
```
## useEffect 有两个参数

第一个参数是自定义的执行内容，作为函数传入。

第二个参数作为浅比较变化的依赖，是由监听值组成的数组。为避免反复执行，比较之后的值不变，副作用逻辑就不再执行。

当第二个参数传入空数组，即没有传入比较变化的变量，则比较结果永远都保持不变，那么副作用逻辑就只能执行一次。

## 执行副作用逻辑: DOM 操作 数据请求 组件更新

我们可以使用多个 useEffect 来执行不同的副作用逻辑

### 相当于生命周期的 componentDidMount,componentDidUpdate,componentWillUnmount

:::warning
要利用 props，去修改内部的 state。
这是受控组件的核心思维
:::

## 实现 componentWillUnmount 的功能如下

```javaScript
useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.id, handleStatusChange);
    };
  });
```

整个过程：

```javaScript
1.传入 props.id = 1
2.组件渲染
3.DOM 渲染完成，副作用逻辑执行，返回清除副作用函数 clear，命名为 clear1
4.传入 props.id = 2
5.组件渲染
6.组件渲染完成，clear1 执行
7.副作用逻辑执行，返回另一个 clear 函数，命名为 clear2
8.组件销毁，clear2 执行
```

## 页面销毁时请求还在进行中，销毁后仍然向向 state 中写入数据造成内存溢出警告

参考:
https://www.debuggr.io/react-update-unmounted-component/

解决办法

```javaScript
import { useRef, useEffect } from 'react';

export default function useIsMountedHook() {
  const isMountedRef = useRef<boolean | null>(null);
  useEffect(() => {
    isMountedRef.current = true;
    return () => {
      isMountedRef.current = false;
    };
  });
  return isMountedRef;
}
```

使用：

```javaScript
import useIsMountedHook from '@/hooks/useIsMountedHook';

export default function IndexPageContainer(): JSX.Element {
  const isMountedRef = useIsMountedHook();
  useEffect(() => {
    getData().then(res => {
      if (!isEmpty(res) && isMountedRef.current) {
        setData(res);
      });
    }
  },[])
 }
```
## useMemo/useCallback 记忆函数
当计算过程或者函数体足够复杂时适用
特点：
记忆函数 利用闭包缓存上次结果
成本：额外的内存
不是绝对优化，而是一种成本的交换，并非适用所有场景

hooks中的记忆函数：
useState useEffect／useLayoutEffect useReducer useRef useMemo(记忆运算结果) useCallback(记忆函数体)

