---
layout: post
title: "[LearningTS] Chapter09. 타입 제한자"
date: 2023-08-17 15:12:00 +900
lastmod: 2023-08-17 15:12:00 +900
categories: [STUDY, Learning TypeScript]
tags: [typescript]
use_math: true
image: 
  path: https://github.com/sineTlsl/sineTlsl/assets/97720335/a367fb26-a4e4-44d1-b2e0-18f617ce2101
  width: 400
  alt: 러닝 타입스크립트 교재
---

# CHAPTER 09. 타입 제한자

```
타입에서 타입의 타입
아네르스는 "거북이들이 계속 내려오고 있어"라고 말하기를 좋아합니다.
```

<br>

## 🌓 top 타입
- top 타입은 시스템에서 가능한 모든 값을 나타내는 타입이다.
- 모든 다른 타입의 값은 타입이 top인 위치에 제공될 수 있다.
- 즉, 모든 타입은 top 타입에 할당할 수 있다.

### 1. any 다시 보기
- any 타입은 모든 타입의 위치에 제공될 수 있다는 점에서 top 타입처럼 작동할 수 있다.
- any는 일반적으로 console.log의 매개변수와 같이 모든 타입의 데이터를 받아들이는 위치에서 사용한다.

**[예제 1]**<br>
```ts
let anyValue: any;
anyValue = 'Lucille Ball'; // Ok
anyValue = 123; // Ok

console.log(anyValue); // Ok
```

- 다만 any는 타입스크립트가 해당 값에 대한 할당 가능성 또는 멤버에 대해 타입 검사를 수행하지 않도록 명시적으로 지시한다는 문제점을 갖는다.
- 이러한 안정성 부족은 타입스크립트의 타입 검사기를 빠르게 건너뛰려고 할 때 유용하지만, 타입 검사를 비활성화하면 해당 값에 대한 타입스크립트의 유용성이 줄어든다.

**[예제 2]**<br>
아래의 name.toUpperCase() 호출은 확실히 문제가 되지만, name이 any로 선언되었기 때문에 타입스크립트는 타입 오류를 보고하지 않는다.

```ts
function greetComedian(name: any) {
  // 타입 오류 없음
  console.log(`Announcing ${name.toUpperCase()!}`);
}

greetComedian({ name: 'Bea Arthur' });
// Runtime error: name.toUpperCase is not a function
```

어떤 값이든 될 수 있음을 나타내려면 unknown 타입이 훨씬 안전하다.

<br>

### 2. unknown
- 타입스크립트에서 unknown 타입은 진정한 top 타입이다.
- 모든 객체를 unknown 타입의 위치로 전달할 수 있다는 점에서 any 타입과 유사하다.

**unknown 타입과 any 타입의 주요 차이점으로는 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로 취급한다는 점이다.**
- 타입스크립트는 unknown 타입 값의 속성에 직접 접근할 수 없다.
- unknown 타입은 top 타입이 아닌 타입에는 할당할 수 없다.

**[예제 1]**<br>
unknown 타입 값의 속성에 접근하려고 시도하면 타입스크립트는 타입 오류를 보고한다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/9915488c-b449-47fa-88d8-8ba6281f8877" width="65%" />
</p>

타입스크립트가 unknown 타입인 name에 접근할 수 있는 유일한 방법은 instanceof나 typeof 또는 타입 어서션을 사용하는 것처럼 값의 타입이 제한된 경우다.

**[예제 2]**<br>
unknown에서 string으로 타입을 좁히기 위해 typeof를 사용한다.

```ts
function greetComedianSafety(name: unknown) {
  if (typeof name === 'string') {
    console.log(`Announcing ${name.toUpperCase()}!`); // Ok
  } else {
    console.log("Well, I'm off.");
  }
}

greetComedianSafety('Betty White'); // Logs: 4
greetComedianSafety({});
```

- unknown 타입 값의 두 가지 제한으로 인해 unknown이 any보다 훨씬 안전한 타입으로 사용된다.
- **가능한 any 대신 unknown을 사용하는 것이 좋다.**

<br>

## 🌓 타입 서술어
- instance, typeof와 같은 자바스크립트 구문을 사용해 타입을 좁히는 방법을 앞서 살펴보면 제한된 검사로 이 방법을 직접 사용할 때는 괜찮지만, 로직을 함수로 감싸면 타입을 좁힐 수 없게 된다.

**[예제 1]**<br>
- isNumberOrString 함수는 value를 받고 그 값이 number 또는 string인지를 나타내는 boolean 값을 반환한다.
- isNumberOrString(value)가 true를 반환하므로 if문 내부의 두 가지 타입 중 하나여야 한다고 유추할 수 있지만 타입스크립트는 그렇지 않다.
- 타입스크립트는 isNumberOrString이 boolean 값을 반환한다는 사실만 알 수 있고, 인수의 타입을 좁히기 위함이라는 건 알 수 없다.

```ts
function isNumberOrString(value: unknown) {
  return ['number', 'string'].includes(typeof value);
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/d60539cf-00fb-4c88-9fd5-dff94b3c2be3" width="70%" />
</p>

- 타입스크립트에는 인수가 특정 타입인지 여부를 나타내기 위해 boolean 값을 반환하는 함수를 위한 특별한 구문이 있다.
- 이를 `타입 서술어`라고 부르며, '사용자 정의 타입 가드'라고도 부른다.
- instanceof 또는 typeof와 유사한 자체 타입 가드를 생성한다.
- 타입 서술어는 일반적으로 매개변수로 전달된 인수가 매개변수의 타입보다 더 구체적인 타입인지 여부를 나타내는 데 사용된다.

타입 서술어의 반환 타입은 매개변수의 이름, is 키워드, 특정 타입으로 선언할 수 있다.

```ts
function typePredicate(input: WideTye): input is NarrowType;
```

**[예제 2]**<br>
- 이전 예제의 isNumberOrString 함수에서 value를 value is number | string으로 명시적으로 변경하면 명시적 반환 타입을 가질 수 있다.
- 그러면 타입스크립트는 value가 number | string인 경우의 코드 블록은 number | string 타입의 값을 가져야 한다고 추론한다.
- 반면에 value가 number | string이 아닌 경우의 코드 블록은 null | undefined 타입의 값을 가져야 한다.

```ts
function isNumberOrString(value: unknown): value is number | string {
  return ['number', 'string'].includes(typeof value);
}

function logValueIfExists(value: number | string | null | undefined) {
  if (isNumberOrString(value)) {
    // value: number | string의 타입
    value.toString(); // Ok
  } else {
    // value: null | undefined의 타입
    console.log('Value does not exist:', value);
  }
}
```

- 타입 서술어는 단순히 boolean 값을 반환하는 것이 아니라 인수가 더 구체적인 타입임을 나타내는 것이라고 생각할 수 있다.
- 타입 서술어는 이미 한 인터페이스의 인스턴스로 알려진 객체가 더 구체적인 인터페이스의 인스턴스인지 여부를 검사하는 데 자주 사용된다.

**[예제 3]**<br>
- StandupComedian 인터페이스는 Comedian 인터페이스 위에 추가 정보를 갖는다.
- isStandupComedian 타입 가드는 Comedian이 구체적으로 StandupComedian인지 여부를 확인하는 데 사용된다.

```ts
interface Comedian {
  funny: boolean;
}

interface StandupComedian extends Comedian {
  routine: string;
}

function isStandupComedian(value: Comedian): value is StandupComedian {
  return 'routine' in value;
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/3ce4937a-e7ea-4911-b7fc-8d75acae660b" width="60%" />
</p>

타입 서술어는 false 조건에서 타입을 좁히기 때문에 타입 서술어가 입력된 타입 이상을 검사하는 경우 예상치 못한 결과를 얻을 수 있다.

**[예제 4]**<br>
- isLongString 타입 서술어는 input 매개변수가 undefined 또는 길이가 7보다 작은 string인 경우 false를 반환한다.
- 결과적으로 else문(false 조건)은 text를 undefined 타입으로 좁힌다.

```ts
function isLongString(input: string | undefined): input is string {
  return !!(input && input.length >= 7);
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/e93ae866-2463-415b-b14b-b4650992e8e0" width="60%" />
</p>

하지만 타입 서술어는 속성이나 값의 타입을 확인하는 것 이상을 수행해 잘못 사용하기 쉬우므로 가능한 피하는 것이 좋으며, 대부분은 간단한 타입 서술어만으로도 충분하다.

<br>

## 🌓 타입 연산자
- 키워드나 기존 타입의 이름만 사용해 모든 타입을 나타낼 수는 없다.
- 때로는 기존 타입의 속성 일부를 변환해서 두 타입을 결합하는 새로운 타입을 생성해야 할 때도 있다.

### 1. keyof
- 자바스크립트 객체는 (반드시 그렇지는 않지만) 일반적으로 string 타입인 동적값을 사용하여 검색된 멤버를 갖는다.
- 타입 시스템에서 이러한 키를 표현하려면 상당히 까다로울 수 있다.
- string 같은 포괄적인 원시 타입을 사용하면 컨테이너 값에 대해 유효하지 않은 키가 허용된다.

**[예제 1]**<br>
- 더 엄격한 구성 설정을 사용할 때 타입스크립트는 다음 예제에서 볼 수 있는 것과 같은 ratings[key]에 대한 오류를 보고한다.
- 타입 string은 Ratings 인터페이스에서 속성으로 허용되지 않는 값을 허용하고, Ratings은 string 키를 허용하는 인덱스 시그니처를 선언하지 않는다.

```ts
interface Ratings {
  audience: number;
  critic: number;
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/86e61754-6626-4e73-817a-a90cc32202b5" width="100%" />
</p>

```ts
const ratings: Ratings = { audience: 66, critic: 84 };

getRatings(ratings, 'audience'); // Ok
getRatings(ratings, 'not valid'); // 허용되지만 사용하면 안 됨
```

**[예제 2]**<br>
- 또 다른 옵션은 허용되는 키를 위한 리터럴 유니언 타입을 사용하는 것이다.
- 이 경우 컨테이너 값에 존재하는 키만 적절하게 제한하는 것이 더 정확하다.

```ts
function getRating(ratings: Ratings, key: 'audience' | 'critic'): number {
  return ratings[key]; // Ok
}

const ratings: Ratings = { audience: 66, critic: 84 };

getRating(ratings, 'audience'); // Ok
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/5dc15499-5454-4d4d-a5b7-20f8217eefc9" width="90%" />
</p>

**"그러나 인터페이스에 수십 개 이상의 멤버가 있다면?"**

=> 각 멤버의 키를 유니언 타입에 모두 입력하고 최신 상태를 유지해야 하는 상당히 번거로운 작업이 필요할 것이다.

- 대신 타입스크립트에서는 기본에 존재하는 타입을 사용하고, 해당 타입에 허용되는 모든 키의 조합을 반환하는 keyof 연산자를 제공한다.
- 타입 에너테이션처럼 타입을 사용하는 모든 곳에서 타입 이름 앞에 keyof 연산자를 배치한다.

**[예제 3]**<br>
keyof Ratings는 'audience' | 'critic'과 동일하지만, 작성하는 것이 훨씬 빠르고 Ratings 인터페이스가 변경되더라도 수동으로 업데이트할 필요가 없다.

```ts
function getCountKeyof(ratings: Ratings, key: keyof Ratings): number {
  return ratings[key]; // OK
}

const ratings: Ratings = { audience: 66, critic: 84 };

getCountKeyof(ratings, 'audience'); // Ok
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/d4918801-d00e-479e-ac08-651aa8f4873f" width="85%" />
</p>

keyof 존재하는 타입의 키를 바탕으로 유니언 타입을 생성하는 훌륭한 기능이다.

<br>

### 2. typeof
- 타입스크립트에서 제공하는 또 다른 타입 연산자는 typeof이다.
- typeof는 제공되는 값의 타입을 반환한다.
- typeof는 값의 타입을 수동으로 작성하는 것이 짜증날 정도로 복잡한 경우에 사용하면 매우 유용하다.

**[예제 1]**<br>
adaptation 변수는 original과 동일한 타입으로 선언되었다.

```ts
const original = {
  medium: 'movie',
  title: 'Mean Girls',
};

let adaptation: typeof original;
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/88e6aa0e-6425-43b6-949d-732adaedfc2c" width="100%" />
</p>

- typeof 타입 연산자는 시각적으로 주어진 값이 어떤 타입인지를 반환할 때 사용하는 런타임 typeof 연산자처럼 보이지만 이 둘은 차이가 있다.
- 둘은 단지 우연히 같은 단어를 사용할 뿐이다.
- 자바스크립트의 `typeof` 연산자는 타입에 대한 문자열 이름을 반환하는 런타임 연산자이며, 타입스크립트의 typeof 연산자는 타입스크립트에서만 사용할 수 있고 컴파일된 자바스크립트 코드에는 나타나지 않는다.

<br>

### 1. keyof typeof
- typeof는 값의 타입을 검색하고, keyof는 타입에 허용된 키를 검색한다.
- 타입스크립트는 두 키워드를 함께 연결해 값의 타입에 허용된 키를 간결하게 검색할 수 있다.
- 두 키워드를 함께 사용하면 typeof 타입 연산자를 keyof 타입 연산자와 함께 작업할 때 매우 유용하다.

**[예제 1]**<br>
- logRating 함수는 ratings 값의 키 중 하나를 받는다.
- 코드는 인터페이스를 생성하는 것 대신, keyof typeof를 사용해 키가 ratings 값 타입의 키 중 하나여야 함을 나타낸다.

```ts
const ratings = {
  imdb: 8.4,
  metacritic: 82,
};

function logRating(key: keyof typeof ratings) {
  console.log(ratings[key]);
}

logRating('imdb'); // Ok
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/0544fad0-cf35-494e-8ded-b6a9403db34f" width="90%" />
</p>

keyof와 typeof를 결합해서 사용하면 명시적 인터페이스 타입이 없는 객체에 허용된 키를 나타내는 타입에 대한 코드를 작성하고 업데이트하는 수고를 줄일 수 있다.

<br>

## 🌓 타입 어서션

- 타입스크립트 코드가 **강력하게 타입화**될 때 가장 잘 작동한다.
- 즉, 코드의 모든 값이 정확히 알려진 타입을 가지는 경우다.
- 타입스크립트의 타입 검사기가 복잡한 코드를 이해할 수 있도록 top 타입과 타입 가드 같은 기능을 제공한다.
- 그러나 경우에 따라서는 코드가 이렇게 작동하는지 타입 시스템에 100% 정확하게 알리는 것이 불가능할 때도 있다.
- 예를 들어, `JSON.parse`는 의도적으로 top 타입인 any를 반환한다.
- JSON.parse에 주어진 특정 문자열 값이 특정한 값 타입을 반환해야 한다는 것을 타입 시스템에 안전하게 알릴 수 있는 방법은 없다.
- 타입스크립트는 값의 타입에 대한 타입 시스템의 이해를 재정의하기 위한 구문으로 **타입 어서션(=타입 캐스트)**을 제공한다.
- 다른 타입을 의미하는 값의 타입 다음에 `as` 키워드를 배치한다.
- 타입 시스템은 어서션을 따르고 값을 해당 타입으로 처리한다.

**[예제 1]**<br>
- JSON.parse로부터 반환된 결과는 string[], [string, string], ["grace', "frankie"] 같은 타입이 될 수 있다.
- 세 줄의 코드에서 타입 어서션을 사용해 any인 타입을 셋 중 하나로 전환한다.

```ts
const rawData = '["grace", "frankie]';

// 타입: any
JSON.parse(rawData);

// 타입: string[]
JSON.parse(rawData) as string[];

// 타입: [string, string]
JSON.parse(rawData) as [string, string];

// 타입: ["grace", "frankie"]
JSON.parse(rawData) as ['grace', 'frankie'];
```

**[예제 2]**<br>
- 타입 어서션은 타입스크립트 타입 시스템에만 존재하며 자바스크립트로 컴파일될 때 다른 타입 시스템 구문과 함께 제거된다.
- 이전 코드를 자바스크립트로 컴파일하면 결과는 다음과 같다.

```ts
const rawData = '["grace", "frankie]';

// 타입: any
JSON.parse(rawData);

// 타입: string[]
JSON.parse(rawData);

// 타입: [string, string]
JSON.parse(rawData);

// 타입: ["grace", "frankie"]
JSON.parse(rawData);
```

- 타입스크립트 모범 사례는 가능한 한 타입 어서션을 사용하지 않는 것이다.
- 코드가 완전히 타입화되고 어서션을 사용해 타입스크립트의 타입 이해를 방해할 필요가 없는 것이 가장 좋다.
- 그러나 타입 어서션이 유용하고 심지어 필요한 경우가 종종 있다.

<br>

### 1. 포착된 오류 타입 어서션
- 오류를 처리할 때 타입 어서션이 매우 유용할 수 있다.
- try 블록의 코드가 예상과 다른 객체를 예기치 않게 발생할 수 있기 때문에 catch 블록에서 포착된 오류가 어떤 타입인지 아는 것은 일반적으로 불가능하다.
- 게다가 자바스크립트의 모범 사례는 항상 Error 클래스의 인스턴스를 발생시키지만, 일부 프로젝트에서는 문자열 리터럴 또는 다른 의외의 값을 발생시키기도 한다.

**[예제 1]**<br>
- 코드 영역이 Error 클래스의 인스턴스를 발생시킬 거라 틀림없이 확신한다면 타입 어서션을 사용해 포착된 어서션을 오류로 처리할 수 있다.
- Error 클래스의 인스턴스라고 가정된 error의 message 속성에 접근한다.

```ts
try {
  // (오류를 발생시키는 코드)
} catch (error) {
  console.warn('Oh no!', (error as Error).message);
}
```

- 발생된 오류가 예상된 오류 타입인지를 확인하기 위해 instanceof 검사와 같은 타입 내로잉을 사용하는 것이 더 안전하다.
- catch 블록에 발생한 error가 Error 클래스의 인스턴스인지를 검사해 콘솔에 Error를 출력할지 error 자체를 출력할지 여부를 확인한다.

```ts
try {
  // (오류를 발생시키는 코드)
} catch (error) {
  console.warn(
    'Oh no!',
    error instanceof Error ? (error as Error).message : error
  );
}
```

<br>

### 2. non-null 어서션
- 타입 어서션이 유용한 경우를 하나 더 살펴보자면, 실제로는 아니고 이론적으로만 null 또는 undefined를 포함할 수 있는 변수에서 null과 undefined를 제거할 때 타입 어서션을 주로 사용한다.
- 타입스크립트에서는 너무 흔한 상황이라 이와 관련된 약어를 제공한다.
- null과 undefined를 제외한 값의 전체 타입을 작성하는 대신 `!`를 사용하면 된다.
- 즉, non-null 어서션 타입이 null 또는 undefined가 아니라고 간주한다.

**[예제 1]**<br>
두 가지 타입 어서션은 둘 다 Date | undefined가 아니고 Date가 된다는 점에서 동일하다.

```ts
// 타입 유추: Date | undefined
let maybeDate = Math.random() > 0.5 ? undefined : new Date();

// 타입이 Date라고 간주됨
maybeDate as Date;

// 타입이 Date라고 간주됨
maybeDate!;
```

non-null 어서션은 값을 반환하거나 존재하지 않는 경우 undefined를 반환하는 Map.get과 같은 API에서 특히 유용하다.

**[예제 2]**<br>
- seasonCounts는 일반적으로 Map<string, number>이다.
- seasonCounts는 "I love Lucy" 키를 포함하고 있으므로 knownValue 변수는 !를 사용해 해당 타입에서 | undefined를 제거할 수 있다.

```ts
const seasonCounts = new Map([
  ['I Love Lucy', '6'],
  ['The Golden Girls', '7'],
]);

// 타입: string | undefined
const maybeValue = seasonCounts.get('I Love Lucy');

console.log(maybeValue.toUpperCase());
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/19d5cf55-016f-4c21-ae35-d8bbecf3407a" width="60%" />
</p>

```ts
// 타입: string
const knownValue = seasonCounts.get('I Love Lucy')!;

console.log(knownValue.toUpperCase()); // Ok
```

<br>

### 3. 타입 어서션 주의 사항
- any 타입과 마찬가지로 타입 어서션은 타입스크립트의 타입 시스템에 필요한 하나의 도피 수단이다.
- 따라서 any 타입을 사용할 때처럼 꼭 필요한 경우가 아니라면 가능한 한 사용하지 말아야 한다.
- 값의 타입에 대해 더 쉽게 어서션하는 것보다 코드를 나타내는 더 정확한 타입을 갖는 것이 좋다.
- 또한 이러한 어서션은 종종 잘못되기도 한다.
- 작성 당시 이미 잘못되었거나 코드베이스가 변경됨에 따라 나중에 달라지기도 한다.

**[예제]**<br>
- seasonCounts 예제에서 Map이 시간이 지남에 따라 다른 값을 갖도록 변경된다고 가정해보겠다.
- non-null 어서션은 여전히 코드가 타입스크립트 타입 검사를 통과하도록 만들지만 런타임 오류가 발생할 수 있다.

```ts
const seasonCounts = new Map([
  ['Broad City', '5'],
  ['Community', '6'],
]);

// 타입: string
const knownValue = seasonCounts.get('I Love Lucy')!;

console.log(knownValue.toUpperCase()); // 타입 오류는 아니지만, 런타임 오류가 발생함
```

타입 어서션은 자주 사용하면 안 되고, 사용하는 것이 안전하다고 확실히 확신할 때만 사용해야 한다.

<br>

### 3.1. 어서션 vs. 선언
- 변수 타입을 선언하기 위해 타입 애너테이션을 사용하는 것과 초깃값으로 변수 타입을 변경하기 위해 타입 어서션을 사용하는 것 사이에는 차이가 있다.
- 변수의 타입 애너테이션과 초깃값이 모두 있을 때, 타입스크립트의 타입 검사기는 변수의 타입 애너테이션에 대한 변수의 초깃값에 대해 할당 가능성 검사를 수행한다.
- 그러나 타입 어서션은 타입스크립트에 타입 검사 중 일부를 건너뛰도록 명시적으로 지시한다.

**[예제]**<br>
- acts 멤버가 없는 동일한 결함을 가지며, 타입이 Entertainer인 객체 두 개를 생성하는 코드다.
- 타입스크립트는 Entertainer 타입 애너테이션으로 인해 declared 변수에서 오류를 잡을 수 있따.
- 하지만 타입 어서션 때문에 asserted 변수에 대해서는 오류를 잡을 수 없다.

```ts
interface Entertainer {
  acts: string[];
  name: string;
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/658f23ca-33ef-40ea-8026-a1e4fe719e19" width="90%" />
</p>

```ts
const asserted = {
  name: 'Moms Mabley',
} as Entertainer; // 허용되지만 런타임 시 오류 발생

// 다음 구문은 런타임 시 다음 오류로 인해 정상적으로 작동되지 않음
console.log(declared.acts.join(', '));
console.log(asserted.acts.join(', '));
```

따라서 타입 애너테이션을 사용하거나 타입스크립트가 초깃값에서 변수의 타입을 유추하도록 하는 것이 매우 바람직하다.

<br>

### 3.2. 어서션 할당 가능성
- 타입 어서션은 일부 값의 타입이 약간 잘못된 상황에서 필요한 작은 도피 수단일 뿐이다.
- 타입스크립트는 타입 중 하나가 다른 타입에 할당 가능한 경우에만 두 타입간의 탕비 어서션을 허용한다.
- 완전히 서로 관련이 없는 두 타입 사이에 타입 어서션이 있는 경우에는 타입스크립트가 타입 오류를 감지하고 알려준다.

**[예제]**<br>
원시 타입은 서로 관련이 없으므로 하나의 원시 타입에서 다른 원시 타입으로 전환하는 것은 허용되지 않는다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/fce4fe01-806b-40f6-9533-2f8b7f6ecf80" width="100%" />
</p>

- 하나의 타입에서 값을 완전히 관련 없는 타입으로 전환해야 하는 경우 이중 타입 어서션을 사용한다.
- 먼저 값을 any나 unknown같은 top 타입으로 전환한 다음, 그 결과를 관련없는 타입으로 전환한다.

```ts
let myValueDouble = "1337" as unknown as number; // 허용되지만 이렇게 사용하면 안 됨
```

- as unknown as... 이중 타입 어서션은 위험하고 거의 항상 코드와 타입이 잘못되었다는 징후를 나타낸다.
- 타입 시스템의 도피 수단으로 이중 타입 어서션을 사용하면, 주변 코드를 변경해서 이전에 작동하던 코드에 문제가 발생할 경우, 타입 시스템이 우리를 구해주지 못할 수 있음을 의미한다.

<br>

## 🌓 const 어서션
**const 어서션**은 배열, 원시 타입, 값, 별칭 등 모든 값을 상수로 취급해야 함을 나타내는 데 사용한다.

특히 `as const`는 수신하는 모든 타입에 다음 세가지 규칙을 적용한다.
- 배열은 가변 배열이 아니라 읽기 전용 튜플로 취급된다.
- 리터럴은 일반적인 원시 타입과 동등하지 않고 리터럴로 취급된다.
- 객체의 속성은 읽기 전용으로 간주된다.

다음 배열이 튜플로 간주되는 것처럼 배열이 튜플이 되는 것을 이미 보았다.

```ts
// 타입: (number | string)[];
[0, ''];

// 타입: readonly [0, '']
[0, ''] as const;
```

<br>

### 1. 리터럴에서 원시 타입으로
타입 시스템이 리터럴 값을 일반적인 원시 타입으로 확장하기보다 특정 리터럴로 이해하는 것이 유용할 수 있다.

**[예제 1]**<br>
- 튜플을 반환하는 함수처럼 일반적인 원시 타입 대신 특정 리터럴을 생성한다고 알려진 함수에서 유용할 수 있다.
- 다음 함수는 좀 더 구체적으로 만들 수 있는 값을 반환한다.
- 여기에서 getNameConst의 반환 타입은 일반적인 string 대신 "Maria Bamford"라는 더 구체적인 값이다.

```ts
// 타입: () => string
const getName = () => 'Maria Bamford';

// 타입: () => "Maria Bamford"
const getNameConst = () => 'Baria Bamford' as const;
```

- 값의 특정 필드가 더 구체적인 리터럴 값을 갖도록 하는 것도 유용하다.
- 많은 인기 있는 라이브러리는 값의 판별 필드가 특정 리터럴이 되도록 요구한다.
- 따라서 해당 코드의 타입이 값을 더 구체적으로 추론할 수 있다.

**[예제 2]**<br>
narrowJoke 변수는 string 대신 "one-liner"를 style 값으로 가지므로 Joke 타입이 필요한 위치에서 제공될 수 있다.

```ts
interface Joke {
  quote: string;
  style: 'story' | 'one-liner';
}

function tellJoke(joke: Joke) {
  if (joke.style === 'one-liner') {
    console.log(joke.quote);
  } else {
    console.log(joke.quote.split('\n'));
  }
}

// 타입: { quote: string; style: "one-liner" }
const narrowJoke = {
  quote: 'If you stay alive for no other reason do it for spite.',
  style: 'one-liner' as const,
};

tellJoke(narrowJoke); // Ok

// 타입: { quote: string; style: string }
const wideObject = {
  quote: 'Time files when you are anxious!',
  style: 'one-liner',
};
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/06949c3b-813d-4c18-9695-58fd39bcb5c8" width="90%" />
</p>

<br>

### 2. 읽기 전용 객체
- 변수의 초깃값으로 사용되는 것과 같은 객체 리터럴은 let 변수의 초깃값이 확장된느 것과 동일한 방식으로 속성 타입을 확장한다.
- apple 같은 문자열 값은 string과 같은 원시 타입이 되고, 배열은 튜플이 아닌 array 타입이 된다.
- 하지만 이러한 값의 일부 또는 전체를 나중에 특정 리터럴 타입이 필요한 위치에서 사용해야 할 때 잘 맞지 않을 수 있다.
- 그러나 `as const`를 사용해 값 리터럴을 어서션하면 유추된 타입이 가능한 한 구체적으로 전환된다.
- 모든 멤버 속성은 `readonly`가 되고, 리터럴은 일반적인 원시 타입 대신 고유한 리터럴 타입으로 간주되며, 배열은 읽기 전용 튜플이 된다.
- 즉, 값 리터럴에 const 어서션을 적용하면 해당 값 리터럴이 변경되지 않고 모든 멤버에 동일한 const 어서션 로직이 재귀적으로 적용된다.

**[예제 1]**<br>
- preferencesMutable 값은 as const 없이 선언되었으므로 이름은 원시 타입인 string이 되고 수정이 허용된다.
- 그러나 preferencesReadonly는 as const로 선언되었으므로 해당 멤버 값은 리터럴이고 수정이 허용되지 않는다.

```ts
function describePreference(preference: 'maybe' | 'no' | 'yes') {
  switch (preference) {
    case 'maybe':
      return 'I suppose...';
    case 'no':
      return 'No thanks.';
    case 'yes':
      return 'Yes please!';
  }
}

// 타입: { movie: string, standuo: string }
const preferencesMutable = {
  movie: 'maybe',
  standup: 'yes',
};
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/2bb03b7c-4e50-4309-a54a-fe557aa16a01" width="85%" />
</p>

```ts
preferencesMutable.movie = 'no'; // Ok

// 타입 readonly { readonly movie: "maybe", readonly standup: "yes" }
const preferenceReadonly = {
  movie: 'maybe',
  standup: 'yes',
} as const;

describePreference(preferenceReadonly.movie); // OK
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/74a0db1b-2951-4858-845c-000ffb626fc3" width="60%" />
</p>

<br>

**Reference**

러닝 타입스크립트 (Learning TypeScript)
