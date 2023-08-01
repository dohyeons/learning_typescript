# 1장 자바스크립트에서 타입스크립트로

## 1.1 자바스크립트의 역사

1995년 브랜던 아이크가 10일만에 설계.

1995년 이후로 발전. 유연성으로 많은 성장을 함

## 1.2 바닐라 자바스크립트의 함정

- 바닐라- 중요한 언어 확장이나 프레임워크 없이 자바스크립트를 사용하는 것 (= 순수 자바스크립트)

### 1.2.1 값 비싼 자유

자바스크립트는 코드를 구성하는 데에 사실상 제한이 없음. 이러한 자유로움은 파일이 커짐에 따라 역으로 자유를 훼손함.

```js
function paintPainting(painter, painting) {
	return painter.prepare().paint(painting, painter.ownMaterials).finish();
}
```

위 코드를 처음 보는 사람은 `paintPainting` 이라는 함수에 대해 막연한 유추만이 가능함(painter가 무슨 타입일지, finish가 무슨 역할을 하는 지). 어쩌다 운좋게 맞아 떨어져도 나중에 코드가 변경되면 소용이 없어짐. 자바스크립트는 동적 타입 언어이기 때문에 코드를 안전하게 실행시키는 데에는 문제가 좀 있다.

### 1.2.2 부족한 문서

<aside>
💡 자바스크립트 언어 사양에는 함수의 매개변수, 함수 반환, 변수 또는 다른 구성 요소의 의미를 설명하는 표준화된 내용이 없음. 때문에 개발자들은 JSDoc표준을 채택해서 사용함

</aside>

- JSDoc표준- 표준으로 형식화된 함수와 변수 코드 바로 위에 문서 주석을 작성하는 방식

```js
/**
*
*
* @param {Painting} painter
* @param {string} painting
* @param {boolean} Whether the painter painted the painting.
*/
function paintPainting(painter, painting){...}
```

JSDoc는 다음과 같은 문제로 규모가 있는 코드에서 문제가 존재함

- JSDoc의 설명이 코드 자체가 잘못되는 것을 막지 못함
- 이전에는 정확했더라도 코드를 수정하고 나서 변경 사항과 JSDoc에서 현재 유효하지 않은 부분을 찾기 어려움
- 복잡한 객체를 다루기 어려움

### 1.2.3 부족한 개발자 도구

자바스크립트는 코드 베이스에 대한 대규모 변경을 자동화하거나 통찰력을 얻기가 매우 어려움

## 1.3 타입스크립트

타입스크립트는 네 가지로 설명됨

1. 프로그래밍 언어: 자바스크립트의 모든 구문 & 타입을 정의하고 사용하기 위한 타입스크립트 고유 구문이 포함된 언어
2. 타입 검사기: 파일의 구성요소(변수, 함수 등)을 이해하고, 잘못 작성된 부분을 알려주는 프로그램
3. 컴파일러: 타입 검사기를 실행한 후 이에 대응하는 자바스크립트 코드를 생성하는 프로그램
4. 언어 서비스: IDE에 개발자에게 유용한 유틸리티 제공법을 알려주는 프로그램

## 1.4 타입스크립트 플레이그라운드에서 시작하기

https://www.typescriptlang.org/play

### 1.4.1 타입스크립트 실전

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70a1b5cf-b47b-4121-b1e2-b5e5c93c42ca/Untitled.png)

타입 검사기가 작동해 해당 코드의 오류를 알려줌(number는 호출이 불가능함). 이렇게 오류를 바로 알려주면 코드를 실행하고 오류를 기다리는 것보다 훨씬 빠름.

### 1.4.2 제한을 통한 자유

<aside>
💡 타입스크립트에서는 매개변수와 변수에 타입을 지정할 수 있다.

</aside>

- 코드를 지정한 방법으로만 사용하게 해서, 코드의 일부분을 변경하더라도 이 코드를 사용하는 다른 코드가 올바르게 사용하게끔 경고를 함.

  ```tsx
  function sayMyName(fullName) {
  	console.log(fullName);
  }

  sayMyName("현수", "도");
  ```

  위 코드의 `sayMyName` 함수는 원래 두개의 인자(`firstName`, `lastName`)을 받는 함수. 그러나 매개변수를 하나로 변경하면, 오류가 발생함
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bbd6a6b8-51f2-434e-8d36-4a25c0e5c6a1/Untitled.png)

### 1.4.3 정확한 문서화

<aside>
💡 타입스크립트 구문은 그 자체로 코드를 문서화, 즉, 코드의 동작과 사용법을 설명할 수 있음(유추 가능).

</aside>

```tsx
interface Painter {
    finish(): boolean;
    ownMaterial: Material[];
    paint(painting: string, materials: Material[]): boolean;
}

function paintPainting(painter: Painter, painting: string): booelan {...}
```

위 코드를 읽어보면 `painter` 는 객체이며, 세가지 속성을 가지는데 그 중 둘은 메서드이고, 하나는 배열임을 알 수 있다. 즉, 객체가 어떻게 생겼는지를 알 수 있고 나아가 함수가 무엇을 반환하는지도 알 수 있다.

### 1.4.4 더 강력한 개발 도구

<aside>
💡 VScode 같은 편집기에서 타입스크립트를 이해하고, 작성한 코드에 대해 좋은 제안을 함.
</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a742415a-4668-4846-b72e-c9c7c66f0148/Untitled.png)

자바스크립트에서는 다음과 같이 `pringStringLength` 의 매개변수 `string` 에 대한 정보가 없기 때문에 추천 기능이 작동하지 않음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/633e3a0c-2d4f-4f2e-a676-92d2e6ec983c/Untitled.png)

반면 타입스크립트에서는 `string` 의 속성이 문자열이라는 것을 알기 때문에 추천을 해줌

### 1.4.5 구문 컴파일하기

<aside>
💡 타입스크립트 컴파일러에 타입스크립트 코드를 입력하면 이에 대응하는 자바스크립트 코드를 내보냄

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9513e5da-0709-4f0c-94a9-3a97f37c9cf4/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18b47126-8311-4bad-845e-f67829b84c77/Untitled.png)

타입스크립트는 결국 자바스크립트를 만들어내기 위함임을 기억하자.

## 1.5 로컬에서 시작하기

먼저, 타입스크립트를 전역적으로 설치하자(아니면 사용할때마다 설치해야하는데 귀찮음)

<aside>
💡 npm i -g typescript

</aside>

확인해주자

<aside>
💡 tsc —version

</aside>

### 1.5.1 로컬에서 실행하기

테스트를 위한 폴더를 만들고, tsconfig.json구성 파일을 생성하자.

<aside>
💡 tsc —init

</aside>

이렇게 생성된 tsconfig.json 파일은 타입스크립트가 코드를 분석할 때 사용하는 설정이다.

이제 여기 index.ts를 생성하고 여기에 코드를 입력해준 후 tsc index.ts 를 터미널에 입력해보자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c0a15df-625a-46bf-bd53-7d0a56a2370c/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86ae2955-86e2-45d0-bf44-b2539089cb94/Untitled.png)

컴파일은 되었지만 `index.js` 는 텅 비어있는 것을 확인할 수 있다. 이는 `console` 에 `bulb` 라는 속성이 없기 때문이다. 이를 `console.log` 로 바꿔준 뒤, 다시 해보자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/147e0137-188c-41c9-9c95-344d3f50a265/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ef56c3d-3d77-494c-97e7-68482917e9a1/Untitled.png)

이번에는 올바르게 컴파일 된 것을 볼 수 있다.

### 1.5.2 편집기 기능

<aside>
💡 tsconfig.json 파일을 생성하면, 이제 편집기가 특정 폴더를 열었을 때, 이 폴더를 타입스크립트 프로젝트로 인식한다.

</aside>

## 1.6 타입스크립트에 대한 오해

### 1.6.1 잘못된 코드 해결책?

<aside>
💡 타입스크립트는 코드 스타일을 강요하지 않으며, 특정 프레임워크와도 관련이 없다.

</aside>

### 1.6.2 자바스크립트로의 확장

<aside>
💡 타입스크립트는 설계 목표는 다음과 같다.

- 현재와 미래의 ECMA스크립트 제안에 맞춘다.
- 모든 자바스크립트 코드의 런타입 동작을 유지한다.

즉 타입스크립트는 자바스크립트 작동 방식을 전혀 변형하지 않는다.

</aside>

### 1.6.3 자바스크립트보다 느리다

<aside>
💡 운영 프레임워크 대다수는 타입스크립트 컴파일러를 사용하지 않고, 트랜스파일을 위한 도구만을 사용하며 타입스크립트는 타입 검사용으로만 사용한다.

</aside>

그러나 타입스크립트는 코드를 빌드하는데 시간이 조금 더 걸림. 브라우저 혹은 Node.js 에서 실행되기 전 자바스크립트로 컴파일 되어야 함.

### 1.6.4 진화가 끝났다

<aside>
💡 당연하지만…새로운 버전이 계속해서 나오고 있다.

</aside>
## 2.1 타입의 종류

<aside>
💡 **타입: 자바스크립트에서 다루는 값의 형태에 대한 설명**

</aside>

자바스크립트의 원시 타입은 다음과 같으며, 타입스크립트에서는 값을 이 원시 타입 중 하나로 간주한다.

1. null
2. undefined
3. string
4. number
5. boolean
6. bigint
7. symbol

### 2.1.1 타입 시스템

<aside>
💡 프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집합

</aside>

작동 방식은 다음과 같다.

1. 코드를 읽고 존재하는 모든 타입과 값 이해
2. 초기 선언에서 가질 수 있는 타입 확인
3. 이들이 코드에서 어떻게 사용될 수 있는지 경우의 수 확인
4. 사용법이 타입과 다르면 오류 표기

### 2.1.2 오류 종류

- 구문 오류: 코드로 이해가 안되는, 즉, 잘못된 구문일 경우 발생
- 타입 오류: 타입 검사기가 오류를 잡아낸 경우

## 2.3 타입 애너테이션

변수에 초깃값이 없는 경우, 암묵적으로 그 타입을 any로 판단함. 이를 진화하는 any라고 부름.

그러나, 이런 any의 사용은 타입스크립트의 타입 검사를 쓸모없게 만든다.

이런 상황을 방지하기 위해, 초깃값을 할당하지 않고도 타입을 선언할 수 있는 타입 애너테이션을 제공한다.

```tsx
let name: string;
name = "이름이에여";
```

### 2.3.1 불필요한 타입 애너테이션

```tsx
let name: string = "이름";
```

타입스크립트가 이미 `name` 이 문자열임을 알기 때문에, 위의 애너테이션은 중복이다. 이와 같은 경우, 수동으로 작성하는 것은 번거롭기 때문에, 지양하는 편이 좋다.

## 2.4 타입 형태

타입스크립트는 단순히 타입을 파악하는 것을 넘어 객체에 어떤 멤버 속성이 있는지를 파악한다.

### 2.4.1 모듈

- 모듈: export 또는 import가 있는 파일
- 스크립트: 모듈이 아닌 파일

모듈에서 작성한 변수는 export 문으로 내보내지기 전까지 해당 모듈 내에서만 사용 가능하다. 하지만 스크립트는, 해당 파일을 전역 스코프로 간주한다. 따라서 모든 파일에서 스크립트의 내용에 접근이 가능하다. 이를 사용해 파일의 아무 곳에 `export {};` 를 추가해 모듈로 만들 수 있다.

```tsx
let name: string = "이름";

export {};
```

# 5장 함수

## 5.1 함수 매개변수

### 5.1.1 필수 매개변수

<aside>
💡 타입스크립트에서는 **함수의 매개변수가 모두 필수라고 가정**한다. 따라서 더 적거나 많은 매개변수는 허용되지 않는다.

</aside>

```tsx
function test(exam1: number, exam2: number) {
	console.log(exa1, exam2);
}

test(1, 2); // ok

test(1); // Error: Expected 2 arguments, but got 1

test(1, 2, 3); // Error: Expected 2 arguments, but got 3
```

이러한 특징은 **모든 인수를 함수 내부에 존재하게 만들어 타입 안정성을 강화**한다.

### 5.1.2 선택적 매개변수

<aside>
💡 타입 애너테이션의 :앞에 ? 를 추가해 매개변수가 선택적이라고 표시한다.

</aside>

```tsx
function test(exam1: number, exam2?: number) {
	console.log(exa1, exam2);
}

test(1, 2); // ok

test(1); // ok 1, undefined
```

- 단, `| undefined` 인 유니온 타입과는 다름을 명심하자. **유니온 타입에서는 값이 `undefined` 일지라도 항상 제공되어야 한다.**

  ```tsx
  function test(exam1: number, exam2: number | undefined) {
  	console.log(exa1, exam2);
  }

  test(1, 2); // ok

  test(1); // Error: Expected 2 arguments, but got 1
  ```

- **선택적 매개변수는 항상 마지막에 위치**해야한다.

  ```tsx
  function test(exam1?: number, exam2: number) {
  	// Error: A required parameter cannot follow an optional parameter.
  	console.log(exa1, exam2);
  }

  test(1, 2); // ok

  test(1); // ok 1, undefined
  ```

### 5.1.3 기본 매개변수

<aside>
💡 선택적 매개변수를 선언해서 암묵적으로 `| undefined` 의 유니온 타입을 추가시킬 수 있다.

</aside>

- 기본값이 있고 타입 애너테이션이 없는 경우 해당 기본값으로 타입을 유추한다.

  ```tsx
  function test(exam1: number, exam2 = 2) {
  	console.log(exa1, exam2);
  }

  test(1); // 1, 2

  test(1, "2"); // Error
  ```

### 5.1.4 나머지 매개변수

<aside>
💡 나머지 인수 배열을 나타내기 위해 `[]` 구문이 추가된다.

</aside>

```tsx
function test(param1: number, ...params: number[]) {
	console.log(param1);
	for (let el of params) {
		console.log(el);
	}
}
```

## 5.2 반환 타입

타입스크립트는 함수가 반환하는 값을 이해하고 그 타입을 파악한다. 여러개의 반환문이 있다면, 반환 타입을 가능한 모든 반환 타입의 조합으로 유추한다.

### 5.2.1 명시적 반환 타입

우선, 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋다.

명시적 반환 타입이 유용한 경우는 다음과 같다.

1. 가능한 반환값이 많은 함수가 항상 동일 타입의 값을 반환하도록 강제
2. 타입스크립트 검사 속도를 높인다.
3. 타입스크립트는 재귀 함수의 반환 타입을 통해 타입을 유추하는 것을 거부한다.

반환 타입은 다음과 같이 배치한다.

```tsx
function test(exam1: number, exam2?: number): number {
	return exam1 + exam2;
}
```

물음표 함수의 경우 다음과 같이 작성한다.

```tsx
const test = (exam1: number, exam2?: number): number => {
	return exam1 + exam2;
};
```

## 5.3 함수 타입

<aside>
💡 함수 타입은 화살표 함수와 유사하지만, 함수 본문 대신 함수 타입이 있다.

</aside>

```tsx
function test(logTest: (str: string) => string) {
	return logTest("테스트");
}

function test2(str: string) {
	return str;
}
```

⭐️**함수 타입은 콜백함수를 설명하는데 자주 사용된다.**⭐️

- 함수 타입이 달라 함수를 할당할 수 없다면 다음과 같은 오류의 내용을 가진다.
  1. 첫번째 들여쓰기: 두 함수 타입을 출력
  2. 일치하지 않는 부분을 지정
  3. 일치하지 않는 부분에 대한 정확한 할당 가능성 오류를 지적

```tsx
function test(logTest: (str: string) => string) {
	return logTest("테스트");
}

function test2(str: string) {
	return 2;
}

test(test2); // Argument of type '(str: string) => number' is not assignable to parameter of type '(str: string) => string'.
// Type 'number' is not assignable to type 'string'.
```

### 5.3.1 함수 타입 괄호

함수 타입은 다른 타입처럼 사용할 수 있다. 따라서 유니온 타입도 만들 수 있다.

```tsx
let test: (() => string) | undefined;
```

### 5.3.2 매개변수 타입 추론

<aside>
💡 타입스크립트는 선언된 타입의 위치에 제공된 함수의 매개변수 타입을 유추 가능

</aside>

```tsx
let test: (param: string) => string;

test = function (param) {
	return param.toUpperCase();
};
```

고차 함수의 콜백함수 역시 타입을 잘 유추한다.

```tsx
const arr = ["안녕", "하세요"];

arr.forEach((el, idx) => {
	console.log(el.length, idx + 1);
});
```

### 5.3.3 함수 타입 별칭

함수 타입 역시 별칭을 사용할 수 있다.

```tsx
type Test = (param: string) => string;

let testFunc: test;

testFunc = param => param.toUpperCase();
```

이를 이용하여 코드의 공간을 확보할 수 있다.

## 5.4 그 외 반환 타입

void, never

### 5.4.1 void

<aside>
💡 어떠한 값도 반환하지 않는다. 즉, return 문이 없거나 값을 반환하지 않는 return 문을 가진 함수이다.

</aside>

```tsx
function voidFunc(param: string | undefined): void {
	if (!param) {
		return;
	}
	console.log(param);
	return param; // Error
}
```

단, undefined 를 반환하는 것이 아님에 주의한다. void는 함수의 반환 타입이 무시된다는 뜻이다.

### 5.4.2 never

<aside>
💡 의도적으로 오류를 발생시키거나 무한 루프를 실행하는 함수 반환 타입

</aside>

함수가 절대 반환하지 않도록 의도적으로 명시

```tsx
function getError(message: string): never {
	throw new Error(`${message}`);
}
```

## 5.5 함수 오버로드

<aside>
💡 이름이 같지만 유형이 다른 여러 함수를 생성한다. 이때 여러 다른 버전의 함수를 오버로드 시그니쳐라고 부르고, 최종적으로 구현된 함수를 구현 시그니쳐라고 부른다.

</aside>

구현 시그니쳐는 마지막에 한번만 제공되어야 하며, 구현 시그니쳐의 반환 타입은 다르게 선언할 수 있으나, 매개변수의 이름은 동일해야한다.

```tsx
function add(a: string, b?: string, c?: string): string; // (1) overload signature
function add(a: number, b?: number, c?: number): number; // (2) overload signature
function add(a: any, b?: any, c?: any): any {
	// (3) implementation signature
	if (b) {
		if (c) {
			return a + b + c;
		}
		return a + b;
	}
	return a;
}
```

이때, 구현 시그니쳐는 반드시 다른 모든 오버로드 시그니쳐와 호환되어야 한다.

# 6장 배열

## 6.1 배열 타입

### 6.1.1 배열과 함수 타입

괄호를 사용해 어느 부분이 함수 반환 부분이고 어느 부분이 배열 타입 묶음인지를 나타낸다.

```tsx
let test: () => string[];

let test2: (() => string)[];
```

### 6.1.2 유니언 타입 배열

```tsx
let arr: string | undefined[];

let arr2: (string | undefined)[];
```

### 6.1.3 any 배열의 진화

초기 빈 배열로 설정된 변수에서 타입 애너테이션을 포함하지 않으면 이를 any[] 로 취급하고 모든 타입을 받을 수 있다.

```tsx
const test = [];

test.push(""); // 타입: string[]

test[0] = 0; // 타입: (string | number) []
```

### 6.1.4 다차원 배열

```tsx
let testArr = number[][];

testArr = [[1,2,3], [4,5,6]];
```

## 6.2 배열 멤버

<aside>
💡 타입스크립트는 배열 멤버를 찾아서 해당 배열의 타입 요소를 되돌려준다.

</aside>

다음 배열의 요소는 배열이 string타입이기 때문에 string타입이다.

```tsx
const strArr = ["test"];

const test = strArr[0];
```

유니온 타입의 배열 요소는 유니온 타입이다. 즉, (string | number)[] 의 요소는 (string | number) 타입을 가진다.

### 6.2.1 불안정한 멤버

타입스크립트에서는 해당 배열의 멤버에 대한 접근이 올바른 타입의 값을 반환한다고 가정하기 때문에 문제가 생길 수 있다.

```tsx
function test(arr: string[]) {
	return arr[3].length; // 에러 없음
}

test(["테스트", "두번째 테스트"]);
```

## 6.3 스프레드와 나머지 매개변수

### 6.3.1 스프레드

… 연산자를 사용해 배열을 배합하는데, 이 중 하나의 값이 결과 배열에 포함되는 것을 인지한다.

만약 서로 다른 타입의 배열을 결합시켜 새 배열을 생성하면, 새 배열은 유니언 타입 배열로 이해된다.

```tsx
const test1 = ["일번", "이번"];

const test2 = [1, 2];

const test3 = [...test1, ...test2];
```

### 6.3.2 나머지 매개변수 스프레드

나머지 매개변수를 위한 인수로 사용되는 배열은 나머지 매개변수와 동일한 배열 타입을 가져야 한다.

### 6.4 튜플

<aside>
💡 크기와 타입이 고정된 배열

</aside>

```tsx
let test: [number, string];

test = [1, "첫번째"];

test = [2, "두번째"];
```

한번에 여러 값을 할당하기 위해 구조분해할당과 튜플을 함께 자주 사용한다.

### 6.4.1 튜플 할당 가능성

가변 길이의 배열 타입은 튜플 타입에 할당할 수 없다.

```tsx
const test = [false, 123];

const test2: [boolean, number] = test' // Error
```

### 나머지 매개변수로서의 튜플

튜플은 함수에 전달할 인수를 저장하는데 특히 유용하다.

```tsx
function test(string, number) {
	console.log(string, number);
}

const arr = ["문자열", 1];

test(...arr); // Error

const arr2: [string, number] = ["문자열", 2];

test(...arr2);
```

### 6.4.2 튜플 추론

<aside>
💡 타입스크립트는 기본적으로 배열을 튜플이 아닌 가변적 길이를 가진 배열이라고 여긴다.

</aside>

```tsx
function arrMaker(input: string) {
	return [input, input.length];
}

const arr = arrMaker("첫번째배열");
```

따라서 값이 튜플이어야 함을 두 가지 방법을 통해서 나타낸다.

### 명시적 튜플 타입

<aside>
💡 타입 애너테이션을 통해 튜플 타입을 할당한다.

</aside>

```tsx
function arrMaker(input: string): [string, number] {
	return [input, input.length];
}

const arr = arrMaker("첫번째배열");
```

### const 타입 어설션

명시적 타입을 입력하는 방식은 코드 변경에 따라서 필요한 구문을 추가해야하기 때문에 복잡하고 번거로워질 수 있다.

이 때, const어설션을 통해 이를 간단화 할 수 있다.

<aside>
💡 배열 리터럴 뒤 as const 가 배치되면 배열이 튜플로 처리되어야 함을 나타낸다.

</aside>

```tsx
const arr = [1, "문자열"] as const;
```

이는 해당 배열이 튜플이며, 읽기 전용이고 값 수정이 예상되는 곳에서 사용할 수 없을을 나타낸다.

```tsx
const arr = [1, "문자열"] as const;

arr[0] = 2; // Error
```

읽기 전용 튜플은 함수 반환에 편리함

# 7장 인터페이스

타입 별칭과 유사하지만,

- 읽기 쉬운 오류메시지
- 더 빠른 컴파일러 성능
- 클래스와의 더 나은 상호 운용성

등의 이유로 선호된다.

## 7.1 타입 별칭 vs 인터페이스

기본적으로, 이 둘은 거의 동일하다

```tsx
type Poet = {
	born: number;
	name: string;
};

interface Poet2 {
	born: number;
	name: string;
}
```

이 둘의 주요 차이점은 다음과 같다.

- 인터페이스는 속성 증가를 위한 병합이 가능하다.
- 인터페이스는클래스가 선언된 구조의 타입을 확인하는데 사용할 수 있다.
- 인터페이스에서 타입 검사기가 더 빨리 작동한다.
- 특이 케이스에서 나타나는 오류 메시지를 더 쉽게 읽을 수 있다.

유니온 타입과 같은 기능이 필요한 것이 아니라면, 인터페이스 사용을 추천한다.

## 7.2 속성 타입

### 7.2.1 선택적 속성 타입

타입 별칭과 비슷하다. 타입 애너테이션 : 앞에 ? 를 붙여서 선택적 속성임을 나타낸다.

```tsx
interface Poet2 {
	born?: number;
	name: string;
}
```

### 7.2.2 읽기 전용 속성

`readonly` 키워드를 사용해 다른 값으로 설정될 수 없음을 나타낸다. 즉 `readonly` 속성은 읽을 수 는 있지만, 재할당이 불가능하다.

```tsx
interface Page {
	readonly text: string;
}

const page: Page = {
	text: "못바꿔요",
};

page.text = "바꿔지나?";
// Error: Cannot assign to 'text' because it is a read-only property.
```

이 `readonly` 키워드는 타입 시스템에만 존재하고 인터페이스에서만 사용할 수 있다.

### 7.2.3 함수와 메서드

타입스크립트에서 인터페이스 멤버를 함수로 선언하는 방법에는 두 가지가 있다.

- 메서드 구문: 인터페이스 멤버를 `member(): void` 와 같이 객체 멤버로 호출되는 함수로 선언
- 속성 구문: 인터페이스 멤버를 `member: () => void` 와 같이 독립 함수와 동일하게 선언

```tsx
interface FuntionType{
	property: () => void;
	method(): string;
}

const hasBoth: FunctionType = {
	property: () => '';
	method() {
		return '';
	}
}
```

둘다 ? 를 붙여 선택적 속성으로 만들 수 있다.

이 둘의 주요 차이점은 다음과 같다.

- 메서드는 `readonly` 키워드의 사용이 가능하다.
- 인터페이스의 병합에서 다르다
- 타입에서 수행되는 일부 작업이 다르게 처리한다.

추천되는 스타일 가이드는 다음과 같다.

- 기본 함수가 this를 참조할 수 있다는 것을 알고 있다면 메서드 사용
- 반대의 경우에는 속성 함수 사용

### 7.2.4 호출 시그니처

인터페이스와 객체 타입은 호출 시그니처로 선언할 수 있다. 호출 시그니처는

<aside>
💡 값을 함수처럼 호출하는 방식에 대한 타입시스템의 설명

</aside>

이다. 즉, 호출 시그니처가 선언한 방식으로 호출되는 값만 인터페이스에 할당할 수 있다.

호출 시그니처는 콜론 대신 화살표로 표시한다.

```tsx
interface Callsigniture {
	(input: string): number;
}

const callSigniture: Callsigniture = input => input.length;
```

### 7.2.5 인덱스 시그니처

컨테이너 객체의 경우, 모든 가능한 키에 대한 필드에 대응하는 인터페이스를 만들기는 매우 어렵다(무한하니까)

이때 타입스크립트는 인덱스 시그니처 구문을 이용해 인터페이스의 객체가 임의의 키를 받고, 해당 키 아래의 특정 타입을 반환할 수 있음을 나타낸다.

키 다음에 타입이 있고, `{[i: string]: ...}` 와 같이 배열의 대괄호를 가진다.

```tsx
interface WordCounts {
	[i: string]: number;
}

const counts: WordCounts = {};
```

하지만, 타입 안정성을 완전히 보장하지는 않는다. 인덱스 시그니처는 “객체의 속성에 접근하면 무조건 값을 반환해야 함”을 나타낸다.

```tsx
interface WordCounts {
	[i: string]: number;
}

const counts: WordCounts = {};

console.log(counts.name.toString());
```

실제로 `counts.name` 은 undefined 이지만 타입 시스템에서는 오류를 발생시키지 않고, 런타임에서야 오류가 발생한다. 따라서 키: 값을 저장할 때 키를 미리 알 수 없다면 차라리 `Map` 을 사용하는 편이 안전하다.

### 7.2.6 중첩 인터페이스

인터페이스 타입도 자체 인터페이스 타입 혹은 객체 타입을 속성으로 가질 수 있다.

```tsx
interface Novel {
	author: string;
	setting: Setting;
}

interface Setting {
	place: string;
	year: number;
}
```

## 7.3 인터페이스 확장

타입스크립트에서는 다른 인터페이스의 모든 멤버를 복사해 선언할 수 있는 확장된 인터페이스를 허용한다. 이름 뒤에 `extends` 키워드를 추가해 다른 인터페이스에서 확장한 인터페이스임을 표시한다.

```tsx
interface Novel {
	author: string;
	setting: Setting;
}

interface Setting {
	place: string;
	year: number;
}

interface Comics extends Novel {
	genre: string;
}

const book: Comics = {
	author: "sd",
	setting: {
		place: "dfnsdf",
		year: 1234,
	},
	genre: "코믹",
};
```

### 7.3.1 재정의된 속성

파생 인터페이스는 다른 타입으로 속성을 다시 선언해 기본 인터페이스의 속성을 재정의 하거나 대체할 수 있다.

- 해당 속성을 유니언 타입의 더 구체적인 하위 집합으로 만듦
- 기본 인터페이스 타입에서 확장된 타입으로 만듦

```tsx
interface Basic {
	basic: string | null;
}

interface ExtendedOnce extends Basic {
	basic: string;
}

interface ExtendedTwice extends Basic {
	basic: number | string;
}
// Error: Interface 'ExtendedTwice' incorrectly extends interface 'Basic'.
//  Types of property 'basic' are incompatible.
//    Type 'string | number' is not assignable to type 'string | null'.
//      Type 'number' is not assignable to type 'string'.
```

### 7.3.2 다중 인터페이스 확장

인터페이스는 여러 개의 다른 인터페이스를 확장해서 선언할 수 있다.

```tsx
interface Basic {
	basic: string;
}

interface Basic2 {
	basic2: number;
}

interface Extended extends Basic, Basic2 {
	basic3: number | string;
}
```

코드 중복을 줄이고, 다른 코드 영역에서 객체의 형태를 더 쉽게 사용 가능하다.

## 7.4 인터페이스 병합

두 개의 인터페이스가 동일한 이름으로 동일 스코프에 선언된 경우, 각 필드를 포함하는 더 큰 인터페이스가 만들어진다.

```tsx
interface Basic {
	basic: string;
}

interface Basic {
	basic2: number;
}

// = interface Basic{
//	basic: string;
//	basic2: number;
//  }
```

그러나 이 병합이 여러 군데에서 이루어질 경우 코드를 파악하기가 어려워지므로 지양하는 편이 좋다.

하지만 외부 패키지나 내장 전역 인터페이스를 보강할 때 사용하면 유용하다.

### 7.4.1 이름이 충돌되는 멤버

병합 시 이름이 같지만 타입이 다른 속성을 여러 번 선언할 수 없다.

```tsx
interface Basic {
	basic: string;
}

interface Basic {
	basic: number; // Error
}
```

그러나 동일한 이름과 다른 시그니처를 가진 메서드는 정의할 수 있다. 이 경우, 함수 오버로드가 발생한다.

```tsx
interface BasicMethods {
	method(input: string): string;
}

interface BasicMethods {
	method(input: number): string;
}
```
