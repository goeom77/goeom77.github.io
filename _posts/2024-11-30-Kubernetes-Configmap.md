---
layout: post
title:  " [Kubernetes] Kubernetes Config Map"
categories: Kubernetes
author: start-easy
---
* content
{:toc}

cloud native spring in action chapter 14장을 참고한 글입니다.

 

설명 내용

1. 쿠버네티스에서 애플리케이션 설정

2. 컨피그 맵과 시크릿 사용

3. 커스터마이즈를 통한 배포 및 설정 관리 방법

 

 

애플리케이션과 함께 패키징 되는 속성파일의 종류

1. 환경 변수 : 호스트 이름, 서비스 이름, 포트 번호 등

2. 설정 서비스 : 스레드 풀, 시간 초과 타임 등

 

* 쿠버네티스에선, 컨피그 맵과 시크릿을 통해 설정 전략을 설정합니다.

 

우선 설정 전략을 설정할 config server도 HTTP로 접근합니다.

따라서 권한 부여된 상대에게만 접속하기 위해 OAuth2 Client Credential 흐름으로 Access Token 기반 보호를 적용합니다.

하지만 Https 통신을 한다면 자체적인 설정 속성이 암호화 되지 않더라도 응답은 암호화 되므로 안전합니다.

 

기존에 단일 서버라면 /actuator/refresh 엔드 포인트에 POST 요청을 통해서 새로고침을 트리거 할수 있습니다.

하지만 모든 인스턴스에 트리거를 하나씩 보낼수 없기 때문에 Spring Cloud Bus를 이용합니다.

 

즉, 변경된 코드를 수정하면 설정 데이터 저장소에서 웹훅으로 트리거를 보냅니다.

그리고 컨피그 서비스가 웹훅을 받으면 설정 변경 이벤트를 각 서비스에 전송하고 각 서비스에서 이벤트를 처리하는 방식입니다.

또한 변경 사항은 다시 config service에서 브로드케스팅합니다.


![](/assets/img/kubernetes/broadcasting.png)

![](/assets/img/kubernetes/webhook.png)D

스프링 클라우드 버스를 통해 어떻게 config를 수정하는지 였습니다.

하지만 모든 코드를 버전관리(git)으로 관리할 수 없습니다.

패스워드와 같은 공개할 수 없는 파일의 경우에는 보안을 향상시킬 필요가 있습니다.

따라서 config server는 /encrypt, /decrept 엔드 포인트를 노출해서 암호화 할수 있지만, Https 환경에서는 설정 서비스에서 전송된 응답은 암호화 되므로 yaml을 수정하는 방법을 통해 수정할 수 있습니다.