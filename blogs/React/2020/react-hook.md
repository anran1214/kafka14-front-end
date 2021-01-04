---
title: react-hooks基础-学习笔记
date: 2020-12-13
tags:
  - react-hook
categories:
  - React
---

## 一.FAQ

### 1.有什么是 Hook 能做而 class 做不到的？

自定义 Hook 解决了以前在 React 组件中无法灵活共享逻辑的问题。

目前为止，在 React 中有两种流行的方式来共享组件之间的状态逻辑: render props 和高阶组件，
:::tip
当我们想在两个函数之间共享逻辑时，我们会把它提取到第三个函数中。
即自定义 Hook
:::

```javascript
// MyResponsiveComponent.js
function MyResponsiveComponent() {
  const width = useWindowWidth(); // Our custom Hook
  return <p>Window width is {width}</p>;
}
// useWindowWidth.js
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handleResize);
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  });

  return width;
}
```

### 2.Hook 能否覆盖 class 的所有使用场景？

我们给 Hook 设定的目标是尽早覆盖 class 的所有使用场景。目前暂时还没有对应不常用的 getSnapshotBeforeUpdate，getDerivedStateFromError 和 componentDidCatch 生命周期的 Hook 等价写法，但我们计划尽早把它们加进来。

### 3.生命周期方法要如何对应到 Hook？

:::tip
constructor：函数组件不需要构造函数。你可以通过调用 useState 来初始化 state。如果计算的代价比较昂贵，你可以传一个函数给 useState。
:::
:::warning
getDerivedStateFromProps：改为 在渲染时 安排一次更新。
:::
:::tip
shouldComponentUpdate：详见 下方 React.memo.
:::
:::warning
render：这是函数组件体本身。
:::
:::tip
componentDidMount, componentDidUpdate, componentWillUnmount：useEffect Hook 可以表达所有这些(包括 不那么 常见 的场景)的组合。
:::
getSnapshotBeforeUpdate，componentDidCatch 以及 getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法，但很快会被添加.
:::

### 4.Hook 对于 Redux connect() 和 React Router 等流行的 API 来说，意味着什么？

:::tip

#### React Redux 从 v7.1.0 开始支持 Hook API 并暴露了 useDispatch useStore 和 useSelector 等 hook。

地址：https://react-redux.js.org/api/hooks

#### React Router 从 v5.1 开始支持 hook。useHistory,useLocation,useParams...

地址：https://reactrouter.com/web/api/Hooks
:::

### 5.在依赖列表中省略函数是否安全？

```javascript
//一般来说，不安全。

function Example({ someProp }) {
  function doSomething() {
    console.log(someProp);
  }

  useEffect(() => {
    doSomething();
  }, []); // 🔴 这样不安全（它调用的 `doSomething` 函数使用了 `someProp`）
}
要记住 effect 外部的函数使用了哪些 props 和 state 很难。这
也是为什么 通常你会想要在 effect 内部 去声明它所需要的函数。
 这样就能容易的看出那个 effect 依赖了组件作用域中的哪些值
function Example({ someProp }) {
  useEffect(() => {
    function doSomething() {
      console.log(someProp);
    }

    doSomething();
  }, [someProp]); // ✅ 安全（我们的 effect 仅用到了 `someProp`）
}
如果这样之后我们依然没用到组件作用域中的任何值，就可以安全地把它指定为 []：
useEffect(() => {
  function doSomething() {
    console.log('hello');
  }

  doSomething();
}, []); // ✅ 在这个例子中是安全的，因为我们没有用到组件作用域中的 *任何* 值

只有 当函数（以及它所调用的函数）不引用 props、state 以及由它们衍生而来的值时，
你才能放心地把它们从依赖列表中省略
```

### 如果处于某些原因你 无法 把一个函数移动到 effect 内部，还有一些其他办法

你可以尝试把那个函数移动到你的组件之外。那样一来，这个函数就肯定不会依赖任何 props 或 state，并且也不用出现在依赖列表中了。
如果你所调用的方法是一个纯计算，并且可以在渲染时调用，你可以 转而在 effect 之外调用它， 并让 effect 依赖于它的返回值。
万不得已的情况下，你可以 把函数加入 effect 的依赖但 把它的定义包裹 进 useCallback Hook。这就确保了它不随渲染而改变，除非 它自身 的依赖发生了改变：

### 6.如果我的 effect 的依赖频繁变化，我该怎么办？

```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // 这个 effect 依赖于 `count` state
      //这个情况，每隔一秒都是(0+1)
      改为;
      setCount((c) => c + 1); // ✅ 在这不依赖于外部的 `count` 变量
    }, 1000);
    return () => clearInterval(id);
  }, []); // 🔴 Bug: `count` 没有被指定为依赖 //更改后。✅ 我们的 effect 不适用组件作用域中的任何变量

  return <h1>{count}</h1>;
}
```

###

## 背景

### 单向数据流

React 采用自上而下单向数据流的方式，管理自身的数据和状态。
单向数据流：数据只能从父组件触发，向下传递到子组件。
::: warning
React 中，state 和 props 的改变，都会引发组件重新渲染
对于 class 组件，重新渲染是执行 render 方法；对于函数式组件，是整个函

数重新执行
:::

### 函数式组件

函数执行完毕式，返回的是一个 jsx 结构 1.接收 props 作为自己的参数

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

2.props 的每次变动，组件都会重新渲染一次，函数重新执行 3.没有 this

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

在 TS 中使用：

```javaScript
// 能根据 false 推导为 boolean 类型
const [visible, setVisible] = useState(false);

// 能根据 [] 推导为 any[] 类型，因此此时还需要专门声明any为何物
const [arr, setArr] = useState<number[]>([]);
```

#### useState 更新 state 变量的时候是替换它,而 class 中的 this.setState，会把更新后的字段 合并 入对象中。

:::tip
我们推荐把 state 切分成多个 state 变量，每个变量包含的不同值会在同时发生变化。
把独立的 state 变量拆分开还有另外的好处。这使得后期把一些相关的逻辑抽取到一个自定义 Hook 变得容易
:::

## useEffect 有两个参数

第一个参数是自定义的执行内容，作为函数传入。

第二个参数作为浅比较变化的依赖，是由监听值组成的数组。为避免反复执行，比较之后的值不变，副作用逻辑就不再执行。

当第二个参数传入空数组，即没有传入比较变化的变量，则比较结果永远都保持不变，那么副作用逻辑就只能执行一次。

## 执行副作用:数据获取，设置订阅以及手动更改 React 组件中的 DOM

我们可以使用多个 useEffect 来执行不同的副作用逻辑

useEffect 相当于生命周期的 componentDidMount,componentDidUpdate,componentWillUnmount

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

## 在 React 组件中有两种常见副作用操作：需要清除的和不需要清除的。

（1）无需清除的 effect
在 React 更新 DOM 之后运行一些额外的代码。比如发送网络请求，手动变更 DOM，记录日志

默认情况下，它在第一次渲染之后和每次更新之后都会执行。React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。
每次我们重新渲染，都会生成新的 effect，替换掉之前的。某种意义上讲，effect 更像是渲染结果的一部分 —— 每个 effect “属于”一次特定的渲染
:::warning
提示

与 componentDidMount 或 componentDidUpdate 不同，使用 useEffect 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。
:::
(2)需要清除的 effect
例如订阅外部数据源。这种情况下，清除工作是非常重要的，可以防止引起内存泄露
如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它.

## 使用 Effect 的提示

(1)使用多个 Effect 实现关注点分离
Hook 允许我们按照代码的用途分离他们。React 将按照 effect 声明的顺序依次调用组件中的每一个 effect。

为什么每次更新的时候都要运行 Effect
useEffect 会在调用一个新的 effect 之前对前一个 effect 进行清理。

```javaScript
在 class 组件中，我们需要添加 componentDidUpdate 来解决这个问题：
  componentDidUpdate(prevProps) {
    // 取消订阅之前的 friend.id
    ChatAPI.unsubscribeFromFriendStatus(
      prevProps.friend.id,
      this.handleStatusChange
    );
    // 订阅新的 friend.id
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  忘记正确地处理 componentDidUpdate 是 React 应用中常见的 bug 来源。
```

```javaScript
function FriendStatus(props) {
  // ...
  useEffect(() => {
    // ...
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  // Mount with { friend: { id: 100 } } props
ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // 运行第一个 effect

// Update with { friend: { id: 200 } } props
ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // 清除上一个 effect
ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // 运行下一个 effect

// Update with { friend: { id: 300 } } props
ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // 清除上一个 effect
ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // 运行下一个 effect

// Unmount
ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // 清除最后一个 effect
```

(2)通过跳过 Effect 进行性能优化
在 class 组件中，我们可以通过在 componentDidUpdate 中添加对 prevProps 或 prevState 的比较逻辑解决.
如果某些特定值在两次重渲染之间没有发生变化，你可以通知 React 跳过对 effect 的调用，只要传递数组作为 useEffect 的第二个可选参数即可：

```javaScript
useEffect(() => {
document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
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

hooks 中的记忆函数：
useState useEffect／useLayoutEffect useReducer useRef useMemo(记忆运算结果) useCallback(记忆函数体)

useMemo 不写数组则监控所有状态值的更新；空数组则不监控任何状态，状态改变不更新；数组有值，则监控该值的变化。
