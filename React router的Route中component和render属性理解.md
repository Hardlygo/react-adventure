最近在学习```react-router```现在写一下我对其中Route标签的三个互斥属性```render```、```component```、```children```的理解。

> The Route component is perhaps the most important component in React Router to understand and learn to use well. Its most basic responsibility is to render some UI when its path matches the current URL.  

Route 就是用来匹配路由的，并且对匹配到的路由就显示，所以会有可能两个以上的路由会匹配到，所以需要Switch标签包围，Switch可以帮助只渲染匹配到的第一个路由。

>  ```Route render methods```
The recommended method of rendering something with a```<Route>```is to **use children elements**. There are, however, a few other methods you can use to render something with a ``<Route>``. These are provided mostly for supporting apps that were built with earlier versions of the router before hooks were introduced.
```<Route component>```
```<Route render>```
```<Route children> function```
```You should use only one of these props on a given <Route>. See their explanations below to understand the differences between them.```
**Route props**
All three render methods will be passed the same three route props
```match```
```location```
```history```


一般情况下，Route推荐使用children元素方式配置路由，但是除此之外还有另外三种配置方法。 
1.***compnent*** (属性或者方法)：
2.***render***(方法)
3.***children***（方法）：children是只要配置了该属性，其里面返回的函数都会渲染，无论路径是否匹配。它能接受所有的路由属性，若不匹配```match```会为```null```
**解释：**

> Sometimes you need to render whether the path matches the location or not. In these cases, you can use the function children prop. It works exactly like render except that it gets called whether there is a match or not.The children render prop receives all the same route props as the component and render methods, except when a route fails to match the URL, then match is null. This allows you to dynamically adjust your UI based on whether or not the route matches. Here we’re adding an active class if the route matches
> 

```javascript
//Here we’re adding an active class if the route matches
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Link,
  Route
} from "react-router-dom";

function ListItemLink({ to, ...rest }) {
  return (
    <Route
      path={to}
      children={({ match }) => (
        <li className={match ? "active" : ""}>
          <Link to={to} {...rest} />
        </li>
      )}
    />
  );
}

ReactDOM.render(
  <Router>
    <ul>
      <ListItemLink to="/somewhere" />
      <ListItemLink to="/somewhere-else" />
    </ul>
  </Router>,
  node
);

//或者用于动画
<Route
  children={({ match, ...rest }) => (
    {/* Animate will always render, so you can use lifecycles
        to animate its child in and out */}
    <Animate>
      {match && <Something {...rest}/>}
    </Animate>
  )}
/>
```

**children的优先级会比其他两个要高**
> Warning: ```<Route children>```takes precedence over both ```<Route component>```and ```<Route render>```so don’t use more than one in the same ```<Route>```.

针对react-router官方文档的原文的理解

> When you use component (instead of render or children, below) the router uses ```React.createElement```to create a new React element from the given component. That means if you provide an ```inline function```to the ```component prop```, you would ```create```a new component ```every render```. ```This results in the existing component unmounting and the new component mounting instead of just updating the existing component```. When using an inline function for inline rendering, use the render or the children prop (below).
> 

> **render: func**
This allows for convenient inline rendering and wrapping ```without the undesired remounting```explained above.Instead of having a new React element created for you using the component prop, you can pass in a function to be called when the location matches. The render prop function has access to all the same route props (```match```, ```location```and  ```history```) as the component render prop.

上面的意思是：```内联的component方法当父组件的render方法每次（非第一次）执行时（比如setState造成）都会使得component方法返回的组件再次重新执行初始化生命周期函数，而render属性的方法就不会，它在父组件再次执行render是时只会触发组件的update生命钩子```

```javascript
//component属性例子
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router, Route } from "react-router-dom";

// All route props (match, location and history) are available to User
function User(props) {
  return <h1>Hello {props.match.params.username}!</h1>;
}

ReactDOM.render(
  <Router>
    <Route path="/user/:username" component={User} />
  </Router>,
  node
);
```

```javascript
//render例子
//例子1
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router, Route } from "react-router-dom";

// convenient inline rendering
ReactDOM.render(
  <Router>
    <Route path="/home" render={() => <div>Home</div>} />
  </Router>,
  node
);

//例子2
// wrapping/composing
// You can spread routeProps to make them available to your rendered Component
function FadingRoute({ component: Component, ...rest }) {
  return (
    <Route
      {...rest}
      render={routeProps => (
        <FadeIn>
          <Component {...routeProps} />
        </FadeIn>
      )}
    />
  );
}

ReactDOM.render(
  <Router>
    <FadingRoute path="/cool" component={Something} />
  </Router>,
  node
);
```

### [ 我的理解](https://codesandbox.io/s/react-router-example-di6gg?file=/src/App.js)

```javascript
import React from "react";
import "./styles.css";

import { BrowserRouter as Router, Link, Route, Switch } from "react-router-dom";

class MyHome extends React.Component {
  constructor(props) {
    super(props);
    console.log("Home created!!!!");
    console.log("props in Home", props);
  }
  componentDidMount() {
    console.log("Home mounted!!!!");
  }
  componentWillUnmount() {
    console.log("Home unmouted...");
  }
  componentDidUpdate() {
    console.log(" Home updated...");
  }
  render() {
    return (
      <div>
        <h3>Here is MyHome !!!</h3>
      </div>
    );
  }
}

const About = function () {
  return (
    <div>
      <h3> This is About!!!</h3>
    </div>
  );
};

class App extends React.Component {
  constructor(props) {
    super(props);
    console.log("APP created!!!!");
    this.state = {
      count: 0
    };
    this.handleClick = this.handleClick.bind(this);
  }
  componentDidMount() {
    console.log(" APP mounted!!!!");
  }
  componentWillUnmount() {
    console.log(" APP unmouted...");
  }

  handleClick() {
    this.setState((state) => ({
      count: state.count + 1
    }));
  }
  render() {
    console.log("App render...");
    return (
      <Router>
        <div>
          <h1>Hello CodeSandbox</h1>
          <h2>Start editing to see some magic happen!</h2>

          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
          </ul>
          <h3>{this.state.count}</h3>
          <button onClick={this.handleClick}> Add Count</button>
          <Switch>
            <Route path="/about" component={() => <About />}></Route>
            {/**
              1.每次路由切换都触发路由对应的组件重新渲染，无论是component属性还是render属性
               2. 当用component属性，值为匿名函数时，父组件的每次重新render（无论有没有props传递给子组件）都会造成子组件重新render（每次render都会执行constructor）
              3.当用component属性时，值为react元素时，父组件的每次重新render都不会造成子组件每次都执行constructor
              4.当用render属性时，值为匿名函数时，父组件的每次重新render都不会造成子组件每次都执行constructor
               5.当用render属性时，值不可以为react元素，只能是函数
              */}

            {/* 方式1开始 */}
            {/* <Route exact path="/" component={(routeProps) => <MyHome {...routeProps} />}></Route>  */}
            {/* 这种方式会造成每次App组件重新render时候MyHome组件都会重新执行一次初始化生命周期，而不是执行update，，但是它能够传递入参作为MyHome的props,可以访问路由属性，但是需要在函数指定入参，然后标记传递到子组件  */}
            {/* 方式1结束 */}

            {/* 方式2开始 */}
            {/* <Route exact path="/" component={MyHome}></Route> */}
            {/*这种方式在App组件render方法再执行时，不会造成MyHome组件重新渲染，只是update,缺点：不能从父组件传递props到子组件，可以在子组件访问路由属性 */}
            {/* 方式2结束 */}

            {/* 方式3开始 */}
            {/* <Route exact path="/">
              <MyHome />
            </Route> */}
            {/* 这种方式每次（第一次除外）App执行render不会造成MyHome重新执行一次初始化生命周期，只会执行update的生命钩子，可以从父组件传递props到子组件，但是这个方式有一个缺点，子组件不能在props访问路由属性 */}
            {/* 方式3结束 */}

            {/* 方式4开始 */}
            <Route
              exact
              path="/"
              render={(routeProps) => <MyHome {...routeProps} />}
            ></Route>
            {/* 这种方式App的非第一次执行render方法，不会造成子组件重新执行constructor等生命周期初始化方法，只会执行update，可以传递props到子组件，可以在子组件取得路由props（只需在传递时指明） */}
            {/* 方式4结束 */}
          </Switch>
        </div>
      </Router>
    );
  }
}

export default App;

```

[React Router 官方文档](https://reactrouter.com/web/api/Route/route-render-methods)
