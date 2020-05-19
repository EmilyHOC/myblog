---
title: react知识点整理
date: 2020-05-18 14:59:42
tags: react 
---

##### react和vue的优势

[比较好的文章](https://libin1991.github.io/2019/09/07/%E9%9D%A2%E8%AF%95%E5%AE%98%EF%BC%9AVue-%E5%92%8C-React-%E7%9A%84%E4%BC%98%E7%82%B9%E5%88%86%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F%E4%BD%A0%E5%BA%94%E8%AF%A5%E8%BF%99%E4%B9%88%E7%AD%94%EF%BC%81/)

简单来说vue和react的区别：

1. `Vue进行数据拦截`，它对数据的变化更敏感。React是函数式编程，它进行局部重新刷新，简单粗暴，但是它不知道怎么去刷新，触发局部的变化是由开发者手动setState完成的。相比之下，vue采用依赖追踪，默认就是优化状态，动了多少数据，就触发多少更新,`react对数据变化毫无感知`，他就提供react.craeteElement调用已生成的Virtual dom,另外react为了弥补不必要的更新，会对setState进行合并，因此setState有时候会是异步更新，有时候会是同步更新
2. react的事件暴露给开发者的不是原生事件，而是`合成事件`，对所有事件 都进行了代理，将所有事件都绑定document上
3. `react中事件处理函数中的this默认不指向组件`，`vue中的this默认指向组件`

优势：

1. 单项数据流
2. 组件化（class componet和function component）
3. 结构更清晰

##### react父子组件通信

- 在 React 中，父组件可以向子组件通过传 props 的方式，向子组件进行通讯。
- 子传父：将父组件的方法传递给子组件，子组件通过this.props调用传递过来的方法，并带上参数（传一个方法把参数带过去）

##### 如何优化一个react项目

- key 设置唯一稳定的key
- `shouldComponetUpdate(nextProps,nextState)`为了避免多余的渲染，return false可以阻止组件的更新
- immutable.js

##### 什么是单向数据流

父组件传递给子组件的信息不允许在子组件内被更改

当想更改时候只需要接收数据后更改自身插件的内容即可

##### 单向数据流与双向数据绑定的联系与区别

[比较好的文章](https://blog.csdn.net/weixin_42755677/article/details/91047876)

单项数据流指的是数据从父组件流向子组件，表现在代码上就是父组件通过props传递给子组件，父级props的更新会向下流动到子组件中，反过来不行，`所以子组件不能改变props,如果要修改只能定义一个本地data，并将prop作为初始值`

`双向数据绑定指的是单纯的数据与视图层之间的绑定`。

对于非 UI 控件来说，不存在双向，只有单向。只有 UI 控件才有双向的问题。单向绑定使得数据流也是单向的，对于复杂应用来说这是实施统一的状态管理（如 Vuex）的前提。双向绑定在一些需要实时反应用户输入的场合会非常方便（比如表单提交）。但通常认为复杂应用中这种便利比不上引入状态管理带来的优势。

##### 单向数据流和双向数据绑定有什么优缺点？单向数据流优缺点

优点：

所有状态的改变可记录、可跟踪，源头易追溯;
`所有数据只有一份，组件数据只有唯一的入口和出口`，使得程序更直观更容易理解，有利于应用的可维护性;
一旦数据变化，就去更新页面(data-页面)，但是没有(页面-data);
如果用户在页面上做了变动，那么就手动收集起来(双向是自动)，合并到原有的数据中。
缺点：

HTML 代码渲染完成，无法改变，有新数据，就须把旧 HTML 代码去掉，整合新数据和模板重新渲染;
代码量上升，数据流转过程变长，出现很多类似的样板代码;
同时由于对应用状态独立管理的严格要求(单一的全局 store)，`在处理局部状态较多的场景时(如用户输入交互较多的“富表单型”应用)，会显得啰嗦及繁琐。`

##### 双向数据绑定的优缺点：

优点：

`用户在视图上的修改会自动同步到数据模型中去，数据模型中值的变化也会立刻同步到视图中去；`
无需进行和单向数据绑定的那些相关操作；
在表单交互较多的场景下，会简化大量业务无关的代码。
缺点：
无法追踪局部状态的变化；
“暗箱操作”，增加了出错时 debug 的难度；
由于组件数据变化来源入口变得可能不止一个，数据流转方向易紊乱，若再缺乏“管制”手段，血崩



##### 什么是属性接收强校验

1. npm install --save prop-types 
2. import PropTypes from 'prop-types' ;
3. 找到组件名(如TodoItem)
4. 对组件的属性做强校验

```
TodoItem.propTypes={

    content:PropTypes.string.isRequired

    //对TodoItem组建下接收的content 属性做字符串校验且必须传

    index:PropTypes.number

    deleteItem:PropTypes.func

}
```



解决未传值带来的错误可以设置默认值

TodoItem.defaultProps={content:'hello world'}



##### prop、state和render之间的关系

`当组件的state或者props发生改变的时候，render函数就会重新执行`。这里面数据不仅包括state，还包括props。`当父组件的render函数运行时，它的子组件的render都将被重新运行一次`

##### 关于super(props)

```
class Counter extends Component{
    constructor(props){
        super(props);
        this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
        this.onClickDecrementButton = this.onClickDecrementButton.bind(this);
        this.state = {
            count: props.initValue || 0
        }
    }
}
```

如果`一个组件需要定义自己的构造函数`，一定要记得在构造函数的第一行通过 super 调用父类也就是React.Component的构造函数。`如果在构造函数中没有调用super(props)，那么组件实例被构造之后，类实例的所有成员函数就无法通过 this.props 访问到父组件传递过来的 props 值`。

##### 什么时候需要组件的构造函数 constructor

- 初始化 state，因为组件生命周期中任何函数都可能要访问 state，那么整个生命周期中第一个被调用的构造函数自然是初始化 state 最理想的地方；

- 绑定成员函数的 this 环境；
  在ES6语法下，类的每个成员函数在执行时的 this 并不是和类实例自动绑定的。而在构造函数中，this 就是当前组件实例，所以，为了方便将来的调用，往往在构造函数中将这个实例的特定函数绑定 this 为当前实例。

  

##### 为什么要引入虚拟DOM

##### 虚拟DOM的Diff算法

##### 为什么不用index做key值

##### react中ref的使用

Refs 是一个 获取 DOM节点或 React元素实例的工具

refs 通常适合在一下场景中使用：

1. 对DOM 元素焦点的控制、内容选择或者媒体播放；

2. 通过对DOM元素控制，触发动画特效；

3. 通第三方DOM库的集成。

   使用方式

   关于refs的使用有两种方式: 1）通过 React.createRef() API【在React 16.3版本之后引入了】；2）在较早的版本中，推荐使用 回调形式的refs。

   不能在函数组件上使用 ref 属性，因为他们没有实例。

##### react的生命周期有哪些

react16.4版本之后

![](/images/react生命周期.png)

getDerivedStateFromProps

static getDerivedStateFromProps(props, state)在组件创建时和更新时的render方法之前调用，它应该返回一个对象来更新状态，或者返回null来不更新任何内容。

- getDerivedStateFromProps前面要加上static保留字，声明为静态方法，不然会被react忽略掉

![img](https:////upload-images.jianshu.io/upload_images/5287253-0a7d203c1428e011.png?imageMogr2/auto-orient/strip|imageView2/2/w/1012/format/webp)



- getDerivedStateFromProps里面的this为undefined

static静态方法只能Class(构造函数)来调用(App.staticMethod✅)，而实例是不能的( (new App()).staticMethod ❌ )；
 当调用React Class组件时，改组件会实例化；

所以
 React Class组件中，静态方法getDerivedStateFromProps无权访问Class实例的this，即this为undefined。

##### 哪个生命周期可以用来优化 

[文章链接](https://www.cnblogs.com/nerosen/p/9952193.html)

`总结：前后不改变state值的setState（理论上）和无数据交换的父组件的重渲染都会导致组件的重渲染，但你可以在shouldComponentUpdate这道两者必经的关口阻止这种浪费性能的行为`

优化性能的场景：  

- setState()中参数还是原来没有发生任何变化的state

  ```
  import React from 'react'
  class Test extends React.Component{
    constructor(props) {
      super(props);
      this.state = {
        Number:1
      }
    }
    //这里调用了setState但是并没有改变setState中的值
    handleClick = () => {
       const preNumber = this.state.Number
       this.setState({
          Number:this.state.Number
       })
    }
    //在render函数调用前判断：如果前后state中Number不变，通过return false阻止render调用
    shouldComponentUpdate(nextProps,nextState){
        if(nextState.Number == this.state.Number){
          return false
        }
    }
    render(){
      //当render函数被调用时，打印当前的Number
      console.log(this.state.Number)
      return(<h1 onClick = {this.handleClick} style ={{margin:30}}>
               {this.state.Number}
             </h1>)
    }
  }
  ```

  

- 如果组件的state没有变化，并且从父组件接受的props也没有变化，那它就还可能重渲染吗？——【可能！】

##### 关于react-transition-group的使用

```csharp
npm install react-transition-group --save 
```

组件中引入CSSTransition模块：

```jsx
import { CSSTransition } from 'react-transition-group'
```

将CSSTransition标签包裹在需要实现动画效果的元素外，然后进行相关属性的配置：

```dart
//jsx
constructor(props){
        super(props);
        this.state = {
            show: true
        }
    }

    render() {
        return (
            <Fragment>
                <CSSTransition
            in={this.state.show} // 如果this.state.show从false变为true，则动画入场，反之out出场
            timeout={1000} //动画执行1秒
            classNames='fade' //自定义的class名
            unMountOnExit //可选，当动画出场后在页面上移除包裹的dom节点
            onEntered={(el) => {
                el.style.color='blue'   //可选，动画入场之后的回调，el指被包裹的dom，让div内的字体颜色等于蓝色
     }}
     onExited={() => {
     xxxxx   //同理，动画出场之后的回调，也可以在这里来个setState啥的操作
     }}
>
              <div>hello</div>
           </CSSTransition>
        <button onClick={this.handleToggole}>toggle</button>
     </Fragment>
        )
    }
    handleToggole: ()=> {
        this.setState({
            show: this.state.show ? false : true
        })
    }
}
```

一旦动画入场，插件将会自动的在包裹住的标签上添加很多css样式，默认class名是fade，所以我们需要给CSSTransition标签加上classNames='fade'，然后去css文件进行配置：

```cpp
//enter是入场前的刹那（点击按钮），appear指页面第一次加载前的一刹那（自动）
.fade-enter, .fade-appear {
    opacity: 0;
}
//enter-active指入场后到入场结束的过程，appear-active则是页面第一次加载自动执行
.fade-enter-active, .fade-appear-active { 
    opacity: 1;
    transition: opacity 1s ease-in;
}
//入场动画执行完毕后，保持状态
.fade-enter-done {
    opacity: 1;
}
//同理，出场前的一刹那，以下就不详细解释了，一样的道理
.fade-exit {
    opacity: 1;
}

.fade-exit-active {
    opacity: 0;
    transition: opacity 1s ease-in;
}

.fade-exit-done {
    opacity: 0;
}
```

如果页面上一组dom都需要添加动画效果时我们需要在最外面再加一个TransitionGroup



```jsx
import React, { Component, Fragment } from 'react';
import { CSSTransition, TransitionGroup } from 'react-transition-group';
import './style.css';

class App extends Component {

    constructor(props){
        super(props);
        this.state = {
            list: []
        }
        this.handleAddItem = this.handleAddItem.bind(this);
    }

    render() {
        return (
            <Fragment>
                <TransitionGroup>
                {
                    this.state.list.map((item, index) => {
                        return (
                            <CSSTransition
                                timeout={1000}
                                classNames='fade'
                                unmountOnExit
                                onEntered={(el) => {el.style.color='blue'}}
                                appear={true}
                                key={index}
                            >
                                <div>{item}</div>
                            </CSSTransition>
                        )
                    })
                }
                </TransitionGroup>
                <button onClick={this.handleAddItem}>toggle</button>
            </Fragment>
        )
    }

    handleAddItem() {
        this.setState((prevState) => {
            return {
                list: [...prevState.list, 'item']
            }
        })
    }
}

export default App;
```

##### 为什么react要配合redux来使用

##### redux工作流程

##### redux设计使用原则

##### redux核心API

##### **react异步数据如ajax请求应该放在哪个生命周期？**

对于同步的状态改变，是可以放在componentWillMount，对于异步的，最好好放在componentDidMount。但如果此时有若干细节需要处理，比如你的组件需要渲染子组件，而且子组件取决于父组件的某个属性，那么在子组件的componentDidMount中进行处理会有问题：因为此时父组件中对应的属性可能还没有完整获取，因此就让其在子组件的componentDidUpdate中处理。