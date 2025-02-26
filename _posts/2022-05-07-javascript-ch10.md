---
layout: post
title: Chapter 10장 객체 리터럴
date: 2022-05-07 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 10장 객체 리터럴

**10.1 객체란?**

자바스크립트는 객체 반의 프로그래밍 언어로 구성하고 있는 대부분이 객체임

객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성됨

![img-ch10-1.png](/public/img/posts/javascript-deep-dive/img-ch10-1.png)

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티로 사용될 수 있음

함수도 프로퍼티 값으로 사용할 수 있는데, 이를 메서드라고 함

![img-ch10-2.png](/public/img/posts/javascript-deep-dive/img-ch10-1.png)

`프로퍼티` : 객체의 상태를 나타내는 값(data)

`메서드` : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작

객체는 프로퍼티와 메서드로 구성된 집합체이며 상태와 동작을 하나의 단위로 구조화할 수 있음

**10.2 객체 리터럴에 의한 객체 생성**

자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 생성 방법을 지원함

- 객체 리터럴 (가장 간단하고 일반적임)
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스

**10.3 프로퍼티**

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성됨

프로퍼티를 나열할 때는 쉼표(,)로 구분하고 마지막에는 사용을 하지 않아도 됨

- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로 식별자 역할을 함

보통은 식별자 네이밍 규칙을 따르되, 따르지 않아도 될 때는 이름에 따옴표를 사용해야 함

```jsx
var person = {
	firstName : 'Ung-mo', //식별자 네이밍 규칙을 준수하는 프로퍼티 키
	'last-name' : 'Lee' //식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};
console.log(person);
```

문자열로 프로퍼티 키를 동적으로 생성도 가능하지만 프로퍼키 키로 사용할 표현식을 대괄호([…])로 묶어야 함

```jsx
var obj = {};
var key = 'hello';

//ESS : 프로퍼티 키 동적 생성
obj[key] = 'world';
//ES6 : 계산된 프로퍼티 이름
//var obj = { [key] : 'world' };

console.log(obj); //{ hello : 'world' }
```

빈 문자열을 프로퍼티 키로 사용해도 에러는 발생하지 않으나 키로서의 의미를 갖지 못해 권장하지 않음

```jsx
var foo = {
	'': '' //빈 문자열도 프로터피 키로 사용할 수 있음
};
console.log(foo); //{"":""}
```

프로퍼티 키에 문자열이나 심벌 값 이외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 됨

예약어를 사용해도 에러가 발생하지 않음

이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어써 에러가 발생하지 않음

**10.4 메서드**

자바스크립트의 함수는 객체라 함수는 값으로 취급할 수 있어 프로퍼티 값으로 사용이 가능함 

프로퍼티 값이 함수일 경우 함수와 구분하기 위해 메서드라고 함 즉, 메서드는 객체에 묶여있는 함수를 의미함

```jsx
var circle = {
  radius : 5, //프로퍼티

	//원의 지름
	getDiaeter: function(){ //메서드
		return 2 * this.radius;//this는 circle을 가리킴
	}
};
console.log(circle.getDiameter); //10
```

 메서드 내부에서 사용한 this 키워드는 객체 자신을 가리킴

**10.5 프로퍼티 접근**

프로퍼티 키가 식별자 네이밍 규칙을 준수하고 사용 가능한 유효한 이름이면 아래 기법을 모두 사용 가능

- 마침표 프로퍼티 접근 연산자(…)를 사용하는 마침표 표기법
- 대괄호 프로퍼티 접근 연산자([ … ])를 사용하는 대괄호 표기법

접근 연산자의 좌측에는 객체로 평가되는 표현식을 기술함

```jsx
var person = {
	name : 'Lee'
};

//마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); //Lee

//대괄호 표기법에 의한 프로퍼티 접근
console.log(person.['name']); //Lee
```

대괄호 표기법을 사용하는 경우 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 함

이 때 만약 대괄호 연산자 내 따옴표로 감싸지 않은 이름이 있다면 식별자로 인식해 ReferenceError를  반환함

객체에 존재하지 않는 프로퍼티에 접근하면 undefined랑 반환함

**10.6 프로퍼티 값 갱신**

이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신됨

```jsx
var person = {
	name : 'Lee'
};

//person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티 값이 갱신
person.name = 'Kim';

console.log(person); //{name : 'Kim'}
```

**10.7 프로퍼티 동적 생성**

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당됨

```jsx
var person = {
	name : 'Lee'
};

//person 객체에는 age 프로퍼티가 존재하지 않아 
//person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당됨
person.age = '20';

console.log(person); //{name: 'Lee', age: 20}
```

**10.8 프로퍼티 삭제**

delete 연산자는 객체의 프로퍼티를 삭제함 만약 존재하지 않는 프로퍼티를 삭제 시 에러없이 무시됨

```jsx
var person = {
	name : 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

//person 객체에 age 프로퍼티가 존재
//따라서 delete 연산자로 age 프로퍼티를 삭제 가능
delete person.age;

//person 객체에 address 프로퍼티가 존재하지않음
//따라서 delete 연산자로 address 프로퍼티를 삭제하면 무시됨
delete person.address;

console.log(person); //{name: 'Lee'}
```

**10.9 ES6에서 추가된 객체 리터럴의 확장 기능**

**10.9.1 프로퍼티 축약 표현**

객체 리터럴의 프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성됨

프로퍼티 값은 변수에 할당된 값이며 식별자 표현식일 수도 있음

```jsx
//ES5
var x = 1, y = 2;

var obj = {
 x: x,
 y: y
};

console.log(obg); //{x: 1, y: 2}
```

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략 할 수 있음, 이때 프로퍼티 키는 변수 이름으로 자동생성

```jsx
//ES6
var x = 1, y = 2;

//프로퍼티 축약 표현
const obj = {x, y};

console.log(obg); //{x: 1, y: 2}
```

**10.9.2 계산된 프로퍼티 이름**

문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있음

프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야 함, 이를 계산된 프로퍼티 이름이라고 함

ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적으로 생성하려면 객체 리터럴 외부에서 대괄호([…]) 표기법을 사용해야 함

```jsx
//ES5
var prefix = 'prop';
var i = 0;

var obj = {};

//계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] =  i;
obj[prefix + '-' + ++i] =  i;
obj[prefix + '-' + ++i] =  i;

console.log(obj) // {prop-1: 1, prop-2: 2, prop-3: 3}

//ES6
const prefix = 'prop';
let i = 0;

//객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i
}

console.log(obj); //{prop-1: 1, prop-2: 2, prop-3: 3}
```

**10.9.3 메서드 축약 표현**

ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당함

ES6에서 메서드를 정의할 때 function 키워드를 생략한 축약 표현 사용가능

```jsx
//ES5
var obj = {
	name: 'Lee',
	sayHi : fiction(){
		console.log('Hi: '+ this.name);
	}
};

obj.sayHi(); //Hi Lee

//ES6
var obj = {
	name: 'Lee',
	//매서드 축약 표현
	sayHi() {
		console.log('Hi: '+ this.name);
	}
};

obj.sayHi(); //Hi Lee
// 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작
```