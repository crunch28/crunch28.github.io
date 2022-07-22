---
layout: post
title: Chapter 18장 함수와 일급객체
date: 2022-05-15 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 18장 함수와 일급객체

### 18.1 일급 객체

일급 객체의 조건

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

**함수 ==  일급 객체**

```jsx
// 1. 무명의 리터럴로 생성
// 2. 변수나 자료구조에 저장
const increase = function(num){
	return ++num;
}

const decrease = function(num){
	return --num;
}

// 2. 변수나 자료구조에 저장
const auxs = {increase, decrease};

// 3. 함수의 매개변수에 전달
// 4. 함수의 반환값으로 사용
function makeCounter(aux){
	let num = 0;
	
	return function(){
		num = aux(num);
		return num;
	};
}

// 3. 함수의 매개변수에 전달
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1

// 3. 함수의 매개변수에 전달
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
```

일급 객체로서 함수의 **특징**은 일반 객체와 같이 함수의 매개변수에 전달 가능하고 함수의 반환값으로 사용 가능함

### 18.2 함수 객체의 프로퍼티

함수는 객체라서 프로퍼티를 가짐

console.dir 메서드

```jsx
function foo(num){
	return num * num;
}
console.dir(foo);
```

![img-ch18-1.png](/public/img/posts/javascript-deep-dive/img-ch18-1.png)

```jsx
function foo(num){
	return num * num;
}
console.dir(foo);
console.log(Object.getOwnPropertyDescriptors(foo));
/*
{
arguments: {value: null, writable: false, enumerable: false, configurable: false}
caller: {value: null, writable: false, enumerable: false, configurable: false}
length: {value: 1, writable: false, enumerable: false, configurable: true}
name: {value: 'foo', writable: false, enumerable: false, configurable: true}
prototype: {value: {…}, writable: true, enumerable: false, configurable: false
}
*/

// __proto__는 foo 함수의 프로퍼티가 아님
console.log(Object.getOwnPropertyDescriptor(foo, '__proto__')); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__')); // {enumerable: false, configurable: true, get: ƒ, set: ƒ}
```

arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티

일반 객체에는 없는 프로퍼티이며 함수 객체 고유의 프로퍼티

_*proto*_는 접근자 프로퍼티이며 함수 객체 고유의 프로퍼티가 아니라 Object.prototype 객체의 프로퍼티를 상속 받음

### 18.2.1 arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체임

함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체임

함수 내부에서 지역 변수로 사용함

```jsx
function multiply(x, y) {
    console.log(arguments);
    return x * y;
}

console.log(multiply());         // NaN
console.log(multiply(1));        // NaN
console.log(multiply(1, 2));     // 2
console.log(multiply(1, 2, 3));  // 2
```

매개변수는 함수 몸체 내부에서 변수와 동일하게 취급 받음

함수 호출 시 암묵적으로 매개변수 선언되고 nudefined로 초기화된 이후 인수가 할당됨

![img-ch18-2.png](/public/img/posts/javascript-deep-dive/img-ch18-2.png)

프로퍼티 키는 인수의 순서임

callee : 객체를 생성한 함수

length : 인수의 개수

arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현

```jsx
function sum() {
	let res = 0;

  	//arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문 순회할 수 있음
  	for (let i = 0; i < arguments.length; i++) {
		res += arguments[i];
  	}
  	return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

### 18.2.2 caller 프로퍼티

ECMAScript 사양에 포함되지 않은 비표준 프로퍼티

함수 자신을 호출한 함수를 가리킴

```jsx
function foo(func){
	return func();
}

function bar(){
    return 'caller : ' + bar.caller;
}

console.log(foo(bar)); // caller : function foo(func){return func();}
console.log(bar()); // caller : null
```

### 18.2.3 length 프로퍼티

함수를 정의할 때 선언한 매개변수의 개수를 가리킴

```jsx
function foo() {}
console.log(foo.length); // 0

function bar(x) {
	return x;
}
console.log(bar.length); // 1

function baz(x, y) {
	return x * y;
}
console.log(baz.length); // 2
```

arguments 객체의 **length** 프로퍼티는 인자의 개수

함수 객체의 **length** 프로퍼티는 매개변수의 개수

### 18.2.4 name 프로퍼티

함수 이름을 가리킴

**ES5**에서는 name 프로퍼티는 빈 문자열을 값으로 가짐

**ES6**에서는 name 프로퍼티는 함수 객체의 식별자를 값으로 가짐

```jsx
//기명 함수 표현식
var namedFunc = function foo(){};
console.log(namedFunc.name);     // foo

//익명 함수 표현식
var anonymousFunc = function(){};
console.log(anonymousFunc.name); // anonymousFunc
//ES5 : 빈 문자열
//ES6 : 함수 객체를 가리키는 변수명

//함수 선언문
function bar(){}
console.log(bar.name);           // bar
```

### 18.2.5 __proto__ 접근자 프로퍼티

모든 객체는 [[Prototype]] 내부 슬롯을 가짐

[[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킴

__proto__ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

내부 슬롯에는 직접 접근 못하고 간접 접근 가능함

```jsx
const obj = {
   a : 1
};

//객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype
console.log(obj.__proto__ === Object.prototype); // true

//객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받음
console.log(obj.hasOwnProperty('a'));            // true
console.log(obj.hasOwnProperty('__proto__'));    // false
```

### 18.2.6 prototype 프로퍼티

생성자 함수로 호출할 수 있는 함수 객체

constructor만 소유하는 프로퍼티

```jsx
//함수 객체는 prototype 프로퍼티를 소유함
(function(){}).hasOwnProperty('prototype'); // true

//일반 객체는 prototype 프로퍼티는 소유하지 않음
({}).hasOwnProperty('prototype');           // false
```

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴