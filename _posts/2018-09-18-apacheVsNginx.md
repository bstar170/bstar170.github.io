---
layout: post
title: Apache 와 Nginx 특징과 비교
date: 2018-09-18 14:04:00 -0600
category: 개발
---

tomcat과 연동할 Web server를 찾다가 요즘 핫하다는 Nginx를 사용하여 연동해보기로 하였다. 연동하기 전에 Apache와 Nginx의 특징에 대해 조사해봤다.

## 1. Apache

### 특징
- 요청 하나 당 프로세스(또는 쓰레드)가 처리하는 구조. 요청이 많을수록 CPU와 메모리 사용이 증가하므로 성능 저하가 있을 수 있음. 또한 프로세스가 blocking되면 처리가 완료될 때까지 계속 대기함.
- 이와 같은 문제는 Keep Alive를 활성화 함으로 해결이 가능하지만 대략 접속 시 효율이 급격하게 떨어짐.

#### Keep Alive?
HTTP프로토콜의 특성상 한 번 통신이 이루어지면 접속을 끊지만, KeepAlive On 상태에서는 KeepAliveTimeOut시간 동안 접속을 끊지 않고 다음 접속을 기다린다.

이 때문에 성능 향상을 기대할 수 있음. (서버가 바쁘지 않은 상태에서만..)

바쁜 서버 환경에서는 모든 요청마다 연결을 유지해야하므로 프로세스수가 늘어나 MaxClient를 초과하게 됨. 

메모리를 많이 사용하여 성능 저하의 원인이 된다. 이에 따라 Apache 2.4부터 Event MPM 방식을 지원.

#### Event MPM?
Keep Alive에 대한 고민에서 출발한 새로운 MPM 방식. 이 방식은 사용자 요청과 Keep Alive한 Apache의 요청을 그대로 맺어주는 것이 아니라, 요청을 처리하는 쓰레드를 따로 두어 분산된 처리를 하는데 그 목적을 두고 있다.

## 2. Nginx

Nginx의 아키텍쳐 구조는 아래의 그림과 같다.

![Nginx 아키텍쳐 구조](https://bstar170.github.io/images/Nginx%20architecture.png)



보안과 속도를 최적화시키려는 노력에서 탄생한 웹서버. 심플하고 규모가 작은 서비스이면서 정적 데이터 처리가 많은 서비스에 적합. 프로그램의 흐름이 이벤트에 의해 결정이 되는 Event Driven 방식의 웹 서버.

적은 수의 쓰레드로 효율적인 일 처리를 하며, 메모리도 적게 사용하는 구조. 쓰레드를 많이 사용하지 않기 때문에 context switching 비용이 적고, CPU 소모도 적음.

그러나 **모듈 개발이 어려우며 다양한 모듈이 없다는 것이 단점**입니다.

## 3. Apache와 Nginx 비교

Nginx 는 비동기 이벤트 기반으로 요청을 처리하고, Apache 서버는 요청 당 쓰레드 또는 프로세스가 처리하는 구조.  

최근 대용량의 정적 파일 및 큰 규모의 사이트가 많아짐에 따라 대량 접속에도 적은 리소스를 사용하며 빠르게 서비스를 할 수 있는 웹 사이트가 대세가 되었는데, 이런 면에서 Nginx가 각광 받기 시작했다.

그러다가 2012년에 빠른 응답 속도와 적은 리소스 사용 부분을 개선한 Apache 2.4를 발표하면서, Nginx에 대응하기 시작했다.

그럼에도 Nginx가 성능 면에서는 Apahce 2.4 보다 좋다고 한다. (엄청 큰 차이는 아님)

하지만 PHP 모듈 등을 직접 적재할 수 있는 Apache가 구조상 이점이 있기에 복잡한 웹 사이트의 경우 Apache가 적합하다.

세션 클러스터링 같은 특별한 목적을 추가적으로 수행하는 세팅을 할 경우에는 별도의 과정을 거쳐야 하기 때문에 이러한 별도의 작업이 많이 필요한 서비스의 경우에도 유지 보수 측면에서 Apache가 유용하다.

즉 안정성과 확장성, 호환성에서 Apache가 우세, 성능 면에서는 Nginx가 우세하다는 것이 결론이다.

##### 출처
- [http://victorydntmd.tistory.com/231](http://victorydntmd.tistory.com/231)  
- [http://blog.naver.com/PostView.nhn?blogId=tmondev&logNo=220737182315](http://blog.naver.com/PostView.nhn?blogId=tmondev&logNo=220737182315)
