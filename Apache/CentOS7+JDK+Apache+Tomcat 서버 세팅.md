# 서버 세팅 방법

CentOS7 에서 JDK 1.8 과 Apache 와 Tomcat 을 사용한 서버 세팅 방법

## 1. Apache 설치

CentOS 에서 아파치를 설치하는 방식은 2가지가 있다.

- `컴파일 설치 방식` : 소스파일을 다운로드하고 컴파일하여 설치하는 컴파일 설치 방식
  - apache root directory : `/usr/local/apache2`
- `YUM 방식` : RPM 패키지 파일을 YUM 방식(의존성이 해결됨)으로 설치하는 방식
  - yum 명령어는 root 권한으로만 가능하다.
  - apache root directory : `/etc/httpd`
  
> YUM을 통해서 설치하는 방식이 손쉽고, 버그나 취약점 업데이트 또한 간단하게 대응할 수 있지만, 컴파일 설치가 설치시에 디테일한 부분들을 설정/튜닝 할 수 있다.(설치를 원하는 버젼이 RPM 파일로 없는 경우에도 컴파일 설치를 해야함).
>
> 80 포트를 사용하는 Apache 같은 경우에는 root 로 설치해야한다.

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
    - 방화벽 재시작 : `firewall-cmd --reload`
  - 다시 해당 IP(or localhost)로 Apache 화면이 뜨는지 확인

> 참고 : [How To Open Port 80 CentOS7](https://linuxhint.com/open-port-80-centos/)

- 추가적으로 리눅스 재부팅 시 apache 도 자동으로 시작하게 하는 설정을 해야한다.
  - reboot 명령어 입력 후 CentOS7 리부팅
  - 아파치가 자동으로 실행 되는지 확인하고, 80 포트 방화벽도 잘 열려있는지 확인
  - 아파치가 자동으로 실행이 안된다면 `chkconfig httpd on` 명령어 입력
  - 자동 재시작 명령어 : `systemctl enable httpd`

## 2. JDK 설치

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

```
//# vi /etc/profile

...

JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar

export JAVA_HOME PATH CLASSPATH
```

환경 변수를 등록했다면 ssh 연결을 재시작하거나 `source /etc/profile` 명렁어를 입력해준다.

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

## 3. Tomcat 설치

Tomcat 8 을 기준으로 설명.

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
그리고 AJP 포트 8001 은 건들면 안된다. 나중에 Apache 와 연동할 때 사용하기 때문이다.

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

### 5. 톰캣 실행 및 테스트

- `/usr/local/tomcat8/bin/startup.sh`
- `ps -ef | grep tomcat`
- 주소창에 IP:port 입력 후 톰캣 화면이 나오는지 확인

### 6. 서비스 등록

tomcat이 자동으로 실행되도록 서비스를 등록한다.

서비스를 등록하지 않으면 매 번 /usr/local/victolee/tomcat8.0.52/bin/catalina.sh start 명령으로 tomcat을 실행해야 한다.

서비스를 등록하기 전에 실행되고 잇는 tomcat 프로세스를 종료 하고 진행한다.

- `/usr/local/victolee/tomcat8.0.52/bin/catalina.sh stop`
- `vi /usr/lib/systemd/system/tomcat.service`

```
[Unit]
Description=tomcat8
After=network.target syslog.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/local/jdk1.8
User=root
Group=root

ExecStart=/usr/local/tomcat8/bin/startup.sh
ExecStop=/usr/local/tomcat8/bin/shutdown.sh

UMask=0007
RestartSec=10
Restart=always



SuccessExitStatus=143


[Install]
WantedBy=multi-user.target
```

### 7. 서비스 관리

마지막으로 서비스를 활성화 시킨다.

서비스가 등록이 되었는지 확인하기 위해 재부팅을 한 후, 프로세스를 확인하는 것으로 테스트를 마친다.

재부팅을 했는데 톰캣이 돌아가고 있으면 서비스 등록이 성공적으로 된 것이다.

```
# systemctl enable tomcat.service
# systemctl start tomcat.service
# ps -ef | grep tomcat

# reboot
# ps -ef | grep tomcat
```

### 8. Tomcat Manager 설정

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
