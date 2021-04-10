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

#### 7. 주소창에 IP(or localhost) 입력 후 apache 화면이 나오는지 확인

나오는 경우 딱히 신경쓸게 없지만, 나오지 않는 경우에는 해당 포트 번호가 뚫려있는지 확인해야 하므로 telnet 을 이용해야한다.

- 안뜨면 Window 제어판에서 telnet 설치 후 cmd 창을 열어서 `telnet ip port`로 붙는지 확인.
  - telnet 192.168.x.x 80
  - [CentOS7 방화벽 열기](https://www.lesstif.com/system-admin/rhel-centos-firewall-22053128.html)
    - `firewall-cmd --permanent --zone=public --add-port=80/tcp`
  - 다시 해당 IP(or localhost)로 Apache 화면이 뜨는지 확인

- 추가적으로 리눅스 재부팅 시 apache 도 자동으로 시작하게 하는 설정을 해야한다.
  - reboot 명령어 입력 후 CentOS7 리부팅
  - 아파치가 자동으로 실행 되는지 확인하고, 80 포트 방화벽도 잘 열려있는지 확인
  - 아파치가 자동으로 실행이 안된다면 `chkconfig httpd on` 명령어 입력
