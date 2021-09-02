# JavaScript Syntax Extension

```js
const element = <div>Hello, world</div>;
```

JSX는 string도 XML도 아닌 JavaScript의 확장된 구문이다.

<br />

React는 UI 로직과 마크업을 동시에 표현하는 여러개의 component를 만들어 조립하는데, 이 component를 표현하기 위해 JSX가 사용된다. 물론 JSX는 React의 필수요소가 아닌 편리함을 위한 `syntactic sugar`일 뿐이다.

| SoC (Seperation of Concerns): 관심사의 분리. 기능별로 component를 분리함으로써 개발의 복잡성을 줄이며 유지보수 하기 좋은 코드를 만들 수 있다.

<br />

JSX는 compile을 거쳐 javascript로 변환되기 때문에 JSX는 함수 형태이든 변수 형태이든, 자유롭게 사용 가능하다.

<br />

React Dom은 JSX 내 값들을 렌더링하기 전에 모두 string으로 변환하기 때문에 XSS 공격으로부터 안전하다.

| XSS (cross-site-scripting): 사용자 input에 악성 script가 담긴 글을 삽입해 클라이언트를 대상으로 하는 공격

<br />

JSX는 HTML보단 Javascript에 가깝기 때문에 HTML의 attribute들을 CamelCase로 표현한다.

```js
const element = <div fontSize="12px">hello, world</div>;
```
