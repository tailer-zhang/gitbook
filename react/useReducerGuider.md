# useReducer Hook使用

如果你已经使用useState()钩子管理非琐碎的状态比如条目列表，你需要新增、更新和移除状态里的条目，你可能已经注意到状态管理逻辑占据了组件一大部分。

这是一个问题，因为 React 组件本质上应该包含计算输出的逻辑。但是状态管理逻辑是一个不同的问题，应该在一个单独的地方进行管理。否则，您会在一个地方混合状态管理和渲染逻辑，这很难阅读、维护和测试！

为了帮助您分离关注点（渲染和状态管理），React 提供了钩子 useReducer()。钩子通过从组件中提取状态管理来实现

让我们继续深究 useReducer() 钩子是如何工作的。作为一个不错的奖励，您会在帖子中找到一个真实的例子，它极大地有助于理解reducer的工作原理。

## 1.useReducer()

useReducer 钩子接受 2 个参数：reducer 函数和初始状态。然后钩子返回一个包含 2 个项目的数组：state与dispatch

```
import { useReducer} from 'react';

funciton MyComponent() {
    const [state, dispatch] = useReducer(reducer, initState);
    
    const action = {
        type: 'ActionType'
    };
    
    return (
        <button type="button" onClick={()=> dispatch(action)}>
            点我
        </button>
    )
    
}

```
现在，让我们解读一下初始sate、action对象、dispatch和reducer这三个术语的含义。

## A. initial state

初始状态是状态被初始化的值。

以一个counter状态为例，它的初始值应该是

```
// initial state
const initialState = {
    counter: 0
}

```

## B.Action object

Action对象是描述如何更新状态的对象。

通常，Action对象会有一个属性类型——一个描述reducer必须进行什么样的状态更新的字符串。

例如，一个增加counter的动作对象可以如下所示

```
const action = {
    type: 'increase'
}

```
如果action对象必须携带一些有用的信息（payload)供reducer使用，你可以增加在这个action对象增加额外的属性

例如，这是一个将新用户添加到用户状态数组的操作对象

```
const action = {
    type: 'add',
    user: {
        name: '小明',
        email: '2239r8298@qq.com'
    }
}

```

user对象有一个保存有关要添加的用户的信息的属性

## C.Dispatch 函数

dispatch函数是通过useReducer()hook创建的

```
    const [state, dispatch] = useReducer(reducer, initialSate)

```
每当您想要更新状态时（通常来自事件回调或在完成获取请求之后），你只需使用适当的操作对象调用dispatch函数：dispatch(actionObject)。

## D.Reducer 函数


reducer 是一个纯函数，它接受 2 个参数：当前状态和动作对象，根据动作对象。reducer 函数必须以不可变的方式更新状态，并返回新状态

```
function reducer (state={}, action) {
    switch(action.type) {
        case 'increase':
            return {counter: state.counter +1}
            break;
        case 'decrease'
            return {counter: state.counter -1}
            break;
         default:
            return {...state}
    }
}

```
上面的 reducer 并没有直接修改 state 变量中的当前状态，而是创建一个新的 state 对象存储在 newState 中，然后返回它。

React 检查新状态和当前状态之间的差异来确定状态是否已更新，因此不要直接改变当前状态。

## E.小结

将所有这些术语连接在一起，以下是使用reducer进行状态更新的流程图

```
插入图片

```

作为事件处理程序的结果或在完成获取请求后，您可以使用 action 对象调用调度函数。

然后 React 将 action 对象和当前状态值重定向到 reducer 函数。

reducer 函数使用 action 对象并执行状态更新，返回新状态

React 然后检查新状态是否与前一个不同。如果状态已经更新，React 会重新渲染组件，并且 useReducer() 返回新的状态值：[newState, ...] = useReducer(...)。

如果所有这些术语听起来都太抽象了，那么您有正确的感觉！让我们在一个有趣的例子中看看 useReducer() 是如何工作的。


## 2.秒表案例

任务是实现一个秒表。秒表有 3 个按钮：开始、停止和重置，并有一个数字显示经过的秒数。

现在让我们考虑构建秒表的状态

有 2 个重要的state属性：一个布尔值指示秒表是否运行（我们将其命名为运行）和一个数字指示经过的秒数（我们将其命名为时间）。结果，初始状态如下所示：

```
const initialState = {
    isRunnig: false,
    time: 0
}

```

初始状态表示秒表在 0 秒时开始处于非活动状态。

然后让我们考虑一下秒表应该有哪些action对象。很容易发现，我们需要4种动作：启动、停止和重置秒表，以及tick。

```
{type: 'start'}

{type: 'stop'}

{type: 'reset' }

{type: 'tick'}

```

有了状态结构，以及可能的动作，让我们使用 reducer 函数来定义动作对象如何更新状态：

```
function reducer (state, action) {
    switch(action.type) {
        case 'start':
            return { ...state, isRunning: true };
            break;
        case 'stop': 
            return {...state, isRunning: false}
            break;
        case 'reset':
            return { isRunning: false, time: 0 };
            break;
        case 'tick':
            return { ...state, time: state.time + 1 };
            break;
        default: 
            throw new Error() 
    }
}

```

最后，这是通过调用 useReducer() 钩子将所有内容连接在一起的组件 Stopwatch：

```
import {useReducer, useRef} from 'react'

function Stopwatch() {
    const [state, dispatch] = useReducer(reduce, {isRunning: false, time: 0})
    const idRef = useRef(0);
    
    useEffect(()=>{
       if(!state.isRunning) return ;
       idRef.curent = setInterval(()=>{
            dispatch({type:'tick'})
        },1000)
       
       return ()=>{
        clearInterval(idRef.curent)
        idRef.curent = 0
       }
    },[state.isRunning])
    
    
  return (
    <div>
      {state.time}s
      <button onClick={() => dispatch({ type: 'start' })}>
        开始
      </button>
      <button onClick={() => dispatch({ type: 'stop' })}>
        结束
      </button>
      <button onClick={() => dispatch({ type: 'reset' })}>
        重置
      </button>
    </div>
    )
}

```

## 3.总结
React 中的 useReducer() 钩子让你将状态管理与组件的渲染逻辑分开。

const [state, dispatch] = useReducer(reducer, initialState) 接受 2 个参数：reducer 函数和初始状态。此外，reducer 返回一个包含 2 个项目的数组：当前状态和调度函数。

当您想更新状态时，只需使用适当的操作对象调用 dispatch(action) 即可。然后将操作对象转发到更新状态的 reducer() 函数。如果reducer 更新了状态，则组件重新渲染，并且[state, ...] = useReducer(...) 钩子返回新的状态值。

useReducer() 非常适合相对复杂的状态更新（至少需要 2-3 个更新操作）。对于简单的状态管理，只需使用 useState()。


