## 为什么这样写？执行时机是什么？这样写的问题是什么？有没有更好的写法？

### 一些优化

#### 1.requestIdleCallback

可以通过在空闲的回调函数中使用requestIdleCallback(),来优化性能

```js
requestIdleCallback(callback,[options])
```

options只有一个参数，timeout，表示超过这个时间后，如果任务还没执行，则强制执行，不必再等待空闲。



# 第一章 一些前置知识

* babel 能将jsx转成js

* react development是react的核心

* react-dom  react扩展库

* 只有先引入核心库才能引入扩展库

* 标签type必须写babel

* 标签内不能加单引号

## 1.1 jsx语法糖：

> React的JSX创建出来的是虚拟DOM，而不是HTML

1.定义虚拟DOM时，不要加引号

2.标签中混入js表达式，要用{}

3.样式的类名指定不要用class，要用classname

4.内联样式。要用style={{key:value}}的形式去写，要有两个花括号

5.只能有一个根标签，即剩下的必须嵌套在里面

6.标签必须闭合，不能缺少根元素，甚至用空标签都可以 

7.标签首字母大写

8.label要用htmlFor

9.react中的列表循环有且只有map可以使用，因为只有map才有返回值

​										1）若小写开头 则将该标签转换为html中的同名元素，若无对应的同名元素则报错								

​										2）若大写开头 则找react的对应组件  没有定义则报错


## 1.2 模块与组件化

模块：提供特定功能的js程序 一般就是一个js文件

组件：用来实现局部功能效果的代码和资源的集合

rcc-react class component

rfc-reactt function component

通过使用组件来提高耦合性，更易于维护

定义组件分为3步：

 1、导入React核心模块

 2、定义组件类

 3、导出组件

# 第二章 类组件ClassComponent

react开发者组件，如果网站未打包上线则是红色

components:记录组件组成 profiler：记录网站性能

## 2.1 生命周期

> 组件从被创建到被销毁的过程。

![image-20220617191329676](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220617191329676.png)

#### Mounting—出生

* constructor

  先执行constructor里面的函数，初始化**数据**

* render

​	初始化数据之后，页面完成初次渲染

* DidMount

​	渲染完成后进行通知

#### Updating—长大

* render

​	数据（new props）的更新引起视图的重新渲染

* DidUpdate

​	更新渲染完成后，进行通知

#### Unmounting-寄

* WillUnmount

​	卸载阶段要做什么

>为什么是Will而不是Did？
>
>只有在挂载的时候才能发出通知，相当于只有在活着的时候才能说话 

具体过程：

1.初始化阶段

componentWillMount：render修改之前最后一次修改状态的机会，在此阶段做一些状态的初始化，状态的定义，拿不到真实的DOM节点

>React16之后已经不推荐使用，需要在前面加上UNSAFE_或直接不用，把该放的放到constructor或DidMount中去

##### 原因：ReactFiber

>根据浏览器每一帧执行的特性，构思出了Finer来将一次任务拆解成单元，以划分时间片的方式，按照FIber的自己的调度方法，根据任务单元优先级，分批处理，将一次更新分散在多次时间片中。
>
>另外，浏览器空闲的时候，也可以继续去执行未完成的任务，充分利用浏览器每一帧的工作特性。

![img](https://pic3.zhimg.com/80/v2-398077dda18dd8a2055dc21c442e39e6_720w.jpg)

通过 React 的 Diff 算法比较旧虚拟 DOM 树和新虚拟 DOM 树之间的 Change ，然后批处理这些改变



WillMount任务优先级比较低（找哪些节点挂载到界面中），有可能会被高优先级（render、

DidMount ）任务打断，打断之后会重新再执行一次，执行次数多了就失去了唯一性，不够安全

componentDidMount：

成功render并渲染完成真实DOM之后触发，可以修改DOM

用来放一些请求，订阅函数调用，setInterval，基于创建完的dom进行初始化

render：只能访问this.props和this.state,不允许修改状态和DOM输出

## 2.2 事件、state与setState

把要更新的值都放在state里，更新数值用setState

仅仅更改数值并不能在界面上直接显示出来，所以需要state

```jsx
import React from 'react'

export default function App2() {
    state={
        num:1
    }
  return (
      <div>
          <h2>数字为：{this.state.num}</h2>
          <button onClick={()=>this.setState({num:this.state.num+1})}></button>
      </div>
  )
}
```



### 对类的复习

若A类继承了B类 且A中写了构造器 那么A类构造器中的super是必须要调用的

类中所定义的方法  都是放在原型对象中的 供实例对象去使用
### setState的三种写法

###  useState

第一个参数，第二个放用来修改参数的方法

### 函数式组件

函数式组件没有生命周期

函数式组件没有this

函数式组件没有state状态



## 2.3受控组件与不受控组件

#### 不受控组件：

和组件本身state数据没有关系，所以不受组件管理

#### 受控组件：

表单元素的value值受组件state数据控制，表单中有onChange事件，可以在事件中对表单做实时验证，验证是否合法然后做相应操作

## 2.4 传值、父传子、子传父

无论是父传子还是子传父，真正在做事的都是父组件，即使是子传父，也是父组件给子组件提供了一个方法来实现的。

### 父传子

以属性的形式传给组件即可

```jsx
let msg = "你好世界"

export default function App(){
    return <Sub msg={msg} />
}

// 子组件
export default function Sub(props) {
  return <h2>{props.msg}</h2>
}
```

### 子传父

```jsx
export default function App(){
    const fn = function(arg){
        console.log(arg)	// 123
    }
    return <Sub msg={msg} fn={fn} />
}

// 子组件
export default function Sub(props) {
  return (
      <>
        <h2>{props.msg}</h2>
        <button onClick={()=>props.fn(123)}>将123传递给父组件</button>
      </>
  )
}
```

### this.state

先在state里面定义 之后再使用

```jsx
// 定义状态数据：
constructor(props){
  super(props)

  this.state = {
    num: 20
  }
}

// 使用状态数据：
return (
  <div>
    <p>{this.state.num}</p>
  </div>
)
```

### this.props

### 



# 第三章 React Hooks

只修改变量如果没有更新视图仍然不能在界面上有所体现

hook只能用在组件函数的最顶层 

### 函数式组件

创建函数式组件时 首字母必须大写 render方法的第一个参数写函数的同名标签，此处标签必须闭合，如果不习惯写自闭合可以写两个。

过程：react解析组件标签，找到相应组件，发现组件是用函数定义的，随后调用该函数，将返回的虚拟dom转为真实dom，随后呈现在界面上

## 3.1 useState

函数组件没有state，无法通过setState来更新视图。

在react包中导入useState钩子，然后在组件函数的顶部调用useState（）

### 3.11读取状态

被调用时，返回一个数组，第一项是一个状态值

```jsx
const stateArray = useState(false);
stateArray[0]; // => 状态值
```

一般通过数组解构提取到变量上。

第二项是一个更新状态的函数，用来更新第一项。

```jsx
const [state, setState] = useState(initialState);
setState(newState);
```

调用更新器后将重新渲染，使新状态变为当前状态。

### 3.12 一些注意事项

* 仅能在顶层调用Hook（不能在循环条件嵌套函数中调用），多个useState()调用中，渲染之间的调用顺序必须相同
* 仅在函数组件或自定义钩子内部调用useState（）
* 在单个组件中可以用多个状态，调用多次useState()

### 3.13 复杂状态管理&useReducer（）

>官方提供的两种状态管理的hook：useState和useReducer.
>
>没有useReducer也能完成正常开发，但使用它可以让代码具有更好的可读性、可维护性、可预测性

#### 什么是reducer

>是一个函数（state,action）=>newState
>
>接受当前应用的state和动作action，计算并返回最新的state

例：计算器reducer

```jsx
 // 计算器reducer，根据state（当前状态）和action（触发的动作加、减）参数，计算返回newState
    function countReducer(state, action) {
        switch(action.type) {
            case 'add':
                return state + 1;
            case 'sub':
                return state - 1;
            default: 
                return state;
        }
    }
```

#### reducer的幂等性

本质上是一个纯函数，相同的输入无论执行多少遍都会返回相同的输出（newState）

#### state和newState的理解

当state是一个复杂的js对象时：
```jsx
  // 返回一个 newState (newObject)
    function countReducer(state, action) {
        switch(action.type) {
            case 'add':
                return { ...state, count: state.count + 1; }
            case 'sub':
                return { ...state, count: state.count - 1; }
            default: 
                return count;
        }
    }

```

1.处理的state对象必须是immutable，永远不要直接修改参数中的state对象的值而是返回一个新的state object;

2.因为reducer要求每次返回一个新的对象，可以用ES6中的解构赋值方式去创建一个新对象，并复写需要改变的state属性。

#### action理解

>action：用来表示触发的行为

1.用type来表示具体的行为类型

2.用payload携带数据

例

```jsx
  const action = {
        type: 'addBook',
        payload: {
            book: {
                bookId,
                bookName,
                author,
            }
        }
    }
```

reducer是一个利用action提供的信息将state从A转换到B的一个纯函数：

* 语法： (state, action) => newState
* Immutable：每次都返回一个newState， 永远不要直接修改state对象
* Action：一个常规的Action对象通常有type和payload（可选）组成
  * type： 本次操作的类型，也是 reducer 条件判断的依据
  * payload： 提供操作附带的数据信息

#### Wen Reducer?

>当state是一个数组或对象，变化复杂，一个操作需要修改很多state的时候

对于复杂的状态管理，可以使用useReducer()这一hook,将reducer提取到一个单独的模块中，并在其他组件中重用它。

```jsx
const [state, dispatch] = useReducer(reducer, initState);
```

```jsx
//官方useReducer Demo
//第一个参数 应用的初始化
 const initialState = {count: 0};
    // 第二个参数：state的reducer处理函数
    function reducer(state, action) {
        switch (action.type) {
            case 'increment':
              return {count: state.count + 1};
            case 'decrement':
               return {count: state.count - 1};
            default:
                throw new Error();
        }
    }
    function Counter() {
        // 返回值：最新的state和dispatch函数
        const [state, dispatch] = useReducer(reducer, initialState);
        return (
            <>
                // useReducer会根据dispatch的action，返回最终的state，并触发rerender
                Count: {state.count}
                // dispatch 用来接收一个 action参数「reducer中的action」，用来触发reducer函数，更新最新的状态
                <button onClick={() => dispatch({type:'increment'})}>+</button>
                <button onClick={() => dispatch({type: 'decrement'})}>-</button>
            </>
        );
    }
```



## 3.2 useRef

使用useRef来获取某个组件或元素，返回一个可变的 ref 对象。

ref就是一个类似id的属性，用于获取dom元素

本质上，useRef就是一个其.current属性保存着一个可变值“盒子”.

### 3.21 Refs and the DOM

>ref提供了一种方法允许我们访问DOM节点或在render方法中创建的React元素

作用：提供一种不同于props的方式，在典型数据流之外强制修改子组件。

#### 三种用法：

* String类型（已过时）
* 回调函数
*  React.createRef() 

#### String类型的ref有什么问题？

<img src="C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220617192926995.png" alt="image-20220617192926995" style="zoom:150%;" />

* 需要React追踪当前呈现的组件（不能猜测this），使React变慢。

* 并不按照大多数人期待的“渲染回调模式”来运行，因为ref会被放置在 DataGrid上由于以上原因。

  >渲染回调模式（React Render Callback Pattern）：
  >
  >将this.props.children当做函数来调用。
  >
  >数据表格（DataGrid）
  >
  >![image-20220617194929473](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220617194929473.png)

* 不可组合（composable）。

#### 何时使用

避免使用refs来做任何可以通过声明式实现来完成的事情。

* 管理焦点，文本选择或媒体播放
* 触发强制动画
* 集成第三方DOM库

#### 创建Refs

通过ref属性附加到React元素

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();//创建
  }
  render() {
    return <div ref={this.myRef} />;//放进ref属性中
  }
}
```

#### 访问Refs

被传递给render中元素时，可以在ref的current属性中被访问。

```jsx
const node =this.myRef.current
```

注意：不能在函数组件上使用ref属性，因为他们没有实例。

但可以在函数组件内部使用ref属性，只要它指向一个DOM元素或class组件。

```jsx
function CustomTextInput(props){
//必须声明 textInput  ref才可以引用它
    const textInput=useRef(null);
    function handleClick(){
       textInput.current.focus();
  }
  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

#### 回调Refs

>可以更精细地控制何时refs被设置和接触。

不同于createRef（）创建的ref属性，通过回调ref会传递一个函数。

这个函数接受react组件实例或HTML DOM元素作为参数，使他们能在其他地方被存储和访问。

例子：使用ref回调函数，在实例的属性中存储对DOM节点的引用。

```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }

  render() {
    // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

React在组件挂载时，会调用ref回调函数并传入DOM元素，当卸载时调用它并传入null。

在componentDidMount或componentDidUpdate触发前，React会保证refs一定是最新的。

```jsx
import {useRef} from 'react'

function App5(){
    const element = useRef(null)

    const handleClick = () => {
        console.log(element.current.)    		// 获取input
        console.log(element.current.value)  // 获取到input中的值
        
    }
    return (
        <>
            <input type="text" ref={element} />
            <button onClick={handleClick}>获取input标签</button>
        </>
    )
}

export default App5;
```

## 3.3 memo

React 中当组件的 props 或 state 变化时，会重新渲染，实际开发会遇到不必要的渲染场景。

父组件更新时，子组件被迫更新，会造成性能损耗，此时需要用memo来缓存子组件，避免被迫更新

```jsx
import React, {useState} from 'react'

// 子组件
function Sub(){
    console.log('子组件被更新了')
    return <div>子组件</div>
}

// 父组件
export default function App(){
    const [msg, setMsg] = useState("你好世界");
    return (
        <>
            <h2>内容为：{msg}</h2>
            <button onClick={()=>setMsg("Hello World")}>修改Msg</button>
            <hr />
            <Sub />
        </>
    )
}

```



![子组件被迫更新](https://tva1.sinaimg.cn/large/e6c9d24egy1gzt1nekhxlg20gn04yqv5.gif)

用memo包裹住子组件即可

```jsx
const Sub = memo(() => {
    console.log('子组件被更新了')
    return <div>子组件</div>
})
```

注意，此处父组件只是简单调用子组件，并未给子组件传递任何属性

### 如果父组件的某个属性给子组件通过props传了值呢？

## 3.4 useCallback与useMemo

>The `useCallback` and `useMemo` Hooks are similar. The main difference is that `useMemo` returns a memoized *value* and `useCallback` returns a memoized *function*. 

一个返回值，一个返回函数。

useCallback示例

```jsx
const doSth=useCallback(()=>{setNum(num=>num+1)
  },[])//useCallback里丢进函数 第二个参数丢一个空数组代表不检测更新
```

### 3.41useCallback基础用法

与useState用法基本一致，但最后会返回一个函数，用一个变量保存起来。

返回的函数a会根据b的变化而变化，如果b始终未发生变化，a也不会生成，避免在不必要的情况下更新

情景：通过属性传递给子组件

比如一个父组件中

```jsx
const a = useCallback(() => {
	return function() {
		console.log(b)
	}
},[b])
console.log(a)
console.log(a())
```

* 第一种用法：父子组件函数式传参

  子组件受到的参数不变（空数组），自然不会更新，从而减少了组件间不必要的更新

### 3.42 useMemo

与useCallback差不多，只是useMemo需要再套一个函数

```jsx
const mySetMsg = useCallback(() => setMsg(()=>"Hello World"), [])	
//注意useMemo

const mySetMsg = useMemo(()=>{
  return () => setMsg("Hello World")
}, [])

```

## 3.5 useEffect

```jsx
//通过使用这个hook 告诉react组价需要在渲染后执行某些操作
useEffect(callback,array)
```

**粗浅**理解：把生命周期的DidMount和DidUpdated做了一个简单的合并

>不同点：
>
>***使用useEffect调度的effect不会阻塞浏览器更新屏幕，让应用看起来响应的更快***																												——React官网

**每次渲染后都会执行**：在组件初次渲染时会执行，在组件完成更新时也会执行。

为什么在**组件内部调用**：可以直接访问count state变量

执行时间点：在**真实**DOM构建完成之后

执行方式：**异步**

>***如果你的effect返回一个函数，React将在执行清除操作时调用它***
>
>​																							-----React官网

清理函数执行时机：在每一次运行副作用函数之前运行

 1.render+useEffect

2.render+清理函数(杀死上一个useEffect)+useEffect

3.render+清理函数(useEffect)+useEffect

4.循环



副作用：纯函数在引用外部变量或调用外部函数时

在进行数据请求，检测数据更新，和垃圾回收时比较常用

用来检测数据更新时

>想监测哪一个更新就把这个变量写在数组中
>
> 当要检测页面中所有变量时 可以把所有变量都填写到数组中 也可以直接不写数组 即第二个参数
>
> 当不想检测页面中任何数据时  直接写一个空数组 即[ ]

只要不是在组件渲染时用到的变量，所有的操作都是副作用

使用场景：

* 修改DOM
* 修改全局变量
* ajax请求
* 计时器
* 存储相关

即和外部变量的交互都需要用到副作用

### 3.51 useEffect&useLayoutEffect

#### 差异：

* useEffect是异步执行的，而useLayoutEffect是同步执行的
* useEffect执行时机是浏览器完成渲染之后，而useLayoutEffect执行时机是浏览器把内容真正渲染到界面之前同步执行，和componentDidMount等价

一个例子

```jsx
import React, { useEffect, useLayoutEffect, useState } from 'react';
import logo from './logo.svg';
import './App.css';

export default  function App() {
  const [state, setState] = useState("hello world")

  useEffect(() => {
    let i = 0;
    while(i <= 100000000) {
      i++;
    };
    setState("world hello");
  }, []);

  // useLayoutEffect(() => {
  //   let i = 0;
  //   while(i <= 100000000) {
  //     i++;
  //   };
  //   setState("world hello");
  // }, []);

  return (
    <>
      <div>{state}</div>
    </>
  );
}
```

如果是用useEffect，点击刷新后会先闪烁一下helloworld然后再变成helloworld，而换成useLayoutEffect之后闪烁现象就会消失

所以最好把操作dom相关的操作放到useLayoutEffect中去，避免导致闪烁。

* 优先使用useEffect，因为它是异步执行的，不会阻塞渲染

* 会影响渲染的操作尽量放到useLayoutEffect中去，避免闪烁问题
* useLayoutEffect和componentDidMount是等价的，会同步调用，阻塞渲染

## 3.6useContext&createContext

绕开父组件，直接从顶级组件传到子组件,实现跨级组件传值

### 方法一：使用useContext来调用上下文

```jsx
// 1、创建上下文
const NumContext = createContext();

// 子组件
function Count(){
    // 3、绕过父组件调用上下文内容
    const num = useContext(NumContext)
    return (
        <h3>{num}</h3>
    )
}

 function Father(){
     return <Count/>
 }

//顶级组件
export default function App(){
    const [num,setNum]=useState(0)
    return(
    <NumContext.Provider value={num}>
            <Father/>
     </NumContext.Provider>   
    )
}
```

### 方法二：使用Provider与Consumer

```jsx
function Child() {
    
    return(
        <NumContext.Consumer>
            {//用花括号是因为要结构出num和setNum
                ({num,setNum})=>
                (
                    <>
                    <h2>{num}</h2>
                    <button onClick={()=>setNum()}>修改num</button>
                    </>
                )
            }
        </NumContext.Consumer>
    )
    
}
export default function App() {
    const [num,setNum]=useState(123)
    //提供给子组件用来修改num的函数
    //直接放在共享空间里面
   return(
       <NumContext.Provider value={{num,setNum}}>
           <Father />
       </NumContext.Provider>
   )
  
```



## 3.7 LazyLoad

## 3.8 Suspense



# 第四章 状态管理工具React-Redux

>只有遇到 React 实在解决不了的问题，你才需要 Redux
>
>​                                                                                      ——Dan Abramov

Redux三大核心概念:

* Action
* Reducer
* Store

## 4.1 Redux使用场景

* 不同身份的用户有不同的使用方式
* 某个组件的状态需要共享
* 某个状态需要在任何地方都可以看到
* 一个组件需要改变全局状态



### Why is a Redux reducer called a reducer?

>It's called a reducer because it's the type of function you would pass to [Array.prototype.reduce(reducer, ?initialValue)](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

redux当中的reducer之所以被叫做reducer，是因为它与Array.prototype.reduce当中传入的回调函数非常相似

### 跳转某个页面并携带参数

### 4.2 提供器与连接器

在顶级组件的外层引入

```jsx
import {Provider}from 'react-redux'
```

在需要调用数据的组件中使用连接器

```jsx
import {connect} from 'react-redux'
```

### 4.3 ReactRedux流程图

![img](https://tva1.sinaimg.cn/large/008eGmZEgy1gpp69hggimj30ya0i1tdp.jpg)



# 第五章 路由

用来管理url与视图之间的关系

## 5.1路由原理

 1、准备视图(html)

 2、准备路由的路线(可以是一个对象，键名是路线名称和值是视图地址)

 3、通过hash地址的路线，获取“视图地址”

 4、在指定标签中，加载需要的视图页面

## 5.2React-router-dom

两种模式 BrowserRouter(History模式)和 HashRouter（hash模式） 所有路由都必须放在这两种里面

```jsx
const BaseRouter=()=>{
    return(
      <BrowserRouter>
       <Routes>
       <Route path="/"element={<App/>}>
           <Route path="//src/pages/home.jsx" element={<home/>}></Route>
           <Route path="//src/pages/list.jsx" element={<list/>}></Route>
           <Route path="//src/pages/detail.jsx" element={<detail/>}></Route>
           
       </Route>
       <Route path="*" element={<Error/>}></Route>   
       </Routes>
       
      </BrowserRouter>
    )//让整个界面显示404  route与routes同级
}
```

定义一个路由，每一个路由就是一个route标签，所有Route放在Routes

#### 跳转

在react-router-dom中用link标签代替

## 5.3涉及到的hooks

#### useSearchParams

用于读取和修改当前位置的url中的查询字符串‘

返回包含两个值的数组，内容分别为：当前的search参数，更新的search参数

```jsx
import {useSearchParams} from 'react-router-dom'
export default function Detail(){
    const [search,setSearch]=useSearchParams()
    const id=search.get('id')
    const title=search.get('title')
    const content=search.get('content')
return(
	<ul>
        <li>
            <button onClick={()=>setSearch('id=008&title=hhh&content&123')}>点我更新</button>
        </li>
        <li>编号:{id}</li>
        <li>标题:{title}</li>
        <li>内容:{content}</li>
    </ul>   
)
  }
```

#### useLocation

可以获取当前地址栏的路径

```jsx
let {pathname}=useLocation()
```

#### useNavigate

使用这个路由hook可以实现路由跳转。

```jsx
export default function App99() {
    const location=useLocation()
    const navigate=useNavigate()
    console.log(location.pathname)
    }
//跳转详情页
const goDetail=()=>{
    navigate('/src/pages/detail.jsx',{
        state:{username:"张三"}
    })
}
```

#### useParams

用于返回当前匹配路由的params参数



#  遇到的Bug（回忆版，原来写的老多忘保存...

## 最长、最折磨

![image-20220613164257182](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220613164257182.png)

todoList is not inheritale

控制台反复提示这一行报错

点击加号按钮后无法弹出列表项，反复检查代码和依赖路径后依然无法找到原因

![image-20220613163700037](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220613163700037.png)

加到课程微信找到源码 每一个文件一行一行文件比较后除了少数变量命名区别仍无法找到任何区别

当时其他部件已经书写完毕，但第一遍写的过程中没有边写边运行，导致当时两种选择：

1.先跳过此步，继续调试后面，看是否仍然会出现错误

2.重新开一个DebugDemo,复现到做添加待办事项时的这一步

再次开了一个demo并且再次安装好所有依赖和路径之后，做到这一步时神奇的事情发生了能正常添加列表项，但在编写三个功能组件时出了问题：

当时已经写好了查看按钮功能，按理说点击后应该跳出详情页，但点击后无反应，也不报错;

此时编辑、删除按钮组件未编写，按下后会正常提示xxx is not a function

对比 源码和视频仍无法找到区别，右键忽略后竟然能顺利运行，再次百思不得其解

用源码覆盖后依然出现问题，思考可能不是代码的问题：

### 1.依赖库的版本？

很快就排除了，scss、boostrap和yarn都是安的最新的，Google了下很快排除了

### 2.浏览器的原因？

一点点查，发现加的数据无法在localstorage里存储，这时恰好断网，看了眼梯子发现挂了

正好用的是Chrome，思考会不会是梯子的原因？

重新连了下网，发现能正常存储了，问题解决...







![image-20220613174929938](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220613174929938.png)

一个视图改动

localhost3000之后的都没问题

## 引入Ant-design

![image-20220614155251103](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220614155251103.png)

 

​                                                          	
