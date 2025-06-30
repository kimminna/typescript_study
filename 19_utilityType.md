# 유틸리티 타입

유틸리티 타입이란 타입스크립트가 자체적으로 제공하는 특수한 타입이다. 제네릭, 맵드, 조건부 타입 등의 타입 조작 기능을 이용해서 실무에서 자주 사용되는 타입들을 모아 놓은 것이다.

https://www.typescriptlang.org/docs/handbook/utility-types.html

# Partial<T>

**특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환**한다.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thummnailURL?: string;
}

const draft: Partial<Post> = {
  title: "",
  content: "",
};
```

Partial<T>는 타입 변수로 전달한 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환한다. Post 인터페이스의 일부 프로퍼티만 갖고 있는 타입을 정의할 수 있다.

## 직접 구현하기

```tsx
type Partial<T> = {
  [key in keyof T]?: T[key];
};
```

# Required<T>

**특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 변환**한다.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const withThumbnailPost: Required<Post> = {
  title: "",
  tags: [""],
  content: "",
  thumbnailURL: "", // 모두 필수 프로퍼티
};
```

Required<Post>는 Post 타입의 모든 프로퍼티가 필수 프로퍼티로 변환된 객체 타입이다.

## 직접 구현하기

```tsx
type Required<T> = {
  [key in keyof T]-?: T[key];
};
```

-? 는 ?가 붙어있는 선택적 프로퍼티가 있으면 ?를 제거하라는 의미다.

# Readonly<T>

**특정 객체 타입의 모든 프로퍼티를 읽기 전용 프로퍼티로 변환**한다.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const readonlyPost: Readonly<Post> = {
  title: "",
  tags: [""],
  content: "",
};

readonlyPost.content = "a"; // 불가능
```

Readonly<Post>는 Post 타입의 모든 프로퍼티를 읽기 전용으로 변환하므로 점 표기법을 이용해서 특정 프로퍼티의 값을 수정하려고 하면 오류를 발생시킨다.

## 직접 구현하기

```tsx
type Readonly<T> = {
  readonly [key in keyof T]: T[key];
};
```

# Pick<T, K>

**특정 객체 타입으로부터 특정 프로퍼티만을 골라낸다.**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const legacyPost: Pick<Post, "title", "content"> = {
  title: "",
  content: "",
};
```

Post 타입으로부터 title, content 프로퍼티만 뽑아낸 객체 타입이 할당된다.

## 직접 구현하기

```tsx
type Pick<T, K extends keyof T> = {
  [key in K]: T[key];
};
```

# Omit<T, K>

**특정 객체 타입으로부터 특정 프로퍼티만 제거한다.**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const noTitlePost: Omit<Post, "title"> = {
  tags: "",
  content: [],
  thumbnailURL: "",
};
```

Post 타입으로부터 title 프로퍼티만 제거한 객체 타입이 할당된다.

## 직접 구현하기

```tsx
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

Exclude 타입은 T로부터 K를 제거한 타입을 반환한다.

# Record<K, V>

**특정 키 집합과 해당 키에 대한 값의 타입을 정의**할 수 있게 해 준다.

```tsx
type Thumbnail = Record<"large" | "medium" | "small", { url: string }>;

// 다음 타입과 같음.
type Thumbnail = {
  large: {
    url: string;
  };
  medium: {
    url: string;
  };
  small: {
    url: string;
  };
};
```

## 직접 구현하기

```tsx
type Record<K extends of keyof any, V> = {
	[key in K]: V;
}
```

# Exclude<T, K>

**T로부터 K를 제거하는 타입**이다.

## 직접 구현하기

```tsx
type Exclude<T, K> = T extends K ? never : T;
```

# Extract<T, K>

**T로부터 K를 추출하는 타입**이다.

## 직접 구현하기

```tsx
type Extract<T, K> = T extends K ? T : never;
```

# ReturnType<T>

**T에 할당된 함수 타입의 반환 값 타입을 추출**한다.

## 직접 구현하기

```tsx
type ReturnType<T extends (...args: any) => any> = T extends (
  ...agrs: any
) => infer R
  ? R
  : never;
```
