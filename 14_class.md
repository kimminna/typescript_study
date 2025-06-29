# 클래스

# 타입스크립트의 클래스

타입스크립트에서는 클래스의 필드를 선언할 때 타입 주석으로 타입을 함께 정의해 주어야 한다. 그렇지 않으면 함수 매개변수와 동일하게 암시적으로 any 타입으로 추론되는데, 엄격한 타입 검사 모드(strict 옵션이 true로 설정되어 있을 경우)일 때에는 오류가 발생하게 된다.

또한 생성자에서 각 필드의 값을 초기화하지 않는 경우에는 초기값도 함께 명시하여 필드 정의를 해 줘야 한다. 

```tsx
class Employee {
	// 필드 정의
	name: string = '';
	age: number = 0;
	position: string = '';

	// method 정의
	work() {}
}

// 생성자를 통해 필드의 값을 초기화하는 경우

class Employee {
	name: string;
	age: number;
	position: string;
	
	constructor(name: string, age: number, position: string) {
		this.name = name;
		this.age = age;
		this.position = position;
	}
	
	work() {}
}
```

클래스에서도 마찬가지로 클래스가 생성하는 객체의 특정 프로퍼티를 선택적 프로퍼티로 만들고 싶다면 물음표를 붙여서 정의해 주면 된다.

## 클래스는 타입

타입스크립트의 클래스는 타입으로도 사용할 수 있다. 클래스를 타입으로 사용하면 해당 클래스가 생성하는 객체의 타입과 동일한 타입이 된다.

```tsx
const employee: Employee = {
	name: 'mina',
	age: 23,
	position: 'student',
	work() {}
}
```

변수 employee의 타입을 Employee 클래스로 정의하면 변수는 Employee가 생성하는 객체의 타입과 동일한 객체 타입으로 정의된다. 

## 상속

타입스크립트에서 클래스의 상속을 이용할 때 파생 클래스에서 생성자를 정의했다면 반드시 super 메소드를 호출하여 슈퍼 클래스의 생성자를 호출하여야 하며, 호출 위치는 생성자의 최상단이어야 한다.

```tsx
class ExecutiveOfficer extends Employee {
	officeNumber: number;
	
	constructor( 
		name: string,
		age: number,
		position: string,
		officeNumber: number
	) {
		super(name, age, position);
		this.officeNumber = officeNumber;
	}
}
```

상속받은 클래스는 부모 클래스의 프로퍼티를 모두 가진다. 

### 접근 제어자

접근 제어자는 타입스크립트에서만 제공되는 기능으로, 클래스의 특정 필드나 메서드를 접근할 수 있는 범위를 설정하는 기능이다.

다음 3가지의 접근 제어자를 갖는다.

1. public - 모든 범위에서 접근 가능, 접근 제어자를 따로 지정하지 않으면 기본으로 public
2. private - 클래스 내부에서만 접근 가능
3. protected - 클래스 내부 또는 파생 클래스 내부에서만 접근 가능

```tsx
class Employee {
	// 필드
	name: string; // 기본 public
	protected age: number;
	private position: string;
	
	// 생성자
	constructor(name: string, age: number, position: string) {
		this.name = name;
		this.age = age;
		this.position = position;
	}
	
	// method
	work() {
		console.log(`${this.position} 일하는 중`}; // method 에서 내부 private 접근 가능
}

class ExecutiveOfficer extends Employee {
	// ...
	// method
	func() {
		this.name; // 파생 클래스에서 부모 클래스의 private 접근 불가
		this.age; // protected 접근 가능
	}
}

const employee = new Employee('mina', 23, 'student');

employee.name = '홍길동';
employee.age = 30;
employee.position = 'developer'; // 외부에서 클래스의 private 프로퍼티 접근 불가능
```

### 필드 생략하기

접근 제어자는 생성자의 매개변수에도 설정할 수 있는데, 생성자 매개변수에 접근 제어자를 설정하면 동일한 이름의 필드를 선언하지 못한다. 생성자 매개변수에 접근 제어자가 설정되면 자동으로 필드도 함께 선언되기 때문이다. 또한, 접근 제어자가 설정된 매개변수들은 `this.필드 = 매개변수` 가 자동으로 수행되기 때문에 생성자 내부 코드도 생략해 줄 수 있다. 

```tsx
class Employee {
	constructor(
		private name: string,
		protected age: number,
		public position: string,
	){}
}
```

따라서 타입스크립트에서 클래스를 사용할 때에는 보통 생성자의 매개변수에 접근 제어자를 설정하여 필드 선언과 생성자 내부 코드를 생략한다. 훨씬 간결하고 빠르게 코드를 작성할 수 있기 때문이다.

## 인터페이스를 구현하는 클래스

타입스크립트의 인터페이스는 클래스의 설계도 역할을 할 수 있다. 

다음 코드와 같이 인터페이스를 이용해서 클래스에 어떤 필드와 메서드가 존재하는지를 정의할 수 있다.

```tsx
interface CharacterInterface {
	name: stirng;
	moveSpeed: number;
	move(): void;
}

class Character implements CharacterInterface {
	constructor(
		public name: string,
		public moveSpeed: number,
		private extra: string
	) {}
	
	move() : void{}
	
}
```

인터페이스를 클래스에서 `implements` 키워드와 함께 사용하면 이제부터 이 클래스가 생성하는 객체는 모두 그 인터페이스 타입을 만족하도록 클래스를 구현해야 한다.