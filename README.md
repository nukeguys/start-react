# start-react

## React

React is a JavaScript library for building user interfaces

> 리액트는 SPA 프레임워크가 아닌, 뷰 라이브러리(View Library)입니다. 여기서 뷰(View)란 MVC 패턴(Model–View–Controller Pattern)의 'V'를 말합니다. 뷰는 브라우저 내 특정 컴포넌트를 보여주는 역할을 하기 때문에, 리액트로 단일 페이지 애플리케이션을 개발할 수 있는 것입니다.

### Create React App

https://github.com/facebook/create-react-app

Boilerplate for react (babel, webpack, eslint, react, HMR, webpack-dev-server, build for production...)

### JSX (JavaScript Xml)

syntax extension to JavaScript.(markup in JS)  
JSX produces React “elements”

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

#### Render

```html
<div id="root"></div>
```

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

#### Update

React elements are immutable.

> React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

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

## Reference

- [React Docs](https://reactjs.org/docs/getting-started.html)
- [Glossary of React Terms](https://reactjs.org/docs/glossary.html#single-page-application)
- [create-react-app](https://github.com/facebook/create-react-app)
- [Creating a React App... From Scratch](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658)
- [깊이 있는 리액트 개발 환경 구축하기](http://sujinlee.me/webpack-react-tutorial/)
- [The the Road to learn React](https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean)
- [Getting Started with React – An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)
- [codesandbox](https://codesandbox.io/s/new)
- [codepdn](https://codepen.io/pen?&editors=0010)