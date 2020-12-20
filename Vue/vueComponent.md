# Component 등록  

```js
Vue.component('componentA', {});
Vue.component('componentB', {});
Vue.component('componentC', {});

new Vue({
    el : '#app'
})
```

위와 같이 생성한 컴포넌트는 **전역등록된 컴포넌트**이다. 어느 루트의 Vue 인스턴스에서도 사용할 수 있으며 하위 컴포넌트에서도 사용 가능하다. 전역으로 등록되어 있기 때문에 더이상 사용하지 않는 컴포넌트도 최종 빌드에 포함된다. 컴포넌트를 js객체로 선언하여 이를 해결할 수 있다.   

```js
var componentA = {};
var componentB = {};
var componentC = {};

new Vue({
    el : "#app",
    components : {
        'componentA' : componentA
    }
})
``` 

이렇게 생성된 컴포넌트는 **지역등록된 컴포넌트**라고 한다. 전역 컴포넌트를 하위 컴포넌트에서도 사용할 수 있던 것에 비해 지역 컴포넌트는 하위 컴포넌트에서 사용될 수 없다. `componentB`에서 `componentA`를 사용하고 싶다면 다음과 같이 선언해야한다. 

```js
// 선언 방법 1
var componentB = {
    components : {
        'componentA' : componentA
    }
}

// 선언 방법 2
import componentA from './componentA'

export default {
    components : {
        componentA
    }
}
```

<br/>  

# Props

## Props 이름 지정

props의 이름을 카멜케이스로 저장한 경우 HTML에서는 케밥케이스로 변경되는 것에 유의해야한다. 

```js
Vue.component('componentA', {
    props : ['postTitle'],
    template : '<h3>{{ postTitle }}</h3>'
}) 
```
```xml
<componentA post-title="props"></componentA>
```

## Prop 타입 지정

props를 객체로 선언함으로써 특정 타입의 값을 갖는 prop list를 만들 수 있다. 

```js
Vue.component('componentA', {
    props : {
        title : String,
        likes : Number,
        isPublished : Boolean,
        commentIds : Array,
        author : Object,
        callback : Function,
        contactPromise : Promise
    }
})
```

## Prop 전달

```xml
<!-- 정적으로 전달 -->
<componentA title="Vue-props"></componentA>

<!-- 동적으로 전달 -->
<componentA v-bind:title="componentB.title"></componentA>
```
prop type이 `Number`, `Boolean`, `Array`, `Object`인 경우 문자열이 아닌 js 표현식이라는 것을 명시해야하므로 정적으로 전달하더라도 `v-bind`를 이용해야한다. 

```xml
<componentA v-bind:like="43"></componentA>
<componentA v-bind></componentA>
<componentA v-bind:is-published="false"></componentA>
<componentA v-bind:commentIds="[user01, user02, user03]"></componentA>
<componentA v-bind:author="{name:'author01', company : 'veridian'}"></componentA>
```
boolean의 경우 prop의 값이 없으면 true를 전달한다.   
객체를 전달하는 경우 객체 이름만 적으면 객체의 모든 속성이 전달된다. 

```js
author : {
    id : `author01`,
    company : 'veridian'
}
```
```xml
<componentA v-bind="author">
```

## Props 값 변경  
prop은 부모와 자식 사이에 단방향 바인딩 형태를 갖는다. 따라서 자식은 부모의 prop 값을 변경할 수 없다. prop 값을 변경하고자 할 경우에는 변경 대신 다음과 같은 방법으로 처리한다. 

* 로컬 데이터로 사용하고자 하는 경우 
```js
Vue.component('componentB', {
    props : ['initial'],
    data : function() {
        return {
            temp : this.initial
        }
    }
})
```
단, 객체나 배열은 복사가 아닌 참조를 하기 때문에 자식요소가 부모 요소를 변경하게 된다. 

* 전달된 prop의 형태를 바꾸고자하는 경우
```js
Vue.component('componentB', {
    props : ['initial'],
    computed : {
        print : function () {
            return this.initial.toLowerCase()
        }
    }
})
```

## Prop 유효성 검사

```js
Vue.component('componentC', { 
    props : {
        // 모든 타입 허용
        propA : Null,
        propB : undefinded,
        // 여러 타입 허용
        propC : [String, Number],
        // 필수 문자열 
        propD : {
            type : String,
            required : true
        }
        // 기본값이 있는 숫자형
        propE : { 
            type : Number,
            default : 100 
        }
        // 객체나 배열은 반드시 기본값을 반환하는 함수 필요
        propF : {
            type : Object,
            default : function() {
                return { message : 'hello'}
            }
        }
    }   
})
```