标签很多属性

## 我看了啥



以及很多小视频

## 我用到了什么

ant-design  react-hooks  yarn

需要用到的东西 webpack ajax   

后缀可以写js也可以写jsx 

各个组件放到app.js中时  以标签的形式放置  在属性中带着自己的方法

关于ant design--在create react app中使用

### 业务（？）逻辑梳理

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
3. span里绑定data.content
4. div里放置三个button
5. 放入app.js
6. 在ul里进行map  

### 数据存到localStorage里

用useEffect

1.从localstorage里用getItem取数据（todoData），将字符串转换为json对象，如果没有直接给一个空数组

2.通过settoDoList把tododata放进去，让新的数据替换原来老的todoList的数据

3.再写一个useEffect 使得当todoList变化时 localstorage存入todoData

## 模态框组件model 插槽

先解构出：isShowModal,modalTitle,children



任何一个组件，先声明状态，是否显示？ isShowModal

modal里面的类 inner

model的标题m-header    里面嵌入modaltitle

盒子content-wrapper里面放：

model的内容：children 

点击确定关闭框 closeModel



## 关于提交数据



## 完成与否与删除

解构出 completedItem



给checkBox加上这一属性

再用usecallback

#### 删除

todoitem里解构出removeitem的





组件化的特性就是通过属性去传递方法和数据

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

## 生命周期

> 组件从被创建到被销毁的过程。

![image-20220609203931805](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220609203931805.png)

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

### 3.12 一些注意事项

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

情景：通过属性传递给子组件

```jsx
const a = useCallback(() => {
	return function() {
		console.log(b)
	}
},[b])
console.log(a)
console.log(a())
```

### 3.4 useEffect

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

### 3.5 useContext

### 3.6 LazyLoad

### 3.7 Suspense



## 第四章 React-Redux

Redux三大核心概念:

* Action
* Reducer
* Store

### Why is a Redux reducer called a reducer?

>It's called a reducer because it's the type of function you would pass to [Array.prototype.reduce(reducer, ?initialValue)](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

redux当中的reducer之所以被叫做reducer，是因为它与Array.prototype.reduce当中传入的回调函数非常相似

### 跳转某个页面并携带参数

### 4.2 提供器与连接器

### 4.3 ReactRedux流程图

![img](https://tva1.sinaimg.cn/large/008eGmZEgy1gpp69hggimj30ya0i1tdp.jpg)



# 遇到的Bug（回忆版，原来写的老多忘保存...

## 最长、最折磨

![image-20220613164257182](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220613164257182.png)

todoList is not inheritale

控制台反复提示这一行报错

点击加号按钮后无法弹出列表项，反复检查代码和依赖路径后依然无法找到原因

![image-20220613163700037](C:\Users\Yang\AppData\Roaming\Typora\typora-user-images\image-20220613163700037.png)

加到课程微信找到源码 一行一行比较后仍无法找到任何区别

当时其他部件已经书写完毕，但第一遍写的过程中没有边写边运行，导致当时两种选择：

1.先跳过此步，继续调试后面，看是否仍然会出现错误

2.重新开一个DebugDemo,复现到做添加待办事项时的这一步

再次开了一个demo并且再次安装好所有依赖和路径之后，做到这一步时神奇的事情发生了能正常添加列表项，但在编写三个功能组件时出了问题：

当时已经写好了查看按钮功能，按理说点击后应该跳出详情页，但点击后无反应，也不报错;
