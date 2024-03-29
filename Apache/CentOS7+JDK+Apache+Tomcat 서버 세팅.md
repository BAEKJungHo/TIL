# 서버 세팅 방법

CentOS7 에서 JDK 1.8 과 Apache 와 Tomcat 을 사용한 서버 세팅 방법

## 1. DB 설치

- 부팅 시 재시작
  - systemctl enable mysqld
- 상태 확인
  - systemctl status mysqld

## 2. Apache 설치

CentOS 에서 아파치를 설치하는 방식은 2가지가 있다.

- `컴파일 설치 방식` : 소스파일을 다운로드하고 컴파일하여 설치하는 컴파일 설치 방식
  - apache root directory : `/usr/local/apache2`
- `YUM 방식` : RPM 패키지 파일을 YUM 방식(의존성이 해결됨)으로 설치하는 방식
  - yum 명령어는 root 권한으로만 가능하다.
  - apache root directory : `/etc/httpd`
  
> YUM을 통해서 설치하는 방식이 손쉽고, 버그나 취약점 업데이트 또한 간단하게 대응할 수 있지만, 컴파일 설치가 설치시에 디테일한 부분들을 설정/튜닝 할 수 있다.(설치를 원하는 버젼이 RPM 파일로 없는 경우에도 컴파일 설치를 해야한다.)
>
> 80 포트를 사용하는 Apache 같은 경우에는 root 로 설치해야한다.


> Syntax 검사
> 
> httpd -t

### YUM 방식

#### 1. 필수 패키지 gcc 설치

`yum -y install gcc make gcc-c++ pcre-devel`

> GCC 란?
>
> GNU 컴파일러 모음(GNU Compiler Collection, 줄여서 GCC)는 GNU 프로젝트의 일환으로 개발되어 널리 쓰이고 있는 컴파일러이다. 자유 소프트웨어 중에 가장 잘 알려진 것들 중 하나이다. 원래 GCC는 C만을 지원했던 컴파일러로 이름도 "GNU C 컴파일러"였지만, 후에 C++, Java, Fortran, Ada 등 여러 언어를 컴파일할 수 있게 되면서, 현재의 이름으로 바뀌었다.


#### 2. 패키지 설치

`yum install -y httpd`

#### 3. 패키지 설치 확인

`rpm -qa httpd`

#### 4. 서비스 생성 확인 및 기동

`systemctl status httpd`

> systemctl status httpd 에 에러가 나와있는지 확인하고 에러가 있으면 구글에 검색 후 처리

#### 5. 서비스 실행

서비스가 기동되어 있지않으면 실행 시켜줘야 한다.

`systemctl start httpd`

#### 6. 포트 리스닝 확인

서비스를 구동하였으니 포트가 정상적으로 리스닝 상태인지 확인한다.

`netstat -tnlp`

#### 7. 자동 재시작 설정 및 80 포트 방화벽 열기

주소창에 IP(or localhost) 입력 후 apache 화면이 나오는지 확인해야한다. 나오는 경우 딱히 신경쓸게 없지만, 나오지 않는 경우에는 해당 포트 번호가 뚫려있는지 확인해야 하므로 telnet 을 이용해야한다.

- 안뜨면 Window 제어판에서 telnet 설치 후 cmd 창을 열어서 `telnet ip port`로 붙는지 확인.
  - telnet 192.168.x.x 80
  - [CentOS7 방화벽 열기](https://www.lesstif.com/system-admin/rhel-centos-firewall-22053128.html)
    - 실행 중인 포트 방화벽 목록 보기 : `firewall-cmd --list-all`
    - 포트 방화벽 열기 : `firewall-cmd --permanent --zone=public --add-port=80/tcp`
    - 방화벽 설정 : `firewall-cmd --permanent --add-service=http / firewall-cmd --permanent --add-service=https`
    - 방화벽 재시작 : `firewall-cmd --reload`
  - 다시 해당 IP(or localhost)로 Apache 화면이 뜨는지 확인

> 참고 : [How To Open Port 80 CentOS7](https://linuxhint.com/open-port-80-centos/)

- 추가적으로 리눅스 재부팅 시 apache 도 자동으로 시작하게 하는 설정을 해야한다.
  - reboot 명령어 입력 후 CentOS7 리부팅
  - 아파치가 자동으로 실행 되는지 확인하고, 80 포트 방화벽도 잘 열려있는지 확인
  - 아파치가 자동으로 실행이 안된다면 `chkconfig httpd on` 명령어 입력
  - 자동 재시작 명령어 : `systemctl enable httpd`

## 3. JDK 설치

JDK 1.8 을 기준으로 설명.

### 1. OPEN-JDK 1.8 설치

- `yum install java-1.8.0-openjdk`
- `yum install java-1.8.0-openjdk-devel`

`java -version` 과 `javac -version` 을 쳤을때 둘다 나와야 한다.

### 2. 환경변수 등록

/usr/bin/java 경로에 심볼릭링크가 걸려있기 때문에 실제 경로를 찾아서 환경변수에 등록해주어야 한다.

- `readlink -f /usr/bin/java`
  - /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java

실제 경로를 찾았으면 /etc/profile을 vi 로 열어준다. 그리고 JAVA_HOME, PATH, CLASSPATH 를 등록한다.

- `vi /etc/profile`

```
...

JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar

export JAVA_HOME PATH CLASSPATH
```

환경 변수를 등록했다면 ssh 연결을 재시작하거나 `source /etc/profile` 명령어를 입력해준다.

등록한 환경 변수가 제대로 적용되었는지 테스트한다.

```
# echo $JAVA_HOME
# echo $PATH
# echo $CLASSPATH
```

### 3. HelloWorld.java 컴파일 후 실행

- `vi HelloWorld.java`

```java
public class HelloWorld{
   public static void main(String[] args){
        System.out.println("Hello World!!");
   }
}
```

```
# javac HelloWorld.java
# java -cp . HelloWorld
Hello World!!
```

## 4. Tomcat 설치

Tomcat 8 을 기준으로 설명.

> 톰캣을 컴파일 버전으로 설치할 때 항상 최신버전으로 한다. 그리고 폴더명은 가급적 version 을 제거한 상태로 변경시킨다. (나중에 최신버전으로 업데이트할 때마다 폴더명 변경하는 것은 너무 번거로움) 

### 1. 의존성 설치

`yum install -y wget`

### 2. 톰캣 설치

컴파일 버전으로 설치한다.

위치는 보통 `/usr/local/tomcat8` 이다.

[톰캣 공식 사이트](https://tomcat.apache.org/download-80.cgi)에서 원하는 버전의 `version.tar.gz` 파일의 주소를 복사한다.

그리고 /usr/local 로 간다음. tar.gz 파일을 받고 압축을 푼다.

```
# wget http://apache.tt.co.kr/tomcat/tomcat-8/v8.0.52/bin/apache-tomcat-8.0.52.tar.gz
# tar xvfz apache-tomcat-8.0.52.tar.gz
```

압축 파일은 삭제한다.

- `rm tomcat + Tab`

### 3. 환경 변수 설정

JDK를 설치했을 때와 마찬가지로, 어느 디렉터리에서나 tomcat을 실행 할 수 있도록 환경변수를 설정해야 한다. Tomcat 은 자바 프로그램이므로 Java 가 설치되어 있어야 한다.

- 편집 : `vi /etc/profile`

```
#tomcat
export CATALINA_HOME=/usr/local/victolee/tomcat8.0.52
```

- 변경 사항 적용 : `source /etc/profile`

### 4. 톰캣 환경 설정

톰캣의 server.xml 에서 톰캣의 port 를 변경해줘야 한다. 일반적으로 8080 은 사용하지 않고 앞에 숫자 하나를 붙여서 사용하곤한다. ex) 58080 이런식
그리고 AJP 포트 건들면 안된다. 나중에 Apache 와 연동할 때 사용하기 때문이다.

포트번호와 URIEncoding 설정을 한다.

- `vi /usr/local/tomcat8/conf/server.xml`

```
<Connector port="58080" protocol="HTTP/1.1"
               URIEncoding="UTF-8"
               connectionTimeout="20000"
               redirectPort="8443" />
```         

그리고 포트번호를 바꿨으니, 해당 포트의 방화벽도 열어야 한다.

- 실행 중인 포트 방화벽 목록 보기 : `firewall-cmd --list-all`
- 포트 방화벽 열기 : `firewall-cmd --permanent --zone=public --add-port=80/tcp`
- 방화벽 재시작 : `firewall-cmd --reload`

그리고 cmd 창에서 `telnet ip port` 로 해당 포트에 접근이 되는지 확인한다.

- 톰캣 server.xml reloadable false 설정

class 파일 반영 후 서버 자동 재시작을 막는 기능이다.

`<Context docBase="/data/project/site" path="/" reloadable="false" allowLinking="true" />`

#### 톰캣 SSL 설정

```
<Connector port="28443" protocol="org.apache.coyote.http11.Http11NioProtocol"
       maxThreads="150" scheme="https" secure="true" SSLEnabled="true">
<SSLHostConfig>
    <Certificate certificateKeyFile="/etc/pki/tls/private/cms_fomo/_wildcard_fomo_or_kr_nopass.key"
		 certificateFile="/etc/pki/tls/private/cms_fomo/_wildcard_fomo_or_kr.crt"
		 certificateChainFile="/etc/pki/tls/private/cms_fomo/ChainCA/rsa-dv.chain-bundle.pem"
		 type="RSA" />
</SSLHostConfig>
</Connector>
```

#### 톰캣 서버 정보 지우기

에러 페이지에 톰캣 버전 정보가 노출이 되면 취약점에 걸리기 때문에 버전 정보를 노출 되지 않게 지워야 한다.

- https://mingzz1.github.io/pentesting/web/2020/04/05/tomcat_hide_version_on_error_page_.html

### 5. 톰캣 실행 및 테스트

- `/usr/local/tomcat8/bin/startup.sh`
- `ps -ef | grep tomcat`
- 주소창에 IP:port 입력 후 톰캣 화면이 나오는지 확인

### 6. Tomcat Manager 설정

> Tomcat Manager 의 경우에는 개발서버는 해도 되지만, 운영 서버의 경우에는 사용하지 않는다.

다음으로는 톰캣에 설치된 애플리케이션 관리 및 톰캣 서버를 관리하는 GUI 환경의 Tomcat Manager 를 사용한다.

우선은 DB를 사용하지 않는 간단한 웹 애플리케이션을 war 파일로 준비한다.

> 참고 : 서버에 DB가 설치되어 있다면 웹 애플리케이션에서 DB의 연결을 서버 환경에 맞게 재설정을 해줘야 합니다.

- tomcat-users.xml 파일을 수정해서 Tomcat Manager 등록
  - `vi /usr/local/victolee/tomcat8.0.52/conf/tomcat-users.xml`

가장 아래에 내용을 추가한다.

ID 와 PW 를 설정한다.

```
<role rolename="manager"/>
  <role rolename="manager-gui" />
  <role rolename="manager-script" />
  <role rolename="manager-jmx" />
  <role rolename="manager-status" />
  <role rolename="admin"/>
  <user username="admin" password="manager" roles="admin,manager,manager-gui, manager-script, manager-jmx, manager-status"/>
```

다음은 manager.xml 파일을 생성하여, Context 를 등록한다.

아래의 설정은 외부 서버에 대한 모든 접근을 허용하겠다는 의미이다.

- `vi /usr/local/tomcat8/conf/Catalina/localhost/manager.xml`

```
<Context privileged="true" antiResourceLocking="false" docBase="${catalina.home}/webapps/manager">
	<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
</Context>
```

- 톰캣 재시작
  - `systemctl restart tomcat.service`
- Tomcat Manager 에 접속이 되는지 확인(IP/manager)

> 톰캣을 2개 띄우고 싶은 경우가 있다. 예를 들어 관리자 cms 의 경우에는 아파치와 연동을 안하고 톰캣으로만 띄우고 싶으면, 똑같이 톰캣을 하나 더 설치한 후에, 기존 톰캣 server.xml 에 있는 server, tomcat, ajp 포트와 다르게 설정한 후에 위와 동일하게 진행하면 된다.
>
> 그리고 톰캣에서 SSL 설정을 하고, 80 to 443 설정을 해야 한다.
 
## 5. Apache 와 Tomcat 연동

아파치와 톰캣을 연동하는 방법은 세 가지가 있다.

- mod_jk
  - 장점
    - 가장 많이 사용하므로 관련 자료가 많다.
    - JkMount 옵션을 이용하면 URL 이나 컨텐츠별로 유연한 설정이 가능(이미지는 웹서버, 서블릿은 톰캣)
  - 단점
    - 별도의 모듈을 설치해야 함.
    - 설정이 어려움.
    - 톰캣 전용이라서 WAS 에 의존적임.
- mod_proxy
  - 장점
    - 별도의 모듈 설치가 필요 없고(apache 기본 모듈) 설정이 간편하다.
    - 특정 WAS 에 의존적이지 않으므로 모든 WAS 에 적용이 가능하다.
  - 단점
    - URL 별 유연한 설정이 어렵다.([ProxyPassMatch](http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypassmatch) 사용 필요)
- mod_proxy_ajp
  - 장점
    - 별도의 모듈 설치가 필요 없고(apache 기본 모듈) 설정이 간편하다.
    - 특정 WAS 에 의존적이지 않으므로 모든 WAS 에 적용이 가능하다.
  - 단점
    - URL 별 유연한 설정이 어렵다.([ProxyPassMatch](http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypassmatch) 사용 필요)

> 참고 : [아파치 웹 서버(apache httpd) 와 톰캣 연동하기 - tomcat connector(mod_jk) , reverse proxy(mod_proxy)](https://www.lesstif.com/system-admin/apache-httpd-tomcat-connector-mod_jk-reverse-proxy-mod_proxy-12943367.html)

일반적으로 가장 많이 사용하는 방법은 `mod_jk`이다. mod_jk 는 [Tomcat Connector](http://tomcat.apache.org/connectors-doc/) 라고 불린다.

mod_proxy 가 mod_jk 에 비해 설정이 간편하고 __AJP 같은 특정 WAS 의존적인 프로토콜을 사용하지 않으므로 성능이 더 좋다__ 고 하지만 mod_jk 가 오랫동안 써왔고 친숙해서 mod_jk 를 많이 사용하는 편이다. mod_jk는 아파치 서버 뒤에 톰캣을 숨기고 URL 접근할 때 포트 번호를 제거하는데 유용하다. 


먼저 AJP 프로토콜에 대해서 알아보겠다.

> AJP 프로토콜이란?
> 
> AJP(Apache JServ Protocol)은 Web Server에서 받은 요청을 WAS로 전달해주는 프로토콜이다. 해당 프로토콜은 Apache HTTP Server, Apahce Tomcat,  웹스피어, 웹로직, JBOSS, JEUS, 등 다양한 WAS에서 지원한다. TCP와 패킷 기반의 프로토콜로 웹 서버 성능이 증가된다. 

- AJP는 HTTP의 내용을 포워드 용도에만 있다.
- AJP는 8K 이상을 전송하지 못한다(헤더는, Body Chunk는 64K 를 보낸다.).
- 그래서 짤라서 여러 번 보내는 것 같다(시퀀셜하게, Body Chunk 또한 64K 씩 잘라서 보낸다.).
- Secure하지 않다. 단지 포워드 용도이기 때문이다. Secure하기 위해서는 HTTPS를 포워딩 하면 된다

mod_jk 를 설치 하려면 `gcc, gcc-c++, httpd-devel` 세가지 패키지가 설치되어 있어야 한다.

`yum -y install gcc gcc-c++ httpd-devel`

mod_jk 를 이용한 연동 방법에 대해서만 설명하겠다.

### 1. mod_jk 설치

톰캣 커넥터를 다운로드 받고 설치한다.

- `wget -c http://mirror.navercorp.com/apache/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.46-src.tar.gz`

> [Tomcat Connector Download](https://tomcat.apache.org/download-connectors.cgi)

- `tar -zxvf tomcat-connectors-version-src.tar.gz`

커넥터를 설치하고 압축을 푼 다음 native 디렉터리에 진입하여 아래의 명령어를 실행한다.

- `cd native`
- `./buildconf.sh`
- `./configure --with-apxs=/usr/bin/apx`
  - Makefile(컴파일 옵션이 설정되는 화일)이 만들어집니다. 소스를 컴파일하는 컴퓨터의 사양에 맞는 환경에 알맞는 Makefile 이 생성됩니다.
- `make`
  - 소스코드를 실제로 컴파일해서 binary 파일을 생성한다.
- `make install`
 - 컴파일된 프로그램, 환경파일, 데이터파일을 지정된 위치에 복사한다.
- `ls /etc/httpd/modules/ | grep mod_jk` 또는 `find / -name mod_jk.so`
  - mod_jk 파일 확인 -> `mod_jk.so` 가 나와야 한다.

### 2. mod_jk 설정파일을 수정한다.

`/etc/httpd/conf.modules.d` 로 들어가면 mod_jk.conf 파일이 있다.

```
<IfModule mod_jk.c>
JkWorkersFile conf/workers.properties
JkShmFile run/mod_jk.shm
JkLogFile logs/mod_jk.log
JkLogLevel info
JkLogStampFormat "[%y %m %d %H:%M:%S] "
</IfModule>
```

### 3. workers.properties 만들기

- `/etc/httpd/conf` 에서 `vi workers.properties`


```
worker.list=tomcat
worker.tomcat.port=톰캣의 AJP 포트 적기
worker.tomcat.host=192.168.x.x
worker.tomcat.type=ajp13
worker.tomcat.lbfactor=1
```

### 4. httpd.conf 수정

- Document Root 수정

```
DocumentRoot "/usr/local/tomcat8/webapps/ROOT"
<Directory "/usr/local/tomcat8/webapps/ROOT">
    AllowOverride none
    Require all granted
</Directory>
```

그런데 나중에 소스배포 하고나서는 위 DocumentRoot 가 필요 없다.

```
<VirtualHost *:80>
ServerName ip or domain

# 확장자 jsp, json, xml, do를 가진 경로는 woker tomcat으로 연결하는 구문.
JkMount /* tomcat
</VirtualHost>
```

- 원하는 확장자마다 지정할 수도 있다.

```
JkMount /*.jsp tomcat
JkMount /*.json tomcat
JkMount /*.xml tomcat
JkMount /*.do tomcat
```

또는 아래와 같은 방식으로도 할 수 있다.

```
<VirtualHost *:80>
        ServerAdmin xxx@xxx.net
        ServerName weave.xxx.net
        DocumentRoot /usr/local/apache2/htdocs

        RemoteIPHeader X-Forwarded-For
        #<Location />
        #         Require all denied
        #         Require ip 192.168.x.x/24
        #</Location>

        ErrorLog logs/weave.xxx.net_error_log
        CustomLog logs/weave.xxx.net_access_log common

        JkMount /* tomcat8
        JkUnMount /robots.txt tomcat8
</VirtualHost>
```

### 5. 아파치 재시작

`service httpd restart`

## 6. AJP 포트 변경

AJP 포트도 기본이 8009 인데 58009 이런식으로 사용하는 것을 추천한다.(취약점 때문에 변경하는걸 추천하는데 굳이 안해도 상관은 없다.)

Tomcat 의 server.xml 에서 58009 로 수정하고 workers.propertiese 에서 AJP 포트만 수정하면 아래와 같은 에러가 발생한다.

`[error] ajp_service::jk_ajp_common.c (2795): (tomcat) connecting to tomcat failed (rc=-3, errors=14, client_errors=0).`

그러면 아래와 같이 문제를 해결해야 한다.

### SELinux 보안 문제 해결(PORT)

위와 같은 ajp 연결 실패라는 로그가 발생할 경우 해결 방법이다.

1. semanage설치(selinux 관리 패키지)

`yum install -y policycoreutils-python`

2. 현재 설정된 포트보기

`semanage port -l|grep http_port_t`

(80,443,8080,8009,8443 등의 selinux가 허용한 기본 포트만 나열됨)

3. 포트추가(ajp 설정한 포트를 추가 해주시면 됩니다.)

`semanage port -a -p tcp -t http_port_t 포트`

## 7. AJP 취약점

최근 Apache Tomcat의 원격코드실행 취약점(CVE-2020-1938)을 악용할 수 있는 개념증명코드(Proof of concept code, PoC)가 인터넷상에 공개되어 사용자의 보안 강화 필요.

즉, 내 Apache 랑 Tomcat 만 서로 통신하게 하는 설정이다. (외부 다른 Apache 에서 내 Tomcat 에 접근하지 못하도록 설정.)

- 2020.03.10 기준
  - tomcat7 의 경우 7.0.100 버전 으로
  - tomcat8 의 경우 8.5.51 버전 으로
  - tomcat9 의 경우 9.0.31 버전 으로 업데이트를 해줘야 한다.

업데이트를 하고 톰캣을 실행하면 톰캣 로그에 다음과 같이 찍힌다.

```
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.catalina.startup.Bootstrap.start(Bootstrap.java:353)
		at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:493)
	Caused by: java.lang.IllegalArgumentException: AJP 연결자는 secretRequired="true"로 구성되었으나 보안 속성이 널 또는 ""입니다. 이 조합은 유효하지 않습니다.
		at org.apache.coyote.ajp.AbstractAjpProtocol.start(AbstractAjpProtocol.java:274)
		at org.apache.catalina.connector.Connector.startInternal(Connector.java:1099)
		... 12 more
```

- server.xml 에서 AJP 포트 부분에 아래 처럼 변경한다.
  - 기존 : `<Connector port="8009" protocol="AJP/1.3" redirectPort="8443"/>`
  - 변경 : `<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" address="0.0.0.0" secretRequired="false"/>`

## 8. Apache Method 제한

- 서버 운영 시 중요한 부분 중 하나인 바로 보안 부분이다.
- 보안 문제로 apache 에서 사용 가능한 method 를 제한하는 경우가 있다.
- 보통 허용하는 method 는 GET, POST, HEAD 이며 조금씩 다를 수 있다.

> [HTTP 메서드 수동 점검 방법](https://webhack.dynu.net/?idx=20161110.001&print=friendly)

- apache 의 httpd.conf 파일 수정

```
LoadModule jk_module modules/mod_jk.so
<VirtualHost *:80>
 ServerName IP OR DOMAIN
         <Directory "/data/projectName">
                Options None
                AllowOverride None
                Order allow,deny
                Allow from all # 모든 접근 허용
                Require all granted
                <LimitExcept GET HEAD POST> # GET HEAD POST 를 제외한 나머지는 허용을 하지 않는다는 의미.
                        Order allow,deny
                        Deny from all
                </LimitExcept>
        </Directory>
 JkMount /* tomcat
</VirtualHost>
Include conf.modules.d/*.conf
```

- 허용 되지 않은 메서드 테스트 방법
  - `curl -v -X OPTIONS http://webhack.dynu.net/icons/`
  - 허용 되지 않은 경우 콘솔에 에러가 발생한다.
 
> 참고로 아파치 2.4 이상 버전에서 VirtualHost 설정할 때 `Allow Deny` 지시자가 `Require`로 변경되었다.
> 
> deny from all -> Require all denied // allow from all -> Require all granted
> 
> https://m.blog.naver.com/PostView.nhn?blogId=itcools&logNo=221354902402&proxyReferer=https:%2F%2Fwww.google.com%2F
 
## 9. 톰캣 메모리 설정

- Window
  - /bin 아래에 catalina.bat
- Linux
  - /bin 아래에 catalina.sh

> [JDK 8 부터는 Perm 영역이 삭제되고 Metaspace 영역이 추가되었다.](https://johngrib.github.io/wiki/java8-why-permgen-removed/)

- 왜 Perm이 제거됐고 Metaspace 영역이 추가된 것인가?

> 최근 Java 8에서 JVM 메모리 구조적인 개선 사항으로 Perm 영역이 Metaspace 영역으로 전환되고 기존 Perm 영역은 사라지게 되었다. Metaspace 영역은 Heap이 아닌 Native 메모리 영역으로 취급하게 된다. (Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분된다) Metaspace가 Native 메모리를 이용함으로서 개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.
> 
> 즉, 각종 메타 정보를 OS가 관리하는 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.

그에 따라 톰캣 메모리 설정 옵션 명도 변경 되었다.

- 옵션
  - `Xms`: 최초 JVM 이 로드될 때 부여할 메모리
  - `Xmx`: 최대 JVM 이 가질 수 있는 메모리

- JDK 8
  - `-XX:MetaspaceSize`
  - `-XX:MaxMetaspaceSize`

```
JAVA_HOME="/usr/lib/jvm/jre-1.8.0-openjdk"
CATALINA_OPTS="$CATALINA_OPTS -server -Xms1G -Xmx2G -XX:MetaspaceSize=1G -XX:MaxMetaspaceSize=2G"
```

- JDK 7
  - `-XX:PermSize`
  - `-XX:MaxPermSize`

```
JAVA_HOME="/usr/lib/jvm/jre-1.7.0-openjdk"
CATALINA_OPTS="$CATALINA_OPTS -server -Xms1G -Xmx10G -XX:PermSize=1G -XX:MaxPermSize=2G -Dorg.owasp.esapi.resources=/data/projectName/WEB-INF/classes/egovframework/egovProps -Dhttps.protocols=TLSv1.1,TLSv1.2 -Djava.awt.headless=true -Duser.timezone=GMT+9"
```

톰캣 7 의 경우 위의 경우와 비슷하게 설정한다.

- 취약점 점검 [ESAPI](https://owasp.org/www-project-enterprise-security-api/) API 를 사용하는 경우 
  - `-Dorg.owasp.esapi.resources=/data/projectName/WEB-INF/classes/egovframework/egovProps` 의 경우 웹 취약점을 대비해서 Path 를 잡아둔 것이다.

- -Djava.awt.headless=true 옵션은 비윈도우 환경에서 GUI 클래스를 사용할수 있게 하는 옵션이다.

## 10. 서버 부팅 시 톰캣 재시작

어떠한 에러로 인해 서버가 부팅되는 경우 톰캣을 재시작 하게끔 설정해야 한다.

### 1. tomcat.service 파일 생성

모든 작업은 root 권한으로 진행한다.
/usr/lib/systemd/system 디렉토리에 아래와 같은 내용으로 tomcat.service 파일을 생성한다.

```
[Unit]
Description=tomcat8
After=network.target syslog.target

[Service]
Type=forking
Environment="JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk"
Environment="CATALINA_HOME=/usr/local/tomcat8"
User=web
Group=web
ExecStart=/usr/local/tomcat8/bin/startup.sh
ExecStop=/usr/local/tomcat8/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

### 2. 데몬 재로드

`$ systemctl daemon-reload`

### 3. 시작 서비스 등록

`$ systemctl enable /usr/lib/systemd/system/tomcat.service`

- 테스트
  - `systemctl start tomcat`
  - `systemctl status tomcat.service`

### 4. 재부팅

`$ reboot`

## 11. 아파치 톰캣 동시 접속자 세팅

사람이 별로 안 몰리는 사이트의 경우에는 1000 명 정도로 하면된다. 디폴트가 250 명 정도로 되어있다.

`/etc/httpd/conf.modules.d` 에 mpm.conf 라는 파일이 있는데 거기서 수정해야한다.

ServerLimit 옵션과 MaxRequestWorkers 옵션을 같이 수정하면된다.

```
<IfModule mpm_prefork_module>
    ServerLimit            1024
    StartServers             5
    MinSpareServers          5
    MaxSpareServers         10
    MaxRequestWorkers      1024
    MaxConnectionsPerChild   0
</IfModule>
```

> 참고1.
>
> 대부분 prefork 방식이 기본적으로 사용되며, 사용자가 많은 경우에는 worker방식을 사용한다. 요즘 몇 몇 사이트에서 사용자 폭주로 인하여 worker 방식을 사용하는 경우가 간혹 있다.

> 참고2.
>
> apache 설치 시에 아래와 같이, 반드시 –with-mpm=worker 옵션을 설정 하고 설치한다. 이 옵션을 주지 않을 경우, Default인 Prefork방식으로 설치된다(Linux에 한함)

## 12. SSL 적용

> [참고 사이트](https://steady-snail.tistory.com/187)
> 
> [VirtualHost](https://httpd.apache.org/docs/2.2/ko/vhosts/name-based.html)

아파치랑 톰캣을 같이 사용하는 경우에는 아파치에만 적용하면된다. 톰캣만 사용하는경우에는 톰캣에 적용하면 된다.

보통 SSL 인증서를 받으면 가이드까지 들어있다. 참고로 톰캣 버전 별로 server.xml 설정이 다르므로 찾아보면서 해야한다. 

> [Tomcat 싱글 인증서 설치](https://www.ucert.co.kr/wiki/w/Tomcat_%EC%8B%B1%EA%B8%80%EC%9D%B8%EC%A6%9D%EC%84%9C_%EC%84%A4%EC%B9%98)

구글 검색해서 하는게 빠름.

> [SSL 적용기](https://yultory.com/12)
> 
> [인증서 파일 형식 및 확장자의 차이점 비교 설명 (Certificate file format & extensions)](https://www.letmecompile.com/certificate-file-format-extensions-comparison/)

- 443 포트 열어줘야 함.

__만약에 ssl 인증서를 두 개 적용해야 하는데 패스워드 key or key.pem 파일이 있는 경우, 패스워드를 제거한 파일로 다시 생성해야 한다.__

- 비밀번호 제거

`root]~]# openssl rsa -in ssl.key -out ssl_nopass.key`

- 비밀번호 생성

`root]~]# openssl rsa -in ssl_nopass.key -passout pass:'password123' -out ssl.key -des3`

### 아파치 재시작 시 SSL 패스워드 자동 입력 설정
  
서버에 SSL 인증서 설치 하고 나서 아파치를 재시작 할때 마다 패스워드를 넣어줘야 아파치가 정상적으로 실행되는 경우 엄청 불편하다.
 
이때, 아래의 방법을 이용하면 간편하게 해결 된다.
 
SSL.conf 에서 아래 열 주석처리

`#SSLPassPhraseDialog  builtin`
 
아래 열을 적당한 위치에 넣는다.

`SSLPassPhraseDialog  exec:/usr/local/apache/conf/extra/sslpass.sh`
 
sslpass.sh 파일을 생성한다. (혹시 기존 ssl.conf 파일에 pass 관련된 파일이 이미 생성되어있으면 해당 위치로 가서 패스워드만 수정한다.)

SSL 암호가 "1234" 라면
 
`vim /usr/local/apache/conf/extra/sslpass.sh`
 
```
#!/bin/sh
echo "1234" 
```

파일에 실행 권한을 주기
 
`chmod 755 sslpass.sh`
 
이렇게 하고 서버를 리스타스 해보면 깔끔히 해결 됨을 확인 할 수 있다.

## 13. 아파치 https 리다이렉트

### 기존에 httpd.conf 에 작성한 VirtualHost 수정

```
LoadModule jk_module modules/mod_jk.so
<VirtualHost *:80>
 ServerName www.ilje.or.kr
 RewriteEngine On
 RewriteCond %{HTTPS} off
 RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
</VirtualHost>
Include conf.modules.d/*.conf
```

### ssl.conf 에 VirtualHost 수정

```
<VirtualHost *:443>
# General setup for the virtual host, inherited from global configuration
#DocumentRoot "/var/www/html"
ServerName domain
ServerAlias domain *.domain
<Directory "/data/projectSource">
      Options None
      AllowOverride None
      Order allow,deny
      Require all granted
      <LimitExcept GET HEAD POST>
              Order allow,deny
              Require all denied
      </LimitExcept>
</Directory>
JkMount /* tomcat
```

## 14. 사용 가능한 SSL 버전만 명시

[Apache 2.4.x version 에 대한 사용 가능한 SSL](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html#sslprotocol)

[TLS 1.0 & TLS 1.1 지원중지 TLS 1.2 / TLS 1.3 권장사용](https://xinet.kr/?p=2012)

TLS 1.3 이상 지원하려면 apache 버전 최소 2.4.37 이상 / openssl 1.1.1 이상

>모든 주요 웹 브라우저는 2020 년 TLS 1.0 및 TLS 1.1 지원을 제거합니다.
Google , Microsoft , Apple 및 Mozilla의 4 대 주요 회사에서 발표 한 보도 자료에 따르면 웹 브라우저는
2020 년 상반기에 TLS 1.0 및 1.1 지원을 완전히 삭제합니다.
10 년 전에 발표 된 TLS 1.2 TLS 1.0 및 1.1의 약점은 그 이후로 널리 채택되어 현재 개발 단계에있는 TLS 1.3이 없으면 기본 TLS 버전이됩니다.
Microsoft에 따르면 TLS 1.0의 시대가 도래함에 따라 많은 웹 사이트가 이미 새로운 버전의 프로토콜로 이동했습니다. 현재 사이트의 94 %는 이미 TLS 1.2를 지원하고 있으며 Microsoft Edge의 일일 연결 수는 TLS 1.0 또는 1.1을 사용하고 있습니다.
애플은 또한 TLS 1.2가 플랫폼상의 표준이며 TLS 1.0과 1.1이 모든 연결의 0.36 퍼센트 미만을 차지하는 반면 사파리로 만들어진 TLS 연결의 99.6 퍼센트를 대표한다고 말한다.
Google은 더 이상 동의 할 수 없으며 현재 Chrome에서 만든 HTTPS 연결의 0.5 %만이 TLS 1.0 또는 1.1을 사용한다고 말합니다.
모든 기술 회사는 TLS 1.2 이상을 지원하지 않는 웹 사이트를 가능한 한 빨리 구형 프로토콜에서 벗어나 실용적으로 권장했습니다.

```
# -all : 모든 것을 제외
# +TLSv1.2, +TLSv1.3 : TLS 1.2 와 1.3 추가
SSLProtocol -all +TLSv1.2 +TLSv1.3
```

- TLS 1.3 지원 체크

`openssl ciphers -v | grep TLSv1.3`

## 15. Apache, Tomcat 정보 숨기기

### apache 정보 숨기기

- httpd.conf

```
ServerTokens Prod
ServerSignature Off
```

### tomcat 정보 숨기기

```
<Host name="localhost" appBase="/data/mec_ilje/core" unpackWARs="true" autoDeploy="true">

<Context docBase="/data/mec_ilje/core" path="/" reloadable="false" allowLinking="true" />
<Context docBase="/data/mec_ilje/core" path="/cms" reloadable="false" allowLinking="true" />

<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
       prefix="localhost_access_log" suffix=".txt"
       pattern="%h %l %u %t &quot;%r&quot; %s %b" />

<Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false"/>
</Host>
```

## 16. MySQL DB 자동 백업 + 소스 자동 백업

- crontab 서비스 시작
  - `systemctl start crond`
  - `ps -ef | grep crond`
- CentOS 시작 시 crontabl 이 자동으로 실행 되도록 설정
  - `systemctl enable crond`
  - `systemctl status crond`
  - `systemctl list-unit-files`

- 루트 디렉터리 아래에 backup directory 를 만든다.
  - `mkdir backup`
- sh 파일이 저장되는 위치로 가서(/usr/libexec/backup) db_backup.sh 를 만든다.
  - libexec 아래에 backup 디텍터리를 만들고 파일을 만든다.
  - `vim db_bakcup.sh`
  - `vim source_backup.sh`

- db_backup

```
#!/bin/sh
DATE=`date +"%Y%m%d"` 
PREV_DATE=`date --date '3 days ago' +"%Y%m%d"` # 3 일 지난 것은 삭제 
/usr/bin/mysqldump -u사용자ID -p패스워드 DB 명 > /data/backup/db/db_bak_${DATE}.sql
chown root:root  /data/backup/db/db_bak_${DATE}.sql
chmod 755  /data/backup/db/db_bak_${DATE}.sql
find /backup/db -type f -mtime +3 | sort | xargs rm -f
```

- source_backup

```
#!/bin/bash
tar -zcvf /backup/source/mec_ilje_source.`date +%Y%m%d%H%M%S`.tgz /data/mec_ilje
find /backup/source -type f -mtime +3 | sort | xargs rm -f
```

- /etc/cron.d 에 0backup 파일 하나 만들기

```
# Run the backup jobs
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
42 10 * * * root run-parts /usr/libexec/backup  # 디렉터리 경로여야함
```

- wget 명령어로 sh 파일 잘 실행되는지 확인
- 서비스 재시작 : `service crond restart`
- 만약에 crontab 이 잘 동작 안하면 아래와 같이 확인
  - `vim /var/spool/mail/root` 여기에 에러 메시지가 나와있을 것임.

## Proxy pass

예를들어, fomo.or.kr/museum 을 museum.ilje.or.kr 로 리다이렉트 시키고 싶을때 `proxy pass` 라는 것을 사용한다.

- 먼저 cmd > nslookup museum.ilje.or.kr 으로 해당 ip 주소를 얻는다.
- 그런 다음 proxy pass 를 설정한다.
	- https://velog.io/@always0ne/Proxy-Pass%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-Apache-Web-Server%EC%97%90-WAS-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0
	- https://playon.tistory.com/87

## Aapche, Tomcat SSL 설정 시 주의 사항

예를 들어 권한이 ROOT 와 WEB 으로 구분되는 경우, 부모 디렉터리 권한이 ROOT 인곳에 WEB 을 넣으면 권한 관리하기가 불편하다. 따라서
Apache 의 SSL 저장 위치랑, Tomcat 의 SSL 의 저장 위치를 다르게 가져가는게 권한 관리하기에 편한다.

## Apache method 제한

보통 GET, POST 를 제외하고 method 를 제한 해야 한다.

> https://shyrilla.tistory.com/19

```
<VirtualHost *:9000>
        ServerName www.weave.net:9000
        DocumentRoot /home/web/web_html
        <Directory "/home/web/web_html">
                Options None
                AllowOverride None
                Order allow,deny
                Allow from all
                Require all granted
                <LimitExcept GET POST>
                        Order allow,deny
                        Deny from all
                </LimitExcept>
        </Directory>

        ErrorLog logs/www.weave.net_error_log
        CustomLog logs/www.weave.net_access_log common
</VirtualHost>
```

## 톰캣 JessionId 비활성화

- web.xml

```xml
<session-config>
    <session-timeout>600</session-timeout>
    <tracking-mode>COOKIE</tracking-mode>
</session-config>
```

> https://offbyone.tistory.com/339
