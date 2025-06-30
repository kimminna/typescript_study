# 조건부 타입

조건부 타입은 **extends와 삼항 연산자를 이용해서 조건에 따라 각각 다른 타입을 정의**하도록 돕는 문법이다.

`type A = number extends string ? number : string;`

```tsx
type ObjA = {
  a: number;
};

type ObjB = {
  a: number;
  b: number;
};

type B = ObjB extends ObjA ? number : string; // number 타입이 된다.
```

예시로, 위 코드에서는 ObjB가 ObjA의 서브 타입이므로 조건식이 참이 되어 B는 number 타입이 된다.

# 제네릭 조건부 타입

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

let varA: StringNumberSwitch<number>;
// string 타입

let varB: StringNumberSwitch<string>;
```

- varA는 T 타입 변수에 number를 할당한다. 조건식이 참이 되므로 string이 반환됨.
- varB는 T 타입 변수에 string을 할당한다. 조건식이 거짓이 되므로 number가 반환됨.

# 분산적인 조건부 타입

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

let c: StringNumberSwitch<number | string>;
// string | number 유니온 타입
```

**조건부 타입의 타입 변수에 Union 타입을 할당하면 분산적인 조건부 타입**으로 된다.

다음과 같은 순서로 동작한다.

- 타입 변수에 할당한 Union 타입 내부의 모든 타입이 분리된다.
  - StringNumberSwitch<number>
  - StringNumberSwitch<string>
- 분산된 각 타입의 결과를 모아서 다시 Union 타입으로 묶는다.
  - 결과: number | string

```tsx
// Union 타입으로부터 특정 타입만 제거하는 타입을 정의하기

type Exclude<T, U> = T extends U ? never : T;

type A = Exclude<number | string | boolean, string>;
// number | boolean
```

위 코드는 다음과 같은 순서로 동작한다.

- Union 타입이 분리된다.
  - Exclude<number, string>
  - Exclude<string, string>
  - Exclude<boolean, string>
- 분산된 타입의 결과를 Union으로 묶는다.
  - 결과: number | boolean | never
    공집합을 의미하는 never 타입은 합집합으로 묶일 경우 사라짐.
    최종적인 결과는 number | boolean

# infer

**infer는 조건부 타입 내에서 특정 타입을 추론하는 문법**이다.

```tsx
type ReturnType<T> = T extends () => infer R ? R : never;

type FuncA = () => string;
type FuncB = () => boolean;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// boolean

type C = ReturnType<number>;
// never
```

`T extends () ⇒ infer R` 에서 `infer R` 은 이 조건식을 참이 되도록 만들 수 있는 최적의 R타입을 추론하라는 의미다.
