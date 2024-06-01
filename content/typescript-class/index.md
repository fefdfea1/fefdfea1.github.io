---
emoji: 😺
title: class를 만들때 타입지정하는 방법
date: '2024-06-01 10:58:00'
author: fefdfea
tags: Typescript Javascript
categories: Typescript
---

# class 생성시 타입 지정방법

이번 포스팅은 class를 생성할때 타입스크립트로 타입을 지정하는 방법에 대해 알아보겠다. 자바를 사용해본 사람이라면 마치 자바를 쓰는 것 같은 느낌을 받을 수 있을 것이다.

```javascript
class Person {
  name: string;
  constructor() {
    this.name = 'kim';
  }
}
```

해당 코드와 같이 constructor에 this.객체이름 = 값을 지정하기 전 위에서 해당 변수의 존재를 알려주어야 하는데 이 부분이 굉장히 자바와 흡사한 면을 가지고 있다. 그래서 자바를 익힌 사람이라면 어렵지 않게 익숙해질 수 있을것이다. 하지만 익히지 않은 사람이라면 이것을 기억하자 <strong>constructor를 사용할때는 반드시 constructor 이전에 변수의 존재와 변수의 타입을 알려야한다</strong>

### 클래스에서 매개변수 자리에 타입을 지정하기

위에서는 아무런 매개변수를 가지지 않고 고정된 값을 가지는 변수 하나만을 만들어 보았다 이번에는 매개변수를 이용하여 유동적인 클래스를 제작하고 타입을 지정해보겠다.

```javascript
class Person {
  name: string;
  constructor(변수1: string) {
    this.name = 변수1;
  }
}
```

고정값과 constructor ()안에 매개변수의 이름을 정하고 : 옆에 타입을 정하면 위의 코드에서 처럼 타입을 지정해줄 수 있으며 기존에 "kim"으로 고정값으로 들어가는 자리를 변수1로 변경함으로서 이제 해당 클래스는 유동적으로 값을 생성 할 수 있게 되었다. 심지어 타입을 지정하여 원치 않는 값이 들어가는 것을 막아 더욱 안전해졌다.
