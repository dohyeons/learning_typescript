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
