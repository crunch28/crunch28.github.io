---
layout: post
title: Chapter 14장 전역변수의 문제점
date: 2022-07-20 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 14 전역변수의 문제점

### 14.1 변수의 생명 주기

### 14.1.1 지역 변수의 생명 주기

함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸함

```javascript
function foo() {
	var x = 'local';
	console.log(x);
	return x;
}

foo(); // local
console.log(x); // Uncaught ReferenceError: x is not defined
```

![img-ch14-1.png](/public/img/posts/javascript-deep-dive/img-ch14-1.png)

`지역 변수의 생명 주기`는 `함수의 생명 주기`와 일치

### 14.1.2 전역 변수의 생명 주기

전역 코드는 명시적인 호출 없이 실행

`전역 변수의 생명 주기`는 `전역 객체의 생명 주기`와 일치

![img-ch14-2.png](/public/img/posts/javascript-deep-dive/img-ch14-2.png)

### 14.2 전역 변수의 문제점

- 암묵적 결합

   코드 어디서든 참조하고 할당할 수 있는 변수를 사용함

   변수의 유효 범위가 크면 클수록 코드의 가독성은 나빠지고 상태가 변경될 수 있음

- 긴 생명 주기

   전역 변수는 생명 주기가 길어서 메모리 리소스 소비가 길음

   같은 변수 이름으로 인한 의도치 않은 재할당

- 스코프 체인 상에서 종점에 존재

   전역 변수는 스코프 체인 상에서 종점에 존재

   전역 변수의 검색 속도가 가장 느림

- 네임스페이스 오염

   파일이 분리 되어 있다 해도 하나의 전역 스코프를 공유

### 14.3 전역 변수의 사용을 억제하는 방법

전역 변수를 반드시 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용해야 함

변수의 스코프는 좁을수록 좋음

### 14.3.1 즉시 실행 함수

모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 됨

라이브러리 등에 자주 사용됨

### 14.3.2 네임스페이스 객체

전역에 네임스페이스 역할을 담당하는 객체를 생성하고 변수를 프로퍼티로 추가

```javascript
var MYAPP = {};

MYAPP.age = 22;
MYAPP.person = {
	name : 'Lee',
	address : 'Seoul'
};
```

### 14.3.3 모듈 패턴

클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만듦

```javascript
var Counter = (function (){
	// private 변수
	var num = 0;
	
	// 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환
	return {
		increase(){
			return ++num;
		},
		decrease(){
			return --num;
		}
	};
}());

console.log(Counter.num);        // undefined
console.log(Counter.increase()); // 1
console.log(Counter.decrease()); // 0
```

모듈 패턴은 전역 네임스페이스의 오염을 막는 기능은 물론 한정적이기는 하지만 `정보 은닉`을 구현하기 위해 사용

### 14.3.4 ES6 모듈

ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공함

script 태그에 type=”module” 속성 추가. 모듈의 확장자는 .mjs

```html
<script type="module" src="lib.mjs"></script>
```

트랜스파일링이나 번들링 필요함

Webpack 등의 모듈 번들러 사용