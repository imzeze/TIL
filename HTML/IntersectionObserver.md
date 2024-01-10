# Intersection Observer API

Intersection Observer API는 뷰포트에 element가 들어왔는지를 감지하며, 들어왔을 때 처리되어야할 이벤트를 실행시킨다. 만약 scroll event를 감지하는 로직을 짰다면, scroll이 발생될 때마다 로직이 실행되겠지만, Intersection Observer를 사용하면 해당 element가 뷰포트에 들어왔을 때만 로직이 실행된다는 장점이 있다.

<br />

## options

- root  
  device의 viewport를 기준으로 교차점을 확인하려면 null 값을 사용하고, 스크롤이 가능한 부모요소에서 자식요소의 교차점을 확인하려면 부모요소를 지정한다.

- rootMargin  
  루트 주변의 여백

- threshold  
  target 요소가 노출된 정도를 백분율로 나타낸다. (1.0 ~ 0.0)  
  만약 특정 비율일 때마다 콜백을 실행시키고자 한다면 배열을 사용하면 된다 (ex. [0, 0.2. 0.5, 1])

<br />

## callback

### entry

특정 전환 순간에 targer과 root 사이의 교차점에 대한 정보를 나타낸다.

- boundingClientRect  
  target의 경계 사각형을 나타낸다.  
  (ex. {x: 123.123, y: 123.123, width: 20, height: 20})

- intersectionRatio  
  교차 비율에 대한 정보를 나타낸다.

- intersectionRect  
  target 요소의 노출된 부분에 대한 정보를 나타낸다.
- isIntersecting  
  target 요소가 노출되었는지 아닌지 판별한다.

- rootBounds  
  root의 범위 정보를 나타낸다.  
   (ex. {x: 123.123, y: 123.123, width: 20, height: 20})

- target  
  변경점에서 교차된 요소를 나타낸다.

- time  
  교차로가 기록된 시간을 나타낸다.

### observer

- .disconnect()  
  모든 target 감지를 멈춘다.

- .observe()  
  감지할 target 요소를 지정한다.

- .unobserver()
  특정 target 요소의 감지를 멈춘다.

<br />

## 예시

```js
const imageEle1 = useRef(null);
const imageEle2 = useRef(null);

useEffect(() => {
  let options = {};
  let callback = (entries, observer) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.src = entry.target.dataset.src;
        observer.unobserve(entry.target);
      }
    });
  };

  let observer = new IntersectionObserver(callback, options);
  observer.observe(imageEle1.current);
  observer.observe(imageEle2.current);

  return () => observer.disconnect();
}, []);
```

```html
<img ref="{imageEle1}" data-src="{main_parts}" />,
<img ref="{imageEle2}" data-src="{main_parts}" />,
```
