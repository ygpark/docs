---
title: '6장: 객체와 배열'
slug: javascript-bible-objects-arrays
description: '자바스크립트의 핵심 데이터 구조인 객체와 배열을 마스터하고, 최신 ES6+ 문법으로 효율적인 데이터 처리 방법을 배워보세요.'
keywords:
  [
    '자바스크립트 객체',
    '자바스크립트 배열',
    '구조분해할당',
    'Optional chaining',
    '배열 메서드',
    'ES6 객체',
    '객체 리터럴',
    '배열 조작',
  ]
sidebar_position: 6
---

# 6장: 객체와 배열

## 학습 목표

이번 장에서는 자바스크립트의 가장 중요한 두 가지 데이터 구조인 객체와 배열을 깊이 있게 다룹니다. 단순한 데이터 저장소를 넘어서, 현실 세계의 복잡한 정보를 효과적으로 모델링하고 조작하는 방법을 배우게 됩니다. 특히 ES6+에서 도입된 최신 문법들을 활용하여 더 안전하고 효율적인 코드를 작성하는 능력을 기를 것입니다.

---

## 6.1 객체의 기초

객체는 현실 세계의 사물이나 개념을 프로그래밍으로 표현할 때 가장 자연스러운 방법입니다. 사람, 자동차, 책과 같은 실체부터 설정값, 상태 정보 같은 추상적인 개념까지 모든 것을 객체로 나타낼 수 있습니다. 자바스크립트에서 객체는 키(key)와 값(value) 쌍으로 구성된 프로퍼티들의 집합이며, 이는 다른 언어의 해시맵이나 딕셔너리와 유사한 개념입니다.

### 객체 생성과 속성 접근

```javascript
// 객체 리터럴 방식으로 객체 생성
const person = {
  name: '김민수',
  age: 25,
  occupation: '개발자',
  isEmployed: true,
};

// 점 표기법으로 속성 접근
console.log(person.name); // '김민수'
console.log(person.age); // 25

// 대괄호 표기법으로 속성 접근
console.log(person['occupation']); // '개발자'

// 동적 속성 접근
const propertyName = 'isEmployed';
console.log(person[propertyName]); // true
```

### 객체 속성 조작

```javascript
const book = {
  title: '모던 자바스크립트',
  author: '김개발',
  pages: 500,
};

// 새 속성 추가
book.publisher = '테크출판사';
book['publishYear'] = 2024;

// 속성 수정
book.pages = 550;

// 속성 삭제
delete book.publishYear;

console.log(book);
// { title: '모던 자바스크립트', author: '김개발', pages: 550, publisher: '테크출판사' }
```

---

## 6.2 Optional Chaining 연산자

중첩된 객체 구조에서 안전하게 속성에 접근하는 것은 실무에서 매우 중요한 기술입니다. 전통적으로는 각 단계마다 존재 여부를 확인해야 했지만, ES2020에서 도입된 Optional Chaining 연산자를 사용하면 훨씬 간결하고 안전한 코드를 작성할 수 있습니다.

```javascript
const user = {
  id: 1,
  profile: {
    personal: {
      name: '이영희',
      address: {
        city: '서울',
        district: '강남구',
      },
    },
  },
};

// 전통적인 방식 (장황하고 실수하기 쉬움)
if (user && user.profile && user.profile.personal && user.profile.personal.address) {
  console.log(user.profile.personal.address.city);
}

// Optional Chaining 사용 (간결하고 안전함)
console.log(user.profile?.personal?.address?.city); // '서울'

// 존재하지 않는 속성에 접근
console.log(user.profile?.work?.company?.name); // undefined (에러 발생하지 않음)

// 메서드 호출에도 사용 가능
const api = {
  getData: function () {
    return '데이터';
  },
};

console.log(api.getData?.()); // '데이터'
console.log(api.deleteData?.()); // undefined (메서드가 없어도 에러 없음)
```

---

## 6.3 배열의 생성과 조작

배열은 순서가 있는 데이터의 집합을 다룰 때 사용하는 핵심 자료구조입니다. 목록, 시퀀스, 컬렉션 등 다양한 형태의 데이터를 효율적으로 관리할 수 있으며, 자바스크립트는 배열 조작을 위한 강력하고 다양한 메서드들을 제공합니다.

### 배열 생성 방법

```javascript
// 배열 리터럴 (가장 일반적)
const fruits = ['사과', '바나나', '오렌지'];

// Array 생성자
const numbers = new Array(1, 2, 3, 4, 5);

// Array.from() - 유사 배열이나 이터러블을 배열로 변환
const chars = Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']

// Array.of() - 전달받은 인수들로 배열 생성
const values = Array.of(1, 2, 3); // [1, 2, 3]

// 빈 배열을 특정 값으로 채우기
const zeros = new Array(5).fill(0); // [0, 0, 0, 0, 0]
```

### 최신 배열 메서드들

```javascript
const items = ['첫번째', '두번째', '세번째', '네번째'];

// Array.at() - 음수 인덱스 지원
console.log(items.at(-1)); // '네번째' (마지막 요소)
console.log(items.at(-2)); // '세번째' (뒤에서 두번째)

// 배열 복사 및 정렬 (원본 배열 변경하지 않음)
const numbers = [3, 1, 4, 1, 5];
const sorted = numbers.toSorted(); // [1, 1, 3, 4, 5] (원본 유지)
const reversed = numbers.toReversed(); // [5, 1, 4, 1, 3] (원본 유지)

console.log(numbers); // [3, 1, 4, 1, 5] (원본 그대로)
```

---

## 6.4 구조 분해 할당

구조 분해 할당은 배열이나 객체의 값을 개별 변수로 쉽게 추출할 수 있게 해주는 ES6의 혁신적인 기능입니다. 함수의 매개변수, 반환값 처리, 변수 교환 등 다양한 상황에서 코드를 더 읽기 쉽고 간결하게 만들어줍니다.

### 배열 구조 분해 할당

```javascript
const colors = ['빨강', '초록', '파랑', '노랑'];

// 기본 구조 분해
const [first, second, third] = colors;
console.log(first); // '빨강'
console.log(second); // '초록'
console.log(third); // '파랑'

// 일부 요소 건너뛰기
const [primary, , tertiary] = colors;
console.log(primary); // '빨강'
console.log(tertiary); // '파랑'

// 나머지 요소들 수집
const [main, ...others] = colors;
console.log(main); // '빨강'
console.log(others); // ['초록', '파랑', '노랑']

// 기본값 설정
const [a, b, c, d, e = '기본색'] = colors;
console.log(e); // '노랑' (실제 값이 있으므로)

const [x, y, z, w, v = '기본색'] = ['red', 'green'];
console.log(v); // '기본색' (값이 없으므로 기본값 사용)
```

### 객체 구조 분해 할당

```javascript
const student = {
  name: '박학생',
  age: 20,
  major: '컴퓨터공학',
  university: '코딩대학교',
  gpa: 3.8,
};

// 기본 구조 분해
const { name, age, major } = student;
console.log(name); // '박학생'
console.log(age); // 20
console.log(major); // '컴퓨터공학'

// 변수명 변경
const { name: studentName, university: school } = student;
console.log(studentName); // '박학생'
console.log(school); // '코딩대학교'

// 기본값 설정
const { gpa, scholarship = '없음' } = student;
console.log(gpa); // 3.8
console.log(scholarship); // '없음'

// 나머지 속성들 수집
const { name: n, age: a, ...restInfo } = student;
console.log(restInfo); // { major: '컴퓨터공학', university: '코딩대학교', gpa: 3.8 }
```

---

## 6.5 객체 단축 표기법

ES6에서 도입된 객체 단축 표기법은 객체를 생성할 때 코드를 더 간결하고 읽기 쉽게 만들어줍니다. 특히 변수명과 속성명이 같을 때, 그리고 메서드를 정의할 때 매우 유용합니다.

```javascript
const name = '김개발';
const age = 28;
const city = '부산';

// 전통적인 방식
const person1 = {
  name: name,
  age: age,
  city: city,
  introduce: function () {
    return `안녕하세요, 저는 ${this.name}입니다.`;
  },
};

// 단축 표기법 사용
const person2 = {
  name, // name: name과 동일
  age, // age: age와 동일
  city, // city: city와 동일

  // 메서드 단축 표기법
  introduce() {
    return `안녕하세요, 저는 ${this.name}입니다.`;
  },

  // 계산된 속성명
  [`${name}_info`]: '추가 정보',
};

console.log(person2.introduce()); // '안녕하세요, 저는 김개발입니다.'
console.log(person2.김개발_info); // '추가 정보'
```

---

## 6.6 실습 1: 학생 성적 관리 시스템

이번 실습에서는 지금까지 배운 객체와 배열의 모든 기능을 종합적으로 활용해보겠습니다. 실제 학교에서 사용할 법한 성적 관리 시스템을 만들어보면서 데이터 구조 설계부터 복잡한 데이터 조작까지 경험해보세요.

```javascript
// 학생 데이터 구조 설계
const students = [
  {
    id: 1,
    name: '김영수',
    grade: 2,
    subjects: {
      math: { score: 85, credit: 3 },
      english: { score: 92, credit: 3 },
      science: { score: 78, credit: 2 },
    },
    contact: {
      email: 'kim@school.com',
      phone: '010-1234-5678',
    },
  },
  {
    id: 2,
    name: '이민정',
    grade: 2,
    subjects: {
      math: { score: 95, credit: 3 },
      english: { score: 88, credit: 3 },
      science: { score: 91, credit: 2 },
    },
    contact: {
      email: 'lee@school.com',
      // phone 정보 없음
    },
  },
];

// 성적 관리 시스템 객체
const gradeManager = {
  students,

  // 평균 점수 계산 (가중평균)
  calculateGPA(student) {
    const { subjects } = student;
    let totalScore = 0;
    let totalCredit = 0;

    for (const subject in subjects) {
      const { score, credit } = subjects[subject];
      totalScore += score * credit;
      totalCredit += credit;
    }

    return Math.round((totalScore / totalCredit) * 100) / 100;
  },

  // 학생별 성적표 생성
  generateReportCard(studentId) {
    const student = this.students.find(s => s.id === studentId);
    if (!student) return null;

    const { name, subjects, contact } = student;
    const gpa = this.calculateGPA(student);

    return {
      studentName: name,
      gpa,
      subjects: Object.entries(subjects).map(([subjectName, info]) => ({
        subject: subjectName,
        ...info,
      })),
      contactInfo: {
        email: contact.email,
        phone: contact.phone || '정보 없음',
      },
    };
  },

  // 상위 학생 찾기
  getTopStudents(limit = 3) {
    return this.students
      .map(student => ({
        ...student,
        gpa: this.calculateGPA(student),
      }))
      .toSorted((a, b) => b.gpa - a.gpa)
      .slice(0, limit);
  },
};

// 사용 예제
const reportCard = gradeManager.generateReportCard(1);
console.log('성적표:', reportCard);

const topStudents = gradeManager.getTopStudents(2);
console.log('상위 학생들:', topStudents);
```

---

## 6.7 실습 2: 중첩 객체 안전하게 접근하기

API에서 받아온 복잡한 데이터 구조를 안전하게 처리하는 것은 실무에서 매우 중요한 스킬입니다. Optional Chaining과 구조 분해 할당을 활용하여 견고한 데이터 처리 함수를 만들어보겠습니다.

```javascript
// API에서 받아온 사용자 데이터 (일부 정보가 누락될 수 있음)
const apiResponse = {
  user: {
    id: 12345,
    profile: {
      name: '홍길동',
      avatar: 'https://example.com/avatar.jpg',
      preferences: {
        theme: 'dark',
        notifications: {
          email: true,
          push: false,
        },
      },
    },
    account: {
      plan: 'premium',
      billing: {
        method: 'card',
        address: {
          country: 'KR',
          city: '서울',
        },
      },
    },
  },
};

// 안전한 데이터 추출 함수
function extractUserInfo(data) {
  // Optional Chaining으로 안전하게 접근
  const userId = data?.user?.id;
  const userName = data?.user?.profile?.name || '이름 없음';
  const userAvatar = data?.user?.profile?.avatar;
  const theme = data?.user?.profile?.preferences?.theme || 'light';
  const emailNotification = data?.user?.profile?.preferences?.notifications?.email ?? false;
  const billingCountry = data?.user?.account?.billing?.address?.country;

  // 구조 분해 할당으로 필요한 정보만 추출
  const { user: { account: { plan = 'free' } = {} } = {} } = data || {};

  return {
    id: userId,
    name: userName,
    avatar: userAvatar,
    settings: {
      theme,
      emailNotification,
      plan,
    },
    location: billingCountry || '정보 없음',
  };
}

// 빈 데이터나 부분 데이터 테스트
const incompleteData = {
  user: {
    id: 67890,
    profile: {
      name: '김철수',
      // preferences 정보 없음
    },
    // account 정보 없음
  },
};

console.log('완전한 데이터:', extractUserInfo(apiResponse));
console.log('불완전한 데이터:', extractUserInfo(incompleteData));
console.log('빈 데이터:', extractUserInfo({}));
```

---

## 정리

이번 장에서는 자바스크립트의 핵심 데이터 구조인 객체와 배열을 깊이 있게 다뤘습니다. 단순한 데이터 저장소를 넘어서 현실 세계의 복잡한 정보를 모델링하고, 최신 ES6+ 문법을 활용하여 안전하고 효율적인 코드를 작성하는 방법을 배웠습니다.

특히 Optional Chaining 연산자는 중첩된 객체 구조에서 안전하게 데이터에 접근할 수 있게 해주며, 구조 분해 할당은 복잡한 데이터에서 필요한 정보만 깔끔하게 추출할 수 있게 해줍니다. 이러한 현대적인 문법들을 마스터하면 더 읽기 쉽고 유지보수하기 좋은 코드를 작성할 수 있습니다.

다음 장에서는 지금까지 배운 모든 기초 지식을 종합하여 실제 프로젝트를 통해 실력을 다져보겠습니다.
