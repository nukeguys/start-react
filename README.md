# start-react

## React

React is a JavaScript library for building user interfaces

> 리액트는 SPA 프레임워크가 아닌, 뷰 라이브러리(View Library)입니다. 여기서 뷰(View)란 MVC 패턴(Model–View–Controller Pattern)의 'V'를 말합니다. 뷰는 브라우저 내 특정 컴포넌트를 보여주는 역할을 하기 때문에, 리액트로 단일 페이지 애플리케이션을 개발할 수 있는 것입니다.

### Create React App

[https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

Boilerplate for react (babel, webpack, eslint, react, HMR, webpack-dev-server, build for production...)

### JSX (JavaScript Xml)

JavaScript에서 markup(xml)을 사용할 수 있는 JavaScript의 문법적 확장  
react의 `element`를 생성

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
// by babel
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

function getGreeting(user) {
  if (user) {
    return (
        <div tabIndex={user.index}>
            <h1>Hello, {formatName(user)}!
            </h1>
        </div>
    );
  }
  return (
        <div tabIndex="0">
            <h1>Hello, Stranger.</h1>
        </div>
    );
}
const user = {
  firstName: 'Harper',
  lastName: 'Perez',
  index: "1"
};

const element = getGreeting(user);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### Element

An element describes what you want to see on the screen

> Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

#### Render Element

```html
<div id="root"></div>
```

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

#### Update Element

React elements are immutable.

> React DOM compares the element and its children to the previous one, and `only applies the DOM updates necessary` to bring the DOM to the desired state.

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

[(State and Lifecycle)](#State-and-Lifecycle)

### Component

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation

> Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called `“props”`) and `return React elements` describing what should appear on the screen.

props
> When React sees an element representing a user-defined component, it passes `JSX attributes` to this component as a single object. We call this object `“props”`.
s

#### Function Component

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

#### Class(ES6) Component

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

#### Render Component

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />; // user-defined component
ReactDOM.render(
  element,  // <Welcome name="Sara" />
  document.getElementById('root')
);
```

> 1. We call `ReactDOM.render()` with the `<Welcome name="Sara"/>`element.
> 2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
> 3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
> 4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

\* **Component 이름은 항상 대문자로 시작해야 한다.**

#### Comopse Component

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

#### Extract Component

AS-IS

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

To-BE

```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
};
```

```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

```js
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div classNmae="Comment-text">
        {props.text}
      </div>
      <div>
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

#### Props are Read-Only

All React components must act like [pure](https://en.wikipedia.org/wiki/Pure_function) functions with respect to their props.

> Pure function
> 1. Its return value is the same for the same arguments (no variation with local static variables, non-local variables, mutable reference arguments or input streams from I/O devices).
> 2. Its evaluation has no side effects (no mutation of local static variables, non-local variables, mutable reference arguments or I/O streams).

### State and Lifecycle

#### State

State is similar to props, but it is `private and fully controlled by the component`. (*a feature available only to `classes`.*)

#### LifeCycle (v16.3~)

`Mount` - Rendered to the DOM for the first time. (render())  
`Unmount` - DOM produced by the component is removed.

![lifecycle](https://cdn-images-1.medium.com/max/1600/0*OoDfQ7pzAqg6yETH.)

- `constructor(props)` - component가 새로 만들어질 때 마다 호출
- `componentWillMount()` - deprecated (UNSAFE_componentWillMount)
- `componentDidMount()` - 외부 라이브러리 연동, ajax 요청, DOM 접근(읽기, 변경...)
- `componentWillReceiveProps(nextProps)` - deprecated (UNSAFE_componentWillReceiveProps)
- `getDerivedStateFromProps(nextProps, prevState)` - props로 state를 동기화 하는 작업, props가 바뀔 때 설정하고 싶은 state값을 리턴 (없으면 null)
- `shouldComponentUpdate(nextProps, nextState)` - component 최적화, [Virtual DOM](https://www.youtube.com/watch?v=muc2ZF0QIO4)에 rendering 여부를 반환)
- `componentWillUpdate(nextProps, nextState)` - deprecated
- `getSnapshotBeforeUpdate(prevProps, prevState)` - DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 리턴값은 componentDidUpdate 에서 3번째 파라미터로 전달
- `componentDidUpdate(prevProps, prevState, snapshot)` - this.props, this.state가 바뀐 상태
- `componentWillUnmount()`

#### We want: [Clock](#Update-Element) update itself

```js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

#### Funtion to Class

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

#### props to state

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

#### Add lifecycle methods

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

> 1. When `<Clock />` is passed to `ReactDOM.render()`, React calls the constructor of the `Clock` component. Since `Clock` needs to display the current time, it initializes `this.state` with an object including the current time. We will later update this state.
> 2. React then calls the `Clock` component’s `render()` method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the `Clock`’s render output.
> 3. When the `Clock` output is inserted in the DOM, React calls the `componentDidMount()` lifecycle method. Inside it, the `Clock` component asks the browser to set up a timer to call the component’s `tick()` method once a second.
> 4. Every second the browser calls the `tick()` method. Inside it, the `Clock` component schedules a UI update by calling `setState()` with an object containing the current time. Thanks to the `setState()` call, React knows the state has changed, and calls the `render()` method again to learn what should be on the screen. This time, `this.state.date` in the `render()` method will be different, and so the render output will include the updated time. React updates the DOM accordingly.
> 5. If the `Clock` component is ever removed from the DOM, React calls the `componentWillUnmount()` lifecycle method so the timer is stopped.

#### Using State Correctly

Do Not Modify State Directly

```js
// Wrong, not re-render a component
this.state.comment = 'Hello';
// Correct
this.setState({comment: 'Hello'});
```

State Updates May Be Asynchronous
> Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state.

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

State Updates are Merged

```js
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

#### The Data Flows Down

> Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.

```js
// in Clock
<h2>
It is {this.state.date.toLocaleTimeString()}.
</h2>
```

```js
// user-defined component
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
<FormattedDate date={this.state.date} />
```

> FormattedDate wouldn’t know whether it came from the Clock’s state, from the Clock’s props, or was typed by hand

## Reference

- [React Docs](https://reactjs.org/docs/getting-started.html)
- [Glossary of React Terms](https://reactjs.org/docs/glossary.html#single-page-application)
- [create-react-app](https://github.com/facebook/create-react-app)
- [Creating a React App... From Scratch](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658)
- [누구든지 하는 리액트 5편: LifeCycle API](https://velopert.com/3631)
- [[번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?](https://velopert.com/3236)
- [깊이 있는 리액트 개발 환경 구축하기](http://sujinlee.me/webpack-react-tutorial/)
- [The the Road to learn React](https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean)
- [Getting Started with React – An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)
- [codesandbox](https://codesandbox.io/s/new)
- [codepdn](https://codepen.io/pen?&editors=0010)