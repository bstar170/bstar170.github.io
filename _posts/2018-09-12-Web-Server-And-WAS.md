---
layout: post
title: Web Server와 WAS란?
date: 2019-09-12 10:30:00 -0600
category: 개발
---

흔히 웹 개발을 하면서 Web Server와 WAS의 차이점을 모르는 경우가 많았다. 그 차이를 명확히 알아보기 위해 조사하고 정리한 자료이다.

## Web Server
- 클라이언트의 요청을 받아 **정적 컨텐츠(.html, .png, .css등)를 제공하는 서버**
- 웹 페이지를 클라이언트로 전달하고, 클라이언트로부터 컨텐츠를 전달 받는 역할을 담당한다.
- 인증, 정적 컨텐츠 관리, HTTPS지원, 컨텐츠 압축, 가상 호스팅, 대용량 파일 지원, 대역폭 스로틀링 등의 기능을 지원한다.


### Web Server의 종류

- Apache web server - the HTTP web server
    - Apache Software Foundation에ㅓㅅ 개발한 세계적으로 가장 유명하고 널리 쓰이는 무료 웹 서버.
- Microsoft사의 Internet Information Services(IIS) Windows Server
    - IIS Windows 웹 서버는 기존 서버에 비해 높은 수준의 성능과 보안을 제공한다.
- Nginx web server
    - IMAP/POP3 프록시 서버를 포함하는 무료 오픈 소스 웹 서버. Nginx는 리퀘스트를 스레드로 처리하지 않고, 확장성이 있는 이벤트 기반 설계로 적은 양의 예측 가능한 양의 메모리를 사용한다.
-Lighttpd
    - lighttpd는 FreeBSD 운영 체제와 함께 제공되는 무료 웹 서버이다.
- Jigsaw
    - Jigsaw는 World Wide Web 컨소시엄(W3C)에서 개발되었다. Java로 쓰여졌으며 CGI 스크립트와 PHP 프로그램도 실행할 수 있다.


### Web server 시장 점유율
![Web server 시장 점유율](https://bstar170.github.io/images/WebServerShare.jpg)

## WAS (Web Application Server)
-	동적 컨텐츠를 제공하기 위해 만들어진 애플리케이션 서버 (DB조회, 로직처리가 요구되는 컨텐츠)
-	JSP, Servlet 구동 환경 제공
-	**J2EE**  스펙을 구현한 서버로 분산 트랜잭션, 보안, 메세징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용되는 미들웨어이고, 웹 서버 + 웹 컨테이너로 웹상에서 사용하는 컴포넌트를 올려놓고 사용하는 서버.
    - **J2EE** : 자바 기술로 기업환경의 어플리케이션을 만드는데 필요한 스펙들을 모아둔 스펙 집합. Servlet, JSP, EJB(Enterprise Java Beans), JDBC등이 포함된다.

### WAS의 종류
- tomcat
- Oracle
- IBM Webspere
- jeus

## Web Server와 WAS의 차이
- 동적 컨텐츠 처리를 수행할 수 있는가? -> WAS는 DB조회나 로직처리 등 동적인 컨텐츠를 제공할 수 있다.
- Web Server, WAS를 따로 두고 쓰는 이유는 하나의 서버에서 php애플리케이션과 java애플리케이션을 함께 사용하거나, httpd 서버를 간단한 **로드밸런싱** 을 위해서 사용해야 할 때 필요하기 때문.
    - **로드밸런싱** : 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때, 여러 대의 서버가 분산처리하여 서버의 로드율 증가, 부하량, 속도 저하 등을 고려하여 적절히 분산 처리하여 해결해주는 서비스

## WAS 도입효과
- 안정된 시스템 구성 : 안정적 서비스 보장, 자동적인 어플리케이션 복구기능 제공, 업무 로직이 중간 어플리케이션 서버에 존재, 쉽고 빠르게 구축할 수 있다.
- DB 성능 보장 : WAS서버가 DB서버와의 최적 사용을 조절화, DB connection pool을 총해 DB connection 관리 및 트랜잭션 처리
- 비용절감 : 서버 리소스의 원할한 사용
- 물리적으로 분리하여 보안강화
- 여러 대의 WAS를 연결 가능 (이는 로드밸런싱의 역할 및 fail over, fail back 처리에 유리)

## Web Server와 WAS의 구성에 따른 분류
- **WAS와 WebServer를 분리하지 않는 경우** : 모든 컨텐츠를 한곳에 집중시켜 웹서버와 WAS의 역할을 동시에 수행, 스위치를 통한 로드 밸러싱, 사용자가 적을 경우 효율적
  
- **WAS와 WebServer를 분리한 경우** : 웹서버와 WAS의 기능적 분류를 통해 효과적인 분산을 유도, 정적인 데이터는 웹서버에서 처리, 동적인 데이터는 WAS가 처리
- **WAS 여러개와 WebServer를 분리한 경우** : WAS단을 프리젠테이션 로직와 비즈니스 로직으로 구분하여 구성, 특정 logic의 부하에 따라 적절한 대응할 수 있지만 설계단계 유지보수 단계가 복잡해 질 수가 있다. 

또한 분리되어 있을 때 좋은 점은 배포를 할 때 웹서버를 끄지 않아도 가능함. (무중단 배포.) 요즘에는 L4 장비가 대신해줌.


출처: [http://blog.naver.com/PostView.nhn?blogId=blanket1210&logNo=220850906218](http://blog.naver.com/PostView.nhn?blogId=blanket1210&logNo=220850906218)  
출처: [http://blackaaron.tistory.com/1](http://blackaaron.tistory.com/1)  
출처: [http://parkbrother.tistory.com/entry/Web-서버와-Was-서버의-차이에-대해서-알아보자](http://parkbrother.tistory.com/entry/Web-서버와-Was-서버의-차이에-대해서-알아보자)  
출처: [http://toma0912.tistory.com/52](http://toma0912.tistory.com/52)
