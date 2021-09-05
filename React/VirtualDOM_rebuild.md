# Virtual DOM

> [Previous](https://github.com/imzeze/TIL/blob/master/React/VirtualDOM_render.md)

<br />

### Rebuilding

React는 `React.createElement()`에서 반환된 Object를 비교하여 Node tree의 어떤 부분이 update되야하는지 파악한다.

```js
// before update:
{ type: 'div', props: { className: 'cn' } }

// after update:
{ type: 'div', props: { className: 'cnn' } }
```

두 Object의 type이 **string이고 같은 경우** React는 DOM tree에서 node를 삭제하지 않고 DOM API를 이용해 property를 수정한다.

```js
// before update:
{ type: 'div', props: { className: 'cn' } }

// after update:
{ type: 'span', props: { className: 'cn' } }
```

만약 type이 **string이지만 다른 경우**라면 Reacts는 DOM tree에서 기존 node를 삭제해버리고 새로운 node로 대체한다.

만약 type이 **Component**라면 Component 구성요소에 변화가 없는지 확인하는 작업이 반복된다.

<br />

React는 element의 children에 수정이 생긴 경우 기존 element의 children과 새로운 element의 children을 **index-by-index**를 기준으로 비교한다.

비교 결과가 다르다면, 기존 children은 삭제되고 새로운 children으로 대체된다. 만약 1000rows를 갖는 Table Component에서 첫번째 row가 삭제되면, 나머지 999rows에 대해 삭제되고 대체되는 로직이 반복된다.

이런 비효율성을 해결하기 위해 React는 **Key**를 이용한다. key는 unique하기 때문에, 어떤 item이 추가, 수정, 삭제되었는지 효율적으로 파악할 수 있다.

> 주의사항
>
> - key가 global하게 unique할 필요는 없지만 한 component내 sibling 사이에선 unique 해야한다.
> - key는 component에 props로 넘겨지지 않는다.
> - key값으로 index값을 사용하는 것은 side effect가 있을 수 있기 때문에 지양된다.

<br />

### state

component 내에서 state가 바뀌면 component 전체가 Rerendering된다. 물론 Parent나 sibling은 Rerendering되지 않는다.

<br />
<br />

---

#### Reference

[Lists and Keys
](https://reactjs.org/docs/lists-and-keys.html)  
[Optimizing React:
Virtual DOM explained](https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained#fixing-things-mountingunmounting)
