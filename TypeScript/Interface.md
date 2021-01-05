## Duck Typing

TypeScript는 **값의 형태**에 대해 타입 검사를 한다. 이를 덕 타이핑(duck typing)" 혹은 "구조적 서브타이핑 (structural subtyping)"이라고도 한다. 인터페이스는 이런 형태를 만드는 역할을 한다. 

```ts
function printLabel(labelObj: {label: string} {
    //
})

let myObj = {size: 10, label: "obj1"}
printLabel(myObj)
```
`printLabel`가 호출되면 매개변수에 최소한으로 필요한 프로퍼티가 있는지, 타입이 일치하는지 타입 검사가 이루어진다. `printLabel`에서 `{label: string}`으로 정의한 타입을 다음과 같이 **Interface**로 분리할 수 있다. 

```ts
interface LabeledValue {
    label: string
}

function printLabel(labelObj: LabeledValue) {
    //
})

let myObj = {size: 10, label: "obj1"}
printLabel(myObj)
```  

<br/>

## property 지정

### Optional Properties
interface의 property 중 필수적이지 않은 것은 `?`를 이용해 표현한다. 
```ts
interface SquareConfig {
    color?: string;
    width?: number;
}
```

### Readonly Properties
객체 생성 후 property 수정을 불가능하게 하기 위해선 `readonly`를 사용한다. 

```ts
interface Point {
    readonly x: number;
    readonly y: number;
}
```
`const`와의 차이점은 `const`는 변수에 사용하고, `readonly`는 프로퍼티에 사용한다는 데에 있다. 

## 함수 표현
property 말고도 함수를 interface로 표현할 수 있다. 
```ts
interface SearchFn {
    (source: string, subString: string): boolean;   
}

let mySearch: SearchFn;
mySearch = function (source: string, subString: string): booelean {
    //
}
```
함수를 inferface로 표현한 경우, 다음과 같이 타입을 생략할 수 있다. 물론 매개변수의 이름이 같을 필요도 없다. TypeScript의 **Context Typing**이 인수 타입을 추론할 수 있기 때문이다. 

```ts
let mySearch: SearchFn;
mySearch = function (src, sub) {
    //
}
```
