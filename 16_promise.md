# 프로미스

프로미스는 제네릭 클래스로 구현되어 있다. 따라서 새로운 Promise를 생성할 때 다음과 같이 타입 변수에 할당할 타입을 직접 설정해 주면 해당 타입이 바로 resolve 결과 값의 타입이 된다. 

```tsx
const promise = new Promise<number>((resolve, reject) => {
	setTimeout(() => {
		resolve(20);
	}, 3000);
});

promise.then((response) => {
	console.log(response);
}); // response 는 number 타입

promise.catch((error) => {
	if (typeof error === 'string') {
		console.log(error);
	}
});
```

만약 어떤 함수가 Promise 객체를 반환한다면 함수의 반환 값 타입 정의를 다음과 같은 방식으로 지정할 수 있다. 

```tsx
function fetchPost(): Promise<Post> {
	return new Promise((resolve, reject) => {
		setTimeout(()=> {
			resolve({
				id: 1,
				title: 'title',
				content: 'content',
			});
		}, 3000);
	});
}

// 혹은 

function fetchPost(){
	return new Promise<Post>((resolve, reject) => {
		setTimeout(()=> {
			resolve({
				id: 1,
				title: 'title',
				content: 'content',
			});
		}, 3000);
	});
}

```

함수의 선언 부분만 보고 해당 함수가 어떤 타입을 반환하는지 직관적으로 확인할 수 있는 게 더 좋으므로 대체적으로 1번째 방법이 더 권장된다.