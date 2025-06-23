# 타입 별칭과 인덱스 시그니처

# 타입 별칭(Type Alias)

타입 별칭을 이용하면 변수를 선언하듯 타입을 별도로 정의할 수 있다.

```tsx
type User = {
	id: number;
	name: string;
	nickname: string;
	birth: string;
	...
};
```

`type 타입이름 = 타입` 형태로 타입을 정의한다.

여러 개의 객체를 정의할 때, 똑같은 타입을 여러 번 쓰면 코드 중복이 길어지므로 타입 별칭을 이용해서 중복 코드를 줄일 수 있다.

```tsx
type User = {
  id: number;
  name: string;
  nickname: string;
  birth: string;
  bio: string;
  location: string;
};

let user: User = {
  id: 1,
  name: "mina",
  nickname: "gom",
  birth: "2003.12.17",
  bio: "안녕하세요",
  location: "seoul",
};

let user2: User = {
  id: 2,
  name: "홍길동",
  nickname: "gildong",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "busan",
};
```

동일한 스코프에 동일한 이름의 타입 별칭을 선언할 수는 없다. 다만 스코프가 다르다면 중복된 이름으로 여러 개의 별칭을 선언할 수 있다.

타입 관련 문법은 컴파일과 함께 모두 사라지므로, 타입 별칭 또한 tsc로 컴파일하면 자바스크립트 코드에서는 모두 사라진다.

# 인덱스 시그니처

인덱스 시그니처를 이용하면 객체 타입을 유연하게 정의할 수 있다.

```tsx
type CountryCodes = {
	[key: string] : string;
};

let countryCodes: CountryCodes = {
	Korea: 'ko',
	UnitedStates: 'us',
	...
};
```

`[key: string] : string` 을 이용하면 key가 string 타입이고 value가 string 타입인 모든 프로퍼티를 포함할 수 있다.

타입을 정의할 때 매우 많은 프로퍼티를 연속해서 작성해야 할 때, 코드 길이가 매우 길어질 수 있기 때문에 인덱스 시그니처를 사용한다.

```tsx
type CountryNumberCodes = {
  [key: string]: number;
  Korea: number;
  // Korea: string; 오류오류!!
};
```

다음과 같이 반드시 포함해야 하는 프로퍼티가 있다면 명시할 수 있다.

주의해야 할 것은 인덱스 시그니처와 추가적인 프로퍼티를 동시에 정의할 때, 둘의 value 타입이 호환되어야 한다는 것이다.
