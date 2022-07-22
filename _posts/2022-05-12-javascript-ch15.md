---
layout: post
title: Chapter 15장 let, const 키워드와 블록 레벨 스코프
date: 2022-05-12 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드로 선언한 변수의 문제점

### 15.1.1 변수 중복 선언 허용

`var` 키워드로 선언한 변수는 중복 선언이 가능

```jsx
var x = 1;
var y = 1;

var x = 100;
var y; // 초기화문이 없는 변수 선언문은 무시

console.log(x); // 100
console.log(y); // 1
```

중복 선언으로 인한 변수 값 변경 발생

### 15.1.2 함수 레벨 스코프

`var` 키워드로 선언한 변수는 오로지 함수 레벨 스코프만 지역 스코프로 인정

코드 블록 내에서 선언해도 모두 전역 변수가 됨

### 15.1.3 변수 호이스팅

변수 선언문 이전에 변수를 참조하는 것은 `변수 호이스팅`에 의해 에러를 발생시키지는 않지만 **프로그램의 흐름상 맞지 않을뿐더러 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남김**

### 15.2 let 키워드

### 15.2.1 변수 중복 선언 금지

`let` 키워드로 이름이 같은 변수를 중복 선언하면 `문법 에러` 발생

```jsx
let x = 1;
let x = 2; // Uncaught SyntaxError: Identifier 'x' has already been declared
```

### 15.2.2 블록 레벨 스코프

모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프

```jsx
let foo = 1;

// 블록
{
	let foo = 2;
	let bar = 3;
}

console.log(foo); // 1
console.log(bar); // Uncaught ReferenceError: bar is not defined
```

![img-ch15-1.png](/public/img/posts/javascript-deep-dive/img-ch15-1.png)

### 15.2.3 변수 호이스팅

`let` 키워드로 선언한 변수는 “**선언 단계**”와 “**초기화 단계**”가 분리되어 진행

런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 **선언 단계**가 먼저 실행되지만 **초기화 단계**는 변수 선언문에 도달했을 때 실행

스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 `일시적 사각지대`

```jsx
// 사각지대 start
console.log(foo); // Uncaught ReferenceError: Cannot access 'foo' before initialization
// 사각지대 end
let foo;
console.log(foo); // undefined

foo = 1;
console.log(foo); // 1
```

![img-ch15-2.png](/public/img/posts/javascript-deep-dive/img-ch15-2.png)

### 15.2.4 전역 객체와 let

`let` 키워드로 선언한 전역 변수는 전역 객체(window)의 프로퍼티가 아님

```jsx
let x = 1;

console.log(window.x); // undefined
console.log(x);        // 1
```

`let` 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재함

### 15.3 const 키워드

### 15.3.1 선언과 초기화

`const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함

```jsx
const t; //Uncaught SyntaxError: Missing initializer in const declaration
```

### 15.3.2 재할당 금지

`const` 키워드로 선언한 변수는 재할당 금지

```jsx
const t = 1;
t = 2; // Uncaught TypeError: Assignment to constant variable.
```

### 15.3.3 상수

상수는 재할당이 금지된 변수를 말함

상수는 상태 유지와 가독성 좋음, 유지보수의 편의성이 있음

`const` 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 `const` 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없음

### 15.3.4 const 키워드와 객체

`const` 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음

```jsx
const person ={
	name : 'Lee'
};

person.name = 'Kim';

console.log(person); // {name: 'Kim'}
```

프러퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능

### 15.4 var vs. let vs. const

변수 선언에는 기본적으로 `const`를 사용하고 `let`은 재할당이 필요한 경우에 사용

권장

- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 **`let`** 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 **`const`** 키워드를 사용한다. **`const`** 키워드는 재할당을 금지하므로 **`var`**, **`let`** 키워드보다 안전하다.