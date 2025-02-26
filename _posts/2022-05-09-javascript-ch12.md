---
layout: post
title: Chapter 12장 함수
date: 2022-05-09 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 12장 함수

**12.1   함수란?**

`함수`는 자바스크립트에서 가장 중요한 핵심 개념으로 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것임

![img-ch12-1.png](/public/img/posts/javascript-deep-dive/img-ch12-1.png)

- 매개변수 :  함수 내부로 입력을 전달받는 변수
- 인수 : 입력하는 값
- 반환값 : 출력하는 값
- 함수 정의 : 함수 생성
- 함수 호출 : 인수를 매개변수를 통해 함수에 전달하며 함수의 실행을 명시적으로 지시하는 것, 호출이 되면 코드 블록에 담긴 문들이 일괄 실행되고 결과값(반환값)을 반환함

```jsx
//함수 정의
function add(x, y){
  return x + y;
}

//함수 호출
var result = add(10, 20);

//반환값 30을 반환
console.log(result); //30
```

**12.2 함수를 사용하는 이유**

- 함수는 여러번 호출이 가능해서 동일한 작업을 반복적으로 수행해야 한다면 정의된 함수를 재사용하는 것이 효율적임
- 함수는 몇 번이든 호출할 수 있으므로 중복성을 줄이고 코드의 재사용이 가능함
- 유지보수의 편의성이 높아짐
- 실수를 줄여 코드의 신뢰성이 높아짐
- 코드의 가독성을 향상시킴

**12.3 함수 리터럴**

자바스크립트의 함수는 객체 타입의 값이라 `함수 리터럴`로 생성이 가능함

리터럴은 값을 생성하기 위한 표기법으로 함수 리터럴도 평가되어 값을 생성함, 이 값은 객체임

함수는 객체지만 일반 객체와는 다르게 호출이 가능함

![img-ch12-2.png](/public/img/posts/javascript-deep-dive/img-ch12-2.png)

**12.4 함수 정의**

함수 정의란 함수를 호출하기 이전에 인수를 전달받을 매개변수와 실행할 문들, 반환할 값을 지정하는 것을 말함

정의된 함수는 자바스크립트 엔진에 의해 평가되어 식별자가 암묵적으로 생성되고 함수 객체가 할당됨

![img-ch12-3.png](/public/img/posts/javascript-deep-dive/img-ch12-3.png)

**12.4.1 함수 선언문**

```jsx
//함수 선언문
function add(x, y){
	return x + y;
}
```

함수 선언문은 함수 리터럴과 형태는 동일하지만 함수 이름을 생략할 수 없음

함수 선언문은 표현식이 아닌 문으로, 개발자 도구 콘솔에서 실행 시 완료 값은 undefined가 출력됨

이름이 있는 기명 함수 리터럴은 코드의 문맥에 따라 함수 선언문 또는 함수 리터럴 표현식으로 해석됨

```jsx
// 기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석됨
// 함수 선언문에서는 함수 이름을 생략할 수 있음
// foo는 자바스크립트가 암묵적으로 생성한 식별자임
function foo(){
	console.log('foo');
}

foo(); //foo

//함수 리터럴을 피연산자로 사용하면 함수 선언문이 아닌 함수 리터럴 표현식으로 해석됨
//함수 리터럴에서는 함수 이름을 생략 가능함
//함수 이름 bar는 함수 몸체 내에서만 참조할 수 있는 식별자이기 때문에 호출이 안됨
(function bar(){
	console.log('foo');
});

bar(); // ReferenceError: Bar is not defind
```

자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당함

![img-ch12-4.png](/public/img/posts/javascript-deep-dive/img-ch12-4.png)

함수는 함수 이름으로 호출하는 것이 아닌 함수 객체를 가리키는 식별자로 호출이 됨

**12.4.2 함수 표현식**

자바스크립트의 함수는 객체 타입의 값으로 값처럼 변수에 할당을 하거나 프로퍼티 값, 배열의 요소가 될 수 있는데 이렇게 성질을 갖는 객체를 일급 객체라고 함

함수가 일급 객체라는 것은 함수를 값처럼 자유롭게 사용할 수 있다는 의미임

```jsx
// 함수 표현식, 기명 함수 표현식
var add = function foo(x, y){
	return x + y;
}

//함수 객체를 가르키는 식별자로 호출됨
console.log(add(2, 5)); //7

// 함수 이름으로 호출하면 ReferenceError 발생
console.log(foo(2, 5)); // ReferenceError: Bar is not defind

```

함수 리터럴의 함수 이름은 생략할 수 있어서 익명 함수라고 함

함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자를 사용해야 함, 함수 이름은 함수 몸체 내부에서만 유효한 식별자여서 호출을 못함

**12.4.3 함수 생성 시점과 함수 호이스팅**

함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있지만, 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출을 할 수 없음

이는 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 달라서임

함수 선언문이 코드 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 함수 호이스팅이라고 함

var 키워드로 선언된 변수는 undefined로 초기화가 되고, 함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화 되며 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출이 됨

함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아닌 변수 호이스팅이 발생함

**12.4.4 Function 생성자 함수**

생성자 함수는 객체를 생성하는 함수를 말함

자바스크립트가 기본으로 제공하는 빌트인 함수인 Function 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달하면서, new 연산자와 함께 호출하면 함수 객체를 생성해서 반환함

```jsx
var add = new Function('x' , 'y', 'return x + y');

console.log(add(2, 5)); //7
```

생성자 함수로 함수를 생성하는 것과 함수 선언문이나 함수 표현식으로 생성한 함수가 동일하게 동작하지 않아서 생성자 함수로 함수를 생성하는 방식은 바람직하지 않음

**12.4.5 화살표 함수**

ES6에서 도입된 화살표 함수는 function 키워드 대신 화살표(⇒)를 사용해 간략한 방법으로 함수를 선언함

화살표 함수는 익명으로 함수를 정의하고 기존 함수보다 표현과 내부 동작 모두가 간략화 되어 있음

```jsx
const add = (x, y) => x + y;
console.log(add(2, 5);
```

화살표 함수는 생성자 함수로 사용할 수 없으며, 기존 함수와 this 바인딩 방식이 다르고 prototype 프로퍼티가 없으며 argument 객체를 생성하지 않음

### 12.5 함수 호출

함수의 형태 : 함수 식별자 ( 인수는 CSV로 표현 );

ex) add(1, 2);

함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮김

### 12.5.1 매개변수의 인수

- 함수 외부에서 내부로, 매개변수를 통해 인수(표현식)를 전달
- 인수는 개수와 타입에 제한이 없음

```jsx
// 함수 선언문
function add(x, y){ X와 Y는 매개변수
	return x + y;
}

// 함수 호출
var result = add(1, 2); 1, 2 는 인수
```

매개변수는 함수를 정의할 때 선언 > 함수 내부에서 매개변수가 생성 > undefined로 초기화 > 인수가 매개변수로 할당

![img-ch12-5.png](/public/img/posts/javascript-deep-dive/img-ch12-5.png)

- 매개변수는 함수 외부에서 참조를 못함
- 매개변수의 개수와 인수의 개수는 달라도 오류가 아님

```jsx
function add(x, y){
	console.log(arguments);
	return x + y;
}

add(2);      // NaN  x=2, y=undefined
add(1, 2, 3) // 3    x=1, y=2

3은 어디갔느냐? 버려진게 아니라 arguments 객체에 남아있음
```

### 12.5.2 인수 확인

```jsx
function add(x, y){ <- 자료형이 정해진게 아님
	return x + y;     <- 자료형이 정해진게 아님
}

add(2);        // NaN
add('a', 'b'); // ab
```

오류가 아님 이유는?

1. 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
2. 자바스크립트는 동적 타입 언어다. 따라서 자바스크립트 함수는 매개변수의 타입을 사전에 지정할 수 없다.

```jsx
function add(x, y){

	if(typeof x !== 'number' || typeof y !== 'number'){
		throw new TypeError('인수는 모두 숫자로 해야함');
	}

	return x + y;
}

// 단축평가 사용
function add(x, y){

x = x || 0;
y = y || 0;

	return x + y;
}

// ES6 매개변수 기본값 설정
function add(x = 0, y = 0){
	return x + y;
}
```

### 12.5.3 매개변수의 최대 개수

**이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 한다!**

이상적인 매개변수 개수는 0개이며 적을수록 좋음! 최대 3개 이상은 넘지 말기를!

매개변수가 많으면 순서에 신경써야함

객체를 인수로 사용 가능함

```jsx
$.ajax({
	method : 'POST',
	url : '/user',
	data : {id: 1, name: 'kim'},
	cache : false
});
```

### 12.5.4 반환문

return 키워드와 표현식으로 함수 외부로 반환함

```jsx
function add(x, y){
	return x + y;  <- 표현식(값으로 평가되는거) 다 들어감
}

var result = add(1, 2);
console.log(result); // 3
```

함수 호출은 표현식으로 return 값을 평가

반환문의 역할

1. 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다.
2. 반환문은 return 키워드 뒤에 오는 표현식을 평가해 반환한다.

```jsx
function add(x, y){
	return x + y;
	console.log(x); //  안찍힘
}

function add(x, y){
	return;
} // undefined 반환
function add(x, y){

} // undefined 반환

// 전역에서 사용할 경우
return; // SyntaxError: Illegal return statement 문법오류!
```

### 12.6 참조에 의한 전달과 외부 상태의 변경

매개변수도 타입에 따라 값에 의한 전달, 참조에 의한 전달로 나뉨

```jsx
function changeValue(num, obj){
	num += 100;
	obj.name = 'Kim';
}

var num = 100;
var person = {name : 'Lee'};

changeValue(num, person);

console.log(num); // 100
console.log(person); // name : 'kim'
```

원시 값은 재할당. 값 자체를 전달.  부수효과 없다

객체는 재할당 없이 변경가능. 참조 값을 전달. 부수효과 발생

부수효과 발생으로 객체의 값이 변경될 수 있음

해결법은 깊은 복사를 통해 새로운 메모리에 객체를 할당

### 12.7 다양한 함수의 형태

### 12.7.1 즉시 실행 함수

**함수 정의와 동시에 즉시 호출하는 함수**

단 한번만 호출되며 다시 호출할 수 없음

```jsx
모양
(function(){}());

(function(a, b){
	return a + b;
}(1, 2));
```

다른 사용의 예

```jsx
(function(){}());  // 가장 일반적인 부분

(function(){})();

!function(){}();

+function(){}();
```

즉시 실행 함수 내에 코드를 모아두면 변수나 함수 이름의 충돌을 방지함

### 12.7.2 재귀 함수

**함수가 자기 자신을 호출하는 함수**

```jsx
var factorial = function test(n){
	if(n <= 1) return 1;
	console.log(n);
	return n * factorial(n - 1); // 함수 이름(factorial) 또는 식별자(test)를 호출
}

factorial(5); // 5 * 4* 3 * 2 * 1
```

![img-ch12-6.png](/public/img/posts/javascript-deep-dive/img-ch12-6.png)

재귀 함수는 탈출 조건을 반드시 만들어야 함

탈출 조건이 없으면 스택 오버 플로우 에러가 발생

그래서 조심해서 사용해야함 for문이나 while문이 있음

### 12.7.3 중첩 함수

**함수 내부에 정의된 함수를 중첩 함수(내부 함수)**

중첩함수를 포함하는 것을 외부함수

중첩 함수는 외부 함수를 돕는 헬퍼 함수

```jsx
function outer(){
	var x = 1;

	function inner(){
		var y = 2;
		console.log(x + y); // 3
	}

inner();

}

outer();
```

### 12.7.4 콜백 함수

**함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수**

매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수

콜백 함수는 고차 함수의 헬퍼 함수

고차 함수는 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출함

콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달 할 수 있음

```jsx
repeat(5, function (i){
	if(i % 2) console.log(i);
}); // 1 3

var logOdds = function (i){
	if(i % 2) console.log(i);
};

repeat(5, logOdds); // 1 3
```

콜백 함수는 비동기 처리 등에 활용

### 12.7.5 순수 함수와 비순수 함수

어떤 외부 상태에 의존하지도 않고 변경하지도 않는(부수 효과가 없는) 함수를 순수함수

외부 상태에 의존하거나 외부 상태를 변경하는(부수 효과가 있는) 함수를 비순수 함수

순수 함수는 최소 하나 이상의 인수를 전달받음

순수 함수는 오직 매개변수를 통해 함수 내부로 전달된 인수에게만 의존해 값을 생성 및 반환

비순수 함수는 외부 상태에 의존하게 되어 반환값이 변할 수 있고, 외부 상태도 변경 할 수 있음

```jsx
var count = 0;

// 순수 함수
function increase(n){
	return ++n;
};

// 비순수 함수
function increase(){
	count = "뭘로 바뀔지 몰랑";
	count += "전역 변수에 할당된 값은 0이지만 내가 여기서 바꿀꺼양~ 이히히";
	return count;
}
```

`우리 개발자들은 비순수 함수를 지양하고 순수 함수를 지향한다!!!`

`복잡성을 줄이자!`