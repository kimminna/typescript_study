# 타입 단언

# 타입 단언이란?

```tsx
type Person = {
  name: string;
  age: number;
};

let person: Person = {};
person.name = "";
person.age = 23;
```

다음 코드와 같이 변수 person을 Person 타입으로 초기화할 때에는 빈 객체를 넣어두고, 그 후에 프로퍼티를 추가하고 싶은 경우 타입스크립트에서는 오류를 발생시킨다.

빈 객체를 허용하고 싶은 경우 타입 단언을 해 줘야 한다.

```tsx
type Person = {
  name: string;
  age: number;
};

let person = {} as Person;
person.name = "";
person.age = 23;
```

`값 as 타입` 으로 특정 값을 원하는 타입으로 단언할 수 있다. 이를 타입 단언이라고 한다.

타입스크립트의 초과 프로퍼티 검사를 피하기 위해서도 사용할 수 있다.

```tsx
type Dog = {
  name: string;
  color: string;
};

let dog: Dog = {
  name: "mina",
  color: "brown",
  breed: "person",
} as Dog;
```

## 타입 단언의 조건

**A as B**

1. A가 B의 슈퍼 타입이어야 한다.
2. A가 B의 서브 타입이어야 한다.

```tsx
let num1 = 10 as never;
let num2 = 10 as unknown;

let num3 = 10 as string; // 오류 발생
```

never은 모든 타입의 서브 타입이고, unknown 타입은 모든 타입의 슈퍼 타입이므로 단언이 가능하다.

하지만 number 타입과 string 타입은 서로소 관계를 가지므로 단언이 불가능하다.

## 다중 단언

타입 단언은 다중으로도 가능하다.

```tsx
let num3 = 10 as unknown as string;
```

다중 단언의 경우 왼→오 순으로 단언이 이루어진다.

1. number 타입의 값을 unknown으로 단언.
2. unknown 타입의 값을 string으로 단언.

이렇게 중간에 unknown으로 타입 단언을 하면 어떤 타입이든 단언이 가능해진다. (권장 x)

## const 단언

타입 단언에만 가능한 const 타입이 존재한다. 특정 값을 const로 단언하면 마치 변수를 const로 선언한 것과 비슷하게 타입이 변경된다.

```tsx
let num4 = 10 as const;
// 10 Number Literal 타입으로 단언

let cat = {
  name: "cat",
  color: "yellow",
} as const; // 모든 프로퍼티가 readonly를 갖도록 단언
```

## Non Null 단언

```tsx
type Post = {
  title: string;
  author?: string;
};

let post: Post = {
  title: "post",
};

const len: number = post.author!.length;
```

값 뒤에 !를 붙여 주면 이 값이 undefined이거나 null이 아닐 것이라고 단언할 수 있다.

타입이 확실할 때 경고 메시지를 방지하거나, api를 사용하기 위해 타입 단언을 사용한다. (타입 단언 하지 않으면 null 일 수도 있다는 경고 메시지 표시됨.)
