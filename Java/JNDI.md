# JNDI (Java Naming and Directory Interface)

디렉터리 서비스에서 제공하는 데이터 및 객체를 발견(discover)하고 참고(lookup)하기 위한 자바 API.   
WAS에 데이터 및 객체 정보를 Naming 해놓고, 해당 Naming을 이용하여 데이터 및 객체를 찾을 수 있다.   
  
![image](https://user-images.githubusercontent.com/67260437/87761410-61142000-c84c-11ea-983e-8c1b03ac156c.png)

WAS 내에는 여러 개의 web application이 있을 수 있는데, 소스 레벨에서 각각 설정하면 관리하기 어렵기 때문이다. 

<br/>  

## Tomcat에서의 JNDI 설정

WAS 중 Tomcat에서는 JNDI를 WEB-INF/web.xml이나 context descriptor에 설정할 수 있다. 

### web.xml에서의 설정

• `<env-entry>` : 응용 프로그램에서 쓸 단일 매개변수를 지정한다.   
• `<resource-entry>` : JDBC Datasource, JavaMail Session 등과 같은 어떤 객체를 생성할 수 있는 Object Factory이다.    
• `<resource-env-ref>` : 인증 정보가 필요 없는 resource를 구성하기 더 쉽다. 

web.xml에 명시할 수 없는 JNDI 자원들은 /conf/server.xml의 `<GlobalNamingResource>`나 기타 context descriptor에 명시되어야 한다. 

### context.xml

•  `<Environment>` :  JNDI의 InitialContext로 web application에서 사용할 수 있는 매개변수를 구성한다. (web.xml의 `<env-entry>`와 동일하다.)  
• `<Resource>` : web application에서 사용할 수 있는 resource의 이름과 타입을 구성한다. (web.xml의 `<resource-ref>`와 동일하다.)  
• `<ResourceLink>` : `<Server>`의 하위 요소인 `<GlobalNamingResource>` (global JNDI Context)에 resource를 정의한 경우 web application이 접근할 수 있는 link를 부여해야한다.   
• `<Transaction>` : java:comp/UserTransaction의 UserTransaction 객체를 인스턴스화 하기 위해 resource factory를 추가한다.  
* JTA : Java Transaction API (분산 트랜잭션을 처리하는 자바  API)  
* UserTransaction :  트랜잭션 영역을 지정할 수 있다.  

`<Context>`에 정의된 resource를 web.xml에도 지정할 필요는 없지만 web.xml에 web application에 필요한 resource를 기록하는 것이 권장된다. `<Environment>` 요소의 override 속성이 true인 경우에 web.xml의 값이 먼저 읽힌다. 

### 사용 예시

**conf/server.xml**
```xml
<GlobalNamingResources>
  <Resource name="jdbc/DatabaseName"
   auth="Container"
   description="DB Connection"
   driverClassName="com.mysql.jdbc.Driver"
   maxActive="50"
   initialSize="10"
   username="springdemo"
   password="demo1234"
   factory="org.apache.naming.factory.BeanFactory"
   type="com.mchange.v2.c3p0.ComboPooledDataSource"
   url="jdbc:mysql://localhost:3306/springdemo?autoReconnect=true&useUnicode=true&characterEncoding=utf8"
/>  
</GlobalNamingResources>
```  
**conf/context.xml**
```xml
<Context>
   <ResourceLink name="jdbc/DatabaseName" global="jdbc/DatabaseName" type="javax.sql.DataSource"/>
</Context>
```
**servlet**
```java
Context initContext = new InitialContext();
Context envContext  = (Context)initContext.lookup("java:/comp/env");
DataSource ds = (DataSource)envContext.lookup("jdbc/DatabaseName");
Connection conn = ds.getConnection();
```

<br/>  

## Servlet에서의 접근

### JNDI Syntax

JNDI lookups.ㅇ

`InitialContext`는 web application이 배포되었을 때 구성된다. 

---
#### Reference

[JNDI 살펴보기](https://heowc.tistory.com/27)  
[JNDI Resources HOW-TO](http://tomcat.apache.org/tomcat-7.0-doc/jndi-resources-howto.html)