# 인터페이스

인터페이스는 타입 별칭과 동일하게 타입에 이름을 지어주는 또다른 문법이다.

```tsx
interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: "mina",
  age: 23,
};
```

타입 별칭과 문법만 조금 다를 뿐 기본적인 기능은 거의 같다.

## 메서드의 타입을 정의하기

```tsx
// 함수 타입 표현식을 이용한 정의
interface Person {
  readonly name: string;
  age?: number;
  sayHi: () => void;
}

// 호출 시그니처를 이용한 정의
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void;
}
```

## 메서드 오버로딩

함수 타입 표현식으로 메서드의 타입을 정의하면 메서드의 오버로딩 구현이 불가능하다.

```tsx
// 함수 타입 표현식을 이용한 정의

interface Person {
  readonly name: string;
  age?: number;
  sayHi: () => void;
  sayHI: (a: number, b: number) => void; // x
}

// 호출 시그니처를 이용한 메서드 오버로딩 구현

interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void;
  sayHi(a: number): void;
}
```

## 하이브리드 타입

인터페이스 또한 함수이자 일반 객체인 하이브리드 타입을 정의할 수 있다.

```tsx
interface Func2 {
  (a: number): string;
  b: boolean;
}

const func2: Func2 = (a) => "hello";
func.b = true;
```

## 주의할 점

```tsx
type Type1 = number | string;
type Type2 = number & string;

interface Person {
	name: string;
	age: number;
} | number; // x
```

인터페이스로 만든 타입을 Union과 Intersection으로 이용해야 한다면 다음과 같이 타입 별칭과 함께 사용하거나, 타입 주석에서 직접 사용해야 한다.

```tsx
type Type1 = number | string | Person;
type Type2 = number & string & Person;

const person: Person & string = {
  name: "mina",
  age: 23,
};
```

## 인터페이스 확장

인터페이스 확장이란 하나의 인터페이스를 다른 인터페이스들이 상속받아 중복된 프로퍼티를 정의하지 않도록 도와주는 문법이다.

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  breed: string;
}

interface Cat extends Animal {
  isScratch: boolean;
}

interface Chicken extends Animal {
  isFly: boolean;
}
```

특정 인터페이스를 기반으로 여러 개의 인터페이스가 파생되는 경우 중복 코드가 발생할 수 있는데, 이럴 때에는 인터페이스의 확장 기능을 이용하면 코드를 간결하게 작성할 수 있다.

확장 대상 타입인 Animal은 나머지 타입들의 슈퍼 타입이 된다.

- 인터페이스는 인터페이스뿐만 아니라 타입 별칭으로 정의된 객체도 확장할 수 있다.

  ```tsx
  type Animal = {
  	name: string;
  	color: string;
  };

  interface Dog extends Animal{}...
  ```

- 인터페이스는 여러 개의 인터페이스를 확장할 수도 있다.
  ```tsx
  interface DogCat extends Dog, Cat {}

  const dogCat: DogCat = {
    name: "",
    color: "",
    breed: "",
    isScratch: true,
  };
  ```

### 프로퍼티 재정의하기

확장과 동시에 프로퍼티의 타입을 재정의할 수도 있다.

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  name: "mina"; // 타입을 String Literal로 재정의함.
  breed: string;
}
```

다음과 같이 확장받는 타입에서 프로퍼티의 타입을 재정의할 수 있다.

한 가지 주의할 점은 프로퍼티를 재정의할 때, 원본 타입의 서브 타입이어야만 재정의할 수 있다는 것이다. Dog 인터페이스에서 name을 number 타입과 같은 타입으로의 재정의는 불가능하다.

Dog 타입이 Animal 타입을 확장한다는 것 자체가 Dog 타입은 Animal 타입의 서브 타입이 된다는 것을 의미하므로, 확장할 때에도 그에 맞게 해 줘야 한다.

## 인터페이스의 선언 합침

타입 별칭은 동일한 스코프 내에 중복된 이름으로 선언할 수 없는 반면, 인터페이스는 중복된 이름으로 선언할 수 있다. 이렇게 되는 이유는 중복된 이름의 인터페이스 선언이 하나로 합쳐지기 때문이다. 이를 **선언 합침**이라고 한다.

```tsx
interface Person {
  name: string;
}

interface Person {
  age: number;
}

const person: Person = {
  name: "mina",
  age: 23,
};
```

주의할 것은 동일한 이름의 인터페이스들이 동일한 이름의 프로퍼티를 서로 다른 타입으로 정의할 수 없다는 것이다.

이렇게 동일한 프로퍼티의 타입을 다르게 정의한 상황을 **충돌**이라고 표현하며, 선언 합침에서 충돌은 허용되지 않는다.

```tsx
interface Person {
  name: string;
}

interface Person {
  name: number; // x
  age: number;
}
```

인터페이스는 타입 별칭과 기능이 비슷하지만 상속, 합침과 같은 더 많은 기능을 제공하므로 객체의 타입을 지정해야 할 때에는 타입 별칭 대신 인터페이스를 쓰는 것이 좋다.
