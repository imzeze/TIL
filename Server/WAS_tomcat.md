# WAS (Web Application Server)

## Web Server

Web server는 클라이언트가 정적 컨텐츠(html, js, css 등과 같이 어딘가 저장되어 있는 컨텐츠)을 요청하면 HTTP 프로토콜을 이용해 제공해주고, 동적인 컨텐츠에 대한 요청이 들어오면 WAS에 전달해주는 역할을 한다. 

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
    
Web Application의 환경을 설정하는 파일 (Deployment Descriptor)  
web.xml 파일은 Web Server가 구동될 때 읽혀져 Web Container가 Servlet 파일을 인식하게 해준다.   Web Container와 Web Program의 파일을 서로 연결하고 관리한다. Web Program은 Web Container에서 실행된다. 
   
### 작성 내용

* ServletContext의 초기 파라미터
* Session 유효시간 
* Servlet/jsp 정의
* Servlet/jsp 매핑 등


## server.xml에서 `<Context>` 설정 분리

context 설정을 변경해도 톰캣을 재시작할 필요가 없어 유지보수가 용이해진다. 
* 위치 : [$CATALINA_BASE]/conf/[engineName]/[hostName]
* [파일명].xml로 context file을 생성하면 tomcat은 [파일명]을 context path로 인식하여 URL로 삼는다. 
* localhost/[파일명]/test.jsp를 호출하면 tomcat은 [파일명].xml의 docBase 값을 참조하여 localhost/[docBase 값]/test.jsp를 호출한다. 

<br/>  

---
#### Reference

[web.xml의 이해(개요,기능,활용)](http://wiki.gurubee.net/pages/viewpage.action?pageId=26740333)  
[Tomcat - Context 경로설정](https://ecspecialist.tistory.com/entry/Tomcat-Context-%EA%B2%BD%EB%A1%9C%EC%84%A4%EC%A0%95)  
[Tomcat server.xml Configuration Example](https://examples.javacodegeeks.com/enterprise-java/tomcat/tomcat-server-xml-configuration-example/)  
[Web Server와 WAS의 차이와 웹 서비스 구조](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)  
[difference between context.xml and server.xml?](https://stackoverrun.com/ko/q/3083493)