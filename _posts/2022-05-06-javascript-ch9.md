---
layout: post
title: Chapter 9장 타입 변환과 단축 평가
date: 2022-05-06 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 9장 타입 변환과 단축 평가

### 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있음

타입 변환 2가지

- 명시적 타입 변환(타입 캐스팅)
- 암묵적 타입 변환(타입 강제 변환)

```jsx
var x = 10;

// 명시적 타입변환
var str = x.toString();

// 암묵적 타입변환
var str = x + '';
```

`타입 변환`은 기존 원시 값을 직접 변경하지는 않음.

기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것임.

개발할 때, 암묵적으로 변하는지? 변하면 뭘로 되는지 알아야 함

그래서 암묵적 타입 변환과 명시적 타입 변환을 쓴다고 하면 코드를 예측할 수 있어야 함

### 9.2 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 코드의 문맥을 고려해서 **암묵적으로 타입 변환함**

```jsx
'10' + 2 // 102 문자열

5 * 10 // 50 숫자

!0 // true boolean 값
```

`암묵적 타입 변환`이 발생하면 문자열, 숫자, 불리언 등 원시 타입 중 하나로 변환함.

### 9.2.1 문자열 타입으로 변환

 `+ 연산자`는 모든 피연산자를 문자열 타입으로 바꿈

```jsx
// 템플릿 리터럴
`1 + 1 = ${1 +1}`    // "1 + 1 = 2"

//숫자 타입
0 + ''              // "0"
-0 + ''             // "0"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
-Infinity + ''      // "-Infinity"

// 불리언 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// 심벌타입
(Symbol()) + ''     // Uncaught TypeError : Cannot convert a Symbol value to a string

//객체 타입
({}) + ''           // '[object Object]'
Math + ''           // '[object Math]'
[] + ''             // ''
[10, 20] + ''       // '10,20'
(function(){}) + '' // 'function(){}'
Array + ''          // 'function Array() { [native code] }'

기억하자 무언갈 문자열로 바꾸고 싶다면 뒤에 + '' 하자
```

### 9.2.2 숫자 타입으로 변환

`산술 연산자(-, *, /)`는 모든 피연산자를 숫자 타입으로 바꿈

숫자 타입으로 변환이 안되는 것은 NaN 됨

`비교 연산자(<, >, ≤, ≥)`는 모든 피연산자를 숫자 타입으로 바꿈

`단항 연산자(+)`는 피연산자를 숫자 타입으로 바꿈

```jsx
// 문자열 타입
+''             // 0
+'0'            // 0
+'1'            // 1
+'string'       // NaN

// 불리언 타입
+true           // 1
+false          // 0

// null 타입
+null           // 0

// undefined 타입
+undefiend      // NaN

// 심벌타입
+Symbol()       // Uncaught TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}             // NaN
+[]             // 0
+[10, 20]       // NaN
+(function(){}) // NaN

기억하자 무언갈 숫자로 바꾸고 싶다면 앞에 + 하자
```

빈 문자열(’’), 빈 배열([]), null, false는 0

true는 1

객체와 빈 배열이 아닌 배열([1, 2]), undefined는 NaN

### 9.2.3 불리언 타입으로 변환

자바스크립트 엔진은 불리언 타입이 아닌 값을 나눔

- truthy 참으로 평가되는 값
- falsy 거짓으로 평가되는 값

```jsx
// false로 평가
!!false
!!undefined
!!null
!!0
!!NaN
!!''

기억하자 무언갈 불리언 값으로 하고 싶으면 앞에 !! 붙이자
```

 

### 9.3 명시적 타입 변환

1. 표준 빌트인 생성자 함수를 new 연산자 없이 호출하는 방법

Number(’3’)

1. 빌트인 메서드를 이용하는 방법

parseInt(’3’)

### 9.3.1 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toString 메서드를 사용하는 방법
3. 문자열 연결 연산자(+)를 이용하는 방법

```jsx
// 1. String 생성자 함수를 new 없이 사용
String(1);    // '1'
String(NaN);  // 'NaN'
String(true); // 'true'

// 2. Object.prototype.toString 메서드를 사용
(1).toString();    // '1'
(NaN).toString();  // 'NaN'
(true).toString(); // 'true'

// 3. 문자열 연산자 사용
1 + '';     // '1'
NaN + '';   // 'NaN'
true + '';  // 'true'
```

### 9.3.2 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. 단항 산술 연산자(+)를 이용하는 방법
4. 산술 연산자(*)를 이용하는 방법

```jsx
// 1. Number 생성자 함수를 new 없이 사용
Number('0');    // 0
Number('10.1'); // 10.1
Number(true);   // 1

// 2. parseInt, parseFloat 함수 사용
parseInt('0');  // 0
parseFloat('10.1'); // 10.1

// 3. + 단항 산술 연산자 이용
+'0';    // 0
+'10.1'; // 10.1
+true;   // 1

// 4. * 산술 연산자 이용
'0'*1;   // 0
'10.1'*1 // 10.1
true*1;  // 1
```

### 9.3.3 불리언 타입으로 변환

1. boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. ! 부정 논리 연산자를 두번 사용하는 방법

```jsx
// 1. boolean 생성자 함수를 new 없이 사용
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true

Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true

Boolean(null);      // false
Boolean(undefined); // false

Boolean({});        // true
Boolean([]);        // true

// 2. ! 부정 논리 연산자를 두번 사용
!!'x';              // true
!!'';               // false
!!'false';          // true

!!0;                // false
!!1;                // true
!!NaN;              // false
!!Infinity;         // true

!!null;             // false
!!undefined         // false

!!{};               // true
!![];               // true
```

### 9.4 단축 평가

### 9.4.1. 논리 연산자를 사용한 단축 평가

```jsx
'Cat' && 'Dog'  // Dog
```

논리곱(&&) 연산자는 피연산자 모두가 true일 때 true임.

**그래서 왼쪽에서 오른쪽으로 진행하면서 맨 마지막에 있는 truthy 값을 반환함**

```jsx
'Cat'|| 'Dog'   // Cat 
```

논리합(||) 연산자는 피연산자 중 하나가 true이면 true임.

**그래서 왼쪽에서 오른쪽으로 진행하면서 truthy 값이 있으면 그걸 반환함**

`단축 평가` **: 논리곱(&&), 논리합(||) 연산자는 논리 연산의 결과를 결정하는 피연산자를 그대로 반환함. 평가가 결정되면 나머지 평가는 생략함.**

단축 평가 규칙

| 단축 평가 표현식 | 평가 결과 |
| --- | --- |
| true || anything | true |
| false || anyting | anyting |
| true && anyting | anyting |
| false && anyting | false |

```jsx
// 논리합(||) 연산자
'Cat' || 'Dog';    // 'Cat'
false || 'Dog';    // 'Dog'
'Cat' || false;    // 'Cat'

// 논리곱(&&) 연산자
'Cat' && 'Dog';    // 'Dog'
false && 'Dog';    // false
'Cat' && false;    // false
```

**단축 평가를 사용하면 if문을 대체 가능하다!!**

```jsx
// 1. 논리곱 &&
var done = true;
var message = '';

// 기존의 if문
if(done) message = '완료';

// 단축 평가
message = done && '완료';

// 2. 논리합 ||
var done = false;
var message = '';

// 기존 if문
if(!done) message = '미완료';

// 단축 평가
message = done || '미완료';

// 3. 삼항 조건 연산자
var done = true;
var message = '';

// 기존 if문
if(done) message = '완료';
else message = '미완료';

// 단축 평가
message = done ? '완료' : '미완료';

```

그러면 실제 실무에서 어떻게 사용할까?

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```jsx
var elem = null;

var value = elem && elem.value;  // null
```

- 함수 매개변수에 기본값을 설정할 때

```jsx
function test(str){
	str = str || '';
	console.log(str);
}

// ES6
function test2(str = ''){
	console.log(str);
}
```

### 9.4.2 옵셔널 체이닝 연산자

`?.` 

좌항의 피연산자가 null 또는 undefined인 경우 undefiend를 반환하고 아니면 우항의 프로퍼티를 참조

```jsx
var elem = null;
var value = elem?.value;
```

### 9.4.3 null 병합 연산자

`??`

좌항의 피연산자가 null 또는 nudefined인 경우 우항을 반환하고 아니면 좌항을 반환

```jsx
var foo = null ?? 'test';
```