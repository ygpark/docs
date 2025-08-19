---
title: '4장: 제어 구조와 반복문'
slug: javascript-bible-control-structures-loops
description: '자바스크립트의 조건문과 반복문을 활용하여 프로그램의 흐름을 제어하는 방법을 학습합니다. if문부터 for...of까지 모던 자바스크립트의 제어 구조를 완벽하게 마스터해보세요.'
keywords:
  [
    'javascript 조건문',
    'javascript 반복문',
    'if문',
    'switch문',
    'for 루프',
    'while 루프',
    'for...of',
    'for...in',
    'break',
    'continue',
    '자바스크립트 제어구조',
  ]
sidebar_position: 4
---

# 4장: 제어 구조와 반복문

프로그램이 단순히 위에서 아래로만 실행된다면 매우 제한적일 것입니다. 실제로는 조건에 따라 다른 코드를 실행하거나, 같은 코드를 여러 번 반복해야 하는 경우가 많습니다. 이번 장에서는 프로그램의 실행 흐름을 제어할 수 있는 다양한 구조들을 학습하고, 실제 문제 해결에 활용하는 방법을 익혀보겠습니다.

## 학습 목표

이번 장을 마치면 다음과 같은 능력을 갖게 됩니다:

- 조건문을 사용하여 상황에 따른 분기 처리를 구현할 수 있습니다
- 다양한 반복문을 적절한 상황에 선택해서 사용할 수 있습니다
- break와 continue를 활용하여 반복문의 흐름을 세밀하게 제어할 수 있습니다
- 실제 프로젝트에서 제어 구조를 조합하여 복잡한 로직을 구현할 수 있습니다

---

## 조건문 (Conditional Statements)

조건문은 특정 조건이 참인지 거짓인지에 따라 프로그램의 실행 경로를 결정하는 구조입니다. 일상생활에서 "만약 비가 오면 우산을 가져간다"와 같은 논리를 코드로 표현할 수 있게 해줍니다. 자바스크립트에서는 주로 if문과 switch문을 사용합니다.

### if문의 기본 구조

if문은 가장 기본적인 조건문으로, 조건식이 true일 때만 코드 블록을 실행합니다.

```javascript title="basic-if.js"
const weather = 'rainy';

if (weather === 'rainy') {
  console.log('우산을 가져가세요!');
}

// 조건이 거짓일 때의 처리
if (weather === 'sunny') {
  console.log('선글라스를 착용하세요!');
} else {
  console.log('날씨를 확인해보세요.');
}
```

### if...else if...else 구조

여러 조건을 순차적으로 검사하고 싶을 때 사용합니다.

```javascript title="multiple-conditions.js"
const score = 85;

if (score >= 90) {
  console.log('A등급: 우수');
} else if (score >= 80) {
  console.log('B등급: 양호');
} else if (score >= 70) {
  console.log('C등급: 보통');
} else if (score >= 60) {
  console.log('D등급: 미흡');
} else {
  console.log('F등급: 재시험');
}
```

### switch문 활용하기

값이 여러 경우 중 하나와 정확히 일치하는지 확인할 때 switch문이 더 읽기 쉬울 수 있습니다.

```javascript title="switch-example.js"
const dayOfWeek = '월요일';

switch (dayOfWeek) {
  case '월요일':
    console.log('새로운 한 주의 시작!');
    break;
  case '화요일':
  case '수요일':
  case '목요일':
    console.log('평일입니다.');
    break;
  case '금요일':
    console.log('불금이에요!');
    break;
  case '토요일':
  case '일요일':
    console.log('주말입니다!');
    break;
  default:
    console.log('올바른 요일을 입력해주세요.');
}
```

---

## 반복문 (Loops)

반복문은 같은 코드를 여러 번 실행할 때 사용합니다. 수동으로 코드를 반복 작성하는 대신, 조건이 만족될 때까지 자동으로 반복 실행되도록 합니다. 자바스크립트에는 다양한 종류의 반복문이 있으며, 상황에 따라 적절한 것을 선택해야 합니다.

### for문 - 가장 기본적인 반복문

for문은 초기값, 조건, 증감식을 한 번에 정의할 수 있어 가장 체계적인 반복문입니다.

```javascript title="for-loop.js"
// 1부터 5까지 출력
for (let i = 1; i <= 5; i++) {
  console.log(`${i}번째 반복`);
}

// 배열 순회하기
const fruits = ['사과', '바나나', '오렌지'];
for (let i = 0; i < fruits.length; i++) {
  console.log(`${i + 1}. ${fruits[i]}`);
}

// 거꾸로 반복하기
for (let i = 5; i >= 1; i--) {
  console.log(`카운트다운: ${i}`);
}
```

### while문과 do-while문

while문은 조건이 true인 동안 계속 반복하며, do-while문은 최소 한 번은 실행됩니다.

```javascript title="while-loops.js"
// while문 - 조건을 먼저 검사
let count = 0;
while (count < 3) {
  console.log(`while: ${count}`);
  count++;
}

// do-while문 - 실행 후 조건 검사
let number = 10;
do {
  console.log(`do-while: ${number}`);
  number++;
} while (number < 10); // 조건이 false여도 한 번은 실행됨
```

### for...of와 for...in 루프

모던 자바스크립트에서 추가된 편리한 반복문들입니다.

```javascript title="modern-loops.js"
const colors = ['빨강', '파랑', '노랑'];

// for...of: 배열의 값을 순회
for (const color of colors) {
  console.log(`색상: ${color}`);
}

// for...in: 객체의 속성을 순회
const person = {
  name: '김철수',
  age: 25,
  city: '서울',
};

for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}

// 문자열도 순회 가능
const message = 'Hello';
for (const char of message) {
  console.log(char);
}
```

---

## break와 continue 활용

반복문의 흐름을 세밀하게 제어할 때 break와 continue를 사용합니다. break는 반복문을 완전히 종료하고, continue는 현재 반복을 건너뛰고 다음 반복으로 넘어갑니다.

```javascript title="break-continue.js"
// break 사용 예시
console.log('=== break 예시 ===');
for (let i = 1; i <= 10; i++) {
  if (i === 5) {
    console.log('5에서 반복 종료!');
    break;
  }
  console.log(i);
}

// continue 사용 예시
console.log('=== continue 예시 ===');
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    console.log('3은 건너뛰기!');
    continue;
  }
  console.log(i);
}

// 실용적인 예시: 유효한 데이터만 처리
const data = [1, null, 3, undefined, 5, '', 7];
for (const item of data) {
  if (!item) {
    continue; // 유효하지 않은 데이터는 건너뛰기
  }
  console.log(`처리할 데이터: ${item}`);
}
```

---

## 실습 1: 숫자 맞추기 게임

사용자가 1부터 100 사이의 숫자를 맞추는 간단한 게임을 만들어보겠습니다. 이 예제는 조건문과 반복문을 함께 활용하는 실전 예시입니다.

```javascript title="number-guessing-game.js"
// 브라우저 환경에서 실행 가능한 숫자 맞추기 게임
function numberGuessingGame() {
  const targetNumber = Math.floor(Math.random() * 100) + 1;
  let attempts = 0;
  const maxAttempts = 7;

  console.log('=== 숫자 맞추기 게임 ===');
  console.log('1부터 100 사이의 숫자를 맞춰보세요!');
  console.log(`최대 ${maxAttempts}번의 기회가 있습니다.`);

  while (attempts < maxAttempts) {
    // 실제 사용 시에는 prompt() 대신 입력 폼을 사용하세요
    const userGuess = parseInt(prompt('숫자를 입력하세요:'));
    attempts++;

    if (isNaN(userGuess)) {
      console.log('올바른 숫자를 입력해주세요!');
      continue;
    }

    if (userGuess === targetNumber) {
      console.log(`🎉 정답입니다! ${attempts}번 만에 맞추셨네요!`);
      break;
    } else if (userGuess < targetNumber) {
      console.log('더 큰 숫자입니다!');
    } else {
      console.log('더 작은 숫자입니다!');
    }

    const remainingAttempts = maxAttempts - attempts;
    if (remainingAttempts > 0) {
      console.log(`남은 기회: ${remainingAttempts}번`);
    }
  }

  if (attempts === maxAttempts) {
    console.log(`게임 종료! 정답은 ${targetNumber}이었습니다.`);
  }
}

// 게임 실행 (브라우저에서만 작동)
// numberGuessingGame();
```

---

## 실습 2: 구구단 출력기 (다양한 반복문 버전)

같은 결과를 다양한 반복문으로 구현해보면서 각각의 특징을 이해해봅시다.

```javascript title="multiplication-table.js"
// 기본 for문 버전
function printMultiplicationTableBasic() {
  console.log('=== 기본 for문 버전 ===');
  for (let i = 2; i <= 9; i++) {
    console.log(`\n${i}단:`);
    for (let j = 1; j <= 9; j++) {
      console.log(`${i} × ${j} = ${i * j}`);
    }
  }
}

// while문 버전
function printMultiplicationTableWhile() {
  console.log('=== while문 버전 ===');
  let i = 2;
  while (i <= 9) {
    console.log(`\n${i}단:`);
    let j = 1;
    while (j <= 9) {
      console.log(`${i} × ${j} = ${i * j}`);
      j++;
    }
    i++;
  }
}

// 배열과 for...of 버전
function printMultiplicationTableModern() {
  console.log('=== 모던 for...of 버전 ===');
  const tables = [2, 3, 4, 5, 6, 7, 8, 9];
  const multipliers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

  for (const table of tables) {
    console.log(`\n${table}단:`);
    for (const multiplier of multipliers) {
      console.log(`${table} × ${multiplier} = ${table * multiplier}`);
    }
  }
}

// 특정 단만 출력하는 함수
function printSpecificTable(targetTable) {
  if (targetTable < 2 || targetTable > 9) {
    console.log('2부터 9까지의 숫자를 입력해주세요.');
    return;
  }

  console.log(`=== ${targetTable}단 ===`);
  for (let i = 1; i <= 9; i++) {
    const result = targetTable * i;
    console.log(`${targetTable} × ${i} = ${result}`);
  }
}

// 함수 실행 예시
printMultiplicationTableBasic();
// printMultiplicationTableWhile();
// printMultiplicationTableModern();
// printSpecificTable(7);
```

---

## 중첩 반복문과 패턴 만들기

복잡한 패턴을 만들 때 중첩 반복문을 활용할 수 있습니다.

```javascript title="pattern-examples.js"
// 별 패턴 만들기
function printStarPatterns() {
  console.log('=== 삼각형 패턴 ===');
  for (let i = 1; i <= 5; i++) {
    let stars = '';
    for (let j = 1; j <= i; j++) {
      stars += '★';
    }
    console.log(stars);
  }

  console.log('\n=== 역삼각형 패턴 ===');
  for (let i = 5; i >= 1; i--) {
    let stars = '';
    for (let j = 1; j <= i; j++) {
      stars += '★';
    }
    console.log(stars);
  }

  console.log('\n=== 피라미드 패턴 ===');
  for (let i = 1; i <= 5; i++) {
    let spaces = '';
    let stars = '';

    // 앞쪽 공백
    for (let j = 1; j <= 5 - i; j++) {
      spaces += ' ';
    }

    // 별 출력
    for (let j = 1; j <= 2 * i - 1; j++) {
      stars += '★';
    }

    console.log(spaces + stars);
  }
}

printStarPatterns();
```

---

## 실전 팁과 주의사항

### 무한 루프 방지하기

반복문을 작성할 때 가장 주의해야 할 점은 무한 루프입니다.

```javascript title="infinite-loop-prevention.js"
// 잘못된 예시 - 무한 루프 위험
/*
let count = 0;
while (count < 10) {
  console.log(count);
  // count++; 이 빠지면 무한 루프!
}
*/

// 올바른 예시
let count = 0;
while (count < 10) {
  console.log(count);
  count++; // 반드시 조건을 변경하는 코드 포함
}

// for문에서도 주의
for (let i = 0; i < 10 /* i++ 빠지면 무한 루프 */; ) {
  console.log(i);
  i++; // 증감식을 놓쳤다면 여기서라도 처리
}
```

### 성능을 고려한 반복문 작성

```javascript title="performance-tips.js"
const largeArray = new Array(1000000).fill(0).map((_, i) => i);

// 비효율적인 방법
console.time('비효율적');
for (let i = 0; i < largeArray.length; i++) {
  // length 속성을 매번 접근
}
console.timeEnd('비효율적');

// 효율적인 방법
console.time('효율적');
const length = largeArray.length;
for (let i = 0; i < length; i++) {
  // length를 미리 저장해서 사용
}
console.timeEnd('효율적');

// 또는 for...of 사용 (더 읽기 쉬움)
console.time('for...of');
for (const item of largeArray) {
  // 값에만 관심이 있다면 이 방법이 가장 깔끔
}
console.timeEnd('for...of');
```

---

## 이번 장에서 배운 내용 정리

이번 장에서는 자바스크립트의 제어 구조와 반복문에 대해 상세히 학습했습니다. 조건문을 통해 프로그램의 분기를 만들고, 다양한 반복문으로 효율적인 코드를 작성하는 방법을 익혔습니다. 특히 숫자 맞추기 게임과 구구단 출력기를 통해 실제 활용 방법을 체험해보았습니다.

다음 장에서는 이러한 제어 구조를 활용하여 더욱 강력한 기능을 제공하는 함수에 대해 알아보겠습니다. 함수를 마스터하면 코드의 재사용성과 가독성을 크게 향상시킬 수 있습니다.
