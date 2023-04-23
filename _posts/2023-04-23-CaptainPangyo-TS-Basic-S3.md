---
layout: post
title: "[캡틴판교 TS] 섹션 3. 타입스크립트 기초 - 변수와 함수 타입 정의하기"
date: 2023-04-23 19:48:00 +900
lastmod: 2023-04-23 19:48:00 +900
categories: [STUDY, Captain Pangyo - TS Basic]
tags: [captainpangyo, typescript]
image: 
  path: https://user-images.githubusercontent.com/97720335/233822587-60d294e1-867c-4cc0-b352-26899b803685.png
  width: 700
  alt: 타입스크립트 입문 - 기초부터 실전까지 강의 표지
---

# 변수와 함수 타입 정의하기

## 타입스크립트 기본 타입
타입스크립트의 기본 타입에는 크게 다음 12가지가 있다.
> `Boolean`, `Number`, `String`, `Object`, `Array`, `Tuple`, `Enum`, `Any`, `Void`, `Null`, `Undefined`, `Never`

### 문자열(String)
타입이 문자열인 경우 아래와 같이 선언해서 사용한다.

```ts
// TS 문자열 선언
let str: string = "hello";
```

### 숫자(Number)
타입이 숫자면 아래와 같이 선언한다.

```ts
// TS 숫자 선언
let num: number = 10;
```

### 배열(Array)
타입이 배열인 경우 아래와 같이 선언한다.

```ts
// TS 배열 선언
let arr: Array<number> = [1, 2, 3];
let heroes: Array<string> = ["Capt", "Thor", "Hulk"];
let items: number[] = [1, 2, 3];
```

### 튜플(Tuple)
`튜플` 은 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식을 의미한다.

```ts
// TS 튜플 선언
let address: [string, number] = ["gangnam", 100];
```

### 객체(Object)
타입이 객체인 경우 아래와 같이 선언한다.

```ts
// TS 객체 선언
let obj: object = {};

// let person: object = {
//   name: "capt",
//   age: 100,
// };
let person: { name: string; age: number } = {
  name: "thor",
  age: 1000,
};
```

### 진위 값(Boolean)
타입이 진위 값인 경우에는 아래와 같이 선언한다.

```ts
// TS 진위값
let show: boolean = true;
```

<br>

## 타입스크립트에서의 함수
웹 애플리케이션을 구현할 때 자주 사용되는 함수는 타입스크립트로 크게 다음 3가지 타입을 정의할 수 있다.
- 함수의 파라미터(매개변수) 타입
- 함수의 반환 타입
- 함수의 구조 타입

### 함수의 기본적인 타입 선언
**1. 함수의 파라미터에 타입을 정의하는 방식**

```ts
function sum(a: number, b: number) {
  return a + b;
}
sum(10, 20);
```

**2. 함수의 반환 값에 타입을 정의하는 방식**

```ts
function sum(): number {
  return 10;
}
```

**3. 함수에 타입을 정의하는 방식**

가장 기본적인 방식이며, 함수의 파라미터 와 타입 둘 다 정의한다.

```ts
function sum(a: number, b: number): number {
  return a + b;
}
```

<br>

### 함수의 인자
- 타입스크립트에서는 함수의 인자를 모두 필수 값으로 간주한다.
- 함수의 매개변수를 설정하면 `undefined` 나 `null` 이라도 인자로 넘겨야하며 컴파일러에서 정의된 매개변수 값이 넘어 왔는지 확인한다.
- 따라서 자바스크립트에서는 매개변수의 갯수가 일치하지 않으도 에러가 발생하지 않지만, 타입스크립트는 에러를 발생시킨다.

<div align="center"><img src="https://user-images.githubusercontent.com/97720335/233835235-8970d344-de91-40a1-888f-c7d496661411.png" width="90%"></div>

- 자바스크립트의 특성과 같이 정의된 매개변수의 갯수 만큼 인자를 넘기지 않고 싶다면 `?` 를 이용해서 값을 생략할 수 있다.

<div align="center"><img src="https://user-images.githubusercontent.com/97720335/233835290-315a13ea-014e-453c-8790-40caf9286bc3.png섹" width="90%"></div>

---

## 🧸 Feelings ...
기본타입과 선언하는 방식에 대해 알아보았다. 생각한 것보다 자바스크립트와 차이가 있어 헷갈리지 않고 사용하는 것이 중요할 것 같다 ⭐️

<div align="center"><img src="https://user-images.githubusercontent.com/97720335/233835322-d134d883-1ed8-4a4b-98e1-ee00eae7aac6.png"></div>

<br>

**Reference**

[[캡틴판교 TS] 타입스크립트 입문 - 기초부터 실전까지](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%85%EB%AC%B8) <br>
[타입스크립트 핸드북](https://joshua1988.github.io/ts/)

<br>

> 본 포스팅은 캡틴판교 강사님의 `타입스크립트 입문 - 기초부터 실전까지` 강의를 수강하고 난 후, 본인의 주관적인 견해에 의하여 작성되었습니다.
{: .prompt-info}