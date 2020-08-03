
CATALINA_HOME : /opt/tomcat/apache-tomcat-7.0.103  
tomcat1의 CATALINA_BASE : /home/jhkim/work/java/clustering/tomcat/conf  
tomcat2의 CATALINA_BASE : /home/jhkim/work/java/clustering/tomcat2/conf

## Apache와 tomcat 연결

클러스터링을 구현하기에 앞서 apach의 로드 밸런싱을 이용하기 위해 tomcat과 apach를 연결한다. 연결하는 방법은 세가지가 있다.

* mod_jk : 가장 많이 사용되는 방식. Tomcat에만 사용할 수 있다. 
* mod_proxy : apache에서 기본으로 제공되는 모듈. WAS에 의족적이지 않다. 
* mod_proxy_ajp : apache에서 기본으로 제공되는 모듈. WAS에 의족적이지 않다.

AJP(Apache JServ Protocol)은 Web Server에서 받은 요청을 WAS로 전달해주는 프로토콜이다. 
세가지 방법 중 mod_jk를 이용해 연결하였다. 

### 관련 모듈 설치

* apache2
* tomcat-connectors-1.2.48-src.tar.gz
* httpd-devel 패키지
* apr-1.5.2.tar.gz
* apr-util-1.5.4.tar.gz

### 로드밸런싱 설정

1. /etc/apache2에 `worker.properties`파일 생성

```properties
workers.tomcat_home=/opt/tomcat/apache-tomcat-7.0.103
workers.java_home=/opt/java/jdk1.8.0_241

worker.list=tomcat1, tomcat2, load_balancer

worker.load_balancer.type=lb
worker.load_balancer.balance_workers=tomcat1,tomcat2

worker.tomcat1.port=8009 // 인스턴스 별로 포트가 달라야한다. 
worker.tomcat1.host=localhost
worker.tomcat1.type=ajp13
worker.tomcat1.lbfactor=1 // 로드밸런싱 비율을 나타낸다. tomcat2에서 1로 설정하였으므로, tomcat1:tomcat2=1:1의 비율로 로드밸런싱된다.
worker.tomcat1.secret=shx@3ed // Tomcat7은 connector가 secretRequired=true로 설정되어 있기 때문에 secret key을 작성한다. 

worker.tomcat2.port=8008 
worker.tomcat2.host=localhost
worker.tomcat2.type=ajp13
worker.tomcat2.lbfactor=1
worker.tomcat2.secret=lc02k3!
```

2. mod_jk 설정  

/etc/apache2/mods-available/jk.conf 파일 수정

```properties
# Configuration for mod_jk
LoadModule jk_module /usr/lib/apache2/modules/mod_jk.so

<IfModule jk_module>
JkWorkersFile /etc/apache2/workers.properties

JkLogFile /var/log/apache2/mod_jk.log

JkLogLevel info

JkShmFile /var/log/apache2/jk-runtime-status

JkMount /* load_balancer
```       

3. 인스턴스별 server.xml 설정  

/home/jhkim/work/java/clustering/tomcat/conf/server.xml 수정
``` xml
<Connector protocol="AJP/1.3"
           secret="shx@3ed"
           address="0.0.0.0"
           port="8009"
           redirectPort="8443" />
```
/home/jhkim/work/java/clustering/tomcat2/conf/server.xml 수정   
```xml
<Connector protocol="AJP/1.3"
           secret="lc02k3!"
           address="0.0.0.0"
           port="8008"
           redirectPort="8443" />
```  

<br/>  

## Cluster 설정

CATALINA_BASE/server.xml의 `<Engine>`이나 `<Host>` 안에 `<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>` 추가


```xml
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="6">
    <Manager className="org.apache.catalina.ha.session.BackupManager"
             expireSessionsOnShutdown="false"
             notifyListenersOnReplication="true"
             mapSendOptions="6"/>
    <!--
    <Manager className="org.apache.catalina.ha.session.DeltaManager" 
            expireSessionsOnShutdown="false"
            notifyListenersOnReplication="true"/>
    -->
    <Channel className="org.apache.catalina.tribes.group.GroupChannel">
    <Membership className="org.apache.catalina.tribes.membership.McastService"
                address="228.0.0.4"
                port="45564"
                frequency="500"
                dropTime="3000"/>
    <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                address="auto"
                port="5000"
                selectorTimeout="100"
                maxThreads="6"/>

    <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
        <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
    </Sender>
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/>
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.ThroughputInterceptor"/>
    </Channel>

    <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
            filter=".*\.gif|.*\.js|.*\.jpeg|.*\.jpg|.*\.png|.*\.htm|.*\.html|.*\.css|.*\.txt"/>

    <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
            tempDir="/tmp/war-temp/"
            deployDir="/tmp/war-deploy/"
            watchDir="/tmp/war-listen/"
            watchEnabled="false"/>

    <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
</Cluster>
```

### elements

#### Manager

<Context>요소에 manager가 정의되어 있지 않으면 <Cluster>내부의 <Manager>로 정의된다. Tomcat 5 이전에는 모든 webapp이 같은 manager를 사용하도록 했지만, 
그 이후 버전에서는 각 webapp마다 manager를 다르게 정의할 수 있다.  

#### channel

channel은  노드 간 메시지를 주고 받는, 그룹 커뮤니케이션을 위한 프레임 워크다. 



### 속성

#### channelSendOptions

simpleTcpCluster를 통해 전달되는 모든 메시지의 전송 방법 flag를 설정한다. 

* 0x0002 : USE ACK
* 0x0004 : 동기
* 0x0008 : 비동기

default = 8 (비동기)   
ACK 방식과 비동기를 사용할 경우 channelSendOptions=6

#### expireSessionsOnShutdown

Web application이 shutdown될 때, Tomcat은 각 세션을 만료시킨다. 만약 value가 true이면 클러스터 내 모든 노드의 모든 세션을 만료시킨다.   
default = false

#### notifyListenersOnReplication

value가 true이면 클러스터의 Tomcat 노드 간에 세션이 삭제되거나 복제되면 listener에게 알린다.

#### mapSendOptions

backupManager option. map이 전달되는 전송 방법 flag를 설정한다.   
default = 6 (동기)



