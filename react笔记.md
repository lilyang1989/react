需要用到的东西

reacthooks webpack ajax   

后缀可以写js也可以写jsx 

各个组件放到app.js中时  以标签的形式放置  在属性中带着自己的方法

关于ant design--在create react app中使用

### 组件梳理

最大：app.js

## 点击加号

1.触发openInput

2.openInput通过触发setInputShow将是否设置输入框isInputShow设置为是,

3.addInput函数一眼鉴定为isInputShow的值为true 展示输入框

### 点击输入框

1.查空，判断inputValue是否等于0

2.输入要干的事情inputValue

3.点击右边的新增，调用addItem把inputValue传进去，通过addItem父子传值

4.todoList是一个数组，放入新的数组就会更新

5.addItem包含dataItem和settodoList和setInputShow

dataItem包含id content  completed。

settodoList把todoList和dateItem合并在一起

6.点击添加 要隐藏输入框 通过setInputShow置为false

### 列表项

1. 判断是否完成此待办事项,即checked还是非checked，通过data.completed来 判断
2. 如果已完成，在textDecoration里面加上line-through
3. 

# 第一章 一些前置知识

* babel 能将jsx转成js

* react development是react的核心

* react-dom  react扩展库

* 只有先引入核心库才能引入扩展库

* 标签type必须写babel

* 标签内不能加单引号



## jsx规则：

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


## 模块与组件化

模块：提供特定功能的js程序 一般就是一个js文件

组件：用来实现局部功能效果的代码和资源的集合

rcc-react class component

rfc-reactt function component



# 第二章 面向组件编程

react开发者组件，如果网站未打包上线则是红色

components:记录组件组成 profiler：记录网站性能

### 函数式组件

创建函数式组件时 首字母必须大写 render方法的第一个参数写函数的同名标签，此处标签必须闭合，如果不习惯写自闭合可以写两个。

过程：react解析组件标签，找到相应组件，发现组件是用函数定义的，随后调用该函数，将返回的虚拟dom转为真实dom，随后呈现在界面上

### 对类的复习

若A类继承了B类 且A中写了构造器 那么A类构造器中的super是必须要调用的

类中所定义的方法  都是放在原型对象中的 供实例对象去使用
## setState的三种写法

##  useState

第一个参数，第二个放用来修改参数的方法

## 函数式组件

函数式组件没有生命周期

函数式组件没有this

函数式组件没有state状态

## 第三章 React Hooks

只修改变量如果没有更新视图仍然不能在界面上有所体现

hook只能用在组件函数的最顶层 

### 3.1 useState

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

### 3.12一些注意事项

* 仅能在顶层调用Hook（不能在循环条件嵌套函数中调用），多个useState()调用中，渲染之间的调用顺序必须相同
* 仅在函数组件或自定义钩子内部调用useState（）
* 在单个组件中可以用多个状态，调用多次useState()

### 3.13 复杂状态管理&useReducer（）

>官方提供的两种状态管理的hook：useState和useReducer.
>
>没有useReducer也能完成正常开发，但使用它可以让代码具有更好的可读性、可维护性、可预测性

###  什么是reducer

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

### reducer的幂等性

本质上是一个纯函数，相同的输入无论执行多少遍都会返回相同的输出（newState）

### state和newState的理解

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

### action理解

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

### Wen Reducer?

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

使用useRef来获取某个组件或元素，返回一个dom对象。

ref就是一个类似id的属性，用于获取dom元素

本质上，useRef就是一个其.current属性保存着一个可变值“盒子”.

```jsx
import {useRef} from 'react'

function App5(){
    const element = useRef(null)

    const handleClick = () => {
        console.log(element.current)    		// 获取input
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

## 3.3 useCallback与useMemo

>The `useCallback` and `useMemo` Hooks are similar. The main difference is that `useMemo` returns a memoized *value* and `useCallback` returns a memoized *function*. 

useCallback示例

```jsx
const doSth=useCallback(()=>{setNum(num=>num+1)
  },[])//useCallback里丢进函数 第二个参数丢一个空数组代表不检测更新
```

#### 3.31useCallback基础用法

与useState用法基本一致，但最后会返回一个函数，用一个变量保存起来。

返回的函数a会根据b的变化而变化，如果b始终未发生变化，a也不会生成，避免在不必要的情况下更新

```jsx
const a = useCallback(() => {
	return function() {
		console.log(b)
	}
},[b])
console.log(a)
console.log(a())
```





## 第四章 React-Redux

Redux三大核心概念:

* Action
* Reducer
* Store

### Why is a Redux reducer called a reducer?

>It's called a reducer because it's the type of function you would pass to [Array.prototype.reduce(reducer, ?initialValue)](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

redux当中的reducer之所以被叫做reducer，是因为它与Array.prototype.reduce当中传入的回调函数非常相似

## 跳转某个页面并携带参数

## 第五章 提供器与连接器

