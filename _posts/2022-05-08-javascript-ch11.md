---
layout: post
title: Chapter 11장 원시 값과 객체의 비교
date: 2022-05-08 20:41:00 +0900
category: Javascript-Deep-Dive
---
# 11장 원시 값과 객체의 비교

데이터 타입(숫자, 문자열, 불리언, null, undefined, 심벌, 객체)

`원시 타입`과 `객체 타입`으로 나누며 서로 다른점

- 원시 타입의 값, 즉 `원시 값은 변경 불가능한 값`이다. 이에 비해 객체(참조) 타입의 값, 즉 `객체는 변경 가능한 값`이다.
- 원시 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장된다. 이에 비해 객체를 변수에 할당하면 변수(확보된 메모리 공간)에는 참조 값이 저장된다.
- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달된다. 이를 값에 의한 전달이라 한다. 이에 비해 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달된다. 이를 참조에 의한 전달이라 한다.

### 11.1 원시 값

### 11.1.1 변경 불가능한 값

원시 값은 변경 불가능한 값, 읽기 전용 값

여기서 변경 불가능한 값은 말 그대로 값에 해당한다. 변수가 아님!

원시 값은 변경 불가능한 값이지만

변수는 재할당이 가능함

상수는 재할당이 금지된 변수

변수 선언과 재할당

![img-ch11-1.png](/public/img/posts/javascript-deep-dive/img-ch11-1.png)

변수에 재할당이 일어나면 변수가 참조하는 메모리의 주소가 바뀔 뿐이다

이런 값의 특성을 `불변성`이라고 하며 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경 할 수 없다.

### 11.1.2 문자열과 불변성

ECMAScript 사양에는 문자열은 2바이트 숫자는 8바이트로 규정하고 있음

문자열 1개 문자는 2바이트 10개 문자면 20바이트

즉, 문자가 많으면 바이트도 늘어남

**문자열은 원시 타입, 유사 배열 객체, 이터러블임**

```jsx
var str = 'string';

str[0] = 'S';
console.log(str); // string

문자열은 변경 불가능한 값
```

### 11.1.3 값에 의한 전달

```jsx
var score = 80;
var copy = score;

score = 100;

console.log(score); // 100;
console.log(copy); // ???
```

**변수에 변수를 할당했을 때 무엇이 어떻게 전달되는가???**

할당 받는 변수(copy)에는 할당되는 변수(score)의 원시 값이 복사되어 전달된다.(값에 의한 전달)

![img-ch11-2.png](/public/img/posts/javascript-deep-dive/img-ch11-2.png)

                                                                      값에 의한 전달

![img-ch11-3.png](/public/img/posts/javascript-deep-dive/img-ch11-3.png)

                                      값에 의해 전달된 값은 다른 메모리 공간에 저장된 별개의 값

따지고 보면 변수에는 값이 전달되는 것이 아니라 `메모리 주소가 전달`된다.

요약하면

변수(copy)에 원시 값을 갖는 변수(score)를 할당하면 두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없다

### 11.2 객체

객체의 특징

- 프로퍼티의 개수가 정해져 있지 않다
- 동적으로 추가되고 삭제할 수 있다
- 프로퍼티 값에 제약이 없다
- 메모리 공간의 크기를 사전에 정해 둘 수 없다

### 11.2.1 변경 가능한 값

객체(참조) 타입의 값. 객체는 변경 가능한 값

```jsx
var person = {
	name : 'Kim'
};
```

객체를 할당한 변수에는 생성된 객체가 실제로 저장된 메모리 공간의 주소가 저장되어 있음(참조 값)

![img-ch11-4.png](/public/img/posts/javascript-deep-dive/img-ch11-4.png)

**person 변수는 객체 {name : ‘Kim’}을 참조하고 있다**

객체는 변경 가능한 값

```jsx
var person = {
	name : 'Kim'
};

// 프로퍼티 값 갱신
person.name = 'Kang';

// 프로퍼티 동적 생성
person.address = 'Seoul';
```

메모리에 저장된 객체를 직접 수정함

![img-ch11-5.png](/public/img/posts/javascript-deep-dive/img-ch11-5.png)

객체는 원시 값과 다르게 여러 개의 식별자로 하나의 객체를 공유 할 수 있음

※참고

얕은 복사 : 한 단계까지만 복사하는 것

깊은 복사 : 객체에 중첩되어 있는 객체까지 모두 복사하는 것

### 11.2.2 참조에 의한 전달

```jsx
var person = {
	name : 'Kim'
};

// 얕은 복사
var copy = person;
```

원본의 참조 값이 복사되어 전달

![img-ch11-6.png](/public/img/posts/javascript-deep-dive/img-ch11-6.png)

메모리는 달라도 참조 값은 같음

두 개의 식별자가 하나의 `객체를 공유`

공유하므로 식별자가 객체를 변경하면 다른 식별자에도 영향을 끼침

```jsx
var person = {
	name : 'Kim'
};

// 참조 값을 복사(얕은 복사)
var copy = person;

// copy와 person은 동일한 객체를 참조
console.log(copy === person); // true

copy.name = 'Kang';
person.address = 'Seoul';

console.log(person); // {name: 'Kang', address: 'Seoul'}
```