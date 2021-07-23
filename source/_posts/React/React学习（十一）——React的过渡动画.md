# React学习（十一）——React的过渡动画

## react-transition-group介绍

React曾为开发者提供过动画插件 `react-addons-css-transition-group`，后由社区维护，形成了现在的 `react-transition-group`。

这个库可以帮助我们方便的实现组件的 `入场` 和 `离场` 动画，使用时需要进行额外的安装：

```
# npm
npm install react-transition-group --save

# yarn
yarn add react-transition-group
```

react-transition-group本身非常小，不会为我们应用程序增加过多的负担。

react-transition-group主要包含四个组件：

- Transition

- - 该组件是一个和平台无关的组件（不一定要结合CSS）；
  - 在前端开发中，我们一般是结合CSS来完成样式，所以比较常用的是CSSTransition；

- CSSTransition

- - 在前端开发中，通常使用CSSTransition来完成过渡动画效果

- SwitchTransition

- - 两个组件显示和隐藏切换时，使用该组件

- TransitionGroup

- - 将多个动画组件包裹在其中，一般用于列表中元素的动画；

## react-transition-group使用

### 2.1. CSSTransition

CSSTransition是基于Transition组件构建的：

- CSSTransition执行过程中，有三个状态：appear、enter、exit；

- 它们有三种状态，需要定义对应的CSS样式：

- - 第一类，开始状态：对于的类是-appear、-enter、exit；
  - 第二类：执行动画：对应的类是-appear-active、-enter-active、-exit-active；
  - 第三类：执行结束：对应的类是-appear-done、-enter-done、-exit-done；

- 注：appear出现在第一次加载的时候，配合对应的css使用

CSSTransition常见对应的属性：

- in：触发进入或者退出状态

- - 如果添加了`unmountOnExit={true}`，那么该组件会在执行退出动画结束后被移除掉；
  - 当in为true时，触发进入状态，会添加-enter、-enter-acitve的class开始执行动画，当动画执行结束后，会移除两个class，并且添加-enter-done的class；
  - 当in为false时，触发退出状态，会添加-exit、-exit-active的class开始执行动画，当动画执行结束后，会移除两个class，并且添加-enter-done的class；

- classNames：动画class的名称

- - 决定了在编写css时，对应的class名称：比如card-enter、card-enter-active、card-enter-done；

- timeout：

- - 过渡动画的时间
  - 这里的时间是控制组件的渲染的；何时移除class类，何时添加新的；何时卸载组件等等；
  - 动画执行时间，一定是css中的时间设置控制的。
  - 最好设置两者的时间是一致的

- appear：

- - 是否在初次进入添加动画（需要和in同时为true），配合appear的css进行使用

- 其他属性可以参考官网来学习：

- - https://reactcommunity.org/react-transition-group/transition

CSSTransition对应的钩子函数：主要为了检测动画的执行过程，来完成一些JavaScript的操作

- onEnter：在进入动画之前被触发；
- onEntering：在应用进入动画时被触发；
- onEntered：在应用进入动画结束后被触发；

```react
import React, { PureComponent } from 'react';
import { CSSTransition } from 'react-transition-group';
import './CssTransitionDemo.css'

export default class CssTransitionDemo extends PureComponent {
  constructor(props) {
    super(props);

    this.state = {
      isShow: true
    };
  }

  changeShow() {
    this.setState({
      isShow: !this.state.isShow
    });
  }

  render() {
    return (
      <div>
        <button onClick={e => this.changeShow()}>显示隐藏</button>
        <CSSTransition
          in={this.state.isShow}
          classNames="card"
          appear
          timeout={1000}
          unmountOnExit={true}
          onEnter={el => console.log("进入动画前")}
          onEntering={el => console.log("进入动画")}
          onEntered={el => console.log("进入动画后")}
          onExit={el => console.log("退出动画前")}
          onExiting={el => console.log("退出动画")}
          onExited={el => console.log("退出动画后")}
        >
          <h2>Hello React</h2>
        </CSSTransition>
      </div>
    )
  }
}
```

```css
.card-appear, .card-enter {
  opacity: 0;
  transform: scale(.6);
}

.card-appear-active, .card-enter-active {
  opacity: 1;
  transform: scale(1);
  transition: opacity 300ms, transform 300ms;
}

.card-enter-done {

}

.card-exit {
  opacity: 1;
  transform: scale(1);
}

.card-exit-active {
  opacity: 0;
  transform: scale(.6);
  transition: opacity 300ms, transform 300ms;
}

.card-exit-done {
  opacity: 0;
}
```

### SwitchTransition

SwitchTransition可以完成两个组件之间切换的炫酷动画：

- 比如我们有一个按钮需要在on和off之间切换，我们希望看到on先从左侧退出，off再从右侧进入；
- 这个动画在vue中被称之为 vue transition modes；
- react-transition-group中使用SwitchTransition来实现该动画；

SwitchTransition中主要有一个属性：mode，有两个值

- in-out：表示新组件先进入，旧组件再移除；
- out-in：表示旧组件先移除，新组建再进入；

如何使用SwitchTransition呢？

- SwitchTransition组件里面要有CSSTransition或者Transition组件，不能直接包裹你想要切换的组件；
- SwitchTransition里面的CSSTransition或Transition组件不再像以前那样接受in属性来判断元素是何种状态，取而代之的是key属性；

```react
import React, { PureComponent } from 'react';
import { SwitchTransition, CSSTransition } from 'react-transition-group';
import './SwitchTransitionDemo.css'

export default class SwitchTransitionDemo extends PureComponent {
  constructor(props) {
    super(props);

    this.state = {
      isOn: true
    };
  }

  render() {
    return (
      <div>
        <SwitchTransition mode="out-in">
          <CSSTransition
            key={this.state.isOn ? 'on' : 'off'}
            classNames="btn"
            timeout={800}
          >
            <button onClick={e => this.setState({ isOn: !this.state.isOn })}>
              { this.state.isOn ? 'on' : 'off' }
            </button>
          </CSSTransition>
        </SwitchTransition>
      </div>
    )
  }
}

```

```css
.btn-enter {
  opacity: 0;
  transform: translateX(100%);
}

.btn-enter-active {
  opacity: 1;
  transform: translateX(0);
  transition: opacity 800ms, transform 800ms;
}

.btn-exit {
  opacity: 1;
  transform: translateX(0);
}

.btn-exit-active {
  opacity: 0;
  transform: translateX(-100%);
  transition: opacity 800ms, transform 800ms;
}
```

### TransitionGroup

当我们有一组动画时，需要将这些CSSTransition放入到一个TransitionGroup中来完成动画：

```react
import React, { PureComponent } from "react";
import { TransitionGroup, CSSTransition } from "react-transition-group";
import './TransitionGroupDemo.css';

export default class TransitionGroupDemo extends PureComponent {
  constructor(props) {
    super(props);

    this.state = {
      numbers: [1, 2],
    };
  }

  addNumber() {
    this.setState({
      numbers: [...this.state.numbers, this.state.numbers.length + 1]
    });
  }

  render() {
    return (
      <TransitionGroup>
        <button onClick={e => this.addNumber()}>+number</button>
        {this.state.numbers.map((item) => (
          <CSSTransition key={item} timeout={300} classNames="num">
            <div>{item}</div>
          </CSSTransition>
        ))}
      </TransitionGroup>
    );
  }
}

```

```css
.num-enter {
  opacity: 0;
  transform: scale(.6);
}

.num-enter-active {
  opacity: 1;
  transform: scale(1);
  transition: opacity 300ms, transform 300ms;
}

.num-enter-done {
  color: red;
}


.num-exit {
  opacity: 1;
  transform: scale(1);
}

.num-exit-active {
  opacity: 0;
  transform: scale(.6);
  transition: opacity 300ms, transform 300ms;
}

```

