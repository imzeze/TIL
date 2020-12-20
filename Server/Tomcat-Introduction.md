Apach Tomcat 7 공식문서 번역

# Introduction

Tomcat을 이해하기 위해 알아야할 정보들

| | |
|--|--|  
| /bin | startup, shutdown 등 Tomcat 관련 script들이 있다. |
| /conf | DTD(Document Type Definition)와 구성 파일들이 있다. 여기서 가장 중요한 문서는 server.xml로, server.xml은 container의 기본 구성 파일이다. |
| /log| default로 로그가 쌓이는 폴더 |
| /webapp | application의 webapp 폴더 |
| CATALINA_HOME | Tomcat이 설치된 루트 |
| CATALINA_BASE | Tomcat 인스턴스의 런타임 구성 루트 |   

* CATALINA_HOME과 CATALINA_BASE는 기본적으로 같은 루트를 갖는다. 한 시스템에서 여러 Tomcat 인스턴스를 실행하려는 경우에는 CATALINA_BASE를 서로 다르게 설정하면 된다.    

* 한 시스템에서 여러 Tomcat 인스턴스를 실행하려는 경우에는 CATALINA_BASE를 서로 다르게 설정한다. 각 인스턴스는 같은 CATALINA_HOME을 갖기 때문에 다음의 장점을 갖는다. 
    * Tomcat 버전 관리가 쉬워진다. 
    * .jar 파일의 중복을 피할 수 있다. 
    * setenv.sh와 같은 특정 설정을 공유할 수 있다.   

* CATALINA_BASE 구성  
    | | |
    | --- | --- |
    |/bin | /bin과 setenv.sh 및 tomcat-juli.jar 파일은 CATALINA_HOME에 있는 것이 권장된다. CATALINA_BASE에 /bin이 있으면, CATALINA_BASE가 먼저 읽히고, 제대로 동작하지 않을 경우 CATALINA_HOME이 읽혀 대처한다. |  
    | /lib | application에 쓰이는 외부 라이브러리가 있는 경우 추가될 수 있다. /lib도 CATALINA_BASE에서 먼저 읽히고, CATALINA_HOME에서 두번째로 읽힌다. |
    | /logs | 특정 instance의 로그 파일들을 위해 권장된다.   |
    | /webapps | web application을 배포하고 자동으로 load할 경우에 필요하다. /webapps는 CATALINA_BASE에서만 읽힌다. |
    | /work | JSP 컨테이너와 다른 파일들이 생성하는 임시 디렉토리. CATALINA_BASE에 포함되는 것이 권장된다. |
    | /temp | 임시 파일을 생성하는 Java File API로 만들어진 파일들이 저장된다. CATALINA_BASE에 포함되는 것이 권장된다.  사용 예) 대용량의 파일을 JVM 메모리에 모두 로드하는 것은 부담이 되므로 임시 파일로 생성해 놓고 후에 실제 파일로 변경한다. |
    | /conf | CATALINA_BASE에서 구성 파일이 누락된 경우 CATALINA_HOME에서 대체 되지 않는다. /conf에는 최소한 server.xml과 web.xml이 있어야한다. |

