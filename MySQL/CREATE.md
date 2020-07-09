MySQL에서는 데이터베이스와 스키마가 동일한 개념이다. 

* 스키마 생성 및 사용
```sql
CREATE SCHEMA mirror;
use mirror;
```
<br/>  

* 테이블 생성 예시
```sql
CREATE TABLE mirror.users ( 
    id INT NOT NULL AUTO_INCREMENT, 
    name VARCHAR(20) NOT NULL, 
    age INT UNSIGNED NOT NULL, 
    married TINYINT NOT NULL, 
    comment TEXT NULL, 
    created_at DATETIME NOT NULL DEFAULT now(), 
    PRIMARY KEY(id), 
    UNIQUE INDEX name_UNIQUE (name ASC)) 
COMMENT = '사용자 정보' DEFAULT CHARSET=utf8 ENGINE=InnoDB;
```
[자료형 참고](https://zetawiki.com/wiki/MySQL_%EC%9E%90%EB%A3%8C%ED%98%95)

> `CHAR(n)`형은 고정 길이 문자열로, 지정한 길이보다 짧은 문자열이 들어오면 공백으로 채워진다. 따라서 가변적인 문자열이라면 `VARCHAR(n)`를 사용하는 것이 낫다. 문자열의 길이가 매우 긴 경우에는 `TEXT`형이 쓰인다. 

> `UNSIGNED`는 음수형을 받지 않는다는 것을 의미한다. 나이와 같이 음수가 들어오지 않는 컬럼에는 `UNSIGNED` 옵션을 주는 것이 좋다.  
`FLOAT`형과 `DOUBLE`형에는 적용할 수 없다. 

> `ZEROFILL`은 빈자리수를 `0`으로 채워준다. `INT(4)`에 1이 들어오면 0001로 저장된다. 

> `UNIQUE INDEX`는 값이 고유해야함을 지정하는 옵션이다. 

```sql 
CREATE TABLE mirror.comments (
    commenter INT NOT NULL,
    INDEX commenter_idx(commenter ASC),
    CONSTRAINT commenter
    FOREGIN KEY (commenter) 
    REFERENCES mirror.users (id)
)
```
> `INDEX`는 SELECT 성능 향상을 위한 일종의 목차로 생각하면 된다. 처음부터 끝까지 데이터를 조회하지 않고 `INDEX`를 기준으로 조회하기 때문에 SELECT의 성능이 향상된다. 따라서 `INDEX`의 기준이 되는 컬럼은 카디널리티가 높아야 한다. 카디널리티가 높다는 것은 데이터의 중복성이 낮다는 것을 의미한다. 


<br/>  

* 테이블 확인 

```sql
DESC users;
```

<br/>  

----
#### Reference
Node.js 교과서, 조현영  
[[mysql] 인덱스 정리 및 팁](https://jojoldu.tistory.com/243)