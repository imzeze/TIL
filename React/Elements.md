# Rendering Element

Element는 Component 구성요소로,  
React Dom은 `root DOM node` 내부의 Elements를 Render한다.   

Element는 immutable하다. 즉, 한번 Rendering되면 해당 Element의 childeren이나 attribute를 수정할 수 없다. 그러므로 UI를 수정하기 위해선 새로운 Element를 생성하여 새로 Rendering 되어야한다.   

React DOM은 새로 생성된 Element와 기존의 Element를 비교하여 바뀐 부분만 update한다.   

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
위 예제의 `element`가 새로 rendering될 때 실제로 update되는 부분은 `{ }` 내 text뿐이다. 

<br /> 
<br />   

---
[React documentation](https://devdocs.io/react/rendering-elements)