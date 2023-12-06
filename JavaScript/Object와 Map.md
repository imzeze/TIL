자바스크립트에서 객체를 생성하기 위해서 `{}`를 사용할 수도 있지만 `Map` 객체를 이용할 수도 있다.

아래 글은 [Use Maps More and Objects Less](https://www.builder.io/blog/maps)의 번역글로, `Map` 객체 사용을 권장하고 있다.

<br />

## 키 접근

plain object의 `delete` 연산자는 성능이 안좋기로 악명높은 반면에, map 객체는 키를 제거하는 데 최적화되어 있다.
<img width="610" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/3aa3e455-7b08-4c02-a33c-7eea67c769a4">
<img width="610" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/7aec8abe-06b9-4a8a-ac53-39c27ad78b85">

이유는 자바스크립트 VM이 plain object의 형태를 가정하여 최적화하는 반면 Map 객체는 키가 동적으로 바뀌는 해시맵의 쓰임새를 바탕으로 만들어졌기 때문이다.

<br />

## 내장된 키

plain object를 생성하더라도 사실 내부에는 이미 값이 선언되어 있는 키들이 있다.

따라서 임의의 키를 사용하는 경우, 생각하지 못한 버그로 이어질 수 있다.

<img width="501" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/688250bc-546a-4294-aa60-6aa0a44da621">

<br />

## 불편한 반복

plain object의 키를 반복하기 위해서는 `Object.keys()` 메서드를 이용해 키 배열을 가져와야한다. 반면에 Map 객체를 사용하면 `for` 연산자를 이용해 key, value에 접근할 수 있다.

```js
Object.keys(myObject).forEach((key) => {
  // 😬
});
```

```js
for (const [key, value] of myMap) {
  // 😍
}

for (const key of myMap.keys()) {
  // 😍
}
```

<br />

## 키 순서

Map 객체는 키의 순서를 유지한다. 따라서 객채에서 직접 키를 분해할 수 있다.

```js
const [[firstKey, firstValue]] = myMap;
```

<br />

## 복사

plain Object를 복사할 때 `spread` 연산자를 사용해 쉽게 복사할 수 있다는 장점이 있다.
Map 객체 역시 복사하기 쉽다.

```js
const copied = new Map(myMap);
```

단, iterable한 객체만 가능하다.

```js
const myMap = new Map([
  ["key", "value"],
  ["keyTwo", "valueTwo"],
]);
```

<img width="399" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/873156a7-28ef-4195-ab3d-4c1c9e02d143">

<br/>

## 키 타입

Map 객체의 키로 모든 타입의 객체를 사용할 수 있다.

```js
myMap.set({}, value);
myMap.set([], value);
myMap.set(document.body, value);
myMap.set(function () {}, value);
myMap.set(myDog, value);
```

하지만 키에 참조가 걸려있기 때문에 GC의 대상이 되지 않아 메모리 누수의 원인이 될 수 있다.

<br />

#### Reference

[Use Maps More and Objects Less](https://www.builder.io/blog/maps)
