# Virtual DOM

### JSX Compilation

JSX는 브라우저에서 인식되지 않기 때문에 plain javascript로 compile 되어진다.

```js
// JSX
<div className="cn">content</div>;

// formal JavaScript
React.createElement("div", { className: "cn" }, "content");
```

React.createElement의 인자는 다음과 같다.

```js
React.createElement(type of Element, Object With Element attributes, ...Element children)
```

첫번째 인자는 Element의 Type으로, HTML의 tag name인 string이거나 Component가 될 수 있다.

```js
// JSX
<Avatar userName={userName} />;

// formal JavaScript
React.createElement(Avatar, { userName });
```

Element의 모든 children은 인자에 포함된다.

```js
// JSX
<div className="cn">
  content1
  <br />
  content2
</div>;

// formal JavaScript
React.createElement(
  "div",
  { className: "cn" },
  "content1",
  React.createElement("br"),
  "content2"
);

// or
React.createElement("div", { className: "cn" }, [
  "content1",
  React.createElement("br"),
  "content2",
]);
```

<br />

### Rendering

`ReactDOM.createElement()`는 `ReactDOM.render()`에서 호출되어 DOM에 포함된다.

```js
ReactDOM.render(
  React.createElement(Avatar, { userName }),
  document.getElementById("#root")
);
```

`ReactDOM.createElement()`는 호출되면 다음과 같은 Object를 return한다.

```js
// React.createElement()
React.createElement(
  'div',
  { className: 'cn' },
  'content1',
  'content2',
);

// return
{
    type: 'div',
    props: {
        className: 'cn',
        children: {
            'content1',
            'content2',
        }
    },
    // ...
}
```

`React.render()`는 `React.createElement()`에서 반환된 Object를 DOM node로 변환시켜 브라우저가 화면에 표시할 수 있도록 한다.

<br />

> [Next](<https://github.com/imzeze/TIL/blob/master/React/VirtualDOM(2).md>)

<br />
<br />

---

#### Reference

[Optimizing React:
Virtual DOM explained](https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained#fixing-things-mountingunmounting)
