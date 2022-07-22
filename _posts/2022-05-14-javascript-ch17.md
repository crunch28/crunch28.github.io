---
layout: post
title: Chapter 17장 생성자 함수에 의한 객체 생성
date: 2022-05-14 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 17장 생성자 함수에 의한 객체 생성

### 17.1 Object 생성자 함수

`생성자 함수` : **new** 연산자와 함께 호출하여 **객체(인스턴스)를 생성**하는 함수

```jsx
const obj = new Object();
const strObj = new String('kim');
const numObj = new Number(123);
const boolObj = new Boolean(true);
const func = new Function('x', 'return 2 + x');
const arr = new Array(1, 2, 3);
const regExp = new RegExp(/ab+c/i);
const date = new Date();
```

### 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성함

```jsx
const circle = {
	radius : 5,   // 프로퍼티만 다름
	getDiameter(){
		return 2 * this.radius;
	}
};

const circle2 = {
	radius : 10,   // 프로퍼티만 다름
	getDiameter(){
		return 2 * this.radius;
	}
};
```

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

구조가 동일한 객체 여러 개를 간편하게 생성

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
};

const circle1 = new Circle(5);  // 생성자 함수로 동작
const circle2 = new Circle(10); // 생성자 함수로 동작
const circle3 = Circle(10);     // 일반 함수로 동작
```

생성자 함수는 객체(인스턴스)를 생성하는 함수

**new** 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작

**new** 연산자와 함께 호출하지 않으면 해당 함수는 일반 함수로 동작

### 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할

- 인스턴스 생성
- 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)

인스턴스 생성 및 인스턴스 초기화한 후 암묵적으로 인스턴스 반환하는 과정

1. **인스턴스 생성과 this 바인딩**

암묵적으로 빈 객체(인스턴스)가 생성 > 인스턴스는 this에 바인딩

```jsx
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
	console.log(this); // Circle {}
	
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
}
```

2. **인스턴스 초기화**

생성자 함수에 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화

```jsx
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
	console.log(this); // Circle {}
	
	// 2. this에 바인딩되어 있는 인스턴를 초기화
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
}
```

3. **인스턴스 반환**

생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

```jsx
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
	console.log(this); // Circle {}
	
	// 2. this에 바인딩되어 있는 인스턴를 초기화
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
	
	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
	
	// if 반환 객체를 명시하면 this 반환이 무시
	return {};
	
	// if 원시 값을 반환하면 원시 값 무시하고 this 반환
	return 'return';
}
```

생성자 함수 내부에서 **return 문은 반드시 생략**

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

함수는 객체이므로 일반 객체와 동일하게 동작

일반 객체는 호출할 수 없지만 함수는 호출할 수 있음

⇒ 일반 객체외에 함수만 가지는 내부 슬롯과 내부 메서드를 추가로 가짐

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 : **[[Call]]**

함수가 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 : **[[Construct]]**

```jsx
function foo(){}

// 일반적인 함수로 호출 **[[Call]]**
foo();

// 생성자 함수로서 호출 **[[Construct]]**
new foo();
```

callable : 내부 메서드 **[[Call]]**을 갖는 함수 객체

constructor : 내부 메서드 **[[Construct]]**를 갖는 함수 객체

non-constructor : **[[Construct]]**를 갖지 않는 함수 객체

모든 함수 객체는 callable/constructor  OR callable/non-constructor

![img-ch17-1.png](/public/img/posts/javascript-deep-dive/img-ch17-1.png)

### 17.2.5 constructor와 non-constructor의 구분

- constructor :  함수 선언문, 함수 표현식, 클래스
- non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

```jsx
// constructor
// 함수 선언문
function foo(){}
// 함수 표현식
const bar = function(){};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수. 메서드 인정 안함
const bat = {
	x : function(){}
}

new foo();
new bar();
new bat.x();

// non-constructor
// 화살표 함수
const arrow = () => {};
// 메서드 축약 표현
const obj = {
	x(){}
}

new arrow(); // Uncaught TypeError: arrow is not a constructor
new obj.x(); // Uncaught TypeError: obj.x is not a constructor
```

### 17.2.6 new 연산자

일반 함수와 생성자 함수에 특별한 형식적인 차이는 없음

```jsx
**// 일반 함수(객체 반환 x)**
function add(x, y){
	return x + y;
}

// 일반 함수를 new 연산자로 호출
let inst1 = new add();
// 일반 함수로 호출
let inst2 = add(1,2);

// 함수가 객체를 반환하지 못함
console.log(inst1); // add {}
// 일반 함수의 반환
console.log(inst2); // 3

**// 일반 함수(객체를 반환)**
function createUser(name, role){
	return {name, role};
}

// 일반 함수를 new 연산자로 호출
inst1 = new createUser('kim', 'minseong');
// 일반 함수로 호출
inst2 = createUser('kim', 'minseong');

console.log(inst1); // {name: 'kim', role: 'minseong'}
console.log(inst2); // {name: 'kim', role: 'minseong'}

**// 생성자 함수**
function Circle(radius){
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
}

// 일반 함수로 호출
const circle = Circle(5);

// 일반 함수로 호출하면 this = window
console.log(circle);        // undefined 반환이 없음 new로 하지 않아서
console.log(radius);        // 5
console.log(getDiameter()); // 10

circle.getDiameter();       // Uncaught TypeError: Cannot read properties of undefined (reading 'getDiameter')
```

### 17.2.7 new.target

생성자 함수가 **new** 연산자 없이 호출되는 것을 방지하기 위해 ES6에서 [**new.target**](http://new.target) 지원

**new** 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킴

**new** 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined

```jsx
// 생성자 함수
function Circle(radius){
	
// new 연산자 없이 함수 호출이면 new.target은 undefined
	if(!new.target){
		return new Circle(radius);
	}

	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
}
```