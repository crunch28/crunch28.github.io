---
layout: post
title: Chapter 2장 변수
date: 2022-07-17 19:20:23 +0900
category: Java
---
## 1. 변수(variable)와 상수
### 1.1 변수란?
단 하나의 값을 저장할 수 있는 메모리상의 공간을 의미

### 1.2 변수의 선언과 초기화
변수의 선언방법

```java
  int age;
```
int는 변수타입<br>
age는 변수이름

`변수타입` 변수에 저장될 값이 어떤 타입인지를 지정하는 것<br>
`변수이름` 변수에 붙인 이름<br>
변수를 선언하면, 메모리의 공간에 `변수타입`에 알맞은 크기의 저장공간이 확보되고, `변수이름`을 통해 사용할 수 있음

###### 변수의 초기화

변수는 반드시 선언 이후 `초기화`를 먼저 해야함<br>
이유는 쓰레기값이 남아있을 수 있음

```java
  // 변수 선언 유형. 한줄에 하나씩 또는 CSV로 한줄에 쫘르르르
  int cnt = 0;
  int cntSum = 0;
  
  boolean isNull, isEmpty;
```

###### 두 변수의 값 교환하기
```java
  남궁성 코드 받기
```

### 1.3 변수의 명명규칙
모든 이름(변수, 메서드, 클래스 등등)은 `식별자`

식별자 규칙

1. 대소문자가 구분되며 길이에 제한이 없다.
  - True와 true는 서로 다른 것으로 간주
2. 예약어를 사용해서는 안 된다.
  - true는 예약어라서 사용할 수 없지만, True는 가능
3. 숫자로 시작해서는 안 된다.
  - top10은 허용하지만, 7up은 허용되지 않음
4. 특수문자는 '_'와 '$'만을 허용한다.
  - $harp은 허용되지만, S#arp은 허용되지 않음

예약어 종류
|:--:|:--:|:--:|:--:|:--:|
|abstrct|default|if|package|this|
|assert|do|goto|private|throw|
|boolean|double|implements|protected|throws|
|brek|else|import|public|transient|
||||||
||||||
||||||
||||||
||||||