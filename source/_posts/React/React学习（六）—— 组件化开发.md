# React学习（六）—— 组件化开发

组件化设计思维3个关键点：

1. 完整组件方案：将组件视为一个独立的产品，从多维度，多场景输出组件的方案和组合标准。
2. 组件化思维：从需求出发，拆解页面表达结构和所需组件。
3. 通用页面规则：通用的页面与组件的栅格体系及替换规则。

注：https://www.zcool.com.cn/article/ZNTQ5NzM2.html

## 组件的分类

React组件灵活多样，按照不同的方式可以分为很多类组件：

* 根据组件的定义方式，可以分为：函数组件`Functional Component` 和类组件`Class Component`；

* 根据组件内部是否有状态需要维护，可以分成：无状态组件`Stateless Component`和有状态组件`Stateful Component`
* 根据组件的不同职责，可以分成：展示型组件`Presentational Component`和容器型组件`Container Component`；
* 还有其他概念的组件，如异步组件、高阶组件等。

这些概念有很多重叠，当时他们最主要是关注数据逻辑和UI展示的分离：

* 函数组件、无状态组件、展示型组件主要关注UI的展示
* 类组件、有状态组件、容器组件主要关注数据逻辑；

## 类组件和函数组件

类组件的定义有如下要求：

* 组件的名称是大写的
* 需要继承`React.Component`
* 必须实现`render`函数

创建一个类组件，我们会使用`class`创建一个组件：

* `constructor`是可选的，我们通常会在`constructor`中初始化一些数据；

* `this.state`中维护的就是我们组件内部的数据

* `render()`方法是class组件中必须实现的方法

  ```react
  import React from 'react';
  
  export default class App extends React.Component {
    render() {
      return (
        <div>Hello World</div>
      );
    }
  }                                           
  ```

  当render被调用时，它会检查`this.props`和`this.state`的变化并返回以下类型之一：

  * React元素：通常通过JSX创建，如`<div>`会被React渲染成DOM节点，`<Component/>`会被React渲染为自定义组件，无论是html标签元素还是`<Compoennt/>`均为React元素。
  * 数组或`fragments`：使得`render`方法可以返回多个元素
  * `Portals`：可以渲染子节点到不同的DOM子树中。
  * 字符串或数值类型：它们在DOM中会被渲染为文本节点
  * 布尔类型或`null`：什么都不渲染

函数组件：

函数组件是使用`function`来进行定义的函数，只是这个函数会返回和类组件中`render`函数返回一样的内容。

函数自建的特点：

* 没有生命周期，也会被更新并挂在，没有生命周期函数；
* 没有`this`
* 没有内部状态`state`

```react
export default function App() {
  return (
    <h2>我是函数组件</h2>
  );
}
```

## 生命周期

> React生命周期详情：
>
> https://zh-hans.reactjs.org/docs/react-component.html
>
> https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

很多的事物都有从创建到销毁的整个过程，这个过程称之为是生命周期。

React组件有自己的生命周期，React内部为了告诉我们当前处于哪些阶段，会对我们组件内部实现的某些函数进行回调，这些函数就是生命周期函数。

我们在谈React的生命周期时，主要谈的是类的生命周期，因为函数式组件是没有生命周期函数的；



![img](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B517.png)

生命周期大致可分为如下几个阶段：

* 挂载（初始化）

  * `constructor()`
    * 如果不初始化或不进行方法绑定，则不需要为React组件实现构造函数
    * 一般做两件事情：
      * 通过给 this.state 赋值对象来初始化内部的state；
      * 为事件绑定实例（this）
  * `getDerivedStateFromProps()` 不常用
    * state 的值在任何时候都
      依赖于 props时使用；该方法返回一个对象来更新state；
  * `render()`
  * `componentDidMount()`
    * 会在组件挂载后（插入 DOM 树中）立即调用
    * 通常进行的操作是：
      * 依赖于DOM的操作可以在这里进行
      * 在此处发送网络请求（官方建议）
      * 在此处添加一些订阅（在`componentWillUnmount`取消订阅）

* 更新

  * `static getDeriveStateFromProps()` 不常用

  * `shouldComponentUpdate()`

    * 根据返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染

  * `render()`

  * `getSnapshotBeforeUpdate()` 不常用

    * 在React更新DOM之前回调的一个函数，可以获取DOM更新前的一些信息（比如说滚动位置）；

  * `componentDidUpdate()`

    * 会在更新后会被立即调用，首次渲染不会执行此方法

    * 当组件更新后，可以在此处对 DOM 进行操作；

    * 如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求；

      ```react
      componentDidUpdate(prevProps) {
        // 典型用法（不要忘记比较 props）：
        if (this.props.userID !== prevProps.userID) {
          this.fetchData(this.props.userID);
        }
      }
      ```

      

* 卸载
  * `componentWillUnmount()`
    * 会在组件卸载及销毁之前直接调用
    * 在此方法中执行必要的清理操作；例如，清除 timer，取消网络请求或清除；在componentDidMount() 中创建的订阅等；

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

* `static getDerivedStateFromError()`
* `componnetDidCatch()`

下面是一个简单的生命周期例子：

![11111](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B518.gif)

效果如上，当首次进入页面时，执行挂在阶段生命周期。当点击`+1`按钮，执行更新生命周期。再点击切换按钮，第一次看到`<TipCom/>`组件执行了卸载生命周期函数，第二次点击，可以看到`<App/>`组件再次执行了更新。

代码如下：

```react
import React, { Component } from 'react';

class TipCom extends Component {
  render() {
    return (
      <div>你好，你来了</div>
    );
  }

  componentWillUnmount() {
    console.log('调用TipCom组件的componentWillUnmount方法');
  }
}

export default class App extends Component {
  constructor() {
    super();
    console.log('执行组件的constructor方法');

    this.state = {
      counter: 0,
      isShowTip: true
    };
  }

  increment() {
    console.log('点击+1按钮更新');
    this.setState({
      counter: this.state.counter + 1
    });
  }

  changeTipShow() {
    console.log('点击切换按钮更新');

    this.setState({
      isShowTip: !this.state.isShowTip
    });
  }

  render() {
    console.log('执行组件的render方法');

    return (
      <div>
        我是生命周期过程组件 | {this.state.counter}
        <button onClick={() => this.increment()}>+</button> 
        <hr/>
        <button onClick={() => this.changeTipShow()}>切换</button>
        { this.state.isShowTip && <TipCom/> }
      </div>
    )
  }
  
  componentDidMount() {
    console.log('执行组件的componentDidMount方法');
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log('执行组件的shouldComponentUpdate方法');
    
    return true;
  }

  componentDidUpdate() {
    console.log('执行组件的componentDidUpdate方法');
  }
}
```

## 组件通信

### 组件间的嵌套

在开发过程中，组件之间的嵌套是很常见的一个现象，正式一个个组件，构成了我们的应用，如果所有的逻辑都放在一个组件中，那么这组件就会变得非常臃肿难以维护。将组件进行拆分，然后再进行组合嵌套一起，最终便形成了应用。

一个页面的构成，往往有多层嵌套，下面是一个简单的例子：

![image-20210109161329779](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B519.png)

```react
import React, { Component } from 'react';

function Header() {
  return (
    <h2>我是Header组件</h2>
  );
}

function Banner() {
  return (
    <h3>我是Banner组件</h3>
  );
}

function ProductList() {
  return (
    <div>
      <ul>
        <li>商品1</li>
        <li>商品2</li>
        <li>商品3</li>
        <li>商品4</li>
        <li>商品5</li>
      </ul>
    </div>
  );
}

function Main() {
  return (
    <div>
      <Banner />
      <ProductList />
    </div>
  );
}


function Footer() {
  return (
    <h2>我是Footer组件</h2>
  );
}

export default class Com01 extends Component {
  render() {
    return (
      <div>
        <Header />
        <Main />
        <Footer />
      </div>
    )
  }
}
```

假如这个例子更复杂一些，比如`Com01`可能使用了多个`Header`，每个地方Header展示的内容不同，那么我们需要使用者传递给`Header`一些数据，让其进行展示

又比如我们在`Main`中一次性请求了`Banner`数据和`ProductList`数据，那么就需要传递给他们来进行展示；也可能是子组件中发生了事件，需要由父组件来完成某些操作，那就需要子组件向父组件传递事件；

React项目中，组件间的通信时非常重要的环节。

### 父组件传递子组件

父组件在展示子组件，可能会传递一些数据给子组件：

* 父组件通过**属性=值**的形式来传递给子组件数据；
* 子组件通过**props **参数获取父组件传递过来的数据；

```react
import React, { Component } from 'react';

// 类组件
class ChildCpn extends Component {
  // 可省略
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <h2>类子组件展示数据： { this.props.name}</h2>
    );
  }
}

// 函数组件
function ChildCpn2(props) {
  return (
    <h2>函数子组件展示数据： { props.name}</h2>
  );
}

export default class Com02 extends Component {
  render() {
    return (
      <div>
        <ChildCpn name="Ccbean" />
        <ChildCpn2 name="Ccbean" />
      </div>
    )
  }
}

```

上面的构造函数实现方法，是派生类的默认实现方法，即没有构造函数时，会执行默认的构造函数，且实现完全相同。

#### 参数propTypes

> https://zh-hans.reactjs.org/docs/typechecking-with-proptypes.html

对于传递给子组件的数据，有时候我们可能希望进行验证，特别是对于大型项目来说。

如果你项目中默认继承了`Flow`或者`TypeScript`，那么直接就可以进行类型验证。如果没有使用，也可以通过 `prop-types`库来进行参数验证；

```react
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class ChildCpn extends Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    height: PropTypes.number,
    letters: PropTypes.array
  };

  static defaultProps = {
    name: 'Tom',
    height: 188,
    letters: ['D', 'E', 'F']
  };

  // 可省略
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
      <h2>函数子组件展示数据： {this.props.name}</h2>
      <ul>
        {
          this.props.letters.map(item => <li key={item}>{item}</li>)
        }
      </ul>
    </div>
    );
  }
}

ChildCpn2.propTypes = {
  name: PropTypes.string.isRequired,
  height: PropTypes.number,
  letters: PropTypes.array
};

ChildCpn2.defaultProps = {
  name: 'Tom',
  height: 188,
  letters: ['D', 'E', 'F']
};

function ChildCpn2(props) {
  return (
    <div>
      <h2>函数子组件展示数据： {props.name}</h2>
      <ul>
        {
          props.letters.map(item => <li key={item}>{item}</li>)
        }
      </ul>
    </div>
  );
}

export default class Com02 extends Component {
  render() {
    return (
      <div>
        <ChildCpn/>
        <ChildCpn2 name='Ccbean' height="182" letters={['A', 'B', 'C']} />
        <ChildCpn2 />
      </div>
    )
  }
}
```

上面的代码会导致React打印一条警告：

```shell
Warning: Failed prop type: Invalid prop `height` of type `string` supplied to `ChildCpn2`, expected `number`
```

假如没有传递`name`，`name`在`PropTypes`中限制是必须的，会出现如下警告：

```shell
Warning: Failed prop type: The prop `name` is marked as required in `ChildCpn2`, but its value is `undefined`.
```

**如果需要做大量的验证，建议直接使用`TypeScript`。**

如果没有传递值，当希望使用有默认值，可以使用`defaultProps`。

### 子组件传递父组件

某些情况，我们也需要子组件向父组件传递消息，在Vue中是通过自定义事件完成的；在React中同样是通过`props`传递消息，只是让父组件给子组件传递一个回调函数，在子组件中调用这个函数即可。

这边还是使用计数器的例子，不过这次将计数器进行拆解，将按钮封装到子组件`CounterButton`中，`CounterButton`发生点击事件，将内容传递到父组件中，修改`counter`的值。

```react
import React, { Component } from 'react';

class CounterButton extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <button onClick={() => this.props.increment(-1)}>-1</button>
    );
  }
}

export default class Com04 extends Component {
  constructor(props) {
    super(props);

    this.state = {
      counter: 0
    };
  }

  increment(count) {
    this.setState({
      counter: this.state.counter + count
    });
  }

  render() {
    return (
      <div>
        <h2>Counter: {this.state.counter}</h2>
        <CounterButton increment={this.increment.bind(this)}/>
        <button onClick={() => this.increment(1)}>+1</button>
      </div>
    )
  }
}
```

### 组件间通信练习案例

一个Tab切换案例，效果如下：

![111332211](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B520.gif)

具体代码如下：

`App.js`实现：

```react
import React, { Component } from 'react';
import TabControl from './TabControl';
import './style.css';

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      titles: ['流行', '精选', '新款'],
      currentIndex: 0
    };
  }

  render() {
    const { titles, currentIndex } = this.state;
 
    return (
      <div>
        <TabControl itemClick={this.itemClick.bind(this)} titles={['流行', '精选', '新款']}/>   
        <h2>{titles[currentIndex]}</h2>
      </div>
    )
  }

  itemClick(index) {
    this.setState({
      currentIndex: index
    });
  }
}
```

`TabControl.js`实现：

```react
import React, { Component } from 'react';
import PropTypes from 'prop-types';

export default class TabControl extends Component {
  static propTypes = {
    titles: PropTypes.array.isRequired
  }

  constructor(props) {
    super(props);

    this.state = {
      currentIndex: 0
    }
  }

  render() {
    const { currentIndex } = this.state;

    return (
      <div className="tab-control">
        {
          this.props.titles.map((item, index) => {
            return (
              <div 
                key={index}
                className={`tab-item ${index == currentIndex ? 'active' : ''}`} 
                onClick={e => this.itemClick(index)}
              >
                {item}
              </div>
            );
          })
        }
      </div>
    )
  }

  itemClick(index) { 
    this.setState({
      currentIndex: index
    });

    this.props.itemClick(index);
  }
}
```

`style.css`

```react
.tab-control {
  display: flex;
  justify-content: space-between;
  width: 200px;
}

.tab-control .tab-item {
  margin-top: 10px;
  cursor: pointer;
}

.tab-control .active {
  color: red;
  border-bottom: 1px solid red;
}
```

### React实现slot

Vue 实现了一套内容分发的 API，将 `<slot>` 元素作为承载分发内容的出口。

在React中，实现Vue的插槽功能很简单，这也是得益于JSX语法的灵活性。

![image-20210109231454105](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B521.gif)

如上图中的导航栏NavBar，如果进行组件封装，我们可能会封装三个不同的组件，但是，如果类似的组件如果有20个甚至更多，那么一个个地进行组件封装显然是不明智的选择。

更好地做法，可能是再找出组件的相似之处，对外层进行封装，而内部具体要放的内容，可以根据预留的Slot业务需求进行开发。

下面是一种实现方法：

![image-20210109233003357](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B522.png)

`NavBar.js`

```react
import React, { Component } from 'react';

export default class NavBar extends Component {
  constructor(props) {
    super(props);
  }

  render() {

    return (
      <div className="nav-bar">
        <div className="nav-item nav-left">{this.props.children[0]}</div>
        <div className="nav-item nav-middle">{this.props.children[1]}</div>
        <div className="nav-item nav-right">{this.props.children[2]}</div>
      </div>
    )
  }
}

```

`App.js`

```react
import React, { Component } from 'react';
import NavBar from './NavBar';
import './style.css';

export default class App extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <NavBar>
        <span>left</span>
        <strong>middle</strong>
        <span>right</span>
      </NavBar>
    )
  }
}
```

`style.css`

```react
.nav-bar {
  display: flex;
  justify-content: space-between;
  width: 300px;
  height: 30px;
  display: flex;
}

.nav-item {
  line-height: 30px;
  text-align: center;
}

.nav-bar .nav-left {
  width: 50px;
  background: blue;
}

.nav-bar .nav-middle {
  flex: 1;
  background: red;
}

.nav-bar .nav-right {
  width: 60px;
  background: green;
}
```

上面的实现存在的一个缺点就是，`children`的顺序不能乱。

所以推荐的做法如下：

`App.js`

```react
import React, { Component } from 'react';
import NavBar from './NavBar';
import NavBar2 from './NavBar2';
import './style.css';

export default class App extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <NavBar2
        leftSlot={<span>left</span>}
        middleSlot={<strong>middle</strong>}
        rightSlot={<span>right</span>}
        />
    )
  }
}
```

`NavBar2.js`

```react
import React, { Component } from 'react';

export default class NavBar2 extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div className="nav-bar">
        <div className="nav-item nav-left">{this.props.leftSlot}</div>
        <div className="nav-item nav-middle">{this.props.middleSlot}</div>
        <div className="nav-item nav-right">{this.props.rightSlot}</div>
      </div>
    )
  }
}
```

这样，我们就不需要再关注传递顺序了。

### 跨组件通信

在开发中，比较常见的数据传递方式是通过props属性自上而下（由父到子）进行传递。但是对于有一些场景：比如一些数据需要在多个组件中进行共享（地区偏好、UI主题、用户登录状态、用户信息等）。

如果我们在顶层的App中定义这些信息，之后一层层传递下去，那么对于一些中间层不需要数据的组件来说，是一种冗余的操作。

#### 使用props

```react
import React, { Component } from 'react';

function ProfileHeader(props) {
  return (
    <div>
      <h2>昵称：{props.nickname}</h2>
      <h2>等级：{props.level}</h2>
    </div>
  );
}

function Profile(props) {
  return (
    <div>
      <ProfileHeader nickname={props.nickname} level={props.level} />
      <ul>
        <li>设置1</li>
        <li>设置2</li>
        <li>设置3</li>
        <li>设置4</li>
      </ul>
    </div>
  );
}

export default class App extends Component {
  render() {
    return (
      <div>
        <Profile nickname={'ccbean'} level={99} />
      </div>
    )
  }
}
```

上面的例子，如果我们通过`props`进行传递，`<ProfileHeader/>`需要的数据，要首先传递到`<Profile/>`中，然后再传递到数据真正使用的组件

存在两个问题：

* 数据在`Profile`中没有任何用处，但是还需要传递；假如嵌套层数更多，那么就需要一层层进行传递；

* 每个需要传递的属性都要一个个明确传递。这个问题的解决是，使用[属性展开](https://zh-hans.reactjs.org/docs/jsx-in-depth.html#spread-attributes)；

  ```react
  <ProfileHeader {...props} />
  ```

  但容易出现的问题是，将不必要的 props 传递给不相关的组件。

#### 使用context

> https://zh-hans.reactjs.org/docs/context.html

React中提供了一个API是`Context`，`Context`提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递`props`；

`Context`设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

##### 相关API

`React.createContext`

```react
const MyContext = React.createContext(defaultValue);
```

* 创建一个需要共享的`Context`对象
* 如果一个组件订阅`Context`，那么这个组件会从离自身最近的那个匹配的`Provider`中读取到当前的`context`值
* `defaultValue`是组件在顶层查找过程中没有找到对应的Provider，那么就使用默认值。

`Context.Provider`

```react
<MyContext.Provider value={/* 某个值 */}>
```

* 每个`Context`对象都会返回一个`Provider React` 组件，它允许消费组件订阅`context`的变化
* `Provider`接收一个`value`属性，传递给消费组件
* 一个`Provider`可以和多个消费组件有对应关系
* 多个`Provider`也可以嵌套使用，里层的会覆盖外层的数据
* 当`Provider`的`value`值发生变化时，它内部的所有消费组件都会重新渲染

`Class.ContextType`

```react
MyClass.contentType = MyContext;
```

* 挂载在`class`上的`contextType`属性会被重赋值为一个由`React.createContext()`创建的`Context` 对象，这能让你使用`this.context`来消费最近 `Context`上的那个值，你可以在任何生命周期中访问到它，包括`render`函数中。

`Context.Consumber`

```react
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```

* React 组件也可以订阅到`context`变更。这能让你在函数式组件中完成订阅`context`
* 这种方法需要一个[函数作为子元素（function as a child）](https://zh-hans.reactjs.org/docs/render-props.html#using-props-other-than-render)
* 这个函数接收当前的`context`值，返回一个 `React`节点

##### 实现通信

![image-20210110110412680](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B523.png)

```react
import React, { Component } from 'react';

// 1. 创建Context对象
const UserContext = React.createContext({
  nickname: 'aaa',
  level: -1
});

class ProfileHeader extends Component {
  // 3. 将context对象赋值给应用数据组件
  static contextType = UserContext

  render() {
    return (
      <div>
        <h2>昵称：{this.context.nickname}</h2>
        <h2>等级：{this.context.level}</h2>
      </div>
    )
  }
}

function Profile(props) {
  return (
    <div>
      <ProfileHeader />
      <ul>
        <li>设置1</li>
        <li>设置2</li>
        <li>设置3</li>
        <li>设置4</li>
      </ul>
    </div>
  );
}

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      nickname: 'ccbean',
      level: 99
    };
  }

  render() {
    return (
      <div>
        {/* 2.使用Provider包裹 */}
        <UserContext.Provider value={this.state}>
          <Profile />
        </UserContext.Provider>
        <Profile />
      </div>
    )
  }
}
```

我们可以看到，如果`   <Profile />`没有在`<UserContext.Provider/>`包裹时，使用了`React.createContext`中的默认值`aaa`和`-1`。

在函数组件中，如何使用`context`呢？使用`Context.Consumer`即可在函数式组件中订阅`Context`

```react
import React, { Component } from 'react';

const UserContext = React.createContext({
  nickname: 'aaa',
  level: -1
});

function ProfileHeader() {
  return (
    <UserContext.Consumer>
      {
        value => {
          return (
            <div>
              <h2>昵称：{value.nickname}</h2>
              <h2>等级：{value.level}</h2>
            </div>
          )
        }
      }
    </UserContext.Consumer>
  )
}

function Profile(props) {
  return (
    <div>
      <ProfileHeader />
      <ul>
        <li>设置1</li>
        <li>设置2</li>
        <li>设置3</li>
        <li>设置4</li>
      </ul>
    </div>
  );
}

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      nickname: 'ccbean',
      level: 99
    };
  }

  render() {
    return (
      <div>
        <UserContext.Provider value={this.state}>
          <Profile />
        </UserContext.Provider>
      </div>
    )
  }
}
```

如果是多个context，那么需要这样写：

```react
import React, { Component } from 'react';

const UserContext = React.createContext({
  nickname: 'aaa',
  level: -1
});

const ThemeContext = React.createContext({
  color: 'black'
});

function ProfileHeader() {
  return (
    <UserContext.Consumer>
      {
        value => {
          return (
            <ThemeContext.Consumer>
              {
                theme => {
                  return (
                    <div>
                      <h2>昵称：{value.nickname}</h2>
                      <h2>等级：{value.level}</h2>
                      <h2>主题：{theme.color}</h2>
                    </div>
                  );
                }
              }
            </ThemeContext.Consumer>
          )
        }
      }
    </UserContext.Consumer>
  )
}

function Profile(props) {
  return (
    <div>
      <ProfileHeader />
      <ul>
        <li>设置1</li>
        <li>设置2</li>
        <li>设置3</li>
        <li>设置4</li>
      </ul>
    </div>
  );
}

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      nickname: 'ccbean',
      level: 99
    };
  }

  render() {
    return (
      <div>
        <UserContext.Provider value={this.state}>
          <ThemeContext.Provider value={{ color: 'red' }}>
            <Profile />
          </ThemeContext.Provider>
        </UserContext.Provider>
      </div>
    )
  }
}
```

上面的代码，添加`ThemeContext`，那么ProfileHeader中嵌套了两层，代码看起来十分混乱。

实际开发中，我们一般不会这么做，会有其它更好的解决方案，如考虑另外创建你自己的渲染组件，以提供这些值；或者使用`redux`等。

