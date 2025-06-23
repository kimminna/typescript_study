# enum

# 열거형(enum) 타입

열거형 타입은 자바스크립트에는 존재하지 않고, 오직 타입스크립트에서만 사용할 수 있는 특별한 타입이다.

다음과 같이 여러 개의 값을 나열하는 용도로 사용한다. 각 객체에 공통으로 사용되는 권한에 대한 값을 숫자로 표현한다고 치면, ADMIN=0, USER=1… 과 같이 미리 정의해서 직관적으로 관리할 수 있게 된다.

```tsx
enum Role {
  ADMIN,
  USER,
  GUEST,
}

const user1 = {
  name: "mina",
  role: Role.ADMIN,
};
```

enum의 각 멤버에는 값을 할당할 수 있다. 만약 아무것도 할당하지 않았다면 기본적으로 0부터 차례대로 값을 가진다.

```tsx
enum Role {
  ADMIN = 10,
  USER,
  GUEST,
}
```

다음과 같이 첫 번째 멤버의 값을 10으로 할당했다면 다음 멤버들의 값은 11, 12로 할당된다. (자동으로 1씩 증가되어 할당)

## 문자열 열거형

enum의 멤버는 숫자 말고 문자열 값도 할당할 수 있다.

```tsx
enum Language {
  korean = "ko",
  english = "en",
}

const user1 = {
  name: "mina",
  language: Language.korean,
};
```

# any와 unknown

# any 타입

any는 타입스크립트에서만 제공되는 특별한 타입으로, **타입 검사를 받지 않는** 특수한 타입이다.

만약 범용적으로 사용되어야 하는 변수가 있다면,

any라는 타입을 이용해서 어떤 타입이든 값으로 할당할 수 있게 정의할 수 있다.

```tsx
let anyVar: any = 10;
anyVar = "hello";

anyVar = true;
anyVar = {};
anyVar.toUpperCase();
anyVar.toFixed();
anyVar.a;

let num: number = 10;
num = anyVar;
```

any는 어떠한 타입 검사도 받지 않기 때문에 아무 타입의 값이나 범용적으로 담아서 사용할 수 있고, 특정 타입의 메서드도 마음대로 호출해서 사용할 수 있다. 또한 특정 타입으로 정의된 변수도 any 타입의 변수에 할당할 수 있다.

하지만 any 타입은 타입 검사를 받지 않기 때문에 타입스크립트를 사용하는 이유가 사라진다. 따라서 정말 어쩔 수 없는 경우가 아니고서는 any 타입을 사용하지 않는 것이 좋다.

# unknown 타입

unknown은 any와 비슷하지만 조금 더 안전한 타입이다.

마찬가지로 어떤 타입의 값이든 다 저장할 수 있다.

하지만 unknown으로 지정된 변수는 어떤 타입의 변수에도 저장할 수 없다.

```tsx
let unknownVar: unknown;

let num: number = 10;
num = unknownVar; // 오류오류!
```

또한 unknown 타입은 연산을 할 수 없고, 어떠한 메서드도 사용할 수 없다.

만약 unknown 타입의 값을 number 타입의 값처럼 취급하고, 곱셈 연산을 수행하게 하고 싶다면 조건문을 이용해서 그 값이 number 타입임을 보장해 줘야 한다.

```tsx
if (typeof unknownVar === "number") {
  unknownVar * 2;
}
```

# void와 never

# void

void는 아무런 값도 없음을 의미하는 타입이다.

다음과 같이 **아무런 값도 반환하지 않는 함수의 반환값 타입을 정의**할 때 주로 사용한다.

```tsx
function func2(): void {
  console.log("hello");
}
```

변수의 타입으로도 void 타입을 지정할 수 있다. 그러나 **void 타입의 변수에는 undefined 이외의 다른 타입의 값은 담을 수 없다.**

- 만약 tsconfig.json의 strictNullChecks 옵션을 해제하면 특별히 void 타입 변수에 null 값을 담을 수 있다.

# never

never는 불가능을 의미하는 타입이다.

다음과 같이 함수가 **어떠한 값도 반환할 수 없는 상황일 때 해당 함수의 반환값 타입을 정의**할 때 사용한다.

```tsx
function func3(): never {
  while (true) {}
}

function func4(): never {
  throw new Error();
}
```

변수의 타입을 never로 지정하면 any를 포함해서 그 **어떠한 타입의 변수도 이 변수에 담을 수 없게 된다.**

- tsconfig.json의 strictNullChecks 옵션을 해제해도 never 타입의 변수에는 null을 할당할 수 없다.
