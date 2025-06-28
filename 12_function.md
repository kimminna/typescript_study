# 함수

# 기본 정의

타입스크립트에서 함수를 정의할 때는 이 함수가 어떤 타입의 매개변수를 받고, 어떤 타입의 값을 반환하는지를 명시한다.

```tsx
// 1. 함수 선언문을 이용한 함수 정의
function func(a: number, b: number): number {
  return a + b;
} // 함수의 반환값 타입은 자동으로 추론되기 때문에 생략해도 된다.

// 2. 화살표 함수를 이용한 함수 정의
const add = (a: number, b: number): number => a + b;
```

## 매개변수 기본 값 설정하기

함수의 매개변수에 기본 값이 설정되어 있으면 타입이 자동으로 추론된다. 따라서 타입 정의를 생략할 수 있다.

```tsx
function introduce(name = "mina") {} // 매개변수 name 은 string으로 추론됨.

function introduce2(name: number = "mina") {} // 기본 값 과 다른 타입의 매개변수 전달 불가능

introduce(1); // 기본 값과 다른 타입의 값으로 전달 불가능
```

## 선택적 매개변수 설정하기

매개변수의 뒤에 ?를 붙여 주면 선택적 매개변수가 되어 생략할 수 있다.

```tsx
function introduce(name = "mina", tall?: number) {}

introduce("mina", 166);

introduce("mina");
```

선택적 매개변수의 타입은 undefined와 함께 유니온 된 타입으로 추론된다. 따라서 만약 tall의 타입이 number 일 것을 기대하고 사용하려면 타입 좁히기를 해야 한다.

```tsx
function introduce(name = "mina", tall?: number) {
  if (typeof tall === "number") {
  }
}
```

또한 선택적 매개변수는 필수 매개변수 앞에 올 수 없고, 반드시 뒤에 배치해야 한다.

## 나머지 매개변수

rest 파라미터를 이용해서 함수의 매개변수를 받을 수 있다.

다음과 같이 정의한다.

```tsx
function getSum(...rest: number[]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}
```

# 함수 타입 표현식

함수 타입을 타입 별칭과 함께 별도로 정의할 수 있다. 이를 **함수 타입 표현식**이라고 한다.

```tsx
type Add = (a: number, b: number) => number;

const add: Add = (a, b) => a + b;
```

이렇게 함수 타입 표현식을 이용하면 함수 선언 코드와 구현 코드를 나눌 수 있어서 유용하다.

함수 타입 표현식은 주로 여러 개의 함수가 동일한 매개변수 타입, 동일한 반환 값 타입을 가질 때 유용하게 사용된다.

```tsx
const add = (a: number, b: number) => a + b;
const sub = (a: number, b: number) => a - b;
const mul = (a: number, b: number) => a * b;
const divide = (a: number, b: number) => a / b;

// 다음 코드로 간결하게 정의할 수 있다.

type Operation = (a: number, b: number) => number;

const add: Operation = (a, b) => a + b;
// ...
```

함수 타입 표현식이 반드시 타입 별칭과 함께 사용되어야 하는 것은 아니다. 그냥 함수 타입 표현식을 타입 주석에 사용해도 괜찮으나 직관적이지 않을 수는 있다.

```tsx
const add: (a: number, b: number) => number = (a, b) => a + b;
```

# 호출 시그니처

호출 시그니처는 함수 타입 표현식과 동일하게 함수의 타입을 별도로 정의하는 방법 중 하나다.

자바스크립트에서는 함수도 객체이기 때문에 객체를 정의하듯 함수의 타입을 별도로 정의할 수 있다.

```tsx
type Operation2 = {
  (a: number, b: number): number;
};

const add2: Operation2 = (a, b) => a + b;
// ...
```

또한 호출 시그니처 아래에 프로퍼티를 추가로 정의할 수도 있다. 이렇게 할 경우 함수이자 일반 객체를 의미하는 타입으로 정의되며, 이를 **하이브리드 타입**이라고도 한다.

```tsx
type Operation2 = {
  (a: number, b: number): number;
  name: string;
};

const add2: Operation2 = (a, b) => a + b;
add2(1, 2);
add2.name;
```

# 함수 타입의 호환성

함수 타입의 호환성은 다음 2가지 기준으로 판단한다.

1. 두 함수의 반환 값 타입이 호환되는가?
2. 두 함수의 매개변수 타입이 호환되는가?

## 두 함수의 반환 값 타입의 호환

A, B 함수 타입이 있다고 가정할 때,

**A가 B의 슈퍼 타입**이라면 두 타입은 호환된다.

```tsx
type A = () => number;
type B = () => 10;

let a: A = () => 10;
let b: B = () => 10;

a = b; // ok
b = a; // 오류 발생
```

## 두 함수의 매개변수 타입의 호환

### 매개변수의 개수가 같은 경우

**A 매개변수의 타입이 C 매개변수의 타입의 서브 타입**이라면 두 타입은 호환된다.

```tsx
type A = (value: number) => void;
type B = (value: 10) => void;

let a: A = (value) => {};
let b: B = (value) => {};

a = b; // 오류
b = a; // ok
```

반환 값 타입과는 반대로 마치 다운캐스팅을 허용하는 것 같아 보인다.

이렇게 되는 이유는 두 함수의 매개변수의 타입이 모두 객체일 때를 통해 이해하면 쉽다.

```tsx
type Animal = {
  name: string;
};

type Dog = {
  name: string;
  color: string;
};

let funcA = (animal: Animal) => {
  console.log(animal.name);
};
let funcB = (dog: Dog) => {
  console.log(dog.name);
  console.log(dog.color);
};

funcA = funcB; // x
funcB = funcA; // ok
```

funcA에서는 funcB의 프로퍼티를 모두 사용할 수 없다. 존재할 거라고 보장할 수 없는 프로퍼티에 접근한다면 오류가 발생한다.

반면에 funcB에서는 funcA의 프로퍼티를 모두 사용할 수 있다.

따라서 두 함수의 매개변수의 타입 호환의 경우에는 마치 다운캐스팅을 허용하는 것처럼 사용하면 되는 것이다.

### 매개변수의 개수가 다를 때

```tsx
type Func1 = (a: number, b: number) => void;
type Func2 = (a: number) => void;

let func1: Func1 = (a, b) => {};
let func2: Func2 = (a) => {};

func1 = func2; // ok
func2 = func1; // x
```

매개변수의 개수가 많은 함수에 적은 함수를 할당하는 것은 되지만, 그 반대는 안 된다.

두 함수의 매개변수의 개수도 다르고, 타입도 다른 경우는 호환 자체가 안 되므로 타입이 동일할 때를 가정한다.

# 함수 오버로딩

함수 오버로딩이란 하나의 함수를 매개변수의 개수나, 타입에 따라 다르게 동작하도록 만드는 문법이다.

JS에서는 구현할 수 없었지만 TS에서는 구현이 가능하다.

타입스크립트에서 함수 오버로딩을 구현하려면 버전 별로 오버로드 시그니처를 만들어야 한다.

```tsx
function func(a: number): void;
function func(a: number, b: number, c: number): void;
```

이렇게 선언부만 만들어 둔 함수를 **오버로드 시그니처**라고 한다.

각 시그니처는 각 함수의 버전을 의미한다.

구현 시그니처는 실제로 함수가 어떻게 실행될 것인지를 정의한다.

```tsx
function func(a: number, b?: number, c?: number) {
  if (typeof b === "number" && typeof c === "number") {
  } else {
  }
}
```

구현 시그니처는 모든 오버로드 시그니처와 호환되어야 하므로, a를 제외한 b와 c를 선택적 매개변수로 만든 뒤, 함수 내부에선 타입 좁히기를 이용하여 각 버전 별로 구현을 다르게 한다.

# 사용자 정의 타입 가드

사용자 정의 타입 가드란 참 또는 거짓을 반환하는 함수를 이용해서 프로그래머가 원하는 타입 가드를 만들 수 있게 해 주는 문법이다.

```tsx
type Dog = {
  name: string;
  isBark: boolean;
};

type Cat = {
  name: string;
  isScratch: boolean;
};

type Animal = Dog | Cat;

function warning(animal: Animal) {
  if ("isBark" in animal) {
  } else if ("isScratch" in animal) {
  }
}
```

만약 타입이 프로그래머가 정의한 타입이 아니라 따로 추가나 수정이 안 되는 타입이라고 가정했을 때, 다음과 같이 커스텀 타입 가드를 이용해서 타입을 좁히는 방식이 더 권장된다.

```tsx
// Dog 타입인지 확인하는 타입 가드

function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined;
}

// Cat 타입인지 확인하는 타입가드
function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
  if (isDog(animal)) {
  } else {
  }
}
```

isDog 함수는 매개변수로 받은 값이 Dog 타입이라면 true, 아니라면 false를 반환한다. 이때 반환 값의 타입으로 animal is Dog을 정의하면, 이 함수가 true를 반환했을 때 조건문 내부에서는 이 값이 Dog 타입임을 보장하는 것이 된다. 따라서 다음과 같은 커스텀 타입 가드를 이용해서 타입을 좁히면 코드를 훨씬 직관적으로 작성할 수 있다.
