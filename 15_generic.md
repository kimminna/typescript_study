# 제네릭

제네릭이란 함수나 인터페이스, 타입 별칭, 클래스 등을 다양한 타입과 함께 동작하도록 만들어 주는 기능이다.

## 제네릭이 필요한 상황

다양한 타입의 매개변수를 받고, 반환하는 값의 타입을 의도한 타입에 맞게 반환하고 싶을 때 제네릭을 사용한다.

any나 unknown 타입을 이용해서 다양한 타입의 매개변수를 받으면 return하는 값의 타입이 그대로 any, unknown이 되는 문제점이 있기 때문이다.

# 제네릭 함수

다음과 같이 제네릭 함수를 선언한다.

```tsx
function func<T>(value: T) : T{
	return value;
}

let num = func(10);
// 변수 num은 number로 추론된다.
```

T는 **타입 변수**라고 한다. T에 어떤 타입이 할당될지는 함수가 호출될 때 결정된다. 

`func(10)` 과 같이 Number 타입의 값을 인수로 전달하면 매개변수 value에 Number 타입의 값이 저장되면서 T가 Number 타입으로 추론된다. 따라서 func 함수의 반환값 타입 또한 Number 타입이 된다. 

![Untitled.webp](attachment:860104ed-4d02-423e-b721-c7a2be93f6a7:Untitled.webp)

다음과 같이 타입 변수에 할당할 타입을 직접 명시할 수도 있다.

```tsx
function func<T>(value: T) : T {
	return value;
}

let arr = func<[number, number, number]>([1, 2, 3]);
```

타입 변수 T에는 [Number, Number, Number] 튜플 타입이 할당된다. 따라서 매개변수 value와 반환 값 타입 모두 튜플 타입이 된다.

만약 타입 변수에 할당할 타입을 튜플로 설정하지 않았다면 T는 number[] 타입으로 추론될 것이다. 타입스크립트는 타입을 추론할 때 항상 범용적인 타입으로 추론하기 때문에, 특정 타입으로 할당하고 싶다면 직접 명시해 주는 것이 좋다.

## 타입 변수의 응용

1. 여러 개의 타입 변수를 사용할 수 있다.
    
    ```tsx
    function swap<T, U>(a:T, b:U) {
    	return [b, a];
    }
    
    const [a, b] = swap('1', 2);
    
    // T는 string, U는 number 타입으로 추론된다.
    ```
    

1. 다양한 배열 타입을 인수로 받는 제네릭 함수
    
    ```tsx
    function returnFirstValue<T>(data: T[]) {
    	return data[0];
    }
    
    let num = returnFirstValue([0, 1, 2]);
    // T는 number
    
    let str = returnFirstValue([1, 'hello', 'mynameis']);
    // T는 number|string
    ```
    

1. 반환 값의 타입이 배열의 첫 번째 타입으로 고정하기
    
    ```tsx
    function returnFirstValue<T>(data: [T, ...unknown[]]) {
    	return data[0];
    }
    
    let str = returnFirstValue([1, 'hello', 'mynameis']);
    // number
    ```
    

1. 타입 변수를 제한하기
    
    ```tsx
    function getLength<T extends {length: number}>(data: T){
    	return data.length;
    }
    
    getLength('123');
    getLength([1, 2, 3]);
    getLength({length:1});
    getLength(undefined); // x
    getLength(null); // x
    ```
    
    - 타입 변수를 제한할 때에는 확장을 이용한다. `T extends {length: number}` 라고 정의하면 T는 Number 타입의 프로퍼티 length를 가지고 있는 타입이 된다.

## 제네릭을 이용해 배열 메서드 구현해 보기

1. Map 메서드 타입 정의
    
    ```tsx
    function map<T, U>(arr: T[], callback: (item: T) => U): U[] {
    	let result = [];
    	for (let i = 0; i < arr.length; i++){
    		result.push(callback(arr[i]);
    	}
    	return result;
    }
    ```
    
    타입 변수를 T, U 두 개 쓰는 이유는 Map 메서드에서는 원본 배열 타입과 다른 타입의 배열로도 변환할 수 있어야 하기 때문이다.
    
2. ForEach 메서드 타입 정의 
    
    ```tsx
    function forEach<T>(arr: T[], callback: (item: T) => void{
    	for (let i = 0; i < arr.length; i++){
    		callback(arr[i]);
    	}
    }
    ```
    

# 제네릭 인터페이스

제네릭은 인터페이스에도 적용할 수 있다.

```tsx
interface KeyPair<K, V> {
	key: K;
	value: V;
}

let keyPair: KeyPair<string, number> = {
	key: 'key',
	value: 0,
};
```

제네릭 인터페이스는 제네릭 함수와는 달리 변수의 타입으로 정의할 때 반드시 꺽쇠와 함께 타입 변수에 할당할 타입을 명시해 주어야 한다. 그 이유는 제네릭 함수는 매개변수에 제공되는 값의 타입을 기준으로 타입 변수를 추론할 수 있지만 인터페이스는 추론할 수 없기 때문이다.

## 인덱스 시그니처와 함께 사용하기

```tsx
interface Map<V> {
	[key: string]: V;
}

let stringMap: Map<string> = {
	key: 'value',
};
```

다음과 같이 인덱스 시그니처와 함께 제네릭 인터페이스를 이용하면 모든 타입의 value를 포함할 수 있다. 기존보다 훨씬 더 유연하게 객체의 타입을 정의할 수 있다. 

# 제네릭 타입 별칭

```tsx
type Map2<V> = {
	[key: string]: V;
}

let stringMap2: Map2<string> = {
	key: 'string',
};
```

제네릭 타입 별칭을 사용할 때에도 제네릭 인터페이스와 마찬가지로 타입으로 정의될 때 반드시 타입 변수에 설정할 타입을 명시해 주어야 한다. 

# 제네릭 클래스

```tsx
class List<T> {
	constructor(private list: T[]) {}
	// ...
}

const numberList = new List<number>([1, 2, 3]);
const stringList = new List(['1', '2']); // T는 string

```

클래스에서도 제네릭을 이용해서 여러 타입의 리스트를 생성하게 정의할 수 있다. 클래스의 이름 뒤에 타입 변수를 선언하면 제네릭 클래스가 된다.

또한, 클래스는 생성자를 통해 타입 변수의 타입을 추론할 수 있기 때문에 생성자에 인수로 전달하는 값이 있을 경우 타입 변수에 할당할 타입을 생략해도 된다.