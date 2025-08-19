---
title: '11. 클래스와 객체'
slug: python-classes-objects-oop
description: '객체지향 프로그래밍의 기초부터 클래스 정의, 생성자, 메서드, 프로퍼티까지 체계적 학습'
keywords: [파이썬 클래스, 객체지향 OOP, class, 생성자 __init__, 메서드, 프로퍼티]
sidebar_position: 11
---

# 11. 클래스와 객체

객체지향 프로그래밍은 현실 세계의 객체들을 프로그램으로 모델링하는 방법입니다. 마치 레고 블록으로 집을 짓듯이, 작은 객체들을 조합해서 복잡한 프로그램을 만들어갈 수 있어요. 이번 챕터에서는 파이썬에서 클래스와 객체를 어떻게 만들고 사용하는지 차근차근 배워보겠습니다. 복잡해 보일 수 있지만, 하나씩 따라하다 보면 객체지향의 매력에 빠지게 될 거예요.

## 학습 목표

- 객체지향 프로그래밍의 기본 개념을 이해한다
- 클래스를 정의하고 객체를 생성할 수 있다
- 생성자와 메서드를 활용할 수 있다
- 인스턴스 변수와 클래스 변수의 차이를 안다
- 프로퍼티와 데코레이터를 활용할 수 있다

## 11-1 객체지향 프로그래밍이란?

객체지향 프로그래밍(Object-Oriented Programming, OOP)은 프로그램을 객체들의 집합으로 보는 프로그래밍 패러다임입니다. 현실 세계에서 우리가 보는 모든 것들(자동차, 사람, 책 등)을 객체로 생각하고, 이들 간의 상호작용으로 프로그램을 구성하는 방식이에요.

### 객체지향의 주요 특징

1. **캡슐화(Encapsulation)**: 데이터와 기능을 하나로 묶어서 관리
2. **상속(Inheritance)**: 기존 클래스의 특성을 물려받아 새로운 클래스 생성
3. **다형성(Polymorphism)**: 같은 이름의 메서드가 다른 동작을 수행
4. **추상화(Abstraction)**: 복잡한 구현 내용을 숨기고 필요한 기능만 제공

```python title=oop_concept.py
# 절차지향적 방식
def calculate_area(width, height):
    return width * height

def calculate_perimeter(width, height):
    return 2 * (width + height)

# 객체지향적 방식
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

    def calculate_perimeter(self):
        return 2 * (self.width + self.height)
```

## 11-2 클래스 정의하기

클래스는 객체를 만들기 위한 설계도입니다. 마치 붕어빵 틀처럼, 클래스라는 틀을 이용해서 동일한 구조를 가진 여러 개의 객체를 만들 수 있어요.

```python title=class_definition.py
class Dog:
    """강아지 클래스"""
    pass  # 아직 내용이 없음을 의미

class Cat:
    """고양이 클래스"""
    # 클래스 변수 (모든 인스턴스가 공유)
    species = "Felis catus"

    def make_sound(self):
        return "야옹"

# 클래스 이름은 파스칼 케이스(PascalCase) 사용
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self.balance = initial_balance
```

## 11-3 객체 생성하고 사용하기

클래스로부터 실제 객체(인스턴스)를 만들어서 사용해보겠습니다.

```python title=object_creation.py
class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        self.grades = []

    def add_grade(self, grade):
        self.grades.append(grade)

    def get_average(self):
        if not self.grades:
            return 0
        return sum(self.grades) / len(self.grades)

# 객체 생성
student1 = Student("김철수", "2023001")
student2 = Student("이영희", "2023002")

# 객체 사용
student1.add_grade(85)
student1.add_grade(92)
student2.add_grade(78)
student2.add_grade(88)

print(f"{student1.name}의 평균: {student1.get_average()}")
print(f"{student2.name}의 평균: {student2.get_average()}")
```

## 11-4 생성자와 소멸자

생성자(`__init__`)는 객체가 생성될 때 자동으로 호출되는 특별한 메서드입니다. 소멸자(`__del__`)는 객체가 메모리에서 삭제될 때 호출됩니다.

```python title=constructor_destructor.py
class FileManager:
    def __init__(self, filename):
        self.filename = filename
        self.file = open(filename, 'w')
        print(f"파일 {filename}이 열렸습니다.")

    def write_data(self, data):
        self.file.write(data)

    def __del__(self):
        if hasattr(self, 'file') and not self.file.closed:
            self.file.close()
            print(f"파일 {self.filename}이 닫혔습니다.")

# 사용 예시
file_manager = FileManager("test.txt")
file_manager.write_data("안녕하세요!")
del file_manager  # 명시적으로 삭제
```

## 11-5 인스턴스 변수와 클래스 변수

인스턴스 변수는 각 객체마다 별도로 가지는 변수이고, 클래스 변수는 모든 객체가 공유하는 변수입니다.

```python title=variables_types.py
class Counter:
    # 클래스 변수 - 모든 인스턴스가 공유
    total_count = 0

    def __init__(self, name):
        # 인스턴스 변수 - 각 인스턴스마다 별도
        self.name = name
        self.individual_count = 0
        Counter.total_count += 1

    def increment(self):
        self.individual_count += 1

    @classmethod
    def get_total_count(cls):
        return cls.total_count

# 사용 예시
counter1 = Counter("첫 번째")
counter2 = Counter("두 번째")

counter1.increment()
counter1.increment()
counter2.increment()

print(f"counter1 개별 카운트: {counter1.individual_count}")
print(f"counter2 개별 카운트: {counter2.individual_count}")
print(f"전체 카운터 개수: {Counter.get_total_count()}")
```

## 11-6 메서드 정의하기

메서드는 클래스 내부에 정의된 함수입니다. 인스턴스 메서드, 클래스 메서드, 정적 메서드가 있어요.

```python title=methods.py
class Calculator:
    def __init__(self, name):
        self.name = name
        self.history = []

    # 인스턴스 메서드
    def add(self, a, b):
        result = a + b
        self.history.append(f"{a} + {b} = {result}")
        return result

    def get_history(self):
        return self.history

    # 클래스 메서드
    @classmethod
    def create_scientific_calculator(cls, name):
        calc = cls(name)
        calc.type = "scientific"
        return calc

    # 정적 메서드
    @staticmethod
    def validate_number(value):
        try:
            float(value)
            return True
        except ValueError:
            return False

# 사용 예시
calc = Calculator("기본 계산기")
result = calc.add(10, 5)
print(f"결과: {result}")
print(f"계산 기록: {calc.get_history()}")

# 클래스 메서드 사용
sci_calc = Calculator.create_scientific_calculator("과학 계산기")
print(f"계산기 타입: {sci_calc.type}")

# 정적 메서드 사용
print(f"'123'은 숫자인가? {Calculator.validate_number('123')}")
print(f"'abc'는 숫자인가? {Calculator.validate_number('abc')}")
```

## 11-7 프로퍼티와 데코레이터

프로퍼티를 사용하면 메서드를 마치 변수처럼 사용할 수 있습니다. 데이터의 유효성 검사나 계산된 값을 제공할 때 유용해요.

```python title=properties.py
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("절대영도보다 낮을 수 없습니다.")
        self._celsius = value

    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        self.celsius = (value - 32) * 5/9

    @property
    def kelvin(self):
        return self._celsius + 273.15

# 사용 예시
temp = Temperature(25)
print(f"섭씨: {temp.celsius}°C")
print(f"화씨: {temp.fahrenheit}°F")
print(f"켈빈: {temp.kelvin}K")

# 화씨로 설정
temp.fahrenheit = 77
print(f"화씨 77도는 섭씨 {temp.celsius}도입니다.")
```

## 11-8 클래스와 타입 힌트

타입 힌트를 사용하면 코드의 가독성과 유지보수성이 향상됩니다.

```python title=type_hints.py
from typing import List, Optional

class Book:
    def __init__(self, title: str, author: str, pages: int) -> None:
        self.title: str = title
        self.author: str = author
        self.pages: int = pages
        self.is_borrowed: bool = False

    def borrow(self) -> bool:
        if not self.is_borrowed:
            self.is_borrowed = True
            return True
        return False

    def return_book(self) -> None:
        self.is_borrowed = False

class Library:
    def __init__(self) -> None:
        self.books: List[Book] = []

    def add_book(self, book: Book) -> None:
        self.books.append(book)

    def find_book(self, title: str) -> Optional[Book]:
        for book in self.books:
            if book.title == title:
                return book
        return None

    def get_available_books(self) -> List[Book]:
        return [book for book in self.books if not book.is_borrowed]
```

## 실습: 학생 클래스 만들기

학생 정보를 관리하는 클래스를 만들어보겠습니다. 실제 학교에서 사용할 수 있는 간단한 학생 관리 시스템을 구현해볼게요.

```python title=student_management.py
from typing import List, Dict
from datetime import datetime

class Student:
    # 클래스 변수 - 학생 총 수
    total_students = 0

    def __init__(self, name: str, student_id: str, grade: int) -> None:
        self.name: str = name
        self.student_id: str = student_id
        self.grade: int = grade
        self.subjects: Dict[str, List[int]] = {}
        self.enrollment_date: datetime = datetime.now()

        # 학생 수 증가
        Student.total_students += 1

    def add_subject(self, subject: str) -> None:
        """과목 추가"""
        if subject not in self.subjects:
            self.subjects[subject] = []

    def add_score(self, subject: str, score: int) -> bool:
        """점수 추가"""
        if subject not in self.subjects:
            self.add_subject(subject)

        if 0 <= score <= 100:
            self.subjects[subject].append(score)
            return True
        return False

    def get_subject_average(self, subject: str) -> float:
        """과목별 평균 점수"""
        if subject not in self.subjects or not self.subjects[subject]:
            return 0.0
        return sum(self.subjects[subject]) / len(self.subjects[subject])

    def get_total_average(self) -> float:
        """전체 평균 점수"""
        total_scores = []
        for scores in self.subjects.values():
            total_scores.extend(scores)

        if not total_scores:
            return 0.0
        return sum(total_scores) / len(total_scores)

    @property
    def is_honor_student(self) -> bool:
        """우등생 여부 (평균 90점 이상)"""
        return self.get_total_average() >= 90

    def __str__(self) -> str:
        return f"학생: {self.name} (학번: {self.student_id}, {self.grade}학년)"

    def __repr__(self) -> str:
        return f"Student('{self.name}', '{self.student_id}', {self.grade})"

# 사용 예시
def main():
    # 학생 생성
    student1 = Student("김철수", "2023001", 1)
    student2 = Student("이영희", "2023002", 1)

    # 과목과 점수 추가
    student1.add_score("수학", 85)
    student1.add_score("수학", 92)
    student1.add_score("영어", 88)
    student1.add_score("과학", 95)

    student2.add_score("수학", 95)
    student2.add_score("수학", 88)
    student2.add_score("영어", 92)
    student2.add_score("과학", 90)

    # 결과 출력
    print(f"총 학생 수: {Student.total_students}")
    print()

    for student in [student1, student2]:
        print(student)
        print(f"수학 평균: {student.get_subject_average('수학'):.1f}")
        print(f"전체 평균: {student.get_total_average():.1f}")
        print(f"우등생 여부: {student.is_honor_student}")
        print("-" * 30)

if __name__ == "__main__":
    main()
```

## 확장 과제: 도서관 관리 시스템 클래스 설계

앞서 배운 내용을 활용해서 더 복잡한 도서관 관리 시스템을 만들어보세요. 다음 기능들을 포함해보세요:

1. **Book 클래스**: 제목, 저자, ISBN, 출간년도, 대출 상태
2. **Member 클래스**: 회원명, 회원번호, 대출 목록, 연체료
3. **Library 클래스**: 도서 목록, 회원 목록, 대출/반납 관리

```python title=library_system.py
# 여러분이 직접 구현해보세요!
# 힌트: 날짜 계산을 위해 datetime 모듈 활용
# 힌트: 연체료 계산 로직 포함
# 힌트: 대출 가능 여부 확인 메서드
```

## 연습문제

1. **은행 계좌 클래스**: 계좌번호, 잔액, 입금/출금 기능을 가진 `BankAccount` 클래스를 만드세요.

2. **자동차 클래스**: 브랜드, 모델, 연료량, 주행거리를 관리하는 `Car` 클래스를 만드세요.

3. **할일 관리 클래스**: 할일 목록을 관리하는 `TodoList` 클래스를 만들어 추가, 완료, 삭제 기능을 구현하세요.

이번 챕터에서는 객체지향 프로그래밍의 기초인 클래스와 객체에 대해 배웠습니다. 처음에는 어려울 수 있지만, 실제 프로젝트에서 클래스를 사용해보면 코드가 훨씬 체계적이고 재사용하기 쉬워진다는 것을 느낄 수 있을 거예요. 다음 챕터에서는 상속과 다형성에 대해 더 자세히 알아보겠습니다!
