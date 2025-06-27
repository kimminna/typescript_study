# 타입 좁히기

```tsx
function func(value: number | string) {
  value.toFixed();
  value.toUpperCase(); // 둘 다 오류
}
```

다음과 같이 매개변수 value 타입이 number | string 이므로 함수 내부에서 관련 타입 메서드를 사용하려고 하면 오류가 발생한다.

만약 변수가 number 타입일 것을 기대하고 숫자 관련 메서드를 사용하고 싶다면 조건문을 이용해서 value의 타입이 number임을 보장해 줘야 한다.

string 타입도 마찬가지로 조건문을 이용해 타입을 보장해 줘야 한다.

```tsx
function func(value: number | string) {
	if (typeof value === 'number'){
		//
	}

	if (typeof value === 'stirng')
		//
	}
};
```

조건문을 이용해서 조건문 내부의 변수가 특정 타입임을 보장하면 해당 조건문 내부에서는 변수의 타입이 보장된 타입으로 좁혀진다. 이를 **타입 좁히기**라고 한다.

`if(typeof===)` 처럼 조건문을 사용해서 타입을 좁히는 표현을 **타입 가드**라고 한다.

## insanceof 타입 가드

내장 클래스 타입을 보장하기 위해 instanceof 타입 가드를 사용한다.

```tsx
function func(value: number | string | Date | null) {
  if (typeof value === "number") {
  } else if (typeof value === "string") {
  } else if (value instanceof Date) {
  }
}
```

## in 타입 가드

프로그래머가 생성한 타입을 좁히기 위해서는 in 연산자를 이용한다.

```tsx
type Person = {
  name: string;
  age: number;
};

function func(value: number | string | Date | null | Person) {
  if (typeof value === "number") {
  } else if (typeof value === "string") {
  } else if (value instanceof Date) {
  } else if (value && "age" in value) {
  }
}
```

value가 null이 아님을 확인하고, value 안에 age라는 프로퍼티가 있는지 확인한다.
