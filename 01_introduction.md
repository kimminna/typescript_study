# 타입스크립트 개론

# 타입 시스템

타입 시스템이란 언어에서 사용할 수 있는 여러 값들을 어떠한 기준으로 묶어서 타입으로 정하고, 또 코드의 타입을 언제 검사할지, 어떻게 검사할지에 관련해서 지켜야 하는 규칙들을 모아둔 체계를 말한다. 타입 시스템은 언어의 타입과 관련된 문법 체계라고도 볼 수 있다.

타입 시스템은 정적 타입 시스템과 동적 타입 시스템 두 가지로 나뉜다.

- 정적 타입 시스템
  코드 실행 전에 모든 변수의 타입을 고정적으로 결정한다.
- 동적 타입 시스템
  코드 실행 전에는 타입을 결정하지 않고 코드를 실행하고 나서 유동적으로 타입을 결정한다.

자바스크립트는 동적 타입 시스템을 사용한다. 동적 타입 시스템은 기본적으로 변수의 타입들을 코드가 실행되는 도중에 결정하기 때문에 미리 변수의 타입을 지정하지 않아도 되는 유연함을 장점으로 갖는다. 변수의 타입이 어떤 하나의 타입으로 고정되지 않고, 현재 변수에 담긴 값에 따라 변수의 타입이 동적으로 계속 변화한다.

정적 타입 시스템은 반대로 변수를 선언함과 동시에 타입도 함께 명시해야 한다. 타입 관련 오류가 있다면 에디터 상에서 오류를 바로 알려주고, 실행되기 전에 타입을 잘못 쓰지 않았는지 검사까지 모두 마치고 실행되기 때문에 오류가 있다면 애초에 실행이 안 된다. 그래서 프로그래머가 의도치 않은 실수를 하더라도 미리 확인할 수 있다는 장점이 있다. 하지만 정적 타입 시스템을 채택한 언어에서는 모든 변수에 일일히 타입을 다 정의해야 하기 때문에 코드 양이 늘어난다는 단점도 가진다.

## 타입스크립트와 점진적 타이핑

타입스크립트는 정적 타입 시스템처럼 변수의 타입을 코드 실행 전에 결정하고 타입 오류가 없는지 프로그램 실행 전에 코드를 검사한다. 정적 타입 시스템처럼 안전하게 검사를 거치지만, 마치 동적 타입 시스템처럼 모든 변수에 직접 타입을 명시할 필요는 없다. 타입스크립트는 변수의 타입을 직접 정의하지 않아도 변수에 담기는 초기값을 기준으로 자동으로 타입을 알아서 추론한다.

결론적으로 타입스크립트는 동적 타입 시스템의 단점을 해결하고, 정적 타입 시스템의 귀찮음도 동시에 해결한다. 이런 타입 시스템을 점진적으로 타입을 결정한다고 해서 점진적 타입 시스템이라고 한다.

![Untitled.webp](attachment:d86b4e58-97f2-4f25-ac5d-f85e454865f8:Untitled.webp)

# 타입스크립트의 동작 원리

![Untitled.webp](attachment:dab79890-ccc1-4915-b77b-740067086df4:Untitled.webp)

1. 코드 → AST(추상 문법 트리)
2. 타입 검사
   1. 검사 성공 → AST를 자바스크립트 코드로 변환
   2. 검사 실패 → 컴파일 종료
3. 자바스크립트 코드 → AST(추상 문법 트리)
4. AST → 바이트 코드
5. 실행

예를 들어 타입 오류가 발생한 타입스크립트 코드는 컴파일 시 타입 검사를 통과할 수 없기 때문에 자바스크립트 코드로 변환되지 않아 실행할 수 없게 된다.

타입 오류가 발생하지 않은 타입스크립트 코드는 컴파일 시 타입 검사를 통과하고, 타입 관련된 문법은 전부 삭제된 자바스크립트 코드로 변환되게 된다.

정리하면 타입스크립트는 자바스크립트를 보다 더 안전하게 사용할 수 있도록 코드를 검사하는 용도로 사용된다고 보면 된다.

# 초기 설정

1. Node.js 패키지 초기화

   `npm init`

2. @types/node 설치하기

   `npm i @types/node`

   - Node.js가 제공하는 기본 기능(내장 함수, 클래스 등)에 대한 타입 정보를 가지고 있다. 만약 이 라이브러리를 설치하지 않으면 Node.js가 제공하는 콘솔 등의 기본 기능들의 타입이 선언되지 않아서 타입스크립트의 컴파일 과정에서 타입 검사가 실패되어 오류가 발생될 수 있다.

3. 타입스크립트 컴파일러 설치하기

   `npm i -g typescript`

4. 타입스크립트 실행하기
   1. tsc로 컴파일하고 실행하기

      `tsc 파일명`

      - 특정 파일을 컴파일하도록 타입스크립트 컴파일러에게 지시하는 명령어다. 컴파일이 완료되면 자바스크립트 파일이 생성된다.
      - 컴파일된 자바스크립트 코드는 `node 파일명` 을 이용해서 실행한다.

   2. tsx로 실행하기

      `npm i -g tsx`

      - 명령어 한 번으로 타입스크립트 코드를 바로 실행할 수 있다. 자바스크립트 파일을 생성하지 않고 한번에 실행 가능하다.

## 타입스크립트 컴파일러 옵션 설정하기

컴파일 과정에서 얼마나 엄격하게 타입 오류를 검사할지, 혹은 컴파일 결과 생성되는 자바스크립트 코드의 버전은 어떻게 설정할지 등의 세부 사항들을 옵션으로 직접 설정할 수 있다.

### 자동으로 생성하기

타입스크립트의 컴파일러 옵션은 패키지 루트 폴더 아래 tsconfig.json 파일에 설정할 수 있으며, 이는 Node.js 패키지 단위로 설정된다.

터미널에 `tsc —init` 을 입력하면 자동으로 기본 설정이 완료된 tsconfig.json 파일이 생성된다.

### 직접 설정하기

- `include`
  컴파일할 타입스크립트 파일의 범위와 위치를 알려주는 옵션.
  이 옵션을 이용하면 파일이 아주 많을 때 일일히 tsc 명령과 함께 파일명을 입력하지 않아도 된다.
- `target`
  컴파일 결과 생성되는 자바스크립트 코드의 버전을 설정한다.
  모든 브라우저에서 최신 ES를 호환하지 않을 수 있기 때문에 상황에 맞춰 바꿔서 작성해 주면 된다.
- `module`
  자바스크립트 코드의 모듈 시스템을 설정한다. 타입스크립트에서는 자바스크립트의 ES 모듈 시스템과 유사하게 내보낼 때에는 export, 불러올 때에는 import를 사용한다.
  만약 module 설정을 CommonJS로 설정해 둔 경우에는 requirt이나 exports 등의 문법으로 변환되게 된다.
- `outDir`
  컴파일 결과 생성할 자바스크립트 코드의 위치를 결정한다. (dist 같은 폴더 안에 컴파일된 코드들을 모아두는 것을 가능하게 한다.)
- `strict`
  타입 검사의 엄격함 수준을 결정한다. strict 옵션을 true로 검사하면 타입을 엄격하게 검사한다. 예를 들어, 타입스크립트의 경우 매개변수 타입을 프로그래머가 직접 지정하도록 권장한다. 만약 타입을 프로그래머가 직접 지정하지 않을 경우 strict가 true인 상태에서는 오류가 발생하지만, false로 설정해 둔 상태에서는 오류가 발생하지 않는다.
- `ModuleDetection`
  타입스크립트의 모든 파일은 기본적으로 전역 파일(모듈)로 취급된다. 따라서 두 파일에 동일한 이름의 변수명을 사용하면 기본적으로 오류가 발생한다. 이럴 때에는 각 파일에 모듈 시스템 키워드(export, import)를 최소 하나 이상 사용하여 해당 파일을 로컬 모듈로 취급되게 만들어야 하는데, 이를 자동화하는 옵션이 ModuleDetection이다.
  ModuleDetection 옵션을 force로 설정할 경우 자동으로 모든 타입스크립트 파일이 로컬 모듈로 취급된다. 보통 파일을 달리 하는 경우는 전역이 아닌 로컬로 취급하기 위한 목적일 때가 많으므로 이 옵션을 설정해 두는 것이 좋다.
- `skipLibCheck`
  타입 정의 파일의 타입 검사를 생략하는 옵션이다. 보통 타입 정의 파일은 라이브러리에서 사용하는데 가끔 라이브러리의 타입 정의 파일에서 타입 오류가 발생하는 일이 생길 수도 있으므로 해당 옵션을 true로 설정하여 불필요한 타입 정의 파일의 타입 검사를 생략하도록 설정해 줘야 한다.

```tsx
{
  "compilerOptions": {
    "skipLibCheck": true,
    "target": "esnext",
    "module": "esnext",
    "outDir": "dist",
    "strict": false,
    "moduleDetection": "force"
  },

  "include": ["src"]
}

```
