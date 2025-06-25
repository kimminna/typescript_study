# 타입 호환성

# 타입은 집합이다

타입스크립트의 타입은 여러 개의 값을 포함하는 집합이라고 할 수 있다. 집합은 동일한 속성을 갖는 여러 개의 요소들을 하나의 그룹으로 묶은 단위를 말한다.

모든 타입들은 집합으로써 서로 포함하고, 또 포함되는 관계를 갖는다. 예를 들어 Number 타입은 부모 타입으로써 Number literal 타입을 포함한다. 여기서 Number literal 타입은 자식 타입이라고 할 수 있다.

타입 계층도에서 화살표로 표시된 부분은 부모-자식 관계를 포함한다는 뜻이다.

![í__ì__ê³_ì¸µë__.webp](attachment:ef3d9940-9701-452f-8904-ebc18bbc921b:_______.webp)

# 타입 호환성

타입 호환성이란 A 타입의 값을 B 타입으로 취급해도 괜찮을지에 대한 판단을 의미한다. 만약 A 타입이 B 타입으로 취급되어도 괜찮다면 호환이 가능하다고 하고, 불가능하다면 호환되지 않는다고 한다.

예를 들어 Number 타입과 Number literal 타입이 있을 때 서브 타입인 Number literal 타입을 Number 타입으로 취급하는 것은 가능하다. 하지만 반대는 불가능하다.

```tsx
let num1: number = 10;
let num2: 10 = 10;

num1 = num2; // 가능
num2 = num1; // 오류
```

타입스크립트에서는 슈퍼 타입의 값을 서브 타입으로 취급하는 것을 허용하지 않는다.

- 업 캐스팅
  서브 타입을 슈퍼 타입의 값으로 취급하는 것.
  → 타입스크립트에서는 모든 상황에서 가능하다.
- 다운 캐스팅
  슈퍼 타입을 서브 타입의 값으로 취급하는 것.
  → 타입스크립트에서는 대부분의 상황에서 불가능하다.

## unknown 타입은 전체 집합

unknown 타입은 타입 계층도의 최상단에 위치한다.

따라서 unknown 타입 변수에는 모든 타입의 값을 할당할 수 있다. (모든 타입은 unknown으로의 업 캐스팅이 가능하다.)

반대로 unknown 타입의 값은 any 타입을 제외한 어떤 타입의 변수에도 할당할 수 없다. (unknown 타입은 any를 제외한 어떤 타입에도 다운 캐스팅 될 수 없다.)

![Untitled.webp](attachment:2ee7d10f-1866-468e-9578-60ebb9e80c39:Untitled.webp)

```tsx
let a: unknown = 1;
let b: unknown = 'string';
let c: unknown = true;
...

```

## never 타입은 공집합

never 타입은 타입 계층도에서 가장 아래에 위치한다.

never 타입은 어떤 값도 할당할 수 없는 타입이다. 따라서 공집합이라고 할 수 있다.

공집합은 모든 집합의 부분집합이므로 never 타입은 모든 타입의 서브 타입이다. 따라서 never 타입은 모든 타입으로 업 캐스팅이 가능하다. (하지만 그 어떤 타입도 never 타입으로 다운 캐스팅 될 수 없다.)

```tsx
let neverVar: never;

let a: number = neverVar;
let b: string = neverVar;
let c: boolean = neverVar;

..
```

## void 타입은 undefined 타입의 슈퍼 타입

void 타입은 아무것도 반환하지 않는 함수의 반환 값 타입으로 주로 사용된다. 타입 계층도에서 void 타입은 undefined 타입의 슈퍼 타입이다. 따라서 반환 값을 void로 선언한 함수에서 undefined를 반환해도 오류가 발생하지 않는다. (undefined 타입은 void 타입으로의 업 캐스팅이 가능하기 때문이다.)

또한 void 타입의 서브 타입으로는 undefined와 never 타입밖에 없으므로 void 타입에는 그 외의 값을 할당할 수 없다.

```tsx
function noReturnFuncA(): void {
  return undefined;
}

let voidVar: void;
voidVar = undefined;

let neverVar: never;
voidVar = neverVar;
```

## any 타입은 모든 타입으로의 업, 다운 캐스팅이 가능하다

any 타입은 타입 계층도를 완전히 무시한다. 모든 타입의 슈퍼 타입이 될 수 있고, 모든 타입의 서브 타입이 될 수도 있다.

```tsx
let anyVar: any;

let num: number = anyVar;
let str: string = anyVar;

anyVar = num;
anyVar = str;
```

## 객체 타입의 호환성

모든 객체 타입은 각각 다른 객체 타입들과 슈퍼-서브 타입 관계를 갖는다. 따라서 업 캐스팅은 허용되고 다운 캐스팅은 허용되지 않는다.

```tsx
type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

let animalObj: Animal = {
  name: "미나",
  color: "brown",
};

let dogObj: Dog = {
  name: "강아지",
  color: "black",
  breed: "포메",
};

animalObj = dogObj; // 가능
dogObj = animalObj; // 불가능
```

타입스크립트는 프로퍼티를 기준으로 타입을 정의하는 구조적 타입 시스템을 따른다. 따라서 Animal 타입은 name과 color 프로퍼티를 갖는 모든 객체들을 포함하는 집합으로 볼 수 있고, Dog 타입은 name, color, breed 프로퍼티를 갖는 모든 객체를 포함하는 집합으로 볼 수 있다.

따라서 어떤 객체가 Dog 타입이라면 Animal 타입에도 포함된다고 볼 수 있다. (Animal 타입은 Dog 타입의 슈퍼 타입이다.)

### 초과 프로퍼티 검사

```tsx
type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

let animal1: Animal = {
  name: "미나",
  color: "brown",
  breed: "사람",
}; // 오류
```

Animal 타입으로 정의된 변수에 Dog 타입의 초기 값을 설정하면 초과 프로퍼티 검사 때문에 오류가 발생한다.

초과 프로퍼티 검사란 변수를 객체 리터럴로 초기화할 때 발동되는 타입스크립트의 특수한 기능이다. 타입에 정의된 프로퍼티 외의 다른 초과된 프로퍼티를 갖는 객체를 변수에 할당할 수 없도록 막는다. 이는 단순히 변수를 초기화할 때, 객체 리터럴을 사용하지만 않으면 발생하지 않는다.

```tsx
let animal1: Animal = Dog; // 가능

function func(animal: Animal) {}

func({
  name: "미나",
  color: "brown",
  breed: "사람",
}); // 오류

func(dogObj); // 가능
```

함수의 매개변수에 인수로 값을 전달하는 과정도 변수를 초기화하는 과정과 동일하게 초과 프로퍼티 검사가 발동된다. 다만 변수에 미리 값을 담아둔 다음 변수 값을 인수로 전달하면 오류가 발생하지 않는다.
