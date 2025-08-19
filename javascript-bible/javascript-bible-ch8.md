---
title: '8장: 고차 함수와 배열 메서드'
slug: javascript-bible-chapter8-higher-order-functions-array-methods
description: '자바스크립트 배열의 고차 함수들을 마스터하여 함수형 프로그래밍의 기초를 다지고, 실무에서 자주 사용되는 map, filter, reduce 등을 활용한 데이터 처리 기법을 학습합니다.'
keywords:
  [
    '자바스크립트 고차함수',
    'map filter reduce',
    '배열 메서드',
    'forEach find some every',
    '함수형 프로그래밍',
    '메서드 체이닝',
    'findLast findLastIndex',
    '데이터 변환',
    '자바스크립트 배열',
    '초급자 자바스크립트',
  ]
sidebar_position: 8
---

# 8장: 고차 함수와 배열 메서드

배열은 자바스크립트에서 가장 중요한 데이터 구조 중 하나입니다. 모던 자바스크립트에서는 배열을 다루는 강력한 메서드들이 제공되며, 이들은 모두 '고차 함수'라는 개념을 기반으로 합니다. 고차 함수란 다른 함수를 매개변수로 받거나 함수를 반환하는 함수를 말합니다. 이번 장에서는 함수형 프로그래밍의 핵심이 되는 배열 메서드들을 마스터하여 더 간결하고 읽기 쉬운 코드를 작성하는 방법을 배워보겠습니다.

## 학습 목표

- 고차 함수의 개념과 배열 메서드의 동작 원리 이해
- map, filter, reduce를 활용한 데이터 변환 기법 습득
- forEach, find, some, every 등 다양한 배열 메서드 활용
- 최신 배열 메서드(findLast, findLastIndex)와 메서드 체이닝 기법 학습
- 함수형 프로그래밍 사고방식을 통한 실무 문제 해결 능력 향상

---

## 고차 함수란 무엇인가?

고차 함수는 함수를 값으로 다루는 함수입니다. 자바스크립트에서 함수는 '일급 객체'이므로 변수에 저장하거나 다른 함수의 매개변수로 전달할 수 있습니다.

```javascript
// 기본적인 고차 함수 예제
function greet(name) {
  return `안녕하세요, ${name}님!`;
}

function processUser(user, callback) {
  return callback(user.name);
}

const user = { name: '김철수' };
console.log(processUser(user, greet)); // "안녕하세요, 김철수님!"
```

배열의 많은 메서드들이 바로 이런 고차 함수입니다. 콜백 함수를 받아서 배열의 각 요소에 적용하는 방식으로 동작합니다.

---

## forEach - 배열 순회의 기본

forEach는 배열의 모든 요소를 순회하면서 주어진 함수를 실행합니다. for 문의 함수형 대안으로 많이 사용됩니다.

```javascript
const fruits = ['사과', '바나나', '오렌지'];

// 기존 for 문
for (let i = 0; i < fruits.length; i++) {
  console.log(`${i}: ${fruits[i]}`);
}

// forEach 사용
fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});

// 0: 사과
// 1: 바나나
// 2: 오렌지
```

forEach는 반환값이 없으므로 주로 side effect(로깅, DOM 조작 등)를 위해 사용합니다.

---

## map - 배열 변환의 핵심

map은 배열의 모든 요소에 함수를 적용하여 새로운 배열을 만듭니다. 원본 배열은 변경되지 않습니다.

```javascript
const numbers = [1, 2, 3, 4, 5];

// 각 숫자를 제곱
const squares = numbers.map(num => num * num);
console.log(squares); // [1, 4, 9, 16, 25]

// 온도 변환 (섭씨 -> 화씨)
const celsius = [0, 20, 30, 40];
const fahrenheit = celsius.map(temp => (temp * 9) / 5 + 32);
console.log(fahrenheit); // [32, 68, 86, 104]
```

실무에서는 객체 배열을 변환할 때 자주 사용됩니다.

```javascript
const users = [
  { id: 1, name: '김철수', age: 25 },
  { id: 2, name: '이영희', age: 30 },
  { id: 3, name: '박민수', age: 28 },
];

// 사용자 이름만 추출
const userNames = users.map(user => user.name);
console.log(userNames); // ['김철수', '이영희', '박민수']

// 나이를 1살씩 증가
const olderUsers = users.map(user => ({
  ...user,
  age: user.age + 1,
}));
console.log(olderUsers);
```

---

## filter - 조건에 맞는 요소 선택

filter는 조건을 만족하는 요소들만 골라서 새로운 배열을 만듭니다.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// 짝수만 선택
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

// 5보다 큰 수
const greaterThanFive = numbers.filter(num => num > 5);
console.log(greaterThanFive); // [6, 7, 8, 9, 10]
```

객체 배열에서 특정 조건을 만족하는 항목을 찾을 때 매우 유용합니다.

```javascript
const products = [
  { name: '노트북', price: 1200000, category: 'electronics' },
  { name: '마우스', price: 30000, category: 'electronics' },
  { name: '책', price: 15000, category: 'books' },
  { name: '키보드', price: 80000, category: 'electronics' },
];

// 전자제품만 필터링
const electronics = products.filter(product => product.category === 'electronics');

// 50만원 이하 상품
const affordable = products.filter(product => product.price <= 500000);
console.log(affordable);
```

---

## reduce - 배열을 하나의 값으로 축약

reduce는 배열의 모든 요소를 하나의 값으로 줄이는 강력한 메서드입니다. 초기값과 누적값을 다루는 개념이 중요합니다.

```javascript
const numbers = [1, 2, 3, 4, 5];

// 합계 구하기
const sum = numbers.reduce((accumulator, current) => {
  return accumulator + current;
}, 0);
console.log(sum); // 15

// 최대값 구하기
const max = numbers.reduce((acc, current) => (current > acc ? current : acc));
console.log(max); // 5
```

reduce는 복잡한 데이터 변환에도 사용할 수 있습니다.

```javascript
const students = [
  { name: '김철수', subject: '수학', score: 85 },
  { name: '이영희', subject: '영어', score: 92 },
  { name: '김철수', subject: '영어', score: 78 },
  { name: '이영희', subject: '수학', score: 88 },
];

// 학생별 평균 점수 계산
const averages = students.reduce((acc, student) => {
  if (!acc[student.name]) {
    acc[student.name] = { total: 0, count: 0 };
  }
  acc[student.name].total += student.score;
  acc[student.name].count += 1;
  return acc;
}, {});

// 평균 계산
Object.keys(averages).forEach(name => {
  const avg = averages[name].total / averages[name].count;
  console.log(`${name}: ${avg.toFixed(1)}점`);
});
```

---

## find와 findIndex - 원하는 요소 찾기

find는 조건을 만족하는 첫 번째 요소를 반환하고, findIndex는 해당 요소의 인덱스를 반환합니다.

```javascript
const users = [
  { id: 1, name: '김철수', email: 'kim@example.com' },
  { id: 2, name: '이영희', email: 'lee@example.com' },
  { id: 3, name: '박민수', email: 'park@example.com' },
];

// ID로 사용자 찾기
const user = users.find(user => user.id === 2);
console.log(user); // { id: 2, name: '이영희', email: 'lee@example.com' }

// 인덱스 찾기
const userIndex = users.findIndex(user => user.name === '박민수');
console.log(userIndex); // 2

// 조건을 만족하는 요소가 없을 때
const notFound = users.find(user => user.id === 999);
console.log(notFound); // undefined
```

---

## some과 every - 조건 검사

some은 배열의 요소 중 하나라도 조건을 만족하면 true를 반환하고, every는 모든 요소가 조건을 만족해야 true를 반환합니다.

```javascript
const numbers = [2, 4, 6, 8, 10];
const mixedNumbers = [1, 2, 3, 4, 5];

// 모든 수가 짝수인지 확인
console.log(numbers.every(num => num % 2 === 0)); // true
console.log(mixedNumbers.every(num => num % 2 === 0)); // false

// 짝수가 하나라도 있는지 확인
console.log(mixedNumbers.some(num => num % 2 === 0)); // true

// 실제 사용 예시
const users = [
  { name: '김철수', age: 25, isActive: true },
  { name: '이영희', age: 17, isActive: true },
  { name: '박민수', age: 30, isActive: false },
];

// 모든 사용자가 활성 상태인가?
const allActive = users.every(user => user.isActive);
console.log(allActive); // false

// 미성년자가 있는가?
const hasMinor = users.some(user => user.age < 18);
console.log(hasMinor); // true
```

---

## 최신 배열 메서드: findLast와 findLastIndex

ES2022에서 추가된 findLast와 findLastIndex는 배열의 뒤에서부터 검색을 시작합니다.

```javascript
const scores = [85, 92, 78, 95, 88, 90];

// 마지막으로 90점 이상인 점수 찾기
const lastHighScore = scores.findLast(score => score >= 90);
console.log(lastHighScore); // 90

// 해당 점수의 인덱스
const lastHighScoreIndex = scores.findLastIndex(score => score >= 90);
console.log(lastHighScoreIndex); // 5

// 실무 예시: 최신 로그 찾기
const logs = [
  { timestamp: '2024-01-01', level: 'info', message: '시스템 시작' },
  { timestamp: '2024-01-02', level: 'error', message: '오류 발생' },
  { timestamp: '2024-01-03', level: 'info', message: '정상 동작' },
  { timestamp: '2024-01-04', level: 'error', message: '다시 오류' },
];

const lastError = logs.findLast(log => log.level === 'error');
console.log(lastError); // 가장 최근 에러 로그
```

---

## 메서드 체이닝 - 우아한 데이터 처리

배열 메서드들은 체이닝하여 연속적으로 사용할 수 있습니다. 이를 통해 복잡한 데이터 처리를 간결하게 표현할 수 있습니다.

```javascript
const sales = [
  { product: '노트북', amount: 1200000, quarter: 'Q1' },
  { product: '마우스', amount: 30000, quarter: 'Q1' },
  { product: '키보드', amount: 80000, quarter: 'Q2' },
  { product: '모니터', amount: 400000, quarter: 'Q1' },
  { product: '스피커', amount: 150000, quarter: 'Q2' },
];

// Q1 분기 매출 중 100만원 이상 상품의 총 매출액
const q1HighValueSales = sales
  .filter(sale => sale.quarter === 'Q1') // Q1 분기만
  .filter(sale => sale.amount >= 1000000) // 100만원 이상
  .map(sale => sale.amount) // 금액만 추출
  .reduce((total, amount) => total + amount, 0); // 합계

console.log(q1HighValueSales); // 1200000

// 더 복잡한 예시: 분기별 평균 매출액
const quarterlyAverage = sales.reduce((acc, sale) => {
  if (!acc[sale.quarter]) {
    acc[sale.quarter] = [];
  }
  acc[sale.quarter].push(sale.amount);
  return acc;
}, {});

Object.keys(quarterlyAverage).forEach(quarter => {
  const avg =
    quarterlyAverage[quarter].reduce((sum, amount) => sum + amount, 0) /
    quarterlyAverage[quarter].length;
  console.log(`${quarter} 평균 매출: ${avg.toLocaleString()}원`);
});
```

---

## 실습: 데이터 변환 파이프라인 구축

이제 학습한 내용을 종합하여 실제 데이터를 처리하는 파이프라인을 만들어보겠습니다. 온라인 쇼핑몰의 주문 데이터를 분석하는 시나리오입니다.

```javascript
// 주문 데이터
const orders = [
  {
    id: 1,
    customer: '김철수',
    items: [
      { name: '노트북', price: 1200000, quantity: 1 },
      { name: '마우스', price: 30000, quantity: 2 },
    ],
    status: 'completed',
    date: '2024-08-15',
  },
  {
    id: 2,
    customer: '이영희',
    items: [
      { name: '키보드', price: 80000, quantity: 1 },
      { name: '모니터', price: 400000, quantity: 1 },
    ],
    status: 'pending',
    date: '2024-08-16',
  },
  {
    id: 3,
    customer: '박민수',
    items: [{ name: '스피커', price: 150000, quantity: 1 }],
    status: 'completed',
    date: '2024-08-17',
  },
];

// 1. 주문 총액 계산하는 함수
function calculateOrderTotal(order) {
  return order.items.reduce((total, item) => total + item.price * item.quantity, 0);
}

// 2. 완료된 주문들의 상세 분석
const completedOrdersAnalysis = orders
  .filter(order => order.status === 'completed') // 완료된 주문만
  .map(order => ({
    // 주문 정보 변환
    id: order.id,
    customer: order.customer,
    total: calculateOrderTotal(order),
    date: order.date,
  }))
  .sort((a, b) => b.total - a.total); // 총액 내림차순 정렬

console.log('완료된 주문 분석:');
completedOrdersAnalysis.forEach(order => {
  console.log(`주문 ${order.id}: ${order.customer} - ${order.total.toLocaleString()}원`);
});

// 3. 전체 매출액과 평균 주문액
const totalRevenue = completedOrdersAnalysis.reduce((sum, order) => sum + order.total, 0);

const averageOrderValue = totalRevenue / completedOrdersAnalysis.length;

console.log(`총 매출액: ${totalRevenue.toLocaleString()}원`);
console.log(`평균 주문액: ${averageOrderValue.toLocaleString()}원`);

// 4. 고객별 구매 통계
const customerStats = orders
  .filter(order => order.status === 'completed')
  .reduce((stats, order) => {
    const customer = order.customer;
    const total = calculateOrderTotal(order);

    if (!stats[customer]) {
      stats[customer] = { orderCount: 0, totalSpent: 0 };
    }

    stats[customer].orderCount += 1;
    stats[customer].totalSpent += total;

    return stats;
  }, {});

console.log('고객별 구매 통계:');
Object.entries(customerStats).forEach(([customer, stats]) => {
  console.log(`${customer}: ${stats.orderCount}건, ${stats.totalSpent.toLocaleString()}원`);
});
```

---

## 실습: 쇼핑몰 상품 필터링 시스템

실무에서 자주 마주치는 상황인 상품 검색과 필터링 기능을 구현해보겠습니다.

```javascript
// 상품 데이터
const products = [
  { id: 1, name: '맥북 프로', price: 2500000, category: 'laptop', brand: 'Apple', rating: 4.8 },
  { id: 2, name: '갤럭시북', price: 1200000, category: 'laptop', brand: 'Samsung', rating: 4.3 },
  { id: 3, name: '아이폰 15', price: 1300000, category: 'phone', brand: 'Apple', rating: 4.7 },
  { id: 4, name: '갤럭시 S24', price: 1100000, category: 'phone', brand: 'Samsung', rating: 4.5 },
  { id: 5, name: '아이패드', price: 800000, category: 'tablet', brand: 'Apple', rating: 4.6 },
  { id: 6, name: '갤럭시탭', price: 600000, category: 'tablet', brand: 'Samsung', rating: 4.2 },
];

// 상품 필터링 클래스
class ProductFilter {
  constructor(products) {
    this.products = products;
  }

  // 가격 범위로 필터링
  filterByPriceRange(min, max) {
    this.products = this.products.filter(product => product.price >= min && product.price <= max);
    return this;
  }

  // 카테고리로 필터링
  filterByCategory(category) {
    this.products = this.products.filter(product => product.category === category);
    return this;
  }

  // 브랜드로 필터링
  filterByBrand(brand) {
    this.products = this.products.filter(product => product.brand === brand);
    return this;
  }

  // 평점으로 필터링
  filterByRating(minRating) {
    this.products = this.products.filter(product => product.rating >= minRating);
    return this;
  }

  // 이름으로 검색
  searchByName(keyword) {
    this.products = this.products.filter(product =>
      product.name.toLowerCase().includes(keyword.toLowerCase())
    );
    return this;
  }

  // 정렬
  sortBy(field, order = 'asc') {
    this.products = this.products.sort((a, b) => {
      if (order === 'asc') {
        return a[field] > b[field] ? 1 : -1;
      } else {
        return a[field] < b[field] ? 1 : -1;
      }
    });
    return this;
  }

  // 결과 반환
  getResults() {
    return this.products;
  }

  // 통계 정보
  getStats() {
    if (this.products.length === 0) {
      return { count: 0, averagePrice: 0, averageRating: 0 };
    }

    const totalPrice = this.products.reduce((sum, product) => sum + product.price, 0);
    const totalRating = this.products.reduce((sum, product) => sum + product.rating, 0);

    return {
      count: this.products.length,
      averagePrice: Math.round(totalPrice / this.products.length),
      averageRating: (totalRating / this.products.length).toFixed(1),
    };
  }
}

// 사용 예시
console.log('1. 100만원 이하 Apple 제품 중 평점 4.5 이상, 가격 순 정렬:');
const appleProducts = new ProductFilter([...products])
  .filterByPriceRange(0, 1000000)
  .filterByBrand('Apple')
  .filterByRating(4.5)
  .sortBy('price')
  .getResults();

appleProducts.forEach(product => {
  console.log(`${product.name}: ${product.price.toLocaleString()}원 (평점: ${product.rating})`);
});

console.log('\n2. 노트북 카테고리 통계:');
const laptopFilter = new ProductFilter([...products]).filterByCategory('laptop');

const laptopStats = laptopFilter.getStats();
console.log(`노트북 상품 수: ${laptopStats.count}개`);
console.log(`평균 가격: ${laptopStats.averagePrice.toLocaleString()}원`);
console.log(`평균 평점: ${laptopStats.averageRating}점`);

console.log('\n3. "갤럭시" 검색 결과:');
const galaxyProducts = new ProductFilter([...products])
  .searchByName('갤럭시')
  .sortBy('rating', 'desc')
  .getResults();

galaxyProducts.forEach(product => {
  console.log(`${product.name}: ${product.price.toLocaleString()}원 (평점: ${product.rating})`);
});
```

---

## 함수형 프로그래밍 팁과 주의사항

배열 메서드를 사용할 때 기억해야 할 중요한 점들을 정리해보겠습니다.

**1. 원본 배열 보존**

```javascript
const numbers = [1, 2, 3];

// 좋은 예: 원본 유지
const doubled = numbers.map(x => x * 2);

// 나쁜 예: 원본 변경
numbers.forEach((x, i) => (numbers[i] = x * 2));
```

**2. 적절한 메서드 선택**

```javascript
// 반환값이 필요 없을 때 - forEach
users.forEach(user => console.log(user.name));

// 새 배열이 필요할 때 - map
const userNames = users.map(user => user.name);

// 조건 검사만 할 때 - some/every
const hasAdmin = users.some(user => user.role === 'admin');
```

**3. 성능 고려사항**

```javascript
// 큰 배열에서는 불필요한 체이닝 피하기
const result = largeArray
  .map(x => x * 2)
  .filter(x => x > 100)
  .slice(0, 10);

// 필요한 만큼만 처리하는 것이 좋을 수 있음
```

이번 장에서는 자바스크립트의 강력한 배열 메서드들을 학습했습니다. 이들 메서드는 함수형 프로그래밍의 기초가 되며, 복잡한 데이터 처리를 간결하고 읽기 쉬운 코드로 표현할 수 있게 해줍니다. 실무에서 이러한 메서드들을 적절히 조합하면 더욱 효율적이고 유지보수하기 쉬운 코드를 작성할 수 있습니다.
