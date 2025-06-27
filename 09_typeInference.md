# 타입 추론

# 타입 추론이란?

타입스크립트는 타입이 정의되어 있지 않은 변수의 타입을 자동으로 추론한다. 따라서 프로그래머에게 모든 변수에 일일이 타입을 정의하지 않아도 되는 편리함을 제공한다.

```tsx
let a = 10;
// 자동으로 number 타입으로 추론합니다.

function func(param) {}
// 오류 발생
```

함수의 매개변수 타입은 자동으로 추론할 수 없다. 타입 추론이 불가능한 변수에는 암시적으로 any 타입으로 추론되는데, 엄격한 타입 검사 모드에서는 이러한 암시적 any 타입의 추론을 오류라고 판단한다.

## 타입 추론이 가능한 상황

1. 변수 선언

   일반적으로 변수 선언의 경우에는 초기값을 기준으로 타입이 잘 추론된다.

   ```tsx
   let a = 10;
   let b = "string";
   let c = {
     id: 1,
     name: "mina",
     profile: {
       nickname: "gom",
     },
   }; // id, name, profile이 있는 객체 타입으로 추론
   ```

2. 구조 분해 할당

   객체와 배열을 구조 분해 할당 하는 상황에서도 타입이 잘 추론된다.

   ```tsx
   let { id, name, profile } = c;
   let [a, b, c] = [1, "true", true];
   ```

3. 함수의 반환 값

   함수 반환 값의 타입은 return문을 기준으로 잘 추론된다.

   ```tsx
   function func() {
     return "hello";
   } // 반환 값이 자동으로 string으로 추론됨.
   ```

4. 기본 값이 설정된 매개변수

   기본 값이 설정된 매개변수의 타입은 기본 값을 기준으로 타입이 잘 추론된다.

   ```tsx
   function func(message = "string") {
     return "hello";
   }
   ```

## 주의해야 할 상황들

1. 암시적으로 any 타입으로 추론되는 경우

   변수를 선언할 때 초기 값을 생략하면 변수는 암시적인 any 타입으로 추론된다.

   함수의 매개변수 타입이 any로 추론될 때와는 달리 일반 변수의 타입이 any로 추론되는 경우에는 오류로 판단하진 않는다.

   그 다음 변수에 값을 할당하면 자동으로 any 타입에서 해당 값의 타입으로 변화한다. - **any 진화**

   ```tsx
   let a;

   a = 10;
   a.toFixed(); // a -> number

   a = "string";
   a.toUpperCase(); // a -> string

   a.toFixed(); // 오류 발생.
   ```

2. const 상수의 추론

   let이 아닌 const로 선언된 상수는 let과는 다른 방식으로 타입이 추론된다.

   ```tsx
   const num = 10;
   // 10 Number Literal 타입으로 추론됨.

   const str = "hello";
   // hello String Literal 타입으로 추론됨.
   ```

   상수는 초기화 때 설정한 값을 변경할 수 없으므로 가장 좁은 타입으로 추론되는 것이다.

## 최적 공통 타입

다양한 타입의 요소를 담은 배열을 변수의 초기값으로 설정하면, 가장 최적의 공통 타입으로 추론한다.

```tsx
let arr = [1, "string", true];
// (number | string | boolean)[] 타입으로 추론됨.
```
