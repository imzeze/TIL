# this

객체지향언어에서 this는 인스턴스를 가르키지만 자바스크립트에서 this는 **상황에 따라 달라진다.**  
js의 this는 실행컨텍스트가 생성될 때 (함수가 호출될 때) 결정된다.

<br />

## 전역 컨텍스트

- 전역에서의 this는 전역 객체를 가르킨다.
  - browser: window
  - node: global

<br />

## 함수와 메소드

함수로 호출된 경우 this는 전역객체를 가르킨다.

```js
const print = function (x) {
  console.log(this, x);
};

print(1); // window, 1
```

메소드를 호출한 경우 this는 메소드를 호출한 객체가 호출 주체가 된다.

```js
const obj = {
  method: print,
};

obj.method(2); // {method: f}, 2
```

단 메소드의 내부 함수에서 호출된 경우에 this는 전역 격체를 가리킨다.

```js
const obj = {
  outer: function () {
    const innerFunc = function () {
      console.log(this);
    };

    innerFunc(); // window
  },
};

obj.outer();
```

![image](https://github.com/imzeze/TIL/assets/67260437/629a07ee-b820-4404-bdc2-510226d8932d)

먼저 `outer` 함수가 실행되면서 Call Stack에 `전역 컨텍스트`와 `outer` 함수가 쌓인 것을 확인할 수 있다.

`obj`의 메소드가 실행된 것이므로 local scope의 this는 `{outer: ƒ}`를 가리키는 것으로 확인된다.

![image](https://github.com/imzeze/TIL/assets/67260437/d008a5fa-dbd9-4da5-b4eb-e896657b9fde)

이후 지역 변수에 `innerFunc`이 할당되고,

![image](https://github.com/imzeze/TIL/assets/67260437/c15f1cc6-4a18-436a-93cc-d65ecc2d7a29)

`innerFunc`이 실행되어 Call Stack에 `innerFunc`이 쌓이고 내부 함수에서 this가 `Window`를 가리키는 것을 확인할 수 있다.

<br />

## 화살표함수

화살표 함수는 this를 바인딩하지 않고, 상위 스코프의 this를 가리킨다. 함수 내부에서 this가 전역객체를 가르키는 문제를 보완한다.

```js
const obj = {
  outer: function () {
    console.log(this); // {outer: f}

    const inner = () => {
      console.log(this);
    };

    inner(); // {outer: f}
  },
};

obj.outer();
```

<br />

## 콜백함수 this 지정

- 콜백함수에 this를 정의하지 않는 경우

전역객체를 가르킨다. 따라서, 클래스형 컴포넌트에서 생성한 메소드를 콜백으로 사용하기 위해선 바인딩을 해줘야한다.

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState((prevState) => ({
      isToggleOn: !prevState.isToggleOn,
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? "ON" : "OFF"}
      </button>
    );
  }
}
```

```js
const result = {
  sum: 0,
  add: function () {
    const args = [...arguments];
    args.forEach((num) => (this.sum += num), this);
  },
};

result.add(1, 2, 3);

console.log(result.sum); // 6
```
