---
emoji: 🐕
title: TypeScript readOnly로 변경 막기
date: '2024-06-01 10:47:00'
author: fefdfea
tags: TypeScript Javascript
categories: Typescript
---

# 타입스크립트의 readOnly로 상수 만들기 ☀️

타입스크립트에는 readOnly라는 키워드가 존재한다 해당 키워드는 자바스크립트를 사용해본 사람이라면 접해보는 const와 비슷하다고 생각하면 쉬운데 const는 값을 변경할 수 없는 수 즉 상수를 만드는 키워드이다 하지만 객체나 배열은 const로 만든다 하여도 그 아의 값 까지 변경이 불가능 하게 되는것은 아니기에 다소 만족스러운 결과를 얻을 수는 없다. 여기서 타입스크립트를 사용한다면 객체의 값을 readOnly로 지정하여 값을 변경하지 못하도록 할 수 있다.

## 타입스크립트 readOnly 사용 🌝

타입스크립트의 readonly Interface & type 2가지 방법 모두 사용 할 수 있으며 사용 방법은 아래와 같다.

```javascript
type a = {
  readonly b : string
}

interface c {
  readonly n : number
}

```

위와 같이 정의하고 변수에 타입을 지정하여 사용하면 해당 객체는 더이상 변경 할 수 없는 객체로 지정되어 값이 변경되지 않는다고 확신 할 수 있게 된다.

## readOnly의 값을 변경하는 방법 🪐

위에서는 객체의 값을 readOnly로 지정해두면 변경이 불가능 하다고 하였다. 하지만 두가지 방법이 있는데 클래스에 constructor를 지정하여 변경하는 방법과 객체 안의 객체가 지정되었을시에만 사용 가능한 방법이다 해당 방법을 바로 코드로 구현하여 보도록 하자

### 첫 번째 방법 ⭐

아래의 상황은 하나의 객체만 사용된 barA를 변경하려는 코드이다 이렇게 2개가 중첩되어 있지 않은 코드는 변경이 불가능하다

```javascript
type readonlyA = {
  readonly barA: string
};

const x: readonlyA = {barA: 'baz'};
x.barA = 'quux'; // ⛔️ 변경 불가
```

해당 코드는 barB가 baz라는 또 다른 객체값을 가지는 코드이다 이렇게 되면 readOnly는 BarB에는 적용이 되지만 baz에는 적용이 불가능한 모습을 보여준다

```javascript
type readonlyB = {
  readonly barB: { baz: string }
};
const y: readonlyB = {barB: {baz: 'quux'}};
y.barB.baz = 'zebranky'; // 👌 변경 가능
```

### 두 번재 방법 🌟

두 번째 방법은 위에서 말했듯 constructor를 이용하여 변경하는 방법이다.

```javascript
class Foo {
    readonly bar = 1; // OK
    readonly baz: string;
    constructor() {
        this.baz = "hello"; // OK
    }
}
```

해당 코드와 같이 readOnly로 선언한 baz는 constructor의 this로 접근하여 변경이 가능하다
