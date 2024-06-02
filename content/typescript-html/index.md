---
emoji: 🤔
title: TypeScript HTML조작법
date: '2024-06-02 10:04:00'
author: fefdfea
tags: TypeScript Javascript
categories: Typescript
---

# 🎲 타입스크립트로 HTML 조작

이번 포스팅은 타입스크립트로 HTML을 조작하는 방법에 대해 포스팅을 진행하겠다. 타입스크립트에서 HTML을 조작하려고하면 어떤 HTML의 종류인지를 알릴 필요가 있다. 알리는 방법은 크게 2가지가 있다.

- Assertion을 이용한 방법 ( as )
- instanceof를 이용한 방법

## ♟️ Assertion을 이욯한 방법

Assertion을 이용한 방법은 특정 dom을 찾았을때 해당 명령어의 가장 오른쪽에 as 를 붙이고 자신이 원하는 태그를 사용한다.

```javascript
const div = document.querySelector(".div") as HTMLDivElement
```

위의 코드와 같이 어떤 HTML인지 as를 통해 지정하면 된다. 만약 어떤 HTML의 타입이 있는지 모른다면 일단 HTML을 붙이고 자신이 사용하는 태그를 함께 붙여보자 거의 다 그렇게 나온다.

ex : a태그 = HTML + a = HTMLAnchorElement
ex2: span태그 = HTML + span = HTMLSpanElement

## 🪅 instanceof를 이용한 방법

instanceof는 이전 포스팅에서 특정 생성자의 속한 객체인지 혹은 생성자를 통해 만들어진 인스턴스인지를 판단하는 키워드라고 하였다.
이를 이용하여 사용하는 HTML 태그가 어떤 객체에서 상속받아 만들어졌는지를 이용하여 Narrowing이 가능하다. 예시를 보도록 하자

```javascript
const div = document.querySelector(".div")
if( div instanceof HTMLDivElement ){
  코드 ~~
}
```

해당 코드에서는 div가 HTMLDivElement의 객체를 상속받은 태그인지를 확인하여 Boolean값으로 반환하는 코드를 if문의 조건문으로 작성하였다. 이러한 방식을 통해 div의 조건문이 true가 반호나이 된다면 이는 div태그가 맞다는 것을 나타낸다.
