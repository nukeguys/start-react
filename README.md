# start-react

## React

React is a JavaScript library for building user interfaces

> 리액트는 SPA 프레임워크가 아닌, 뷰 라이브러리(View Library)입니다. 여기서 뷰(View)란 MVC 패턴(Model–View–Controller Pattern)의 'V'를 말합니다. 뷰는 브라우저 내 특정 컴포넌트를 보여주는 역할을 하기 때문에, 리액트로 단일 페이지 애플리케이션을 개발할 수 있는 것입니다.

### Create React App

[https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

Boilerplate for react (babel, webpack, eslint, react, HMR, webpack-dev-server, build for production...)

```bash
npx create-react-app my-app
cd my-app
npm start
```

### JSX (JavaScript Xml)

JavaScript에서 markup(xml)을 사용할 수 있는 JavaScript의 문법적 확장  
react의 `element`를 생성(React.crateElement())

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

#### 꼭 닫혀야 하는 태그

```js
class App extends Component {
  render() {
    return (
      <div>
        <input type="text"> // <input type="text" />
      </div>
    );
  }
}
```

#### 감싸져 있는 엘리먼트 ([Fragment](https://reactjs.org/docs/fragments.html))

```js
class App extends Component {
  render() {
    return (
      // <Fragment>
      <div>
        Hello
      </div>
      <div>
        Bye
      </div>
      // </Fragment>
    );
  }
}
```

#### JSX 안에 자바스크립트 값 사용하기

```js
class App extends Component {
  render() {
    const name = 'react';
    return (
      <div>
        hello {name}!
      </div>
    );
  }
}
```

#### 조건부 렌더링

[Conditional Rendering in JSX](#Conditional-Rendering-in-JSX)

#### style 과 className

```js
class App extends Component {
  render() {
    const style = {
      backgroundColor: 'black',
      padding: '16px',
      color: 'white',
      fontSize: '12px'
    };

    return (
      <div style={style}>
        hi there
      </div>
    );
  }
}
```

#### 주석

```js
class App extends Component {
  render() {
    return (
      <div>
        {/* 주석은 이렇게 */}
        <h1
          // 태그 사이에
        >리액트</h1>
      </div>
    );
  }
}
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

### Event Handliing

#### React event

- lowercase > camelCase
- string > function
- return false > preventDefault()

> Your event handlers will be passed instances of `SyntheticEvent`, a cross-browser wrapper around the browser’s native event. It has the same interface as the browser’s native event, including `stopPropagation()` and `preventDefault()`, except the events work identically across all browsers.

```js
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

#### bind or arrow function

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

```js
class LoggingButton extends React.Component {
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

### in loop

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### Conditional Rendering in JSX

#### `&&` operator

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

#### `condition ? true : false`

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

#### prevent rendering

```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

\* lifecycle methods will still be called.

### List (array)

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

#### key

```js
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

>- Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity  
>- The best way to pick a key is to use a string that `uniquely identifies a list item among its siblings`. Most often you would use IDs from your data as keys  
>- If you choose not to assign an explicit key to list items then React will default to using indexes as keys  
>- Keys only make sense in the context of the surrounding array

map() in JSX

```js
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

### Form

> In a `controlled component`, form data is handled by a React component. The alternative is `uncontrolled components`, where form data is handled by the DOM itself.

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

#### textarea

`<textarea>` uses a `value` attribute.

```js
<textarea value={this.state.value} onChange={this.handleChange} />
```

#### select

instead of using this `selected` attribute, uses a `value` attribute on the root `select` tag.

```js
<select value={this.state.value} onChange={this.handleChange}>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

#### multiple inputs

> When you need to handle multiple controlled input elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

```js
lass Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value // computed property name
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

#### Controlled Input Null Value

> Specifying the value prop on a controlled component prevents the user from changing the input unless you desire so. If you’ve specified a value but the input is still editable, you may have accidentally set `value` to `undefined` or `null`.

#### Uncontrolled Component (v16.3~)

Managing focus, text selection, or media playback.
Triggering imperative animations.
Integrating with third-party DOM libraries.

[React.createRef()](https://github.com/reactjs/rfcs/blob/master/text/0017-new-create-ref.md)

DOM Element

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />

        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

Class Component

```js
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />
    );
  }
}
```

Do not use ref attribute on function components

```js
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will *not* work!
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```

Default Values

```js
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={this.input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

### Lifting State Up

> Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let’s see how this works in action.

child의 state를 common parent의 state로 옮기고, props를 통해 child로 전달.  
child의 변화는 props에 callback을 전달해서 child에서 호출시 parameter로 전달.

### Composition vs Inheritance

> React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.

#### props.children

```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

#### composition better then inheritance

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />

  );
}
```

### Thinking in React

#### Step 1: Break The UI Into A Component Hierarchy

[single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)

#### Step 2: Build A Static Version in React

> props are a way of passing data from parent to child. If you’re familiar with the concept of state, don’t use state at all to build this static version.

#### Step 3: Identify The Minimal (but complete) Representation Of UI State

>1. Is it passed in from a parent via props? If so, it probably isn’t state.  
>2. Does it remain unchanged over time? If so, it probably isn’t state.  
>3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

#### Step 4: Identify Where Your State Should Live

>1. Identify every component that renders something based on that state.
>2. Find a common owner component (a single component above all the components that need the state in the hierarchy).
>3. Either the common owner or another component higher up in the hierarchy should own the state.
>4. If you can’t find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

#### Step 5: Add Inverse Data Flow

## Reference

- [React Docs](https://reactjs.org/docs/getting-started.html)
- [Glossary of React Terms](https://reactjs.org/docs/glossary.html#single-page-application)
- [create-react-app](https://github.com/facebook/create-react-app)
- [Creating a React App... From Scratch](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658)
- [누구든지 하는 리액트 5편: LifeCycle API](https://velopert.com/3631)
- [[번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?](https://velopert.com/3236)
- [List of top 222 ReactJS Interview Questions & Answers](https://github.com/sudheerj/reactjs-interview-questions#what-are-synthetic-events-in-reactjs)
- [깊이 있는 리액트 개발 환경 구축하기](http://sujinlee.me/webpack-react-tutorial/)
- [The the Road to learn React](https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean)
- [Getting Started with React – An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)
- [codesandbox](https://codesandbox.io/s/new)
- [codepdn](https://codepen.io/pen?&editors=0010)