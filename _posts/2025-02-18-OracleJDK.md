---
layout: post
title:  " [Java] Oracle JDK vs Open JDK"
categories: Java
author: start-easy
---
* content
{:toc}

## Open JDK란 무엇일까?

![](/assets/img/java/openjdk.png)

3월과 6월마다 새로운 버전을 릴리즈합니다.

결론적으로 open JDK는 '무료' 입니다.

오라클, 레드햇, Azul, AdoptOpenJDK 등의 OpenJDK 바이너리 파일을 제공받을 수 있습니다.

‘Java SE의 무료 및 오픈 소스 구현체’로 Java SE와 같은 API와 기능을 제공하며 호환성을 유지합니다.


## Oracle JDK란 무엇일까?

역사를 확인해보면, 최초 썬 마이크로시스템즈에서 출시된 이후 Java를 Oracle에서 2010년 인수하면서 변화가 생겼습니다.

Java 언어 자체는 GPL라이센스로 무료이고, Java를 실행시키기 위한 JVM과 JRE를 합친 JDK가 유료가 된 것입니다.

결론, 구독 및 유료로 사용하는 만큼 Oracle JDK가 제공하는 서비스들이 있고, 보안 요소들이 있습니다.
하지만 대체가 가능하거나 안쓰는 부분이 존재한다면 무료인 Oracle JDK도 충분히 사용하기 편하다는 것입니다.
결국 개발자 개인의 생각에 따라 바뀌는 문제로 보입니다.

> JVM (Java Virtual Machine)
```
Java 바이트코드를 실행하는 가상 머신입니다.
운영체제에 맞게 Java 프로그램을 실행할 수 있도록 해줍니다.
단독으로는 실행 파일이 아니라 바이트코드(.class 파일)를 실행하는 역할을 합니다.
```

> JRE (Java Runtime Environment)
```
JVM + Java 표준 라이브러리(rt.jar 등) + 기본 실행 환경을 포함합니다.
Java 프로그램을 실행할 수 있는 환경을 제공합니다.
하지만 개발 도구(컴파일러 javac 등)는 포함되어 있지 않습니다.
```

> JDK (Java Development Kit)
```
JRE + 개발 도구(javac, javadoc, jdb 등)
Java 애플리케이션을 개발하고 실행할 수 있는 도구 모음입니다.
Java 개발을 위해 필요한 컴파일러, 디버거, 빌드 도구 등을 포함합니다.
```

### Oracle JDK 변화

1. 2019년

* Oracle Java JDK 8부터 구독을 해야 SE를 이용할 수 있게 변경되었습니다.

2. 2021년

* NFTC 라이센스로 Oracle JDK 17이상을 상업적으로 무료로 사용할 수 있게 되었습니다. 다만, 보안 패치 제한으로 인해 지속적인 이용을 위해서는 구독이 필요합니다.


3. 2023년

* Java SE Universal Subscription 시작, 조직 전체 인원에 대한 비용을 라이선스비로 요구하는 구독권
ex) 100명인 기업에서 1명이 사용 ==> 100명분의 구독권 신청 필요

4. 2024년

* 9월에 출시된 보안 패치 라이센스를 적용하면 유료화 재적용

> 정리
```
Oracle JDK 무료 조건

**Oracle JDK 17 이상**

- Oracle JDK 17부터 "NFTC (No-Fee Terms and Conditions)" 라이선스로 제공
- 상업적 용도 포함하여 무료 사용 가능
- 단, **최대 1년간 최신 업데이트만 무료 제공** → 이후 장기 지원이 필요한 경우 유료 라이선스 필요
```

### 계약별 이해

1. Oracle 바이너리 코드 라이센스 계약(BCL)

해당 버전 : Java 11 이전

상업적 사용 허용 : 일반 목적 컴퓨팅에서만 무료로 사용 가능

ex) 웹 브라우징, 일발적인 사무용 애플리케이션을 위해 사용자 컴퓨터에서 Java를 실행하는 것은 유료 라이선스가 필요하지 않음

2. Oracle Technology Network 라이선스(OTN)

해당 버전 : Java 8 보안 패치 이후

사업적 사용 허용 : 상업적 사용 금지 

3. Oracle 무료 이용 약관(NFTC)

해당 버전 : 2024년 9월 이후 중요 보안 업데이트가 필요한 회사는 구독 비용을 지불해야 이용 가능

상업적 목적으로 Java 17이상을 사용할 수 있음

보안 패치를 하려면, 제한되고 2024년 9월 유료 구독이 필요

### Java, JDK, JRE의 차이점

![](/assets/img/java/jdk.png)

Java : 프로그램 언어(GPL)

JDK : 개발자가 java 프로그램을 개발하기 위한 Java Developemnt Kit

JRE : 사용자가 Java 프로그램을 실행하기 위한 Java Runtime Enviroment

### Java SE, Java EE, Java ME 차이점

Java SE : 표준 Java 개인 개발자들이 사용하는 버전

Java EE : 기업에서 사용하는 Java 

Java ME : 임베디드 / 모바일 디바이스에서 사용하는 경량화된 Java

