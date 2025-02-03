---
layout: post
title:  " [NodeJS] NodeJS 시작하기 "
categories: NodeJS
author: start-easy
---
* content
{:toc}

* 작성 중

## NodeJS

* 일반적으로 프론트와 백을 같은 언어로 하는 프레임워크는 적다.

* NodeJS는 JavaScript로 서버단 로직을 처리할 수 있다.

* 다만, 비동기 방식으로 처리하기 때문에 설계와 콜백에 주의를 해야한다.

---


### Node.JS 시작하기


``` shell
# node.js 설치 및 버전 확인
apt-get install nodejs
node-v

# npm 설치 및 버전 확인
apt-get install npm
npm -v

# 노드 프로젝트 생성
npm init


# 익스프레스 생성
# react나 다른 프론트 framework가 있다면 설치하지 않아도 됩니다.
# 빠르게 node js의 기능을 화면에 보여주기 위한 용도입니다.
npm install express

# njs
# 익스프레스에서 html 내에 javascript를 쓰기위한 패키지
npm install ejs

```

* 기본 코드 실행해보기


``` shell

# express 가져오기
const express = require('express');
const app = express();
const port = 8000; # port number - ncp에서 열었던 포트 번호 

app.get('/', (req, res) => {
  res.send('하이');
});

app.listen(port, () => {
  console.log('8000!');
});

```
## Reference

* [Coding-X 쉽고 재밌는 웹개발](https://coding-x.com/class/10046/%EC%89%BD%EA%B3%A0-%EC%9E%AC%EB%B0%8C%EB%8A%94-%EC%9B%B9%EA%B0%9C%EB%B0%9C4-node-js)
