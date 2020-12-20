# WAS (Web Application Server)

## Web Server

Web server는 클라이언트가 정적 컨텐츠(html, js, css 등과 같이 어딘가 저장되어 있는 컨텐츠)을 요청하면 HTTP 프로토콜을 이용해 제공해주고, 동적인 컨텐츠에 대한 요청이 들어오면 Web Container에 전달해주는 역할을 한다. 

> Apach HTTP Server, IIS 등

## Web Container (=Servlet Container)

동적인 데이터(서버에 의해 실행된 결과)를 처리하는 소프트웨어 모듈로, Servlet Container라고도 한다. Servlet Container는 Servlet과 jsp를 위한 런타임 환경을 제공한다.   
Web Server에서 jsp를 요청하면 Web Container는 jsp를 Servlet 파일로 변환한 뒤 컴파일 하여 실행한 결과를 Web Server에 반환한다. 

> Application Server는 Java EE 전체 ( EJB, servlet API 등)를 지원한다.

## WAS 
   
WAS는 Web Server와 Web Container를 합친 것이다. WAS만으로도 정적, 동적 컨텐츠에 대한 요청을 처리할 수 있지만 앞단에 Web Server를 따로 두어 기능을 분리한다. 

### Web Server와 WAS 분리 이유

* 기능을 분리하여 서버 부하 방지 및 페이지 노출 시간 단축
* web server에서 WAS를 load balancing하여 고가용성을 높임 
* 보안이나 세션 관리에 Web Server 이용 등

### 동작 과정 

1. 요청이 들어오면 context.xml의 web.xml 경로를 참조하여 web.xml을 읽고, mapping url을 참조하여 해당 servlet에 대한 thread를 생성한다. 
2. Thread는 Serlvet의 service()를 호출하고, 요청에 따라 doGet() / doPost()가 실행된다. 
3. 처리 결과를 Web Server에 전달한다. 

> Tomcat, Jeus 등
  
<br/>  

# Tomcat 

**properties 파일**

* catalina.properties
* logging.properties
    
**configuration xml 파일**  

* server.xml : Tomcat main configuration file 
* web.xml : global web application deployment descriptors (server independency)
* context.xml : global Tomcat-specific configuration options   
(각 application에 대한 deployment ex. Datasource - server dependency)  
* tomcat-user.xml : 접근과 권한을 위한 사용자 계정

## web.xml 
    
web.xml은 Web Application의 환경을 설정하는 파일(Deployment Descriptor)로, 서버에 독립적인 내용들 즉, servlet관련 환경 설정, session 관리 등에 대한 설정들을 다룬다. 
   
### 작성 내용

* ServletContext의 초기 파라미터
* Session 유효시간 
* Servlet/jsp 정의
* Servlet/jsp 매핑 등

### conf/web.xml 과 WEB-INF/web.xml 차이

conf/web.xml은 Tomcat 인스턴스에 load된 모든 application에 대한 설정을 갖고 있고, WEB-INF/web.xml은 특정 application에 대한 설정을 갖고 있다. Tomcat은 conf/web.xml을 먼저 읽은 다음, WEB-INF/web.xml을 읽는다. 

## context.xml

web.xml이 서버에 독립적인 application 설정을 다룬다면, context.xml은 Datasource와 같은 서버 종속적인 설정을 다룬다. 

tomcat은 다중화를 지원하기 때문에 context.xml의 저장위치는 다양하다. 

### context.xml 저장위치 

* conf/context.xml   
    서버에 대한 전역적인 context 설정을 다룬다. 
* $CATALINA_HOME/conf/[enginename]/[hostname]/[contextpath].xml   
    특정 호스트, 특정 context에 한정되는 context 설정을 다룬다. `enginename`과 `hostname`은 server.xml에서 `<service>` 하위 요소들의 값들이다. 한 `<server>`는 여러 `<service>`를 가질 수 있으므로 하나의 포트에 여러 `<service>`를 띄우는 등 여러 다중화가 가능하다.
 * $CATALINA_HOME/conf/[enginename]/[hostname]/context.xml.default      
 특정 호스트의 전역적인 Context 설정에 쓰인다. 호스트 안에서의 디폴트 설정이 필요할 때 쓰인다.

### web.xml과 context.xml

context.xml의  `<parameter>`요소는 web.xml의 `<context-param>`과 동등하다.   
web.xml에서는 `<init-param>`과 `<context-param>`을 등록할 수 있는데, `<init-param>`은 특정 servlet class를 등록하여 해당 servlet class에서만 param을 사용할 수 있는 반면, `<context-param>`은 application의 모든 servlet이 param을 공유한다. 

## tomcat-user.xml

접근과 권한을 갖는 사용자 계정 목록

<br/>  

---
#### Reference

[web.xml의 이해(개요,기능,활용)](http://wiki.gurubee.net/pages/viewpage.action?pageId=26740333)  
[Tomcat - Context 경로설정](https://ecspecialist.tistory.com/entry/Tomcat-Context-%EA%B2%BD%EB%A1%9C%EC%84%A4%EC%A0%95)  
[Tomcat server.xml Configuration Example](https://examples.javacodegeeks.com/enterprise-java/tomcat/tomcat-server-xml-configuration-example/)  
[Web Server와 WAS의 차이와 웹 서비스 구조](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)  
[difference between context.xml and server.xml?](https://stackoverrun.com/ko/q/3083493)  
[[톰캣] Context 설정](https://parkcheolu.tistory.com/130)