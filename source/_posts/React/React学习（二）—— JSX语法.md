---

title: React学习（二）—— JSX语法

date: 2020-12-21 23:07:23

categories: React

tag: 

- React 
- HTML

---

# React学习（二）—— JSX语法

本节记录JSX语法相关内容。

<!--more-->

## JSX核心语法

电影列表的例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app">app</div>
</body>

</html>
<!-- 添加React的依赖 -->
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script type="text/babel">
  // 封装APP组件
  class App extends React.Component {
    constructor() {
      super();

      this.state = {
        movies: [ '大话西游', '美人鱼', '夏洛特烦恼', '西红柿首富' ]
      };
    }


    render() {
      const listArray = this.state.movies.map(item => <li key={item}>{item}</li>);

      return (
        <div>
          <h2>电影列表</h2>
          <ul>
            {listArray}
          </ul>
          <h2>电影列表2</h2>
          {this.state.movies.map(item => <li key={item}>{item}</li>)}
        </div>
      );
    }
  }

  // 渲染App组件
  ReactDOM.render(<App />,
    document.getElementById('app')
  );
</script>
```

计数器例子：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <div id="app">app</div>
</body>

</html>

<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script type="text/babel">
  class App extends React.Component {
    constructor() {
      super();

      this.state = {
        counter: 0
      };
    }

    increment() {
      this.setState({
        counter: this.state.counter + 1
      });
    }

    decrement() {
      this.setState({
        counter: this.state.counter - 1
      });
    }

    render() {
      return (
        <div>
          <h2>当前计数：{this.state.counter}</h2>
          <button onClick={this.increment.bind(this)}>+1</button>
          <button onClick={this.decrement.bind(this)}>-1</button>
        </div>
      );
    }
  }

  // 渲染App组件
  ReactDOM.render(<App />, document.getElementById('app'));
</script>
```

### 认识JSX

```jsx
<script type="text/babel">
	const element = <h2>Hello world</h2>
	ReactDOM.render(element, document.getElementById('app'));
</script>
```

这段element变量的声明右侧赋值的标签语法，它不是一段字符串（因为没有使用引号包裹），它看起来是一段`HTML`原生，但是我们能在`JS`中直接给一个变量赋值`html`吗？其实是不可以的，如果去掉`type="text/babel"` 去除掉，那么就会出现语法错误；它其实就是JSX语法。

#### JSX是什么

* JSX是一种JavaScript的语法扩展（eXtension），也在很多地方称之为JavaScript XML，因为看起就是一段XML语法；
* 它用于描述我们的UI界面，并且其完成可以和JavaScript融合在一起使用；
* 它不同于Vue中的模块语法，不需要专门学习模块语法中的一些指令（比如v-for、v-if、v-else、v-bind）；Vue也支持JSX语法。

#### 为什么React选择了JSX？

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合。

* 比如UI需要绑定事件（button、a原生等等）；
* 比如UI中需要展示数据状态，在某些状态发生改变时，又需要改变UI；
* 他们之间是密不可分，所以React没有讲标记分离到不同的文件中，而是将它们组合到了一起，这个地方就是组件。

#### JSX的书写规范

1. JSX的顶层只能有一个根元素，所以很多时候会在外层包裹一个`div`原生（或者使用`Fragment`）
2. 为了方便阅读，通常会在JSX的外层包裹一个小括号`()`，这样可以方便阅读，并且`JSX`可以进行换行书写；
3. JSX中的标签可以是单标签，也可以是双标签；

### JSX的使用

#### JSX注释

```jsx
{/* 我是一个注释 */}
```

#### JSX嵌入变量

JSX嵌入变量：

* 当变量是`Number`、`String`、`Array`类型时，可以直接显示
* 当变量是`null`、`undefined`、`Boolean`类型时，内容为空
  * 如果希望可以显示`null`、`undefined`、`Boolean`，那么需要转成字符串；
  * 转换的方式有很多，比如`toString()`方法、和空字符串拼接，`String(变量)`等方式；
* 对象类型不能作为子元素（`not valid as a React child`）

```jsx
 <!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <div id="app"></div>
</body>

</html>

<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script type="text/babel">
  class App extends React.Component {
    constructor() {
      super();

      this.state = {
        name: 'ccbean',
        age: 18,
        hobby: [ '唱跳', 'rap', '篮球'],
        
        test1: null, // null
        test2: undefined, // undefined
        test3: true, // Boolean

        // 对象不能作为jsx的子类
        friend: {
          name: 'tom',
          age: 18
        }
        
      };
    }

    render() {
      return (
        <div>
          <h2>{this.state.name}</h2>
          <h2>{this.state.age}</h2>
          <h2>{this.state.hobby}</h2>
          
          {/* 没有任何显示 */}
          <h2>{this.state.test1}</h2>
          <h2>{this.state.test2}</h2>
          <h2>{this.state.test3}</h2>

          {/* 转为字符串显示 */}
          <h2>{String(this.state.test1)}</h2>
          <h2>{this.state.test2 + ''}</h2>
          <h2>{this.state.test3.toString()}</h2>

          {/* 报错  Objects are not valid as a React child... */}
          {/* <h2>{this.state.friend}</h2> */}
        </div>
      );
    }
  }

  // 渲染App组件
  ReactDOM.render(<App />, document.getElementById('app'));
</script>
```

#### JSX嵌入表达式

嵌入表达式可以是：

* 运算表达式
* 三元运算符
* 执行一个函数
* 总之js中常用的表达式

```jsx
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <div id="app">app</div>
</body>

</html>

<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script type="text/babel">
  class App extends React.Component {
    constructor() {
      super();

      this.state = {
        firstname: 'ccbean',
        lastname: 'go',

        isLogin: false,
      };
    }

    getFullname() {
      return `${this.state.firstname} ${this.state.lastname}`
    }

    render() {
      const { firstname, lastname } = this.state;
      return (
        <div>
          {/* 运算符表达式 */}
          <h2>{`${firstname} ${lastname}`}</h2>
          <h2>{2 * 10}</h2>

          {/* 三元运算符 */}
          <h2>{this.state.isLogin ? '欢迎回来' : '请登录'}</h2>

          {/* 进行函数调用 */}
          <h2>{this.getFullname()}</h2>
        </div>
      );
    }
  }

  // 渲染App组件
  ReactDOM.render(<App />, document.getElementById('app'));
</script>
```



