# 타입 조작하기

타입 조작이란 기본 타입이나 별칭 또는 인터페이스로 만든 원래 존재하던 타입들을 상황에 따라 유동적으로 다른 타입으로 변환할 수 있는 기능을 말한다.

제네릭도 마찬가지로 인터페이스나 타입 별칭, 클래스 등에 적용해서 상황에 따라 달라지는 가변적인 타입을 정의할 수 있기 때문에 타입을 조작하는 기능에 포함된다.

## 종류

- 제네릭
- 인덱스드 엑세스 타입
- keyof 연산자
- Mapped 타입
- 템플릿 리터럴 타입
- 조건부 타입

# 인덱스드 엑세스 타입

인덱스드 엑세스 **타입은 인덱스를 이용해서 다른 타입 내의 특정 프로퍼티의 타입을 추출**하는 타입이다. 객체, 배열, 튜플에 사용할 수 있다.

## 객체 프로퍼티의 타입 추출

```tsx
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}

function printAuthorInfo(author: Post["author"]) {
  console.log(`${author.id} - ${author.name}`);
}
```

`Post[’author’]` 는 Post 타입으로부터 author 프로퍼티의 타입을 추출한다. 그 결과 author 매개변수의 타입은 `{id: number, name: string; age: number}` 가 된다.

이때 대괄호 속에 들어가는 String Literal 타입인 ‘author’ 를 인덱스라고 부른다. 인덱스를 이용해서 특정 타입에 접근한다고 하여 인덱스드 엑세스라고 하는 것이다.

인덱스에는 값이 아니라 타입만 들어갈 수 있다. 따라서 ‘author’를 문자열 값으로 다른 변수에 저장하고, 변수를 인덱스로 사용하려고 하면 오류가 발생한다.

```tsx
function printAuthorInfo(author: Post["whia"]) {} // x

function printAuthorInfo(authorId: Post["author"]["id"]) {
  console.log(authorId);
}
```

인덱스에 존재하지 않는 프로퍼티 이름을 쓰면 오류가 발생한다.

또한, 인덱스를 중첩하여 타입을 추출할 수도 있다.

## 배열 요소의 타입 추출하기

```tsx
type PostList = {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}[];

const post: PostList[number] = {
  title: "title",
  content: "content",
  author: {
    id: 1,
    name: "name",
    age: 23,
  },
};
```

배열의 요소 타입을 추출할 때에는 인덱스에 number 타입을 넣어주면 된다.

혹은 인덱스에 Number Literal 타입을 넣어도 된다.

## 튜플의 요소 타입 추출하기

```tsx
type Tup = [number, string, boolean];

type Tup0 = Tup[0];
// number

type Tup1 = Tup[1];
// string

type Tup2 = Tup[2];
// boolean

type Tup3 = Tup[number];
// number | string | boolean
```

튜플 타입에 인덱스드 엑세스 타입을 사용할 때 인덱스에 number 타입을 넣으면 마치 튜플을 배열처럼 인식하여 배열 요소의 타입을 추출하게 된다.

# keyof 연산자

keyof 연산자는 **객체 타입으로부터 프로퍼티의 모든 key들을 String Literal Union 타입으로 추출**하는 연산자다.

```tsx
interface Person {
  name: string;
  age: number;
}

function getPropertyKey(person: Person, key: keyof Person) {
  return person[key];
}

const person: Person = {
  name: "mina",
  age: 23,
};
```

`keyof 타입` 형태로 사용하며 타입의 모든 프로퍼티 key를 String Literal Union 타입으로 추출한다. 따라서 `keyof Person` 의 결과값은 `‘name’ | ‘age’` 가 된다.

주의할 점은 keyof 연산자는 오직 타입에만 적용할 수 있는 연산자라는 것이다. 따라서 만약 객체 타입의 key를 뽑기 위해 사용한다면 오류가 발생할 것이다.

## typeof와 함께 사용하기

typeof 연산자를 사용하면 특정 변수의 타입을 추론할 수 있다. 이를 keyof 연산자와 함께 사용할 수 있다.

```tsx
function getPropertyKey(person: Person, key: keyof typeof person) {
  return person[key];
}

const person: Person = {
  name: "mina",
  age: 23,
};
```

`typeof person`에서 `{name: ‘string’, age: ‘number}` 타입을 추론하고, 이를 `keyof`를 이용해서 key 타입만 추출해서 사용할 수 있다.

# Mapped 타입

맵드는 **기존의 객체 타입을 기반으로 새로운 객체 타입을 만드는 기능**이다.

```tsx
interface User {
  id: number;
  name: string;
  age: number;
}

type PartialUser = {
  [key in "id" | "name" | "age"]?: User[key];
};
```

`[key in 'id' | 'name' | 'age']` 는 key가 한 번은 id, 한 번은 name, 한 번은 age가 된다는 뜻이다. 따라서 3개의 프로퍼티를 갖는 객체 타입으로 정의된다.

대괄호 뒤에 선택적 프로퍼티를 의미하는 물음표 키워드가 붙어있으므로 모든 프로퍼티는 선택적 프로퍼티가 된다.

`keyof` 연산자를 이용하여 사용할 수도 있다.

```tsx
type PartialUser = {
  [key in keyof User]?: User[key];
};
```

주의해야 할 점은 Mapped는 **타입에만 적용**할 수 있다는 것이다.

# 템플릿 리터럴 타입

템플릿 리터럴을 이용해 특정 패턴을 갖는 String 타입을 만들 수 있다.

```tsx
type Color = "red" | "black" | "green";
type Animal = "dog" | "cat" | "chicken";

type ColoredAnimal = `${Color}-${Animal}`; // 'red-dog' 'red-cat' ...
```

Color 나 Animal 타입에 String Literal 타입이 추가되어 경우의 수가 많아질수록 코드 작성이 복잡해지는데, 이럴 때 템플릿 리터럴 타입을 이용하면 좋다.
