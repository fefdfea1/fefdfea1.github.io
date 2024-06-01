---
emoji: 🍇
title: TypeScript Narrowing과 Assertion
date: '2024-06-01 11:53:00'
author: fefdfea
tags: TypeScript Javascript
categories: Typescript
---

# 타입스크립트의 타입 확정

타입스크립트에서는 타입을 확정하기 위한 방법이 존재한다. 이러한 방법 2가지를 Narrowing과 Assertion으로 부르는데 이번에는 이 2가지에 대해 알아보도록 하자

## 타입을 설정한 대로 인식하게 하는 Assertion

Assertion은 타입을 다른 타입으로 변경하는 것이 아닌 이 타입으로 생각해라~ 라는 의미이다. 즉 string 타입을 number로 Assertion한다고 number로 바뀌는 것은 아니라는 의미이다

```javascript
 interface Foo {
    bar: number;
    bas: string;
}
var foo = {} as Foo;
foo.bar = 123;
foo.bas = 'hello';
```

다음 코드를 보면 as를 통해 foo의 타입을 Foo로 타입 표명하여 foo.bar, foo.bas로 접근하여 값을 수정할 수 있게 되었다 원래는 해당 방법 없이 foo에 접근하려 하면 bar와 bas가 없다는 오류를 발생시킨다.

이처럼 as는 타입을 사용하여 일시적으로 해당 타입으로 인식시키는 것이 가능하지만 이를 너무 악용할시 타입스크립트를 사용하는 의미가 없어질 수도 있으니 주의해서 사용해야한다!

## 타입의 범위를 줄이는 Narrowing

Narrowing은 타입을 확정하기도 하지만 더 정확히는 타입의 범위를 줄인다고 표현하는게 맞는 표현이며 Narrowing으로 범위를 줄이는방법은 많은데 이번 포스팅에서 5가지 정도의 방법을 알아보겠다.

### 조건문을 이용한 방법

조건문을 이용한 방법은 if 문을 이용하여 타입의 벙위를 좁히는 것이다.

```javascript
type Animal = {
  name: string,
  legs: number | undefined,
};
```

이러한 타입이 있다고 가정하였을때 legs라는 값에 + 1을 하면 undefined 타입이 존재하여 +연산을 할 수 없다. 그래서 이를 해결하기 위해 if문으로 legs가 undefined가 아닐때 +연산을 진행하도록 할 수 있는데 이것이 조건문을 이용한 Narrowing이다 해당 방법을 통하여 undefined말고 null 또한 해결이 가능하다

```javascript
function addLeg(animal: Animal) {
  if (animal.legs) {
    animal.legs = animal.legs + 1;
  }
}
```

### typeof를 이용하여 타입을 체크하는 방법

typeof는 왼쪽의 갑을 오른쪽의 타입과 비교해주는 키워드이다 바로 코드를 보면서 이해해보자

```javascript
function double(item: string | number) {
  if (typeof item === 'string') {
    return item.concat(item); // item이 string 타입
  } else {
    return item + item; // item이 number 타입
  }
}
```

여기서 item은 string과 number 둘다 허용하고 있지만 typeof로 string 타입과 같을때만 true를 반환하고 if문을 통과할 수 있어 타입의 범위를 줄일 수 있다.

### instanceof를 이용한 방법

instanceof는 객체가 Class에 속하거나 class의 인스턴스인지를 확인하는 방법으로 만약 한 타입에 2가지의 클래스를 가질 수 있는 타입이라고 하면 instanceof를 사용 할 수 있다.

```javascript
class Person {
    constructor(
        public firstName: string,
        public surname: string
    ) {}
}
class Organisation {
    constructor(public name: string) {}
}
type Contact = Person | Organisation;
```

```javascript
function sayHello(contact: Contact) {
  console.log('Hello ' + contact.firstName);
}
```

해당 코드를 실행하게 될시 오류를 발생하게 된다. 괘냐하면 contact는 firstName을 가지고 있지 않은 Organisation을 담고 잇을 수도 있기 때문에 타입스크립트가 이를 차단한다. 바로 이런 상황에 상송의 여부를 확인하는 instanceof를 사용하면 될것이다.

```javascript
function sayHello(contact: Contact) {
  if (contact instanceof Person) {
    console.log('Hello ' + contact.firstName);
  }
}
```

이렇게 작성하게 된다면 contact는 조건문에서 Person 클래스의 값을 받았을때에 한해서 if을 통과하고 문제없이 console.log()가 실행 될것이다.

### in을 이용한 Narrowing

객체를 좀 다루다 보면 in이라는 키워드를 접해본적이 있을텐데 in은 키워드의 의미와 부합하게 왼쪽의 값이 오른쪽의 객체에 존재하는지를 확인하는 키워드이다 이를 이용하여 타입의 범위를 좁힐 수 있다

```javascript
interface Person {
  firstName: string;
  surname: string;
}
interface Organization {
  name: string;
}
type Contact = Person | Organisation;
```

이러한 코드가 있다고 할때 in을 이용하여 firstName을 찾아 Person클래스로 범위를 줄여보도록 하자

```javascript
function sayHello(contact: Contact) {
  if ('firstName' in contact) {
    console.log('Hello ' + contact.firstName);
  }
}
```

코드를 작성하면 다음과 같은 것이다 firstName이라는 key를 가진 타입은 Person이 유일함으로 자동으로 Person 타입으로 범위가 줄어든다.

### type predicate를 이용한 타입 Narrowing

이번에는 promise 객체를 반환하는 상황에서 타입의 범위를 줄이는 방법에 대해 알아보자

```javascript
async function getRating(productId: string) {
const response = await fetch(
/products/${productId}
);
const product = await response.json();
const rating = product.rating;
return rating;
}
```

다음과 같이 fetch 코드가 있다고 하면 들어오는 타입이 확정적으로 하나 밖에 없을시에는 <>를 이용하여 사용 할 수도 있겠지만 예상치 못한 값이 들어올 수도 있는 상황이라면 type predicate 방식의 사용을 고려해보는 것도 좋을 것이다.

```javascript
function isValidRating(
  rating: any
): rating is Rating {
  if (!rating || typeof rating !== "number") {
    return false;
  }
  return (
    rating === 1 ||
    rating === 2 ||
    rating === 3 ||
    rating === 4 ||
    rating === 5
  );
}
```

type predicate를 위한 isValidRating함수이다 해당 함수를 해석해보자면 isValidRating의 return 값이 true일시 rating변수를 해당 함수가 호출이 된 범위 내에서는 Rating 타입으로 취급해라 라는 의미이다. 중간에 if을 이용할 수 있어 예상치 못한 타입이 왔을시 예외처리가 가능하다

```javascript
async function getRating(productId: string) {
  const response = await fetch(`/products/${productId}`);
  const product = await response.json();
  const rating = product.rating;
  if (isValidRating(rating)) {
    return rating; // type of rating is `Rating`
  } else {
    return undefined;
  }
}
```

위의 함수를 적용한 모습이다 이 함수의 return 값은 type predicate에 의해 Rating으로 범위가 좁혀진 것을 실행하면 확인해볼 수 있다.
