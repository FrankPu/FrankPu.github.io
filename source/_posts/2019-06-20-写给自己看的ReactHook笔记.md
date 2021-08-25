---
title: 写给自己看的ReactHook笔记
date: 2019-06-20 10:19:44
tags: Hook
categories: React
---

[Hook](https://react.docschina.org/docs/hooks-effect.html)是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性

<!-- more -->

## Hook简介

### 什么是Hook

Hook是一个特殊的函数，它可以让你“钩入” React 的特性。

### 什么时候我会用Hook

如果你在编写函数组件并意识到需要向其添加一些 state，以前的做法是必须将其它转化为 class。现在你可以在现有的函数组件中使用 Hook。

### 为什么以use开头

你可能想知道：为什么叫 `useState` 而不叫 `createState`?

“Create” 可能不是很准确，因为 state 只在组件首次渲染的时候被创建。**在下一次重新渲染时，`useState` 返回给我们当前的 state。**否则它就不是 “state”了！这也是Hook的名字*总是*以 `use` 开头的一个原因。

## 使用State Hook

在State Hook中我们会用到`useState`，它是允许你在 React 函数组件中添加 state 的Hook

### 等价代码

在这里我们举一个简单的计数器例子，以前class的写法：

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

现在Hook的写法：

```javascript
// 1.引入useState Hook
import React, { useState } from 'react';

function Example() {
  // 2.声明一个叫"count"的state变量，和可以更新它的函数"setCount"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      // 3.点击后，更新"count"的值，重新渲染"Example"组件，并传入更新后的"count"
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

在函数组件中，我们**没有**`this`，所以不再通过`this.state`来读取属性，而是通过`useState`调用。

### useState在调用时做了什么

1. 它定义一个 “state 变量”。
   1. 这个例子中我们的变量叫 `count`， 但是我们可以叫他任何名字，比如 `banana`。
   2. 这是一种在函数调用时保存变量的方式 —— `useState`是一种新方法，它与 class 里面的 `this.state` 提供的功能完全相同。
   3. 一般来说，在函数退出后变量就就会”消失”，而 state 中的变量会被 React 保留。

### useState的返回值是什么

返回值为：①当前 state，②更新 state 所调用的函数。

`const [count, setCount] = useState(0);`

1. 我们声明了一个叫 `count` 的 state 变量，然后把它设为 `0`。React 会在重复渲染时记住它当前的值，并且提供最新的值给我们的函数。
2. 我们可以通过调用 `setCount` 来更新当前的 `count`。

### 读取State

在函数写法中，我们直接用`count`：

```javascript
<p>You clicked {count} times</p>
```

以前的class写法中，是`this.state.count`

### 更新State

在函数写法中，我们调用在`useState`时返回的第二个参数，来更新数据。

在本例子中，即`setCount`：

```javascript
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```

以前的class写法中，是`this.setState({count: this.state.count + 1})`

### 使用多个state变量

```javascript
function ExampleWithManyStates() {
  // 声明多个 state 变量
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: '学习 Hook' }]);
```

## 使用Effect Hook

使用Effect Hook的一个主要目的是解决class中同一生命周期经常包含不相关的逻辑，但又把相关的逻辑分离到了几个不同方法中的问题。

### 什么是Effect Hook

useEffect 相当于componentDidMount，componentDidUpdate，componentWillUnmount 这三个函数的组合

```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // 订阅
    document.title = `You clicked ${count} times`;

    // 清除（可选）
    return () => {
      // do something cleanup
    }
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

在以前class写法中，如果我们需要在组件加载和更新时都调用同样的方法，就需要在componentDidMount和componentDidUpdate写重复的代码。

使用useEffect，react会保存你传递的函数（我们称之为'effect'），并且在**DOM更新之后**调用它。减少了重复代码。useEffect调用effect不会阻塞浏览器刷新屏幕，这让你的应用响应更快。（如果需要同步地执行，如瀑布流，使用useLayoutEffect Hook）

Hook使用的JS的闭包机制：useEffect放在组件内部让我们可以在effect中直接访问count state变量（或其他props），它已经保存在了函数作用域中，而不需要其他API。

区别于vue Hook，useEffect在第一次渲染之后和每次更新之后都会执行。

### 清除Effect

在class写法中，我们在 `componentDidMount`中订阅，在`componentWillUnmount`中清除。

使用useEffect，在尾部**返回**一个清除函数，react就会在**组件卸载时执行清除操作**来调用它。这样订阅和移除订阅的逻辑都放在了一起。

另外，返回的清除函数也可以使用箭头函数而不需要命名。

### 使用多个Effect分离逻辑

就像你可以使用多个state的Hook，你也可以使用多个effect来将不同的逻辑分离。**Hook允许我们按照代码的用途分离他们，** 而不是像生命周期函数那样。React 将**按照 effect 声明的顺序依次调用**组件中的*每一个* effect（vue Hook是没有顺序的）。

### 为什么每次更新的时候都要运行Effect

例如一个好友在线状态的展示，如果是class写法，我们从props中获取`friend.id`，然后在`componentDidMount`订阅和`componentWillUnmount`清除订阅，**但是当组件已经显示在屏幕上时，`friend.id`改变导致BUG**，组件将继续展示原来的好友状态，而且我们还会因为取消订阅时使用错误的好友 ID 导致内存泄露或崩溃的问题。我们必须使用`componentDidUpdate`来解决问题，如下代码：

```javascript
  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  // 添加这里的代码解决BUG
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

  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
```

使用Hook版本：

```javascript
function FriendStatus(props) {
  // ...
  useEffect(() => {
    // ...
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

没有添加任何额外的逻辑改动，却可以不受此BUG影响。**因为默认在调用一个新的effect之前会对前一个effect进行清理。**

### 跳过 Effect 进行性能优化

在某些情况下，每次渲染后都执行或清理effect会导致性能问题。

在class写法中，通过`componentDidUpdate`解决：

```javascript
// 判断是否更新了
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
```

在Hook中，传递数组作为useEffect的第二个参数即可：

```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```

对于清除effect同样适用：

```javascript
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
}, [props.friend.id]); // 仅在 props.friend.id 发生变化时，重新订阅
```

## Hook规则

### 只在最顶层使用Hook

1. **不要**在**循环**，**条件**，**嵌套函数**中调用Hook

   1. 如果想要有条件的执行一个effect，可以在Hook内部判断

      ```javascript
        useEffect(function persistForm() {
          // 👍 将条件判断放置在 effect 中
          if (name !== '') {
            localStorage.setItem('formData', name);
          }
        });
      ```

2. 确保总在React函数**最顶层**调用Hook

3. 确保Hook**按照顺序**执行

4. **不要**在**普通JavaScript函数**中调用Hook

5. Hook在class内部是**不**起作用的

### ESLint插件

[eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks)插件可以强制Hook只在React函数中而非Js函数中调用（之后可能并入create react app）

`npm install eslint-plugin-react-hooks --save-dev`

```json
// 你的 ESLint 配置
{
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    "react-hooks/rules-of-hooks": "error", // 检查 Hook 的规则
    "react-hooks/exhaustive-deps": "warn" // 检查 effect 的依赖
  }
}
```

### 按顺序执行的Hook

在单个组件中可以使用多个**State Hook**或**Effect Hook**，因为Hook**必须按顺序执行**，所以React才能知道`state`与哪一个`useState`对应。

```javascript
function Form() {
  // 1. Use the name state variable
  const [name, setName] = useState('Mary');

  // 2. Use an effect for persisting the form
  useEffect(function persistForm() {
    localStorage.setItem('formData', name);
  });

  // 3. Use the surname state variable
  const [surname, setSurname] = useState('Poppins');

  // 4. Use an effect for updating the title
  useEffect(function updateTitle() {
    document.title = name + ' ' + surname;
  });

  // ...
}
```

上述代码的工作如下：

```javascript
// ------------
// 首次渲染
// ------------
useState('Mary')           // 1. 使用 'Mary' 初始化变量名为 name 的 state
useEffect(persistForm)     // 2. 添加 effect 以保存 form 操作
useState('Poppins')        // 3. 使用 'Poppins' 初始化变量名为 surname 的 state
useEffect(updateTitle)     // 4. 添加 effect 以更新标题

// -------------
// 二次渲染
// -------------
useState('Mary')           // 1. 读取变量名为 name 的 state（参数被忽略）
useEffect(persistForm)     // 2. 替换保存 form 的 effect
useState('Poppins')        // 3. 读取变量名为 surname 的 state（参数被忽略）
useEffect(updateTitle)     // 4. 替换更新标题的 effect

// ...
```

## 自定义Hook

1. 自定义Hook**解决了**以前在 React 组件中**无法灵活共享逻辑**的问题。
2. 自定义Hook**必须以 “use” 开头**，必须遵守[Hook的规则](https://react.docschina.org/docs/hooks-rules.html)。
3. 现在有3种方式来**共享组件之间的状态逻辑**：
   1. [render props](https://react.docschina.org/docs/render-props.html)
   2. [高阶组件](https://react.docschina.org/docs/higher-order-components.html)
   3. 自定义Hook

### 简单的`useReducer`Hook

```javascript
function useReducer(reducer, initialState) {
  const [state, setState] = useState(initialState);

  function dispatch(action) {
    const nextState = reducer(state, action);
    setState(nextState);
  }

  return [state, dispatch];
}
```

**在组件中使用它：**

```javascript
function Todos() {
  //使用useReducer管理state
  const [todos, dispatch] = useReducer(todosReducer, []);

  function handleAddClick(text) {
    dispatch({ type: 'add', text });
  }

  // ...
}
```
