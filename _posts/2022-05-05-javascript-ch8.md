---
layout: post
title: Chapter 8장 제어문
date: 2022-05-05 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 8장 제어문

제어문은 조건에 따라 코드 블록을 실행하거나 반복 실행할 때 사용함

**8.1 블록문**

`블록문`은 0개 이상의 문을 중괄호로 묶은 것으로 코드 블록, 블록이라고 부름

자바스크립트에서는 블록문을 하나의 실행 단위로 취급함, 주로 제어문이나 함수를 정의할 때 사용함

**블록문 끝에는 세미콜론을 붙이지 않음**

```jsx
//블록문
{
	var foo = 10;
}

//제어문
var x = 1;
if(x < 10){
	x++;
}

//함수 선언문
function sum(a, b){
	return a + b;
}
```

**8.2 조건문**

**8.2.1 if .. else 문**

`if..else 문`은 참 거짓에 따라 실행할 코드 블록을 결정함

조건식의 평과 결과가 true일 경우 if 문의 코드 블록이 실행, false일 경우 else 문의 코드 블록이 실행됨

```jsx
if(조건식){
	// 조건식이 참이면 코드블록 실행
}else{
	// 조건식이 거짓이면 코드블록 실행
}
```

if문의 조건식은 불리언 값으로 평가되며, 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환됨

조건을 추가하면 실행될 코드 블록을 else if문을 통해 사용

```jsx
if(조건식1){
	// 조건식1이 참이면 코드블록 실행
}else if(){
	// 조건식2가 참이면 코드블록 실행
}else{
	//조건식1과 조건식2가 거짓이면 코드블록 실행
}
```

코드 블록 내의 문이 하나뿐이면 중괄호를 생략 가능함

if..else문은 삼항 조건 연산자로도 바꿔서 사용이 가능함

```jsx
var x = 2;
//0은 false로 취급
var result = x % 2 ? '홀수' : '짝수';
console.log(result); //짝수

// 조건이 추가된 삼항연산자
var num = 2;
//0은 false로 취급
var kind = num ?(num > 0 ? '양수' : '음수') : '영';
console.log(kind); //양수
```

삼항 조건 연산자 표현식은 값처럼 사용이 가능해 변수에 할당할 수 있으나 if..else문은 표현식이 아닌 문이라 변수에 할당 불가함

**8.2.2 switch문**

`switch문`은 주어진 표현식을 평가해 값과 일치하는 표현식을 갖는 case문을 실행함

```jsx
switch(표현식){
	case 표현식1:
		switch 문의 표현식과 표현식1이 일치하면 실행될 문;
		break;
	case 표현식2:
		switch 문의 표현식과 표현식2이 일치하면 실행될 문;
		break;
	default:
		switch 문의 표현식과 일치하는 case문이 없을 때 실행될 문;
}
```

switch 문 사용 시는 break 문을 잘 사용 해야 함

```jsx
//break 문 미 사용 시
var month = 1;
var monthName;

switch (month) {
	case 1 : monthName = 'January';
	case 2 : monthName = 'February';
	case 3 : monthName = 'march';
	default : monthName = 'Invalid month';
}

console.log(monthName); //Invalid month

//break 문 사용 시 
var month = 1;
var monthName;

switch (month) {
	case 1 : monthName = 'January';
		break;
	case 2 : monthName = 'February';
		break;	
	case 3 : monthName = 'march';
		break;
	default : monthName = 'Invalid month';
}

console.log(monthName); //January
```

**8.3 반복문**

**8.3.1 for 문**

조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행함

```jsx
for(변수선언문 또는 할당문; 조건식; 증감식){
	조건식이 참인 경우 반복 실행될 문;
}

for(var i = 0; i < 2; i++){
	console.log(i);
}
결과 값 : 0
				1

for(var i = 1; i <= 6; i++){
	for(var j = 1; j <= 6; j++){
		if(i + j === 6){
			console.log(`[${i}, ${j}]`);
		}
	}
}
결과 값 : [1, 5]
				[2, 4]
				[3, 3]
				[4, 2]
				[5, 1]
```

**8.3.2 while 문**

주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행함

for 문은 반복 횟수가 명확할 때 사용하고 while 문은 반복 횟수가 불명확할 때 사용함

while 문은 조건문의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료하고 결과가 불리언 값이 아니면 강제 변환해 참, 거짓을 구별함

조건식의 평가 결과가 참이면 무한 루프가 됨

```jsx
var count = 0;
// count가 3보다 작을 때까지 코드 블록을 반복
while(count < 3){
	console.log(count); //0 1 2
	count++;
}

var count = 0;
//무한루프
while(true){
	console.log(count);
	count++;
	
	//count가 3이면 코드 블록을 탈출
	if(count === 3)break;
}// 0 1 2
```

**8.3.3 do…while문**

`do…while문`은 코드 블록을 먼저 실행하고 조건식을 평가해 무도건 한번 이상은 코드블록 실행을 함

```jsx
var count = 0;

//count가 3보다 작을 때까지 코드 블록을 계속 반복 실행
do{
	console.log(count);// 0 1 2
	count++;
}while (count < 3);
```

**8.4 break 문**

`break 문`은 반복문, switch 문 코드 블록을 탈출하기 위해 사용됨

또 반복문을 더 이상 진행하지 않아도 될 때 불필요한 반복을 회피할 수 있음

```jsx
var string = 'Hello';
var search = 'l';
var index;

//문자열은 유사 배열이므로 for 문으로 순회
for(var i = 0; i < string.length; i++){
	//문자열의 개별 문자가 'l'이면
	if( string[i] === search){
		index = i;
		break; //반복문을 탈출
	}
}
console.log(index); //2
```

**8.5 continue 문**

`continue`문은 반복문의 코드 블록 실행을 현재 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킴, break 문처럼 반복문을 탈출하는 것은 아님

```jsx
var string = 'Hello';
var search = 'l';
var index;

//문자열은 유사 배열이므로 for 문으로 순회
for(var i = 0; i < string.length; i++){
	//'l'이아니면 현재 지점에서 실행을 중단하고 반복문의 증감식으로 이동
	if( string[i] === search) continue; 
		count++; //continue 문이 실행되면 이 문은 실행되지 않음
}
console.log(index); //3

//참고로 String.prototype.match 메서드 사용해도 같은 동작을 함
const regexp = new.RegExp(search, 'g');
console.log(string.match(regexp).length);//3
```