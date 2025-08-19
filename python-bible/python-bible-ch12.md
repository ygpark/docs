---
title: '12. 상속과 다형성'
slug: python-inheritance-polymorphism
description: '객체지향의 상속, 다형성, 메서드 오버라이딩, 데이터 클래스까지 고급 OOP 개념 완전 정복'
keywords: [파이썬 상속, 다형성, 메서드 오버라이딩, super(), 데이터클래스, 추상클래스]
sidebar_position: 12
---

# 12. 상속과 다형성

## 학습 목표

이번 챕터에서는 객체지향 프로그래밍의 핵심 개념인 상속과 다형성을 완전히 마스터합니다. 상속을 통해 기존 클래스의 특성을 물려받아 코드 재사용성을 높이고, 다형성을 활용해 더 유연하고 확장 가능한 프로그램을 작성하는 방법을 배웁니다. 또한 현대적인 파이썬의 데이터 클래스까지 다뤄보며 실무에서 바로 활용할 수 있는 객체지향 설계 능력을 기를 것입니다.

---

## 서론

프로그래밍 세계에서 상속은 마치 부모가 자녀에게 특성을 물려주는 것과 같습니다. 자동차를 예로 들어보면, 모든 자동차는 바퀴, 엔진, 문과 같은 공통적인 특성을 가지고 있지만, 승용차, 트럭, 오토바이는 각각 고유한 특징도 있죠. 상속은 이런 공통 특성을 효율적으로 관리하면서도 각자만의 특별한 기능을 추가할 수 있게 해줍니다. 다형성은 같은 명령이라도 객체에 따라 다르게 동작하는 것으로, 마치 "달려!"라는 명령이 사람에게는 두 발로 뛰기를, 자동차에게는 바퀴로 굴러가기를 의미하는 것과 같습니다.

---

## 12-1 상속의 개념

상속은 기존 클래스의 속성과 메서드를 새로운 클래스가 물려받는 메커니즘입니다. 상속을 사용하면 코드의 재사용성이 높아지고, 계층적인 클래스 구조를 만들 수 있습니다.

```python
# 부모 클래스 (상위 클래스, 기반 클래스)
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
        self.energy = 100

    def eat(self):
        self.energy += 20
        print(f"{self.name}이 음식을 먹어서 에너지가 {self.energy}가 되었습니다.")

    def sleep(self):
        self.energy += 30
        print(f"{self.name}이 잠을 자서 에너지가 {self.energy}가 되었습니다.")

    def make_sound(self):
        print(f"{self.name}이 소리를 냅니다.")

# 자식 클래스 (하위 클래스, 파생 클래스)
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "개")  # 부모 클래스의 생성자 호출
        self.breed = breed
        self.loyalty = 100

    def wag_tail(self):
        print(f"{self.name}이 꼬리를 흔듭니다!")

    def fetch(self):
        self.energy -= 10
        print(f"{self.name}이 공을 가져왔습니다. 에너지: {self.energy}")

class Cat(Animal):
    def __init__(self, name, fur_color):
        super().__init__(name, "고양이")
        self.fur_color = fur_color
        self.independence = 80

    def purr(self):
        print(f"{self.name}이 그르르 소리를 냅니다.")

    def climb_tree(self):
        self.energy -= 15
        print(f"{self.name}이 나무에 올라갔습니다. 에너지: {self.energy}")

# 사용 예제
my_dog = Dog("바둑이", "골든 리트리버")
my_cat = Cat("나비", "검은색")

# 부모 클래스에서 상속받은 메서드 사용
my_dog.eat()  # 바둑이이 음식을 먹어서 에너지가 120가 되었습니다.
my_cat.sleep()  # 나비이 잠을 자서 에너지가 130가 되었습니다.

# 자식 클래스의 고유 메서드 사용
my_dog.wag_tail()  # 바둑이이 꼬리를 흔듭니다!
my_cat.purr()  # 나비이 그르르 소리를 냅니다.
```

---

## 12-2 메서드 오버라이딩

메서드 오버라이딩은 부모 클래스의 메서드를 자식 클래스에서 재정의하는 것입니다. 같은 이름의 메서드를 자식 클래스에서 다르게 구현할 수 있습니다.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def make_sound(self):
        print(f"{self.name}이 소리를 냅니다.")

    def introduce(self):
        print(f"안녕하세요, 저는 {self.name}입니다.")

class Dog(Animal):
    def make_sound(self):  # 메서드 오버라이딩
        print(f"{self.name}이 멍멍하고 짖습니다!")

    def introduce(self):  # 메서드 오버라이딩
        print(f"안녕하세요! 저는 충성스러운 개 {self.name}입니다. 멍멍!")

class Cat(Animal):
    def make_sound(self):  # 메서드 오버라이딩
        print(f"{self.name}이 야옹하고 웁니다.")

    def introduce(self):  # 메서드 오버라이딩
        print(f"안녕... 나는 고양이 {self.name}야. 귀찮게 하지 마.")

# 사용 예제
animals = [Dog("바둑이"), Cat("나비"), Animal("알수없는동물")]

for animal in animals:
    animal.make_sound()
    animal.introduce()
    print("---")

# 출력:
# 바둑이이 멍멍하고 짖습니다!
# 안녕하세요! 저는 충성스러운 개 바둑이입니다. 멍멍!
# ---
# 나비이 야옹하고 웁니다.
# 안녕... 나는 고양이 나비야. 귀찮게 하지 마.
# ---
# 알수없는동물이 소리를 냅니다.
# 안녕하세요, 저는 알수없는동물입니다.
# ---
```

---

## 12-3 super() 함수

super() 함수는 자식 클래스에서 부모 클래스의 메서드를 호출할 때 사용합니다. 특히 부모 클래스의 기능을 확장하거나 생성자를 호출할 때 유용합니다.

```python
class Vehicle:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year
        self.mileage = 0

    def start(self):
        print(f"{self.brand} {self.model}의 엔진이 시동됩니다.")

    def drive(self, distance):
        self.mileage += distance
        print(f"{distance}km 주행했습니다. 총 주행거리: {self.mileage}km")

class ElectricCar(Vehicle):
    def __init__(self, brand, model, year, battery_capacity):
        super().__init__(brand, model, year)  # 부모 클래스 생성자 호출
        self.battery_capacity = battery_capacity
        self.battery_level = 100

    def start(self):
        super().start()  # 부모 클래스의 start 메서드 호출
        print("전기 모터가 조용히 가동됩니다.")

    def drive(self, distance):
        super().drive(distance)  # 부모 클래스의 drive 메서드 호출
        battery_used = distance * 0.2
        self.battery_level = max(0, self.battery_level - battery_used)
        print(f"배터리 잔량: {self.battery_level:.1f}%")

    def charge(self):
        self.battery_level = 100
        print("배터리가 완전히 충전되었습니다.")

# 사용 예제
tesla = ElectricCar("Tesla", "Model 3", 2023, 75)
tesla.start()
tesla.drive(50)
tesla.charge()
```

---

## 12-4 다중 상속과 MRO

파이썬은 다중 상속을 지원합니다. 하나의 클래스가 여러 부모 클래스로부터 상속받을 수 있습니다. MRO(Method Resolution Order)는 메서드 호출 순서를 결정하는 규칙입니다.

```python
class Flyable:
    def fly(self):
        print("하늘을 날아갑니다!")

    def move(self):
        print("날아서 이동합니다.")

class Swimmable:
    def swim(self):
        print("물에서 헤엄칩니다!")

    def move(self):
        print("헤엄쳐서 이동합니다.")

class Animal:
    def __init__(self, name):
        self.name = name

    def move(self):
        print("일반적인 방법으로 이동합니다.")

# 다중 상속
class Duck(Animal, Flyable, Swimmable):
    def __init__(self, name):
        super().__init__(name)

    def quack(self):
        print(f"{self.name}이 꽥꽥 웁니다!")

# MRO 확인
print("Duck 클래스의 MRO:")
for cls in Duck.__mro__:
    print(f"  {cls.__name__}")

# 사용 예제
donald = Duck("도널드")
donald.quack()    # 도널드이 꽥꽥 웁니다!
donald.fly()      # 하늘을 날아갑니다!
donald.swim()     # 물에서 헤엄칩니다!
donald.move()     # 날아서 이동합니다. (Flyable의 move가 먼저 호출됨)
```

---

## 12-5 추상 클래스와 인터페이스

추상 클래스는 직접 인스턴스를 생성할 수 없는 클래스로, 자식 클래스에서 반드시 구현해야 하는 메서드를 정의할 때 사용합니다.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    def __init__(self, color):
        self.color = color

    @abstractmethod
    def calculate_area(self):
        pass

    @abstractmethod
    def calculate_perimeter(self):
        pass

    def describe(self):
        print(f"이것은 {self.color} 색깔의 도형입니다.")

class Rectangle(Shape):
    def __init__(self, color, width, height):
        super().__init__(color)
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

    def calculate_perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius

    def calculate_area(self):
        return 3.14159 * self.radius ** 2

    def calculate_perimeter(self):
        return 2 * 3.14159 * self.radius

# 사용 예제
rectangle = Rectangle("빨간색", 5, 3)
circle = Circle("파란색", 4)

shapes = [rectangle, circle]

for shape in shapes:
    shape.describe()
    print(f"넓이: {shape.calculate_area():.2f}")
    print(f"둘레: {shape.calculate_perimeter():.2f}")
    print("---")

# 다음 코드는 오류가 발생합니다 (추상 클래스는 인스턴스 생성 불가)
# shape = Shape("초록색")  # TypeError 발생
```

---

## 12-6 데이터 클래스 (dataclass)

dataclass는 데이터를 저장하는 클래스를 쉽게 만들 수 있게 해주는 데코레이터입니다. 자동으로 `__init__`, `__repr__`, `__eq__` 등의 메서드를 생성해줍니다.

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Student:
    name: str
    age: int
    grade: float = 0.0
    subjects: List[str] = field(default_factory=list)

    def add_subject(self, subject: str):
        self.subjects.append(subject)

    def get_grade_description(self) -> str:
        if self.grade >= 90:
            return "우수"
        elif self.grade >= 80:
            return "양호"
        elif self.grade >= 70:
            return "보통"
        else:
            return "노력 필요"

@dataclass
class Course:
    code: str
    name: str
    credits: int
    students: List[Student] = field(default_factory=list)

    def enroll_student(self, student: Student):
        self.students.append(student)
        student.add_subject(self.name)

    def get_enrollment_count(self) -> int:
        return len(self.students)

# 사용 예제
student1 = Student("김철수", 20, 85.5)
student2 = Student("이영희", 19, 92.0)

course = Course("CS101", "파이썬 프로그래밍", 3)
course.enroll_student(student1)
course.enroll_student(student2)

print(f"과목: {course.name}")
print(f"수강생 수: {course.get_enrollment_count()}")

for student in course.students:
    print(f"학생: {student.name}, 성적: {student.grade}, 평가: {student.get_grade_description()}")
    print(f"수강 과목: {student.subjects}")
```

---

## 실습: 동물 클래스 상속 구조

다양한 동물들의 특성을 표현하는 클래스 계층 구조를 만들어보겠습니다. 이 실습을 통해 상속, 메서드 오버라이딩, 다형성의 개념을 실제로 적용해볼 수 있습니다.

```python title=animal_hierarchy.py
from abc import ABC, abstractmethod
from dataclasses import dataclass
from typing import List

class Animal(ABC):
    def __init__(self, name: str, species: str, age: int):
        self.name = name
        self.species = species
        self.age = age
        self.energy = 100
        self.hunger = 0

    @abstractmethod
    def make_sound(self) -> str:
        pass

    def eat(self, food_amount: int = 20):
        self.energy = min(100, self.energy + food_amount)
        self.hunger = max(0, self.hunger - food_amount)
        print(f"{self.name}이 음식을 먹었습니다. 에너지: {self.energy}, 배고픔: {self.hunger}")

    def sleep(self, hours: int = 8):
        energy_gained = hours * 5
        self.energy = min(100, self.energy + energy_gained)
        print(f"{self.name}이 {hours}시간 잠을 잤습니다. 에너지: {self.energy}")

    def status(self):
        print(f"=== {self.name}의 상태 ===")
        print(f"종류: {self.species}, 나이: {self.age}세")
        print(f"에너지: {self.energy}, 배고픔: {self.hunger}")
        print(f"울음소리: {self.make_sound()}")

class Mammal(Animal):
    def __init__(self, name: str, species: str, age: int):
        super().__init__(name, species, age)
        self.body_temperature = 37.0

    def give_birth(self):
        print(f"{self.name}이 새끼를 낳았습니다!")

class Dog(Mammal):
    def __init__(self, name: str, breed: str, age: int):
        super().__init__(name, "개", age)
        self.breed = breed
        self.loyalty = 100

    def make_sound(self) -> str:
        return "멍멍!"

    def fetch(self):
        if self.energy >= 20:
            self.energy -= 20
            self.hunger += 10
            print(f"{self.name}이 공을 가져왔습니다! 에너지: {self.energy}")
        else:
            print(f"{self.name}이 너무 피곤해서 공을 가져올 수 없습니다.")

    def wag_tail(self):
        print(f"{self.name}이 기쁘게 꼬리를 흔듭니다!")

class Cat(Mammal):
    def __init__(self, name: str, fur_color: str, age: int):
        super().__init__(name, "고양이", age)
        self.fur_color = fur_color
        self.independence = 80

    def make_sound(self) -> str:
        return "야옹~"

    def climb_tree(self):
        if self.energy >= 15:
            self.energy -= 15
            self.hunger += 5
            print(f"{self.name}이 나무에 올라갔습니다! 에너지: {self.energy}")
        else:
            print(f"{self.name}이 너무 피곤해서 나무에 올라갈 수 없습니다.")

    def purr(self):
        print(f"{self.name}이 만족스럽게 그르르 소리를 냅니다.")

class Bird(Animal):
    def __init__(self, name: str, species: str, age: int, wing_span: float):
        super().__init__(name, species, age)
        self.wing_span = wing_span
        self.can_fly = True

    def fly(self):
        if self.can_fly and self.energy >= 25:
            self.energy -= 25
            self.hunger += 15
            print(f"{self.name}이 하늘을 날아갑니다! 날개폭: {self.wing_span}m")
        else:
            print(f"{self.name}이 날 수 없습니다.")

class Parrot(Bird):
    def __init__(self, name: str, age: int, vocabulary_size: int):
        super().__init__(name, "앵무새", age, 0.3)
        self.vocabulary_size = vocabulary_size

    def make_sound(self) -> str:
        return "안녕하세요!"

    def speak(self, phrase: str):
        print(f"{self.name}이 말합니다: '{phrase}'")

# 동물원 관리 시스템
@dataclass
class Zoo:
    name: str
    animals: List[Animal] = None

    def __post_init__(self):
        if self.animals is None:
            self.animals = []

    def add_animal(self, animal: Animal):
        self.animals.append(animal)
        print(f"{animal.name}({animal.species})이 {self.name}에 추가되었습니다.")

    def feed_all_animals(self):
        print(f"\n=== {self.name} 모든 동물들 급식 시간 ===")
        for animal in self.animals:
            animal.eat()

    def daily_exercise(self):
        print(f"\n=== {self.name} 동물들 운동 시간 ===")
        for animal in self.animals:
            if isinstance(animal, Dog):
                animal.fetch()
                animal.wag_tail()
            elif isinstance(animal, Cat):
                animal.climb_tree()
                animal.purr()
            elif isinstance(animal, Bird):
                animal.fly()

    def zoo_status(self):
        print(f"\n=== {self.name} 현황 ===")
        print(f"총 동물 수: {len(self.animals)}마리")
        for animal in self.animals:
            animal.status()
            print()

# 실습 실행
if __name__ == "__main__":
    # 동물원 생성
    my_zoo = Zoo("우리 동물원")

    # 동물들 생성
    dog = Dog("바둑이", "골든 리트리버", 3)
    cat = Cat("나비", "검은색", 2)
    parrot = Parrot("폴리", 5, 50)

    # 동물원에 동물 추가
    my_zoo.add_animal(dog)
    my_zoo.add_animal(cat)
    my_zoo.add_animal(parrot)

    # 동물원 운영
    my_zoo.feed_all_animals()
    my_zoo.daily_exercise()
    my_zoo.zoo_status()

    # 앵무새 특별 활동
    print("\n=== 앵무새 특별 쇼 ===")
    parrot.speak("안녕하세요!")
    parrot.speak("파이썬 재미있어요!")
```

이 실습을 통해 상속의 계층 구조, 메서드 오버라이딩, 추상 클래스 활용, 그리고 isinstance()를 사용한 다형성 구현을 모두 경험할 수 있습니다. 각 동물 클래스는 Animal이라는 공통 인터페이스를 가지면서도 고유한 특성을 표현하고 있어, 실제 프로그램에서 객체지향 설계가 어떻게 활용되는지 보여줍니다.

---

## 연습문제

1. **게임 캐릭터 클래스 만들기**: Warrior, Mage, Archer 클래스를 Character 부모 클래스로부터 상속받아 만들어보세요. 각 클래스는 고유한 스킬을 가져야 합니다.

2. **도형 계산기**: Shape 추상 클래스를 만들고, Triangle, Square, Pentagon 클래스를 상속받아 각각의 넓이와 둘레를 계산하는 메서드를 구현해보세요.

3. **교통수단 시뮬레이터**: Vehicle 클래스를 만들고 Car, Bicycle, Airplane 클래스를 상속받아 각각의 이동 방식을 구현해보세요. 연료 소모량도 계산해보세요.

4. **dataclass 활용**: 학생 정보를 관리하는 프로그램을 dataclass를 사용해 만들어보세요. 성적 계산, 학점 변환 기능을 포함해야 합니다.

5. **다중 상속 실습**: Flyable, Swimmable, Walkable 인터페이스를 만들고, 이를 다중 상속받는 동물 클래스들을 구현해보세요.
