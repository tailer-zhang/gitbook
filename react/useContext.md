# useContext Hook使用

无论组件在组件树中的深度如何，React 上下文都为组件提供数据。上下文用于管理全局数据，例如全局状态、主题、服务、用户设置等。

在这篇帖子中，您将学习如何在 React 中使用上下文概念。

## 1. 如何使用 context

在 React 中使用上下文需要 3 个简单的步骤：创建上下文、提供上下文和使用上下文。

## A. 创建上下文

内置的工厂函数（createContext)创建一个上下文实例：

```
import { createContext } from 'react'

const Context = createContext('default value')

```
工厂函数接受一个可选参数：default value

## B. Providing the context

contenxt实例上可用的 Context.Provider 组件用于为其子组件提供上下文，无论它们有多深

```
function Main() {
    const value = 'my context value'
    return (
        <Context.Provider value={value}>
            <Mycomponent />
        </Context.Provider>
    )
}

```

同样，这里重要的是所有稍后要使用上下文的组件都必须包装在提供程序组件中。

如果要更改上下文值，只需更新 value 属性即可。

## C. Consuming the context

可以通过两种方式使用上下文。

首先，我推荐使用react的useContext(Context) hook

```
import {useContext} from 'react'

functon Mycomponent () {
    const value = useContext(Context)
    
    return (
        <span>{value}</span>
    )
}

```

hook返回上下文的值：value = useContext(Context)。该钩子还确保在上下文值更改时重新渲染组件。

第二种方法是使用作为子组件提供给上下文实例上可用的 Context.Consumer 特殊组件的渲染函数：
```
function Mycomponent() {
    return (
        <Context.Consumer>
            {value => (<span>{value}</span>)}
        </Context.Consumer>
    )
}

```
同样， 在这个案例中如果context value改变， Context.Consumer将会重新渲染render函数

<img src="./react-context-3.svg" width="500" />

对于单个上下文，您可以拥有任意数量的consumers。如果上下文值发生变化（通过更改提供者的 value 属性 <Context.Provider value={value} />），那么所有consumers都会立即收到通知并重新渲染。

如果consumer没有被包装在提供者中，但仍然尝试访问上下文值（使用 useContext(Context) 或 <Context.Consumer>），那么上下文的值将是提供给 createContext(defaultValue) 的默认值参数) 创建上下文的工厂函数。

