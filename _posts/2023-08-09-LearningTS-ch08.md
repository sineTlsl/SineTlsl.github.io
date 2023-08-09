---
layout: post
title: "[LearningTS] Chapter08. 클래스"
date: 2023-08-09 22:50:00 +900
lastmod: 2023-08-09 22:50:00 +900
categories: [STUDY, Learning TypeScript]
tags: [typescript]
use_math: true
image: 
  path: https://github.com/sineTlsl/sineTlsl/assets/97720335/a367fb26-a4e4-44d1-b2e0-18f617ce2101
  width: 400
  alt: 러닝 타입스크립트 교재
---

# CHAPTER 08. 클래스

```
저는 일부 기능적 장치인 클래스를 절대 사용하지 않으려고 합니다.
저에게는 클래스가 너무 강렬해요!
```

- 타입스크립트가 탄생되고 릴리스된 2010년 초반의 자바스크립트 세계는 오늘날과 상당히 달랐다.
- 나중에 ES2015에서 표준화된 화살표 함수 let/const 변수 같은 기능은 아직 먼 희망이었다.
- 바벨은 첫번째 커밋이 있은 후 몇 년이 흘렸고, 최신 자바스크립트 구문을 오래된 구문으로 변환하는 Traceur와 같은 이전 도구들을 완전히 주류로 채택 받지 못했다.
- 타입스크립트의 초기 마케팅과 기능들은 이런 자바스크립트 세계에 맞춰져 있었다.
- 타입스크립트의 타입 검사기 외에도 트랜스파일러가 강조되었고, 그 예시로 클래스가 자주 등장했다.
- 오늘날 타입스크립트의 클래스 지원은 모든 자바스크립트 언어 기능을 지원하느 많은 기능 중 하나에 불과한다.
- 타입스크립트는 클래스 사용이나 다른 인기 있는 자바스크립트 패턴을 권장하지도 막지도 않는다.

<br>

## 🌓 클래스 메서드
- 타입스크립트는 독립 함수를 이해하는 것과 동일한 방식으로 메서드를 이해한다.
- 매개변수 타입에 타입이나 기본값을 지정하지 않으면 any 타입을 기본으로 갖는다.
- 메서드를 호출하려면 허용 가능한 수의 인수가 필요하고, 재귀 함수가 아니라면 대부분 반환 타입을 유추할 수 있다.

**[예제 1]**<br>
string 타입의 단일 필수 매개변수를 갖는 greet 클래스를 가진 Greeter 클래스를 정의하는 코드다.

```ts
class Greeter {
  greet(name: string) {
    console.log(`${name}, do your stuff!`);
  }
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/130d4930-c818-4745-89d6-cc452eeaa440" width="65%" />
</p>

- 클래스 생성자는 매개변수와 관련하여 전형적인 클래스 메서드처럼 취급된다.
- 타입스크립트는 메서드 호출 시 올바른 타입의 인수가 올바른 수로 제공되는지 확인하기 위해 타입검사를 수행한다.

**[예제 2]**<br>
Greeted 생성자는 message: string으로 매개변수가 제공되어야 한다.

```ts
class Greeted {
  constructor(message: string) {
    console.log(`As I always say: ${message}`);
  }
}

new Greeted('take chances, make mistakes, get messy');
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/c7d60f2d-bb24-424b-9369-887ea9a5e0cc" width="65%" />
</p>

<br>

## 🌓 클래스 속성
- 타입스크립트에서 클래스의 속성을 읽거나 쓰려면 클래스에 명시적으로 선언해야 한다.
- 클래스 속성은 인터페이스와 동일한 구문을 사용해 선언한다.
- 클래스 속성 이름 뒤에는 선택적으로 타입 애너테이션이 붙는다.
- 타입스크립트는 생성자 내의 할당에 대해서 그 멤버가 클래스에 존재하는 멤버인지 추론하려고 시도하지 않는다.

**[예제]**<br>
- destination은 string으로 명시적으로 선언되어 있어 FieldTrip 클래스 인스턴스에 할당되고 접근할 수 있다.
- 클래스가 nonexistent 속성을 선언하지 않았기 때문에 생성자에게 this.nonexistent 할당은 허용되지 않는다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/4a847a13-93b2-4a89-8e83-f82a13e1fc94" width="65%" />
</p>

- 클래스 속성을 명시적으로 선언하면 타입스크립트는 클래스 인스턴스에서 무엇이 허용되고, 허용되지 않는지 빠르게 이해할 수 있다.
- 나중에 클래스 인스턴스가 사용될 때, 코드가 trip.nonexistent와 같은 클래스 인스턴스에 존재하지 않는 멤버에 접근하려고 시도하면 타입스크립트는 타입 오류를 발생시킨다.
  <p align="center">
    <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/8547e99b-a3d2-4967-8eaf-99803cf51ba3" width="65%" />
  </p>

<br>

### 1. 함수 속성
자바스크립트에는 클래스의 멤버를 호출 가능한 함수로 선언하는 두 가지 구문이 있다.

**[예제 1]**<br>
- myFunction(){}과 같이 멤버 이름 뒤에 괄호를 붙이는 메서드 접근 방식을 앞서 살펴보면, 메서드 메서드 접근 방식은 함수를 클래스 프로토타입에 할당하므로 모든 클래스 인스턴스는 동일한 함수 정의를 사용한다.
- WithMethod 클래스는 모든 인스턴스가 참조할 수 있는 myMethod 메서드를 선언한다.

```ts
class WithMethod {
  myMethod() {}
}

new WithMethod().myMethod === new WithMethod().myMethod; // true
```

- 값이 함수인 속성을 선언하는 방식도 있다.
- 이렇게 하면 클래스의 인스턴스당 새로운 함수가 생성되며, 항상 클래스 인스턴스를 가리켜야 하는 화살표 함수에서 this 스코프를 사용하면 클래스 인스턴스당 새로운 함수를 생성하는 시간과 메모리 비용 측면에서 유용할 수 있다.

**[예제 2]**<br>

```ts
class WithProperty {
  myProperty: () => {};
}

new WithProperty().myProperty === new WithProperty().myProperty; // false
```

- 함수 속성에는 클래스 메서드와 독립 함수의 동일한 구문을 사용해 매개변수와 반환 타입을 지정할 수 있다.
- 결국 함수 속성은 클래스 멤버로 할당된 값이고, 그 값은 함수다.

**[예제 3]**<br>
WithPropertyParameters 클래스는 타입이 (input: boolean) => string인 takesParameters 속성을 가진다.

```ts
class WithPropertyParameters {
  takesParameters = (input: boolean) => (input ? 'Yes' : 'No');
}

const instance = new WithPropertyParameters();

instance.takesParameters(true); // Ok
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/1677300d-21f9-4cc1-8969-99930fa82d2f" width="80%" />
</p>

<br>

### 2. 초기화 검사
- 엄격한 컴파일러 설정이 활성화된 상태에서 타입스크립트는 undefined 타입으로 선언된 각 속성이 생성자에게 할당되었는지 확인한다.
- 이와 같은 엄격한 초기화 검사는 클래스 속성에 값을 할당하지 않는 실수를 예방할 수 있어 유용하다.

**[예제 1]**<br>
WithValue 클래스는 unused 속성에 값을 할당하지 않았고, 타입스크립트는 이 속성을 타입 오류로 인식한다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/c5586b4c-6f02-43c5-89b6-b634d565ee6f" width="80%" />
</p>

엄격한 초기화 검사가 없다면, 비록 타입 시스템이 undefined 값에 접근할 수 없다고 말할지라도 클래스 인스턴스는 undefined 값에 접근할 수 있다.

**[예제 2]**<br>
엄격한 초기화 검사가 수행되지 않으면 올바르게 컴파일되지만, 결과 자바스크립트는 런타임 시 문제가 발생한다.

```ts
class MissingInitializer {
  property: string;
}

new MissingInitializer().property.length;
// TypeError: Caanot read property 'length' of undefined
```

<br>

### 2.1. 확실하게 할당된 속성
- 엄격한 초기화 검사가 유용한 경우가 대부분이지만 클래스 생성자 다음에 클래스 속성을 의도적으로 할당하지 않는 경우가 있을 수도 있다.
- 엄격한 초기화 검사를 적용하면 안 되는 속성인 경우에는 이름 뒤에 !를 추가해 검사를 비활성화하도록 설정한다.
- 이렇게 하면 타입스크립트에 속성이 처음 사용되기 전에 undefined 값이 할당된다.

**[예제]**<br>
ActivitiesQueue 클래스는 생성자와는 별도로 여러 번 다시 초기화될 수 있으므로 pending 속성은 !와 함께 할당되어야 한다.

```ts
class ActivitiesQueue {
  pending!: string[]; // Ok

  initialize(pending: string[]) {
    this.pending = pending;
  }

  next() {
    return this.pending.pop();
  }
}

const activities = new ActivitiesQueue();

activities.initialize(['eat', 'sleep', 'learn']);
activities.next();
```

> 🌿 클래스 속성에 대해 엄격한 초기화 검사를 비활성화하는 것은 종종 타입 검사에는 적합하지 않은 방식으로 코드가 설정된다는 신호다. ! 어서션을 추가하고 속성에 대한 타입 안정성을 줄이는 대신 클래스를 리팩터링해서 어서션이 필요하지 않도록 해야한다.

<br>

### 3. 선택적 속성
- 인터페이스와 마찬가지로 클래스는 선언된 속성 이름 뒤에 `?`를 추가해 속성을 옵션으로 선언한다.
- 선택적 속성은 `| undefined`를 포함하는 유니언 타입과 거의 동일하게 작동한다.
- 엄격한 초기화 검사는 생성자에서 선택적 속성을 명시적으로 설정하지 않아도 문제가 되지 않는다.

**[예제]**<br>
MissingInitializer 클래스는 property를 옵션으로 정의했으므로 엄격한 속성 초기화 검사와 관계없이 클래스 생성자에서 할당하지 않아도 된다.

```ts
class MissingInitializer {
  property?: string;
}

new MissingInitializer().property?.length; // Ok

new MissingInitializer().property?.length;
// Error: Object is possibly 'undefined'.
```

<br>

### 4. 읽기 전용 속성
- 인터페이스와 마찬가지로 선언된 속성 이름 앞에 readonly 키워드를 추가해 속성을 읽기 전용으로 선언한다.
- readonly 키워드는 타입 시스템에만 존재하며 자바스크립트는 컴파일할 때 삭제된다.
- readonly로 선언된 속성은 선언된 위치 또는 생성자에서 초깃값만 할당할 수 있다.
- 클래스 내의 메서드를 포함한 다른 모든 위치에서 속성은 읽을 수만 있고, 쓸 수는 없다.

**[예제 1]**<br>
Quote 클래스의 text 속성은 생성자에서는 값이 지정되지만 다른 곳에서 값을 지정하려고 하면 타입 오류가 발생한다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/281288be-6afe-4247-9de2-91008ce88b39" width="60%" />
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/04fdd9eb-4345-46b1-bd75-ff858862731d" width="60%" />
</p>

- 원시 타입의 초깃값을 갖는 readonly로 선언된 속성은 다른 속성과 조금 다르다.
- 이런 속성은 더 넓은 원싯값이 아니라 값의 타입이 가능한 한 좁혀진 리터럴 타입으로 유추된다.
- 타입스크립트는 값이 나중에 변경되지 않는다는 것을 알기 때문에 더 공격적인 초기 타입 내로잉을 편하게 느낀다.
- const 변수가 let 변수보다 더 좁은 타입을 갖는 것과 유사하다.

**[예제 2]**<br>
클래스 속성은 처음에는 모두 문자열 리터럴로 선언되므로 둘 중 하나를 string으로 확장하기 위해서는 타입 애너테이션이 필요하다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/5b13564c-4ad0-4172-96e7-f567eebca4ce" width="100%" />
</p>

- 속성의 타입을 명시적으로 확장하는 작업이 자주 필요하지는 않는다.
- 그럼에도 불구하고 RandomQuote에서 등장하는 생성자의 조건부 로직처럼 경우에 따라 유용할 수 있다.

<br>

## 🌓 타입으로서의 클래스
타입 시스템에서의 클래스는 클래스 선언이 런타임 값(클래스 자체)과 애너테이션에서 사용할 수 있는 타입을 모두 생성한다는 점에서 상대적으로 독특하다.

**[예제 1]**<br>
- Teacher 클래스의 이름은 teacher 변수에 주석을 다는 데 사용된다.
- teacher 변수에는 Teacher 클래스의 자체 인스턴스처럼 Teacher 클래스에 할당할 수 있는 값만 할당해야 함을 타입스크립트에 알려준다.

```ts
class Teacher {
  sayHello() {
    console.log('Take chances, make mistakesm get messy!');
  }
}

let teacher: Teacher;

teacher = new Teacher(); // Ok
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/cd370a38-9af3-44d8-ba6f-a7091ba29c6c" width="60%" />
</p>

- 타입스크립트는 클래스의 동일한 멤버를 모두 포함하는 모든 객체 타입을 클래스에 할당할 수 있는 것으로 간주한다.
- 타입스크립트의 구조적 타이핑이 선언되는 방식이 아니라 객체의 형태만 고려하기 때문이다.

**[예제 2]**<br>
- withSchoolBus는 SchoolBus 타입의 매개변수를 받는다.
- 매개변수로 SchoolBus 클래스 인스턴스처럼 타입이 () => string[]인 getAbilities 속성을 가진 모든 객체를 할당할 수 있다.

```ts
class SchoolBus {
  getAbilities() {
    return ['magic', 'shapeshifting'];
  }
}

function withSchoolBus(bus: SchoolBus) {
  console.log(bus.getAbilities());
}

withSchoolBus(new SchoolBus()); // Ok

// Ok
withSchoolBus({
  getAbilities: () => ['transmogrification'],
});
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/d8266518-65a9-4acc-972c-da591648663d" width="80%" />
</p>

> 🌿 대부분의 실제 코드에서 우리는 클래스 타입을 요청하는 위치에 객체의 값을 전달하지 않는다. 이러한 구조적인 확인 동작은 예상하지 못하는 것처럼 보일 수 있지만 자주 나타나지는 않는다.

<br>

## 🌓 클래스와 인터페이스
- 타입스크립트는 클래스 이름 뒤에 `implements` 키워드와 인터페이스 이름을 추가함으로써 클래스의 해당 인스턴스가 인터페이스를 준수한다고 선언할 수 있다.
- 이렇게 하면 클래스를 각 인터페이스에 할당할 수 있어야 함을 타입스크립트에 나타낸다.
- 타입 검사기에 의해 모든 불일치에 대해서 타입 오류가 발생한다.

**[예제 1]**<br>
Student 클래스는 name 속성과 study 메서드를 포함해 Learner 인터페이스를 올바르게 구현했지만 Slaker에는 study가 누락되어 타입 오류가 발생한다.

```ts
interface Learner {
  name: string;
  study(hours: number): void;
}

class Student implements Learner {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
  study(hours: number) {
    for (let i = 0; i < hours; i += 1) {
      console.log('...studyinh...');
    }
  }
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/5cf188d7-2669-4c3c-9d18-8b0ed0feec59" width="90%" />
</p>

- 인터페이스를 구현하는 것으로 클래스를 만들어도 클래스가 사용되는 방식은 변경되지 않는다.
- 클래스가 이미 인터페이스와 일치하는 경우 타입스크립트의 타입 검사기는 인터페이스의 인스턴스가 필요한 곳에서 해당 인스턴스를 사용할 수 있도록 해야한다.
- 타입스크립트는 인터페이스에서 클래스의 메서드 또는 속성 타입을 유추하지 않는다.
- Slacker 예제에서 study(hours){} 메서드를 추가했다면 타입스크립트는 타입 애너테이션을 지정하지 않는 한 hours 매개변수를 암시적 any로 간주한다.

**[예제 2]**<br>
다른 형태로 구현한 Student 클래스는 멤버에 타입 애너테이션을 제공하지 않기 때문에 암시적인 any 타입 오류가 발생한다.

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/664e99bb-f2da-4c48-8710-c869546cd8fe" width="60%" />
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/ad648418-a899-4698-a370-2793a3263917" width="60%" />
</p>

- 인터페이스를 구현하는 것은 순전히 안정성 검사를 위해서다.
- 모든 인터페이스 멤버를 클래스 정의로 복사하지 않는다.
- 대신 인터페이스를 구현하면 클래스 인스턴스가 사용되는 곳에서 나중에 타입 검사기로 신호를 보내고 클래스 정의에서 표면적인 타입 오류가 발생한다.
- 변수에 초깃값이 있더라도 타입 애너테이션을 추가하는 것과 용도가 비슷하다.

<br>

### 1. 다중 인터페이스 구현
- 타입스크립트의 클래스는 다중 인터페이스를 구현해 선언할 수 있다.
- 클래스에 구현된 인터페이스 목록은 인터페이스 이름 사이에 쉼표를 넣고, 개수 제한 없이 인터페이스를 사용할 수 있다.

**[예제 1]**<br>
- 두 클래스에서 모두 Graded를 구현하려면 grades 속성을 가져야 하고, Reporter를 구현하려면 report 속성을 가져야 한다.
- Empty 클래스에는 Graded와 Reporter 인터페이스를 제대로 구현하지 못했으므로 두 가지 타입 오류가 발생한다.

```ts
interface Graded {
  grades: number[];
}

interface Reporter {
  report: () => string;
}

class ReportCard implements Graded, Reporter {
  grades: number[];

  constructor(grades: number[]) {
    this.grades = grades;
  }

  report() {
    return this.grades.join(',');
  }
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/d8b73bfe-2d7f-48b4-9928-deb31b667954" width="85%" />
</p>

- 실제로 클래스가 한 번에 두 인터페이스를 구현할 수 없도록 정의하는 인터페이스가 있을 수 있다.
- 두 개의 충돌하는 인터페이스를 구현하는 클래스를 선언하려고 하면 클래스에 하나 이상의 타입 오류가 발생한다.

**[예제 2]**<br>
- AgeIsNumber와 AgeIsNotNumber 인터페이스는 age 속성을 서로 다른 타입으로 선언한다.
- AsNumber 클래스와 NotAsNumber 클래스 모두 두 인터페이스를 제대로 구현하지 못했다.

```ts
interface AgeIsNumber {
  age: number;
}

interface AgeIsNotNumber {
  age: () => string;
}
```

<p align="center">
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/2453a2f4-0e26-485b-ad1d-e2487fb6dc9e" width="100%" />
  <img src="https://github.com/FE-BookStudy/LearningTS/assets/97720335/cd09103a-117d-4015-bcaa-16822bea8f8f" width="100%" />
</p>

두 인터페이스가 매우 다른 객체 형태를 표현하려는 경우에는 동일한 클래스로 구현하지 않아야 한다.

<br>

**Reference**

러닝 타입스크립트 (Learning TypeScript)
