---
layout: post
title: Chapter 13장 스코프
date: 2022-05-10 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 13장 스코프

### 13.1 스코프란

모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정됨. 즉, 스코프는 **`식별자의 유효한 범위`**

```jsx
// 전역
var x1 = 1;

// 블록 레벨
if(true){
	var x2 = 2;
	if(true){
		var x2= 22;
		var x3= 3;
	}
}

// 함수 레벨
function fnOuter(){
	var x4 = 4;
	
	function fnInner(){
		var x3= 33;
		var x5= 5;
	}
}

console.log(x1); // 1
console.log(x2); // 22
console.log(x3); // 3
// 유효범위 지역 스코프
console.log(x4); // Uncaught ReferenceError: var4 is not defined
console.log(x5); // Uncaught ReferenceError: var5 is not defined
```

### 13.2 스코프의 종류

| 구분 | 설명 | 스코프 | 변수 |
| --- | --- | --- | --- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부 | 지역 스코프 | 지역 변수 |

변수는 자신이 선언된 위치(전역 또는 지역)에 의해 유효한 범위인 스코프가 결정

### 13.2.1 전역과 전역 스코프

```jsx
/*
* 초록 : 전역 스코프
* 파랑 : 지역 스코프
* 빨강 : 지역 스코프
*/

var x = 'global x'; // 전역 변수
var y = 'global y'; // 전역 변수

function fnOuter(){
	var z = "outer's loval z"; // 지역 변수
	
	console.log(x); // global x
	console.log(y); // global y
	console.log(z); // outer's loval z
	
	function fnInner(){
		var x = "inner's local x"; // 지역 변수
		
		console.log(x); // inner's local x
		console.log(y); // global y
		console.log(z); // outer's loval z
	}
	
	fnInner();
}

fnOuter();

console.log(x); // global x
console.log(y); // global y
```

전역 변수는 어디서든 참조 가능

### 13.2.2 지역과 지역 스코프

지역 변수는 함수 몸체 내부. 자신의 지역 스코프(파랑)와 하위 지역 스코프(빨강)에서 유효함

### 13.3 스코프 체인

스코프 체인이란? **스코프가 계층적(함수의 중첩 등등) 구조로 연결된 것**

스코프 체인을 통해 변수를 참조하는 코드의 스코프(빨강)에서 시작하여 상위 스코프(파랑, 초록) 방향으로 이동하며 선언된 변수를 검색함

### 13.3.1 스코프 체인에 의한 변수 검색

변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색

1. 변수를 참조하는 코드의 스코프에서 변수가 선언되었는지 검색
2. 선언된 변수가 존재하지 않으면 상위 스코프에서 변수가 선언되었는지 검색
3. 선언된 변수가 존재하면 검색 종료

상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없음

### 13.3.2 스코프 체인에 의한 함수 검색

```jsx
function foo() {
	console.log('global function foo');
}

function bar() {
	// 중첩 함수
	function foo() {
	console.log('local function foo');
	}
	foo(); //???
}

bar(); // local function foo
```

1. 함수 선언문으로 함수를 정의하여 런타임 이전에 객체가 먼저 생성
2. 함수 이름과 동일한 이름의 식별자를 암묵적으로 선언
3. 모든 함수는 함수 이름과 동일한 식별자에 할당

foo 식별자 참조

### 13.4 함수 레벨 스코프

코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성

`var` 함수 레벨 스코프

`let`, `const`  블록 레벨 스코프

```jsx
var x = 1;
let y = 1;
const z = 1;

if(true){
	var x = 10;
	let y = 11;
	const z = 12;
}

console.log(x); // 10
console.log(y); // 1
console.log(z); // 1
```

### 13.5 렉시컬 스코프

```jsx
var x = 1;

function foo() {
	var x = 10;
	bar();
}

// 함수 정의 위치
function bar() {
	console.log(x);
}

foo(); // ???
bar(); // ???
```

함수의 상위 스코프 결정

1. 함수를 어디서 호출했는지에 따라 함수의 상위 스코프를 결정한다. 동적 스코프
2. **함수를 어디서 정의했는지에 따라 함수의 상위 스코프를 결정한다. 정적(렉시컬) 스코프**

자바스크립트는 함수를 어디서 정의했는지에 따라 상위 스코프를 결정.