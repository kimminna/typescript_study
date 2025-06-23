# 타입

![Untitled.webp](attachment:64a68b30-fcd8-408d-a3ed-0220ebbdd070:Untitled.webp)

# 원시 타입

원시 타입은 동시에 한 개의 값만 저장할 수 있는 타입.

예를 들어 배열이나 객체 같은 비 원시 타입들은 동시에 여러 개의 값들을 저장할 수 있는 반면에, number, string, boolean 등의 원시 타입들은 숫자면 숫자, 문자열이면 문자열 값만 저장할 수 있다.

## number 타입

```tsx
let num1: number = 123;

num1 = "hello"; // 오류
num1.toUpperCase(); // 오류
```

number 타입은 자바스크립트에서 숫자를 의미하는 모든 값을 포함한다. 정수, 소수, 음수, infinity, NaN 등의 특수한 숫자들도 포함한다.

변수의 이름 뒤에 콜론과 함께 변수의 타입을 number로 지정한다. 이런 문법을 ‘타입 주석’ 혹은 ‘타입 어노테이션’이라고 부른다.

number 타입으로 정의된 변수에 다른 타입의 값을 할당하거나, 배열 메서드 등을 사용할 수 없다.

## string 타입

```tsx
let str1: string = "hello";
let str2: string = `hello2`;
let str3: string = "hello";
let str4: string = `hello${str1}`;
```

단순 쌍따옴표 문자열뿐만 아니라 작은 따옴표, 백틱, 템플릿 리터럴로 만든 문자열도 포함한다.

## boolean 타입

```tsx
let boo1: boolean = true;
let boo2: boolean = false;
```

## null 타입

```tsx
let null1: null = null;
```

null 타입은 오직 null 값만 포함한다.

## undefined 타입

```tsx
let unde1: undefined = undefined;
```

undefined 타입은 오직 undefined 값만 포함한다.

### null 값을 다른 타입의 변수에 할당하기

자바스크립트에서 아직 값이 정해지지 않은 상태에 대해 변수에 null을 임시로 넣어 둘 수 있었다.

그러나 타입스크립트는 타입과 맞는 값만 할당해야 오류가 발생하지 않기 때문에

`let numA: number = null;` 과 같은 코드는 사용할 수 없을 것이다.

null 값을 변수의 임시 값으로 활용하고 싶은 경우, tsconfig.json 파일의 strictNullChecks 옵션을 false 로 설정해 둬야 한다.

```tsx
{
  "compilerOptions": {
    ...
    "strictNullChecks": false,
		...
  },
  "ts-node": {
    "esm": true
  },
  "include": ["src"]
}

```

## 리터럴 타입

```tsx
let numA: 10 = 10;
let strA: "hello" = "hello";
let booA: true = true;
```

string, number 타입처럼 범용적으로 많은 값을 포함하는 타입뿐만 아니라 딱 하나의 값만 포함하는 타입도 존재한다.
