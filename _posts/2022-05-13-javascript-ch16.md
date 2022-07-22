---
layout: post
title: Chapter 16장 프로퍼티 어트리뷰트
date: 2022-05-13 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 16장 프로퍼티 어트리뷰트

### 16.1 내부 슬롯과 내부 메서드

`내부 슬롯` : 의사 프로퍼티

`내부 메서드` : 의사 메서드

자바스크립트 엔진의 내부 로직이므로 직접 접근하거나 호출할 수 있는 방법을 제공하지 않음

단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공

```jsx
const obj = {};

obj.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['

// 접근자 프로퍼티
obj.__proto__     // Object.prototype
```

### 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의

프로퍼티의 상태 : 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부

프로퍼티 어트리뷰트 == 내부 상태 값인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]

하지만 내부 슬롯으로 직접 접근 못함

Object.getOwnPropertyDescriptor 메서드를 사용하여 접근

```jsx
const person = {
	name : 'Lee'
};

// Object.getOwnPropertyDescriptor(객체참조, 프로퍼티 키)
// return ==> 프로퍼티 디스크립터 객체
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: 'Lee', writable: true, enumerable: true, configurable: true}
```

### 16.3 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티

     키와 값으로 구성된 일반적인 프로퍼티

- 접근자 프로퍼티

     자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티 (ex. get, set)

### 16.3.1 데이터 프로퍼티

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Value]] | value | *프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값
*프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재활당함. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장함 |
| [[Writable]] | writable | *프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖음
*[[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨 |
| [[Enumerable]] | enumerable | *프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖음
*[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for…in문이나 Object.keys 메서드 등으로 열거할 수 없음 |
| [[Configurable]] | configurable | *프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖음
*[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지됨. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용 |

### 16.3.2 접근자 프로퍼티

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | get | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수임.
즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값.
getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환 |
| [[Set]] | set | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수임.
즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값.
setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장 |
| [[Enumerable]] | enumerable | *프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖음
*[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for…in문이나 Object.keys 메서드 등으로 열거할 수 없음 |
| [[Configurable]] | configurable | *프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖음
*[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지됨. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용 |

```jsx
const person = {
	firstName : 'minseong',
	lastName : 'kim',
	
	// fullName은 접근자 프로퍼티
	// getter 함수
	get fullName(){
		return `${this.firstName} ${this.lastName}`;
	},
	
	// setter 함수
	set fullName(name){
		[this.firstName, this.lastName] = name.split(' ');
	}
};

console.log(person.firstName + ' ' + person.lastName); // minseong kim

// 접근자 프로퍼티에 값을 저장하면 setter 함수 호출
person.fullName = 'gildong hong';
console.log(person); // {firstName: 'gildong', lastName: 'hong'}

// 접근자 프로퍼티에 접근하면 getter 함수 호출
console.log(person.fullName); // gildong hong

// 데이터 프로퍼티
console.log(Object.getOwnPropertyDescriptor(person, 'firstName')); // {value: 'gildong', writable: true, enumerable: true, configurable: true}

// 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(person, 'fullName')); // {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

내부 슬롯/메서드 관점에서 동작 원리

1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다. 프로퍼티 키 “fullName”은 문자열이므로 유효한 프로퍼티 키다.
2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. fullName 프로퍼티는 접근자 프로퍼티다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값. 즉 getter 함수를 호출하여 그 결과를 반환한다. 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

`프로토타입` : 어떤 객체의 상위(부모) 객체의 역할을 하는 객체

`프로토타입 체인` : 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조

접근자 프로퍼티와 데이터 프로퍼티를 구별하는 방법

```jsx
// 일반 객체의 __proto__는 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__')); // {enumerable: false, configurable: true, get: ƒ, set: ƒ}

// 함수 객체의 prototype은 데이터 프로퍼티
console.log(Object.getOwnPropertyDescriptor(function() {}, 'prototype')); // {value: {…}, writable: true, enumerable: false, configurable: false}
```

### 16.4 프로퍼티 정의

`프로퍼티 정의` : 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 어트리뷰트를 재정의하는 것

```jsx
const person = {};

// 데이터 프로퍼티 정의
// Object.defineProperty(객체 참조, 데이터 프로퍼티의 키, 프로퍼티 디스크립터 객체)
Object.defineProperty(person, 'firstName', {
	value : 'minseong',
	writable : true,
	enumerable : true,
	configurable : true
});

Object.defineProperty(person, 'lastName', {
	value : 'kim',
});

console.log(Object.getOwnPropertyDescriptor(person, 'firstName')); // {value: 'minseong', writable: true, enumerable: true, configurable: true}
console.log(Object.getOwnPropertyDescriptor(person, 'lastName'));  // {value: 'kim', writable: false, enumerable: false, configurable: false}

// enumerable: false인 경우
console.log(Object.keys(person)); // ['firstName']
// writable: false인 경우
person.lastName = 'Hong';
// configurable: false인 경우
delete person.lastName;

console.log(Object.getOwnPropertyDescriptor(person, 'lastName'));  // {value: 'kim', writable: false, enumerable: false, configurable: false}

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
	get() {
		return `${this.firstName} ${this.lastName}`;
	},
	
	set(name) {
		[this.firstName, this.lastName] = name.split(' ');
	},
	enumerable : true,
	configurable : true
});

console.log(Object.getOwnPropertyDescriptor(person, 'fullName')); // {enumerable: true, configurable: true, get: ƒ, set: ƒ}

person.fullName = 'gildong Hong';
console.log(person); // {firstName: 'gildong', lastName: 'kim'}
```

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| --- | --- | --- |
| value | [[value]] | undefined |
| get | [[get]] | undefined |
| set | [[set]] | undefined |
| writable | [[writable]] | false |
| enumerable | [[enumerable]] | false |
| configurable | [[configurable]] | false |

### 16.5 객체 변경 방지

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 16.5.1 객체 확장 금지

`Object.preventExtensions` : 객체 확장 금지 메서드이며 프로퍼티 추가 금지

```jsx
const person = {
	name : 'kim'
};

// 확장 가능한 객체인지 확인
console.log(Object.isExtensible(person)); // true

// 객체 확장 금지
Object.preventExtensions(person);

// 확장 가능한 객체인지 확인
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가 안됨
person.age = 20;
console.log(person); // {name: 'kim'}

// 프로퍼티 삭제는 가능
delete person.name;
console.log(person); {}

// 프로퍼티 정의에 의한 프로퍼티 추가 안됨
Object.defineProperty(person, 'age', {
	value : 20
}); // Uncaught TypeError: Cannot define property age, object is not extensible at Function.defineProperty (<anonymous>)
```

### 16.5.2 객체 밀봉

`Object.seal` : 객체 밀봉 메서드이며 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지

```jsx
const person = {
	name : 'kim'
};

// 객체 밀봉인지 확인
console.log(Object.isSealed(person)); // false

// 객체 밀봉
Object.seal(person);

// 객체 밀봉인지 확인
console.log(Object.isSealed(person)); // true

console.log(Object.getOwnPropertyDescriptors(person)); // name: {value: 'kim', writable: true, enumerable: true, configurable: false}

// 프로퍼티 추가 안됨
person.age = 20;
console.log(person); // {name: 'kim'}

// 프로퍼티 삭제 안됨
delete person.name;
console.log(person); {} // {name: 'kim'}

// 프로퍼티 값 갱신 가능
person.name = 'hong';
console.log(person); // {name: 'hong'}

// 프로퍼티 어트리뷰트 재정의 안됨
Object.defineProperty(person, 'name', {
	configurable : true
}); // Uncaught TypeError: Cannot redefine property: name at Function.defineProperty (<anonymous>)
```

### 16.5.3 객체 동결

`Object.freeze` : 객체 동결 메서드이며 프로퍼티 읽기만 가능

```jsx
const person = {
	name : 'kim'
};

// 객체 동결인지 확인
console.log(Object.isFrozen(person)); // false

// 객체 동결
Object.freeze(person);

// 객체 동결인지 확인
console.log(Object.isFrozen(person)); // true

console.log(Object.getOwnPropertyDescriptors(person)); // name: {value: 'kim', writable: false, enumerable: true, configurable: false}

// 프로퍼티 추가 안됨
person.age = 20;
console.log(person); // {name: 'kim'}

// 프로퍼티 삭제 안됨
delete person.name;
console.log(person); {} // {name: 'kim'}

// 프로퍼티 값 갱신 안됨
person.name = 'hong';
console.log(person); // {name: 'kim'}

// 프로퍼티 어트리뷰트 재정의 안됨
Object.defineProperty(person, 'name', {
	configurable : true
}); // Uncaught TypeError: Cannot redefine property: name at Function.defineProperty (<anonymous>)
```

### 16.5.4 불변 객체

객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 변경 방지 메서드를 실행해야 함

```jsx
function deepFreeze(target){
	
	if(target && typeof target === 'object' && !Object.isFrozen(target)){
		Object.freeze(target);
		
		Object.keys(target)
			.forEach(key => deepFreeze(target[key]));
	}
	return target;
}

const person = {
	name : 'kim',
	address : {
		city : 'Seoul'
	}
};

// 깊은 동결
deepFreeze(person);

// 객체 동결
console.log(Object.isFrozen(person));		  // true

// 중첩 객체도 동결
console.log(Object.isFrozen(person.address)); // true

person.address.city = 'Busan';
console.log(person); // {name: 'kim', address: {city: 'Seoul'}}
```