# URL 구분 방법

`url`은 인터넷 주소를 쉽게 조작하도록 도와주는 모듈이다.   
`url` 사용 방법에 따라 주소 체계가 다른 url을 생성, 사용할 수 있다. 

<br/>  

## WHATWG 방식

### 생성 방식 
```js
const url = require('url');

const URL = url.URL;
const myURL = new URL('http://user:password@sub.host.com:8080/p/a/t/h?query=string#hash');
```
WHATWG의 url은 `url` 모듈의 `URL` 생성자를 이용해 생성된다. 

### 주소 체계

| 주소체계          | url               |  
| ---               | ---                   |
| protocal          | http:                 |
| **origin**        | **http://**           |
| **username**      | **user**              |
| **password**      | **password**          |
| hostname          | sub.host.com          | 
| port              | 8080                  |
| **host(=origin)** | **sub.host.com:8080** |
| pathname          | /p/a/t/h              |
| **search**        | **?query=String**     |
| hash              | #hash                 |
| href              | http://user:password@sub.host.com:8080/p/a/t/h?query=string#hash |

### 속성 및 특징

* searchParams
    ```js
    const url = require('url');

    const URL = url.URL;
    const myURL = new URL('http://user:password@sub.host.com:8080/p/a/t/h?query=string#hash');

    console.log(myURL.searchParams);

    // URLSearchParams { 'query' => 'string' }
    ```
* 값이 없는 경우 return 값 = ''
     ```js
    const url = require('url');

    const URL = url.URL;
    const myURL = new URL('http://sub.host.com/p/a/t/h?query=string#hash');

    console.log(myURL.username);
    console.log(myURL.password);
    console.log(myURL.port);

    // ''
    // ''
    // ''
    ```
* `host` 부분 없이 `pathname`만 가지고 있는 url도 사용할 수 없다. 


<br/>  

## 기존 node에서 사용되던 방식

### 생성 방식
```js
const url = require('url');

const myURL = url.parse('http://user:password@sub.host.com:8080/p/a/t/h?query=string#hash');
```

### 주소 체계

| 주소체계          | url                        |    
| ---               | ---                        |
| protocal          | http:                      |
| **auth**          | **user:password**          |
| hostname          | sub.host.com               | 
| port              | 8080                       |
| **href**          | **sub.host.com:8080**      |
| pathname          | /p/a/t/h                   |
| search            | ?query=String              |
| **query**         | **query=String**           |
| **path**          | **/p/a/t/h/?query=String** |
| hash              | #hash                      |
| href              | http://user:password@sub.host.com:8080/p/a/t/h?query=string#hash |

### 특징

* WHATWG 방식에 `searchParams` 속성과 같이 search 부분을 쉽게 객체화 하기 위해 `queryString` 모듈을 사용한다. 
    ```js
    const url = require('url');
    const querystring = require('querystring');

    const myURL = url.parse('http://sub.host.com/p/a/t/h?query=string#hash');
    const query = querystring.parse(myURL.query);

    console.log(query);

    // { query: 'string' }
    ```
* 값이 없는 경우 return 값 = null
    ```js
    const url = require('url');

    const myURL = url.parse('http://sub.host.com/p/a/t/h?query=string#hash');

    console.log(myURL.auth);
    console.log(myURL.port);

    // null
    // null
    ```
*  `host` 부분이 없어도 url을 사용할 수 없다.

<br/>
<br/>  

--- 
#### Reference 

조현영,「Node.js 교과서」p99 - p103