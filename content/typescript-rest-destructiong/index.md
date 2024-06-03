---
emoji: 😲
title: TypeScript rest 파라미터와 destructuring 타입지정
date: '2024-06-03 12:45:00'
author: fefdfea
tags: TypeScript Javascript
categories: Typescript
---

# rest 파라미터와 destructuring 타입지정

이번 시간에는 rest 파라미터와 destructuring 문법에서 타입을 지정하는 방법에 대해 알아보겠다. 하지만 진행하기전 이 문법들이 어떤 문법인지 간략하게 알면 더 도움이 될 것 같으니 간략하게 짚고 넘어가보자

## rest 파라미터

rest 파라미터는 함수의 매개변수에 사용 할 수 있는 문법으로 보통 함수에 변수 하나를 생성하면 인자 값으로는 하나의 값만을 넘긴다.

```javascript
function = (매개변수1) => {
  console.log(매개변수1)
}

function("인자값1")
```

이러한 코드에서 rest 파라미터는 받는 인자값을 모두 배열의 원소로 저장하여 보관하는 문법이다. 사용방법은 어렵지 않고 ...만 붙여주면 된다.

```javascript
function = (...rest) => {
  console.log(rest);
};

console.log(1, 2, 3, 4, 5, 6, 7);
```

> 결과 : [ 1,2,3,4,5,6,7 ]

여기서 주의해야할 점은 rest 문법 이후에 오는 모든 인자값을 배열의 원소로 저장하기 때문에 rest 파라미터 하나만 쓸 것이 아니라면 항상 가장 뒤에 사용하여야 한다.

## destructuring( 구조 분해 할당 )

destructuring은 배열이나 객체의 값을 다른 변수로 따로 분리하는 문법이라고 생각하면 쉬울 것이다. 이 부분은 바로 코드를 보도록 하자

```javascript
const arr = [1, 2, 3];
const one = arr[1];
const two = arr[2];
const three = arr[3];
```

우리는 arr의 값을 가져오기 위해 다음과 같이 배열의 index로 접근하여 특정 값을 가져올 수 있다. 하지만 destructuring문법을 이용한다면 이러한 부분을 좀더 간편하게 개선할 수 있게된다.

```javascript
const arr = [1];
const [one, two, three] = arr;
console.log(one, two, three);
```

> 출력 결과: 1 2 3

변수를 []로 감싸고 대입의 값으로써 배열으 넘기게 된다면 arr의 배열안에 원소가 순차적으로 변수에 들어가게 되고 one two three에는 각각 1,2,3이 들어가게 된다.

## rest 파라미터에 타입을 지정하는 방법

위에서 언급했듯 rest 파라미터는 모든 인자값을 가져오기 때문에 그대로 사용한다면 타입스크립트의 장점을 살리기 힘들다. 그래서 rest 파라미터 또한 타입을 제한할 수 있는데 일반적으로 함수에서 타입을 제한하던 방법과 동일하다

### 함수 선언문

함수 선언문에서 타입 지정은 ()안의 매개변수 자리에 밖에 지정이 불가능하기 때문에 아래의 코드처럼 작성한다.

```javascript
type 타입 = number;

function(...rest:타입[]){
  코드~~

}
```

### 함수 표현식

함수 표현식에서는 위의 방식을 사용하여도 가능하고 아래와 같이 모든 매개변수를 한번에 지정하는 방법 또한 가능하다

```javascript
type 타입 = number;

const 함수:타입[] = function = (...rest) => {
  코드~~
}
```

## destructuring의 타입을 지정하는 방법

destructuring 또한 rest와 마찬가지로 배열의 아무 값이나 변수로 저장하게 된다면 우리가 원하는 값이 아닐 수도 있으므로 타입을 지정하는 방법이 존재한다.

### 기존 destructuring 방법

```javascript
const [a, b, c] = [1, 2, 3];
```

### 타입스크립트 destructuring 방법

```javascript
각 원소별로 하나하나 정하는 방법

const [ a,b,c ]:{a:number,b:number,c:number} = [ 1 , 2, 3 ]

타입 alias를 이용한 방법

type 원소타입 = number

const [ a,b,c ]:원소타입[] = [ 1 , 2, 3 ]
```

이번에는 객체를 destructuring하는 방법에 대해 알아보자 객체 또한 배열과 다른점은 없다.

```javascript
각 프로퍼티별로 하나하나 정하는 방법

const object = {
  a: 1,
  b: 2,
  c: 3
}

const { a,b,c }:{a:number,b:number,c:number } = object
```

```javascript
타입 alias를 이용한 방법

type 원소타입 = {
  a: number,
  b:number,
  c:number
}

const { a,b,c }:원소타입 = object
```
