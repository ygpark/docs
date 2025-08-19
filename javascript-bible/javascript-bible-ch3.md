---
title: '3장: 모던 변수 선언과 스코프'
slug: javascript-bible-modern-variables-and-scope
description: '모던 자바스크립트의 let, const 변수 선언과 스코프에 대해 학습합니다. var를 피해야 하는 이유와 호이스팅, 템플릿 리터럴까지 완벽 정리!'
keywords:
  [
    'javascript',
    'let',
    'const',
    'var',
    '스코프',
    '호이스팅',
    '템플릿 리터럴',
    '블록 스코프',
    '모던 자바스크립트',
  ]
sidebar_position: 3
---

# 3장: 모던 변수 선언과 스코프

안녕하세요! 이번 장에서는 모던 자바스크립트의 핵심인 변수 선언 방법과 스코프에 대해 알아보겠습니다. 과거 `var`만 사용하던 시절을 벗어나 `let`과 `const`가 가져온 혁신적인 변화들을 살펴보고, 스코프라는 개념이 어떻게 코드의 안전성을 높여주는지 함께 탐구해보겠습니다. 또한 ES6에서 도입된 템플릿 리터럴로 문자열을 더욱 우아하게 다루는 방법도 배워보겠습니다.

## 학습 목표

이 장을 완료하면 다음과 같은 능력을 갖게 됩니다:

- `let`, `const`, `var`의 차이점을 명확히 이해하고 적절한 상황에서 사용할 수 있습니다
- 블록 스코프와 함수 스코프의 개념을 파악하여 변수의 유효 범위를 예측할 수 있습니다
- 호이스팅 현상을 이해하고 관련된 버그를 예방할 수 있습니다
- 템플릿 리터럴을 활용하여 동적이고 가독성 높은 문자열을 생성할 수 있습니다

---

## let과 const: 모던 변수 선언의 시작

과거 자바스크립트에서는 `var`만으로 모든 변수를 선언했습니다. 하지만 ES6(ES2015)에서 `let`과 `const`가 도입되면서 변수 선언이 훨씬 안전하고 예측 가능해졌습니다.

### let: 재할당 가능한 변수

`let`은 값을 재할당할 수 있는 변수를 선언할 때 사용합니다.

```javascript title="let-basic.js"
let message = '안녕하세요!';
console.log(message); // "안녕하세요!"

message = '좋은 하루되세요!'; // 재할당 가능
console.log(message); // "좋은 하루되세요!"

let count; // 초기값 없이 선언 가능
count = 0; // 나중에 값 할당
```

### const: 상수 선언

`const`는 한 번 할당된 값을 변경할 수 없는 상수를 선언합니다.

```javascript title="const-basic.js"
const PI = 3.14159;
console.log(PI); // 3.14159

// PI = 3.14; // TypeError: Assignment to constant variable.

const user = { name: '김철수', age: 25 };
user.age = 26; // 객체 내부 속성은 변경 가능
console.log(user); // { name: "김철수", age: 26 }

// user = {}; // TypeError: Assignment to constant variable.
```

### var를 피해야 하는 이유

`var`는 여러 문제점을 가지고 있어 현재는 사용을 권장하지 않습니다.

```javascript title="var-problems.js"
// 문제 1: 함수 스코프만 지원 (블록 스코프 무시)
if (true) {
  var oldWay = '문제가 있어요';
  let newWay = '안전해요';
}
console.log(oldWay); // "문제가 있어요" - 접근 가능 (위험!)
// console.log(newWay); // ReferenceError: newWay is not defined

// 문제 2: 재선언 허용
var name = '첫 번째';
var name = '두 번째'; // 에러 없이 재선언됨
console.log(name); // "두 번째"

// 문제 3: 호이스팅으로 인한 혼란
console.log(hoistedVar); // undefined (에러가 아님)
var hoistedVar = '값';
```

---

## 블록 스코프와 함수 스코프

스코프는 변수가 접근 가능한 범위를 의미합니다. 이를 이해하면 변수가 어디서 사용될 수 있는지 예측할 수 있어 버그를 예방할 수 있습니다.

### 블록 스코프

`let`과 `const`는 블록 스코프를 가집니다. 중괄호 `{}`로 둘러싸인 영역이 하나의 블록입니다.

```javascript title="block-scope.js"
function demoBlockScope() {
  let outerVariable = '외부 변수';

  if (true) {
    let innerVariable = '내부 변수';
    const blockConstant = '블록 상수';

    console.log(outerVariable); // "외부 변수" - 접근 가능
    console.log(innerVariable); // "내부 변수" - 접근 가능
  }

  console.log(outerVariable); // "외부 변수" - 접근 가능
  // console.log(innerVariable); // ReferenceError: innerVariable is not defined
  // console.log(blockConstant); // ReferenceError: blockConstant is not defined
}

demoBlockScope();
```

### 함수 스코프

`var`는 함수 스코프를 가집니다. 함수 내에서 선언된 변수는 해당 함수 내에서만 접근 가능합니다.

```javascript title="function-scope.js"
function demoFunctionScope() {
  if (true) {
    var functionScoped = '함수 스코프';
    let blockScoped = '블록 스코프';
  }

  console.log(functionScoped); // "함수 스코프" - 접근 가능
  // console.log(blockScoped); // ReferenceError: blockScoped is not defined
}

// console.log(functionScoped); // ReferenceError: functionScoped is not defined
```

---

## 호이스팅의 이해

호이스팅은 변수 선언이 해당 스코프의 최상단으로 끌어올려지는 자바스크립트의 특성입니다. 하지만 `var`, `let`, `const`는 각각 다르게 동작합니다.

### var 호이스팅

```javascript title="var-hoisting.js"
console.log(varVariable); // undefined (에러 아님)
var varVariable = 'var 변수';
console.log(varVariable); // "var 변수"

// 위 코드는 실제로 다음과 같이 동작합니다:
// var varVariable; // undefined로 초기화
// console.log(varVariable);
// varVariable = "var 변수";
// console.log(varVariable);
```

### let과 const 호이스팅

```javascript title="let-const-hoisting.js"
// console.log(letVariable); // ReferenceError: Cannot access 'letVariable' before initialization
let letVariable = 'let 변수';

// console.log(constVariable); // ReferenceError: Cannot access 'constVariable' before initialization
const constVariable = 'const 변수';

// let과 const도 호이스팅되지만 초기화 전까지는 접근할 수 없습니다.
// 이를 'Temporal Dead Zone'이라고 합니다.
```

---

## 템플릿 리터럴과 문자열 처리

ES6에서 도입된 템플릿 리터럴은 백틱(`)을 사용하여 문자열을 더욱 편리하게 처리할 수 있게 해줍니다.

### 기본 사용법

```javascript title="template-literals.js"
const name = '김철수';
const age = 25;

// 기존 방식
const oldWay = '안녕하세요! 저는 ' + name + '이고, ' + age + '살입니다.';

// 템플릿 리터럴
const newWay = `안녕하세요! 저는 ${name}이고, ${age}살입니다.`;

console.log(oldWay);
console.log(newWay);
// 둘 다 같은 결과: "안녕하세요! 저는 김철수이고, 25살입니다."
```

### 다중 라인 문자열

```javascript title="multiline-strings.js"
// 기존 방식
const oldMultiline = '첫 번째 줄\n' + '두 번째 줄\n' + '세 번째 줄';

// 템플릿 리터럴
const newMultiline = `첫 번째 줄
두 번째 줄
세 번째 줄`;

console.log(newMultiline);
```

### 표현식 활용

```javascript title="template-expressions.js"
const price = 15000;
const quantity = 3;

const receipt = `
=== 영수증 ===
상품 가격: ${price.toLocaleString()}원
수량: ${quantity}개
총 금액: ${(price * quantity).toLocaleString()}원
할인 적용: ${price * quantity > 50000 ? '10% 할인' : '할인 없음'}
`;

console.log(receipt);
```

---

## 실습 1: 스코프 실험실

다양한 스코프 상황을 직접 체험해보면서 변수의 유효 범위를 확인해보겠습니다.

```javascript title="scope-lab.js"
// 스코프 실험실: 변수들의 접근 범위를 테스트해보세요
function scopeLab() {
  // 전역 스코프 (함수 내 최상위)
  let globalInFunction = '함수 내 전역';
  const CONSTANT_VALUE = 42;

  console.log('=== 함수 최상위 레벨 ===');
  console.log('globalInFunction:', globalInFunction);
  console.log('CONSTANT_VALUE:', CONSTANT_VALUE);

  // 블록 스코프 테스트 1: if 문
  if (true) {
    let blockVariable = 'if 블록 변수';
    const blockConstant = 'if 블록 상수';

    console.log('\n=== if 블록 내부 ===');
    console.log('globalInFunction:', globalInFunction); // 외부 접근 가능
    console.log('blockVariable:', blockVariable);
    console.log('blockConstant:', blockConstant);
  }

  // 블록 스코프 테스트 2: for 루프
  for (let i = 0; i < 2; i++) {
    let loopVariable = `루프 ${i}`;
    console.log(`\n=== for 루프 ${i} ===`);
    console.log('loopVariable:', loopVariable);
    console.log('globalInFunction:', globalInFunction);
  }

  // 블록 스코프 테스트 3: 일반 블록
  {
    let justBlock = '그냥 블록';
    console.log('\n=== 일반 블록 ===');
    console.log('justBlock:', justBlock);
    console.log('globalInFunction:', globalInFunction);
  }

  console.log('\n=== 함수 최하위 (블록 외부) ===');
  console.log('globalInFunction:', globalInFunction); // 접근 가능
  // console.log("blockVariable:", blockVariable); // 에러!
  // console.log("loopVariable:", loopVariable); // 에러!
  // console.log("justBlock:", justBlock); // 에러!
}

scopeLab();
```

이 실습에서는 다양한 블록에서 선언된 변수들이 어떻게 접근되는지 확인할 수 있습니다. 각 변수가 어느 범위에서 접근 가능한지 직접 경험해보세요.

---

## 실습 2: 템플릿 리터럴로 동적 메시지 생성기

템플릿 리터럴의 강력함을 활용하여 다양한 상황에 맞는 메시지를 생성하는 시스템을 만들어보겠습니다.

```javascript title="message-generator.js"
// 동적 메시지 생성기: 상황에 맞는 개인화된 메시지를 생성합니다
function createMessageGenerator() {
  // 사용자 정보
  const users = [
    { name: '김철수', age: 28, job: '개발자', level: 'senior' },
    { name: '이영희', age: 24, job: '디자이너', level: 'junior' },
    { name: '박민수', age: 35, job: '마케터', level: 'expert' },
  ];

  // 레벨별 인사말
  const getLevelGreeting = level => {
    switch (level) {
      case 'junior':
        return '화이팅!';
      case 'senior':
        return '오늘도 수고하세요!';
      case 'expert':
        return '언제나 존경합니다!';
      default:
        return '좋은 하루되세요!';
    }
  };

  // 시간대별 인사
  const getTimeGreeting = () => {
    const hour = new Date().getHours();
    if (hour < 12) return '좋은 아침이에요';
    if (hour < 18) return '즐거운 오후예요';
    return '편안한 저녁이에요';
  };

  // 개인화된 메시지 생성
  users.forEach(user => {
    const message = `
╭─────────────────────────────╮
│        개인화된 메시지        │
╰─────────────────────────────╯

${getTimeGreeting()}, ${user.name}님!

👤 기본 정보
   • 이름: ${user.name}
   • 나이: ${user.age}세
   • 직업: ${user.job}
   • 경력: ${user.level} 레벨

🎯 맞춤 인사말
   ${getLevelGreeting(user.level)}

📊 활동 통계
   • 가입일로부터: ${Math.floor(Math.random() * 365) + 1}일
   • 총 접속 횟수: ${Math.floor(Math.random() * 100) + 50}회
   • 완료한 작업: ${Math.floor(Math.random() * 50) + 10}개

💡 오늘의 목표
   ${
     user.job === '개발자'
       ? '버그 없는 깨끗한 코드 작성하기'
       : user.job === '디자이너'
         ? '사용자 경험을 향상시키는 디자인 만들기'
         : '효과적인 마케팅 전략 수립하기'
   }

────────────────────────────────
`;

    console.log(message);
  });

  // 특별한 날 메시지 생성
  const createSpecialMessage = (eventType, details) => {
    const templates = {
      birthday: `
🎉 생일 축하합니다! 🎉

${details.name}님의 ${details.age}번째 생일을 진심으로 축하드립니다!
올해도 ${details.wish || '건강하고 행복한 한 해'}가 되시길 바랍니다.

🎂 생일 선물: ${details.gift || '특별 할인 쿠폰'}
`,
      promotion: `
🎊 승진을 축하드립니다! 🎊

${details.name}님이 ${details.position}으로 승진하셨습니다!
그동안의 노력이 결실을 맺은 것 같아 정말 기쁩니다.

💼 새로운 역할: ${details.position}
📈 승진일: ${details.date}
`,
      achievement: `
🏆 성과 달성을 축하드립니다! 🏆

${details.name}님이 "${details.achievement}"를 달성하셨습니다!
목표 달성률: ${details.percentage}%

🎯 달성 내용: ${details.description}
⭐ 보상: ${details.reward}
`,
    };

    return templates[eventType] || '축하합니다!';
  };

  // 특별한 날 메시지 예제
  console.log(
    createSpecialMessage('birthday', {
      name: '김철수',
      age: 29,
      wish: '꿈을 향해 달려가는 한 해',
      gift: '프리미엄 멤버십 1개월',
    })
  );

  console.log(
    createSpecialMessage('promotion', {
      name: '이영희',
      position: '시니어 디자이너',
      date: '2024년 8월 20일',
    })
  );
}

createMessageGenerator();
```

이 실습에서는 템플릿 리터럴의 다양한 활용법을 경험할 수 있습니다. 변수 삽입, 표현식 계산, 조건부 메시지, 다중 라인 문자열 등을 통해 실무에서 자주 사용되는 패턴들을 익혀보세요.

---

## 마무리

이번 장에서는 모던 자바스크립트의 핵심인 변수 선언과 스코프에 대해 자세히 알아보았습니다. `let`과 `const`의 도입으로 코드가 더욱 안전하고 예측 가능해졌으며, 블록 스코프 개념으로 변수의 유효 범위를 더 정밀하게 제어할 수 있게 되었습니다.

또한 템플릿 리터럴을 통해 문자열 처리가 얼마나 편리해졌는지도 확인했습니다. 이러한 문법들은 단순히 새로운 기능이 아니라, 더 나은 코드를 작성하고 버그를 예방하는 데 도움을 주는 강력한 도구들입니다.

다음 장에서는 제어 구조와 반복문을 통해 프로그램의 흐름을 제어하는 방법을 배워보겠습니다. 지금까지 배운 변수 선언 방법들과 함께 활용하면 더욱 강력한 프로그램을 만들 수 있을 것입니다!

앞으로도 계속 실습하면서 이런 개념들을 자연스럽게 사용할 수 있도록 연습해보세요. 프로그래밍은 이론보다는 실제로 손으로 코딩해보는 것이 가장 중요합니다!
