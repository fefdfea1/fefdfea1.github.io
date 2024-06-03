---
emoji: 🍓
title: TypeScript Never에 대해
date: '2024-06-03 20:19:00'
author: fefdfea
tags: TypeScript Javascript
categories: Typescript
---

# TypeScript Never타입

이번 포스팅은 Never 타입에 대해 알아보도록 하겠다. Never은 any나 unknown과는 다른 타입으로 그 어떤 값도 가질 수 없는 타입이다. 이렇게 아무것도 가질 수 없도록 하는 타입은 어떤 경우에 사용해야 할까?

> any도 예외없이 Never에 사용 불가능하다

## 함수의 매개변수로 사용

첫 번째로 함수의 매개변수의 타입을 never로 지정하여 값이 들어오는 경우를 방지 할 수 있다.

```javascript
function fn(input: never) {}

// 오직 `never` 만 받는다.
declare let myNever: never
fn(myNever) // ✅

// 아무 값이나 전달하거나 아무 값도 전달하지 않으면 타입 에러 발생
fn() // ❌ 인자 'input'에 아무 값도 주어지지 않음
fn(1) // ❌ 'number' 타입은 'never' 타입에 할당할 수 없음
fn('foo') // ❌ 'string' 타입은 'never' 타입에 할당할 수 없음

// `any`도 통과할 수 없다.
declare let myAny: any
fn(myAny) // ❌ 'any' 타입은 'never' 타입에 할당할 수 없음
```

```javascript
function unknownColor(x: never): never {
  throw new Error('unknown color');
}

type Color = 'red' | 'green' | 'blue';

function getColorName(c: Color): string {
  switch (c) {
    case 'red':
      return 'is red';
    case 'green':
      return 'is green';
    default:
      return unknownColor(c); // 'string' 타입은 'never' 타입에 할당할 수 없음
  }
}
```

## 타이핑을 부분적으로 허용하지 않는다.

만약 2가지의 타입을 가질 수 있는 매개변수가 있다고 가정해보자 그렇게 된다면 다음 코드와 같이 본래 의도와는 다르게 두 가지 중 하나가 아닌 둘다를 사용하는 경우가 생길 수동 있다.

```javascript
type VariantA = {
  a: string,
};

type VariantB = {
  b: number,
};

declare function fn(arg: VariantA | VariantB): void;

const input = { a: 'foo', b: 123 };
fn(input); // 타입스크립트 컴파일러는 아무런 문제도 지적하지 않지만, 우리의 목적에는 맞지 않는다.
```

이를 막기위해 never를 사용하여 각각의 타입에 하나씩 객체를 추가하면 이런식으로 2가지 타입을 한번에 사용하는 것을 막을 수 있다.

```javascript
type VariantA = {
    a: string
    b?: never
}

type VariantB = {
    b: number
    a?: never
}

declare function fn(arg: VariantA | VariantB): void


const input = {a: 'foo', b: 123 }
fn(input) // ❌ 속성 'a'의 타입은 호환되지 않는다.
```

## 유니언 타입에서 멤버 필터링

never 타입은 유니언 타입이에 사용할시 그대로 사라진다. 하지만 이러한 특성을 이용해 제네릭을 이용하면 훌륭한 타입 필터링기를 만들 수 있다.

```javascript
type Foo = {
    name: 'foo'
    id: number
}

type Bar = {
    name: 'bar'
    id: number
}

type All = Foo | Bar

type ExtractTypeByName<T, G> = T extends {name: G} ? T : never

type ExtractedType = ExtractTypeByName<All, 'foo'> // 결과 타입은 Foo
```

이 코드를 보면 타입을 2개를 지정하고 All이라는 타입을 유니언 타입으로 지정한다 그 후 제네릭에 값과 함께 넘겨서 넘긴 값 ( 여기서는 foo )이 어떤 타입에 속하는지 반환 받을 수 있다. 만약 틀린 값을 넣는다면 never를 반환할 것이다.
