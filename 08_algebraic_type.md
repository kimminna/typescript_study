# 대수 타입

대수 타입이란 여러 개의 타입을 합성해서 만드는 타입이다.

대수 타입에는 합집합 타입(Union), 교집합 타입(Intersection)이 있다.

# 합집합 타입(Union)

## 기본 정의

```tsx
let a: string | number;

a = 1;
a = "hello";
```

유니온 타입에 참여하는 타입들의 개수에는 제한이 없다.

## 배열 타입 정의하기

```tsx
let arr: (number | string | boolean)[] = [1, 'hello', true];
```

## 객체 타입 정의하기

```tsx
type Dog = {
	name: string;
	color: string;
};

type Person = {
	name: string;
	language: string;
};

type Union1 = Dog | Person;
```

Union1 타입에는 Dog 타입과 Person 타입의 합집합을 포함한다. 

name 프로퍼티만 존재하는 객체는 Union1 타입에 해당하지 않는다. 

# 교집합 타입(Intersection)

## 기본 정의

```tsx
let variable: number & string;
```

number 타입과 string 타입은 서로소 집합이므로 변수 variable의 타입은 never 타입으로 추론된다.

대다수의 기본 타입들 간은 서로소 집합이기 때문에 인터섹션 타입은 보통 객체 타입들에 자주 사용된다.

## 객체 타입 정의하기

```tsx
type Dog = {
	name: string;
	color: string;
};

type Person = {
	name: string;
	language: string;
};

type Intersection = Dog & Person;

let intersec: Intersection = {
	name: "",
	color: "",
	language: "",
};
```

Dog 타입과 Person 타입의 교집합이 되는, name color language 프로퍼티들을 가지는 객체만 Intersection이 된다.