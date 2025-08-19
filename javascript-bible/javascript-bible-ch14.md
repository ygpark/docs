---
title: '14장: 객체 지향 프로그래밍'
slug: javascript-bible-14-object-oriented-programming
description: '자바스크립트의 객체 지향 프로그래밍을 학습합니다. 생성자 함수, 프로토타입, ES6 클래스, 상속, 정적 메서드, 프라이빗 필드까지 단계적으로 알아봅니다.'
keywords:
  [
    '자바스크립트',
    '객체지향',
    'OOP',
    '클래스',
    '상속',
    '프로토타입',
    '생성자함수',
    '정적메서드',
    '프라이빗필드',
    'ES6',
    '캡슐화',
    '다형성',
  ]
sidebar_position: 14
---

# 14장: 객체 지향 프로그래밍

객체 지향 프로그래밍(OOP)은 현실 세계의 사물을 객체로 모델링하여 프로그램을 구성하는 패러다임입니다. 자바스크립트는 프로토타입 기반 언어이지만, ES6부터 클래스 문법을 제공하여 더욱 직관적인 객체 지향 프로그래밍이 가능해졌습니다. 이 장에서는 생성자 함수부터 최신 클래스 문법까지, 자바스크립트의 객체 지향 개념을 체계적으로 학습하겠습니다.

---

## 생성자 함수와 프로토타입

생성자 함수는 ES6 이전부터 사용되던 객체 생성 방식입니다. new 키워드와 함께 호출되어 새로운 객체 인스턴스를 생성합니다.

### 기본 생성자 함수

간단한 사람(Person) 객체를 생성하는 생성자 함수를 만들어보겠습니다.

```javascript title="person_constructor.js"
// 생성자 함수 정의
function Person(name, age) {
  // this는 새로 생성될 객체를 가리킴
  this.name = name;
  this.age = age;

  // 메서드 정의 (권장하지 않는 방식)
  this.greet = function () {
    return `안녕하세요, 저는 ${this.name}입니다.`;
  };
}

// 인스턴스 생성
const person1 = new Person('김철수', 25);
const person2 = new Person('이영희', 30);

console.log(person1.name); // '김철수'
console.log(person2.greet()); // '안녕하세요, 저는 이영희입니다.'

// 각 인스턴스마다 메서드가 복사되어 메모리 낭비 발생
console.log(person1.greet === person2.greet); // false
```

### 프로토타입을 활용한 메서드 정의

메모리 효율성을 위해 프로토타입에 메서드를 정의하는 것이 좋습니다.

```javascript title="person_prototype.js"
// 생성자 함수
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// 프로토타입에 메서드 추가
Person.prototype.greet = function () {
  return `안녕하세요, 저는 ${this.name}입니다.`;
};

Person.prototype.getAge = function () {
  return `저는 ${this.age}살입니다.`;
};

Person.prototype.introduce = function () {
  return `${this.greet()} ${this.getAge()}`;
};

// 인스턴스 생성
const person1 = new Person('김철수', 25);
const person2 = new Person('이영희', 30);

console.log(person1.introduce());
// '안녕하세요, 저는 김철수입니다. 저는 25살입니다.'

// 모든 인스턴스가 같은 메서드를 공유
console.log(person1.greet === person2.greet); // true
```

### 프로토타입 체인 확인

객체의 프로토타입 관계를 확인하는 다양한 방법을 알아보겠습니다.

```javascript title="prototype_chain.js"
function Person(name) {
  this.name = name;
}

Person.prototype.speak = function () {
  return `${this.name}이(가) 말합니다.`;
};

const person = new Person('홍길동');

// 프로토타입 확인 방법들
console.log(person.__proto__ === Person.prototype); // true
console.log(Object.getPrototypeOf(person) === Person.prototype); // true
console.log(person instanceof Person); // true

// 메서드 존재 확인
console.log('speak' in person); // true
console.log(person.hasOwnProperty('speak')); // false
console.log(Person.prototype.hasOwnProperty('speak')); // true

// 프로토타입 체인
console.log(person.__proto__.__proto__ === Object.prototype); // true
console.log(person.__proto__.__proto__.__proto__); // null
```

---

## ES6 클래스 문법

ES6에서 도입된 클래스 문법은 생성자 함수와 프로토타입을 더 직관적으로 사용할 수 있게 해줍니다.

### 기본 클래스 정의

Person 클래스를 ES6 문법으로 다시 작성해보겠습니다.

```javascript title="person_class.js"
class Person {
  // 생성자 메서드
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // 인스턴스 메서드 (자동으로 prototype에 추가됨)
  greet() {
    return `안녕하세요, 저는 ${this.name}입니다.`;
  }

  getAge() {
    return `저는 ${this.age}살입니다.`;
  }

  introduce() {
    return `${this.greet()} ${this.getAge()}`;
  }

  // 한 해가 지나는 메서드
  birthday() {
    this.age++;
    return `생일축하합니다! 이제 ${this.age}살이 되었습니다.`;
  }
}

// 인스턴스 생성
const person1 = new Person('김철수', 25);
const person2 = new Person('이영희', 30);

console.log(person1.introduce());
console.log(person2.birthday());
console.log(person2.getAge());
```

### Getter와 Setter

클래스에서 프로퍼티에 대한 접근을 제어하는 getter와 setter를 정의할 수 있습니다.

```javascript title="getter_setter.js"
class Person {
  constructor(name, birthYear) {
    this.name = name;
    this.birthYear = birthYear;
  }

  // getter - 계산된 속성처럼 사용
  get age() {
    const currentYear = new Date().getFullYear();
    return currentYear - this.birthYear;
  }

  // setter - 값 설정 시 검증 로직 추가 가능
  set name(newName) {
    if (typeof newName === 'string' && newName.length > 0) {
      this._name = newName;
    } else {
      throw new Error('이름은 빈 문자열이 될 수 없습니다.');
    }
  }

  get name() {
    return this._name;
  }

  getInfo() {
    return `${this.name}님은 ${this.age}살입니다.`;
  }
}

const person = new Person('김철수', 1998);
console.log(person.age); // getter 호출 (현재 연도 - 1998)
console.log(person.getInfo());

// setter 사용
person.name = '김영수';
console.log(person.name);
```

---

## 상속과 super 키워드

클래스는 extends 키워드를 사용하여 다른 클래스를 상속받을 수 있습니다.

### 기본 상속

Person 클래스를 상속받는 Student 클래스를 만들어보겠습니다.

```javascript title="inheritance_basic.js"
// 부모 클래스
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `안녕하세요, 저는 ${this.name}입니다.`;
  }

  getInfo() {
    return `${this.name}, ${this.age}살`;
  }
}

// 자식 클래스
class Student extends Person {
  constructor(name, age, school) {
    // 부모 생성자 호출
    super(name, age);
    this.school = school;
  }

  // 메서드 오버라이딩
  greet() {
    return `${super.greet()} 저는 ${this.school}에 다닙니다.`;
  }

  study() {
    return `${this.name}이(가) 공부하고 있습니다.`;
  }
}

const student = new Student('이철수', 20, '한국대학교');
console.log(student.greet());
console.log(student.study());
console.log(student.getInfo()); // 부모 메서드 사용
```

### 다단계 상속

여러 단계의 상속 관계를 만들어보겠습니다.

```javascript title="multilevel_inheritance.js"
// 기본 생명체 클래스
class LivingBeing {
  constructor(name) {
    this.name = name;
  }

  breathe() {
    return `${this.name}이(가) 숨을 쉽니다.`;
  }
}

// 동물 클래스
class Animal extends LivingBeing {
  constructor(name, species) {
    super(name);
    this.species = species;
  }

  move() {
    return `${this.name}이(가) 움직입니다.`;
  }

  getSpecies() {
    return `종: ${this.species}`;
  }
}

// 포유류 클래스
class Mammal extends Animal {
  constructor(name, species, furColor) {
    super(name, species);
    this.furColor = furColor;
  }

  nurseYoung() {
    return `${this.name}이(가) 새끼에게 젖을 먹입니다.`;
  }

  getDescription() {
    return `${this.name}은(는) ${this.furColor} 털을 가진 ${this.species}입니다.`;
  }
}

const dog = new Mammal('멍멍이', '개', '갈색');
console.log(dog.breathe());
console.log(dog.move());
console.log(dog.nurseYoung());
console.log(dog.getDescription());
```

---

## 정적 메서드와 프라이빗 필드

ES6+ 에서 제공하는 고급 클래스 기능들을 알아보겠습니다.

### 정적 메서드 (Static Methods)

정적 메서드는 클래스 자체에 속하며, 인스턴스를 생성하지 않고도 호출할 수 있습니다.

```javascript title="static_methods.js"
class MathUtils {
  // 정적 속성
  static PI = 3.14159;

  // 정적 메서드
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }

  static circleArea(radius) {
    return this.PI * radius * radius;
  }

  static createCalculator() {
    return new Calculator();
  }
}

class Calculator {
  constructor() {
    this.result = 0;
  }

  add(num) {
    this.result += num;
    return this;
  }

  getResult() {
    return this.result;
  }
}

// 정적 메서드 사용 (인스턴스 생성 없이)
console.log(MathUtils.add(5, 3));
console.log(MathUtils.circleArea(5));

// 정적 메서드로 인스턴스 생성
const calc = MathUtils.createCalculator();
console.log(calc.add(10).add(5).getResult());
```

### 프라이빗 필드 (#private)

ES2022에서 도입된 프라이빗 필드를 사용하여 캡슐화를 구현할 수 있습니다.

```javascript title="private_fields.js"
class BankAccount {
  // 프라이빗 필드 (클래스 내부에서만 접근 가능)
  #balance = 0;
  #accountNumber;

  constructor(accountNumber, initialBalance = 0) {
    this.#accountNumber = accountNumber;
    this.#balance = initialBalance;
  }

  // 프라이빗 메서드
  #validateAmount(amount) {
    if (amount <= 0) {
      throw new Error('금액은 0보다 커야 합니다.');
    }
  }

  deposit(amount) {
    this.#validateAmount(amount);
    this.#balance += amount;
    return `${amount}원이 입금되었습니다. 잔액: ${this.#balance}원`;
  }

  withdraw(amount) {
    this.#validateAmount(amount);
    if (amount > this.#balance) {
      throw new Error('잔액이 부족합니다.');
    }
    this.#balance -= amount;
    return `${amount}원이 출금되었습니다. 잔액: ${this.#balance}원`;
  }

  // 공개 메서드로 프라이빗 필드 접근
  getBalance() {
    return this.#balance;
  }

  getAccountInfo() {
    return `계좌번호: ${this.#accountNumber}, 잔액: ${this.#balance}원`;
  }
}

const account = new BankAccount('123-456-789', 10000);
console.log(account.deposit(5000));
console.log(account.withdraw(3000));
console.log(account.getAccountInfo());

// 프라이빗 필드에 직접 접근 시도 (에러 발생)
// console.log(account.#balance); // SyntaxError
```

---

## 실습: 은행 계좌 클래스 시스템

실제 은행 시스템을 모델링한 종합적인 예제를 만들어보겠습니다.

### 기본 계좌 클래스

```javascript title="bank_account_system.js"
class BankAccount {
  #balance = 0;
  #accountNumber;
  #accountHolder;
  #transactions = [];

  static #accountCounter = 1000;

  constructor(accountHolder, initialBalance = 0) {
    this.#accountNumber = `ACC-${++BankAccount.#accountCounter}`;
    this.#accountHolder = accountHolder;
    this.#balance = initialBalance;

    if (initialBalance > 0) {
      this.#addTransaction('초기입금', initialBalance);
    }
  }

  #addTransaction(type, amount) {
    this.#transactions.push({
      type,
      amount,
      balance: this.#balance,
      timestamp: new Date().toISOString(),
    });
  }

  #validateAmount(amount) {
    if (typeof amount !== 'number' || amount <= 0) {
      throw new Error('유효하지 않은 금액입니다.');
    }
  }

  deposit(amount) {
    this.#validateAmount(amount);
    this.#balance += amount;
    this.#addTransaction('입금', amount);
    return this.#balance;
  }

  withdraw(amount) {
    this.#validateAmount(amount);
    if (amount > this.#balance) {
      throw new Error('잔액이 부족합니다.');
    }
    this.#balance -= amount;
    this.#addTransaction('출금', amount);
    return this.#balance;
  }

  getBalance() {
    return this.#balance;
  }

  getAccountInfo() {
    return {
      accountNumber: this.#accountNumber,
      accountHolder: this.#accountHolder,
      balance: this.#balance,
    };
  }

  getTransactionHistory() {
    return [...this.#transactions];
  }
}
```

### 저축 계좌 클래스 (상속)

```javascript title="savings_account.js"
class SavingsAccount extends BankAccount {
  #interestRate;
  #minimumBalance;

  constructor(accountHolder, initialBalance = 0, interestRate = 0.02) {
    super(accountHolder, initialBalance);
    this.#interestRate = interestRate;
    this.#minimumBalance = 1000;
  }

  withdraw(amount) {
    const currentBalance = this.getBalance();
    if (currentBalance - amount < this.#minimumBalance) {
      throw new Error(`최소 잔액 ${this.#minimumBalance}원을 유지해야 합니다.`);
    }
    return super.withdraw(amount);
  }

  calculateInterest() {
    const interest = this.getBalance() * this.#interestRate;
    if (interest > 0) {
      this.deposit(Math.floor(interest));
      return interest;
    }
    return 0;
  }

  getAccountType() {
    return 'Savings Account';
  }

  getInterestRate() {
    return this.#interestRate;
  }
}

// 사용 예제
const savings = new SavingsAccount('김철수', 50000, 0.03);
console.log(savings.getAccountInfo());
savings.deposit(20000);
console.log(`이자 계산: ${savings.calculateInterest()}원`);
console.log(savings.getTransactionHistory());
```

---

## 실습: 게임 캐릭터 상속 구조 만들기

RPG 게임의 캐릭터 시스템을 객체 지향으로 설계해보겠습니다.

### 기본 캐릭터 클래스

```javascript title="game_character.js"
class Character {
  #name;
  #level = 1;
  #health;
  #maxHealth;
  #experience = 0;

  constructor(name, baseHealth = 100) {
    this.#name = name;
    this.#maxHealth = baseHealth;
    this.#health = baseHealth;
  }

  // 공격 메서드 (자식 클래스에서 오버라이드)
  attack() {
    return Math.floor(Math.random() * 20) + 10;
  }

  takeDamage(damage) {
    this.#health = Math.max(0, this.#health - damage);
    if (this.#health === 0) {
      return `${this.#name}이(가) 쓰러졌습니다!`;
    }
    return `${this.#name}이(가) ${damage}의 피해를 받았습니다. 남은 체력: ${this.#health}`;
  }

  heal(amount) {
    this.#health = Math.min(this.#maxHealth, this.#health + amount);
    return `${this.#name}이(가) ${amount}만큼 회복했습니다. 현재 체력: ${this.#health}`;
  }

  gainExperience(exp) {
    this.#experience += exp;
    if (this.#experience >= this.#level * 100) {
      this.#levelUp();
    }
  }

  #levelUp() {
    this.#level++;
    this.#experience = 0;
    this.#maxHealth += 20;
    this.#health = this.#maxHealth;
    return `레벨업! ${this.#name}이(가) 레벨 ${this.#level}이 되었습니다!`;
  }

  getStats() {
    return {
      name: this.#name,
      level: this.#level,
      health: this.#health,
      maxHealth: this.#maxHealth,
      experience: this.#experience,
    };
  }

  isAlive() {
    return this.#health > 0;
  }
}
```

### 전사 클래스 (상속)

```javascript title="warrior_class.js"
class Warrior extends Character {
  #armor;
  #weapon;

  constructor(name) {
    super(name, 150); // 전사는 높은 기본 체력
    this.#armor = 10;
    this.#weapon = '검';
  }

  // 공격 메서드 오버라이드
  attack() {
    const baseDamage = super.attack();
    const bonusDamage = Math.floor(Math.random() * 15) + 5;
    return baseDamage + bonusDamage;
  }

  // 전사 특수 스킬
  powerStrike() {
    const damage = this.attack() * 1.5;
    return Math.floor(damage);
  }

  // 방어 효과 적용
  takeDamage(damage) {
    const reducedDamage = Math.max(1, damage - this.#armor);
    return super.takeDamage(reducedDamage);
  }

  upgradeWeapon() {
    this.#weapon = '강화된 ' + this.#weapon;
    return `무기가 업그레이드되었습니다: ${this.#weapon}`;
  }

  getCharacterType() {
    return 'Warrior';
  }

  getEquipment() {
    return {
      weapon: this.#weapon,
      armor: this.#armor,
    };
  }
}

// 마법사 클래스
class Mage extends Character {
  #mana = 100;
  #maxMana = 100;
  #spells = ['파이어볼', '힐링', '아이스볼트'];

  constructor(name) {
    super(name, 80); // 마법사는 낮은 체력
  }

  attack() {
    if (this.#mana >= 10) {
      this.#mana -= 10;
      return Math.floor(Math.random() * 25) + 15;
    }
    return super.attack(); // 마나 부족시 기본 공격
  }

  castSpell(spellName) {
    if (!this.#spells.includes(spellName)) {
      return '알 수 없는 주문입니다.';
    }

    if (this.#mana < 20) {
      return '마나가 부족합니다.';
    }

    this.#mana -= 20;
    return `${spellName} 주문을 시전했습니다!`;
  }

  restoreMana(amount) {
    this.#mana = Math.min(this.#maxMana, this.#mana + amount);
  }

  getMana() {
    return this.#mana;
  }

  getCharacterType() {
    return 'Mage';
  }
}

// 게임 사용 예제
const warrior = new Warrior('아서');
const mage = new Mage('머린');

console.log(warrior.getStats());
console.log(warrior.upgradeWeapon());
console.log(warrior.powerStrike());

console.log(mage.getStats());
console.log(mage.castSpell('파이어볼'));
console.log(`마나: ${mage.getMana()}`);
```

---

## 정리

이 장에서는 자바스크립트의 객체 지향 프로그래밍에 대해 학습했습니다:

- **생성자 함수와 프로토타입**: ES6 이전의 객체 생성 방식과 프로토타입 체인의 이해
- **ES6 클래스 문법**: 더 직관적이고 깔끔한 클래스 정의 방법
- **상속과 super**: extends 키워드를 통한 클래스 상속과 부모 클래스 메서드 호출
- **정적 메서드**: 클래스 자체에 속하는 메서드와 속성 정의
- **프라이빗 필드**: # 문법을 통한 캡슐화 구현

객체 지향 프로그래밍은 코드의 재사용성과 유지보수성을 크게 향상시킵니다. 특히 대규모 애플리케이션 개발에서 코드를 체계적으로 구조화하는 데 매우 유용합니다. 다음 장에서는 현대적인 웹 개발 도구들에 대해 알아보겠습니다.
