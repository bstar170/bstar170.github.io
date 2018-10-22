---
layout: post
title: 서킷 브레이커?
date: 2018-10-22 16:40:00 -0600
category: 개발
---

### 서킷 브레이커
- 서킷브레이커는 회로 차단기에서 유래한 용어이며, 전기 회로에 과부하가 걸리거나 단락으로 인한 피해를 막기 위해 자동으로 회로를 정지시켰다가, 어느 정도 시간이 지나면 다시 켜는 원래의 기능이 동작하도록 복귀하는 장치

#### 분산 서비스 환경에서 서킷 브레이커 적용
- MSA로 구성한 서비스는 여러개의 연결된 네트워크로 구성됨.
- 이로 인한 새로운 문제들
    - 서비스 하나의 장애가 전파가 되어 다른 곳의 장애가 됨
    - 서비스가 복잡해지다 보니 장애의 원인을 발견하기 힘듦
    - 연쇄적인 장애 발생을 막기 위해 fail fast 함.
    - 이 시스템을 자동화한 것이 Circuit Breaker

![](https://engineering.linecorp.com/image/2016/07/cascading_failure.png)

![](https://engineering.linecorp.com/image/2016/07/fail_fast.png)

-출처 ([분산 서비스 환경에 대한 Circuit Breaker 적용](https://engineering.linecorp.com/ko/blog/detail/76))

-출처 ([
쿠팡 서비스 Cloud Migration을 통해 배운 것들](https://www.slideshare.net/deview/115-119061611))