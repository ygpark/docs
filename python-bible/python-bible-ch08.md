---
title: '8. 함수 정의하고 사용하기'
slug: python-functions-guide
description: '함수 정의, 매개변수, 반환값, 스코프, 타입 힌트까지 파이썬 함수를 체계적으로 마스터'
keywords: [파이썬 함수, def 키워드, 매개변수, return, 스코프, 타입 힌트, 함수 정의]
sidebar_position: 8
---

# 8. 함수 정의하고 사용하기

## 학습 목표

이 챕터를 완료하면 여러분은 파이썬 함수를 능숙하게 다룰 수 있게 됩니다. 함수의 기본 개념부터 시작해서 매개변수 활용, 반환값 처리, 변수의 스코프, 그리고 현대적인 타입 힌트까지 체계적으로 학습하게 됩니다. 이를 통해 코드의 재사용성을 높이고 더 체계적인 프로그램을 작성할 수 있는 능력을 기르게 됩니다.

---

## 8-1 함수란 무엇인가?

함수는 특정한 작업을 수행하는 코드의 묶음입니다. 마치 요리할 때 사용하는 믹서기처럼, 재료(입력값)를 넣으면 원하는 결과(출력값)를 만들어주는 도구라고 생각하면 됩니다. 함수를 사용하면 같은 코드를 반복해서 작성할 필요가 없어지고, 프로그램을 더 체계적으로 구성할 수 있습니다.

파이썬에는 이미 많은 내장 함수들이 있습니다. `print()`, `len()`, `input()` 등이 모두 함수입니다. 이제 우리만의 함수를 만들어 보겠습니다.

```python title=function_example.py
# 간단한 인사 함수
def greet():
    print("안녕하세요!")
    print("파이썬 함수를 배워봅시다.")

# 함수 호출
greet()
```

함수를 정의할 때는 `def` 키워드를 사용하고, 함수 이름 뒤에 괄호를 붙입니다. 함수의 내용은 들여쓰기로 구분합니다.

---

## 8-2 함수 정의와 호출

함수를 정의하는 기본 문법은 다음과 같습니다:

```python title=basic_function.py
def 함수이름():
    실행할 코드
    실행할 코드
```

함수를 호출할 때는 함수 이름 뒤에 괄호를 붙여서 호출합니다.

```python title=function_definition.py
def introduce_myself():
    print("제 이름은 파이썬입니다.")
    print("저는 1991년에 태어났습니다.")
    print("만나서 반가워요!")

def calculate_area():
    width = 5
    height = 3
    area = width * height
    print(f"가로 {width}, 세로 {height}인 직사각형의 넓이는 {area}입니다.")

# 함수 호출
introduce_myself()
print("-" * 30)
calculate_area()
```

함수는 정의만 해서는 실행되지 않습니다. 반드시 호출해야 함수 안의 코드가 실행됩니다.

---

## 8-3 매개변수와 인수

매개변수(parameter)는 함수가 입력받을 값의 이름이고, 인수(argument)는 함수를 호출할 때 실제로 전달하는 값입니다.

```python title=parameters_arguments.py
def greet_person(name):  # name은 매개변수
    print(f"안녕하세요, {name}님!")

def introduce(name, age):  # name과 age는 매개변수
    print(f"제 이름은 {name}이고, 나이는 {age}살입니다.")

def calculate_rectangle_area(width, height):
    area = width * height
    print(f"가로 {width}, 세로 {height}인 직사각형의 넓이는 {area}입니다.")

# 함수 호출 (괄호 안의 값들이 인수)
greet_person("김파이")  # "김파이"는 인수
introduce("이썬", 25)  # "이썬"과 25는 인수
calculate_rectangle_area(10, 7)  # 10과 7은 인수
```

매개변수를 사용하면 함수를 더 유연하게 만들 수 있습니다. 같은 함수로 다양한 값을 처리할 수 있기 때문입니다.

---

## 8-4 return문과 반환값

함수는 작업을 수행한 후 결과값을 돌려줄 수 있습니다. 이를 반환값이라고 하며, `return` 문을 사용합니다.

```python title=return_values.py
def add_numbers(a, b):
    result = a + b
    return result  # 결과값을 반환

def multiply(x, y):
    return x * y  # 계산 결과를 바로 반환

def get_circle_area(radius):
    pi = 3.14159
    area = pi * radius * radius
    return area

# 반환값을 변수에 저장
sum_result = add_numbers(5, 3)
print(f"5 + 3 = {sum_result}")

multiply_result = multiply(4, 6)
print(f"4 × 6 = {multiply_result}")

# 반환값을 바로 사용
print(f"반지름이 5인 원의 넓이: {get_circle_area(5)}")

# return문이 없는 함수는 None을 반환
def just_print():
    print("출력만 하는 함수")

result = just_print()
print(f"반환값: {result}")  # None이 출력됨
```

`return` 문을 만나면 함수는 즉시 종료되고 값을 반환합니다. `return` 문이 없는 함수는 자동으로 `None`을 반환합니다.

---

## 8-5 지역변수와 전역변수

변수의 스코프(scope)는 변수가 사용될 수 있는 범위를 의미합니다.

```python title=variable_scope.py
# 전역변수 (프로그램 어디서든 사용 가능)
global_var = "전역 변수입니다"

def local_variable_example():
    # 지역변수 (함수 내에서만 사용 가능)
    local_var = "지역 변수입니다"
    print(local_var)
    print(global_var)  # 전역변수도 접근 가능

def modify_global():
    global global_var  # global 키워드로 전역변수 수정
    global_var = "전역 변수가 수정되었습니다"

print(global_var)
local_variable_example()

# print(local_var)  # 오류! 지역변수는 함수 밖에서 접근 불가

modify_global()
print(global_var)

# 매개변수와 지역변수
def calculate_tax(price, tax_rate):
    # price와 tax_rate는 지역변수
    tax = price * tax_rate
    total = price + tax
    return total

result = calculate_tax(1000, 0.1)
print(f"세금 포함 가격: {result}원")
```

지역변수는 함수가 끝나면 사라지고, 전역변수는 프로그램이 끝날 때까지 유지됩니다.

---

## 8-6 기본값 매개변수

매개변수에 기본값을 지정하면, 인수를 전달하지 않아도 함수를 호출할 수 있습니다.

```python title=default_parameters.py
def greet(name, greeting="안녕하세요"):
    print(f"{greeting}, {name}님!")

def calculate_power(base, exponent=2):  # 제곱을 기본값으로
    return base ** exponent

def create_profile(name, age, city="서울"):
    print(f"이름: {name}")
    print(f"나이: {age}")
    print(f"거주지: {city}")

# 다양한 호출 방법
greet("김파이")  # 기본값 사용
greet("이썬", "좋은 아침입니다")  # 기본값 대신 다른 값 사용

print(f"2의 제곱: {calculate_power(2)}")  # 기본값 사용 (2^2)
print(f"3의 4제곱: {calculate_power(3, 4)}")  # 기본값 대신 4 사용

create_profile("박파이", 30)  # city는 기본값 "서울" 사용
create_profile("최썬", 25, "부산")  # 모든 값 직접 지정
```

기본값이 있는 매개변수는 기본값이 없는 매개변수보다 뒤에 위치해야 합니다.

---

## 8-7 가변 매개변수 (\*args, \*\*kwargs)

때로는 함수에 전달할 인수의 개수를 미리 알 수 없는 경우가 있습니다. 이때 가변 매개변수를 사용합니다.

```python title=variable_args.py
# *args: 여러 개의 위치 인수를 튜플로 받음
def sum_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

# **kwargs: 여러 개의 키워드 인수를 딕셔너리로 받음
def create_student(**info):
    print("학생 정보:")
    for key, value in info.items():
        print(f"  {key}: {value}")

# 둘 다 사용하는 함수
def flexible_function(required_arg, *args, **kwargs):
    print(f"필수 인수: {required_arg}")
    print(f"추가 위치 인수: {args}")
    print(f"키워드 인수: {kwargs}")

# 사용 예제
print(f"합계: {sum_all(1, 2, 3, 4, 5)}")
print(f"합계: {sum_all(10, 20)}")

create_student(name="김파이", age=20, grade="A+")

flexible_function("필수값", 1, 2, 3, name="이썬", city="서울")
```

`*args`는 여러 개의 위치 인수를, `**kwargs`는 여러 개의 키워드 인수를 받을 때 사용합니다.

---

## 8-8 타입 힌트 완전 활용

타입 힌트는 함수의 매개변수와 반환값의 타입을 명시하여 코드의 가독성과 안정성을 높여줍니다.

```python title=type_hints.py
from typing import List, Dict, Optional, Union

# 기본 타입 힌트
def add_numbers(a: int, b: int) -> int:
    return a + b

def greet_person(name: str) -> str:
    return f"안녕하세요, {name}님!"

# 리스트와 딕셔너리 타입 힌트
def calculate_average(scores: List[int]) -> float:
    return sum(scores) / len(scores)

def create_student_record(name: str, grades: Dict[str, int]) -> Dict[str, Union[str, float]]:
    average = sum(grades.values()) / len(grades)
    return {
        "name": name,
        "average": average
    }

# Optional (None이 될 수 있는 값)
def find_student(students: List[str], name: str) -> Optional[int]:
    try:
        return students.index(name)
    except ValueError:
        return None

# Union (여러 타입 중 하나)
def process_id(user_id: Union[int, str]) -> str:
    return f"사용자 ID: {user_id}"

# 사용 예제
result = add_numbers(5, 3)
print(f"결과: {result}")

greeting = greet_person("김파이")
print(greeting)

scores = [85, 92, 78, 96, 88]
avg = calculate_average(scores)
print(f"평균 점수: {avg:.1f}")

student_data = create_student_record("이썬", {"수학": 95, "영어": 87, "과학": 91})
print(f"학생 기록: {student_data}")

students_list = ["김파이", "이썬", "박코드"]
index = find_student(students_list, "이썬")
print(f"이썬의 인덱스: {index}")

print(process_id(12345))
print(process_id("user123"))
```

타입 힌트를 사용하면 IDE에서 더 나은 자동완성과 오류 검출을 제공받을 수 있습니다.

---

## 실습: 계산기 함수 만들기

간단한 계산기 프로그램을 함수들로 구성해보겠습니다. 이 예제는 지금까지 배운 모든 개념을 종합적으로 활용합니다.

```python title=calculator.py
from typing import Union

def add(a: float, b: float) -> float:
    """두 수를 더합니다."""
    return a + b

def subtract(a: float, b: float) -> float:
    """두 수를 뺍니다."""
    return a - b

def multiply(a: float, b: float) -> float:
    """두 수를 곱합니다."""
    return a * b

def divide(a: float, b: float) -> Union[float, str]:
    """두 수를 나눕니다. 0으로 나누면 오류 메시지를 반환합니다."""
    if b == 0:
        return "오류: 0으로 나눌 수 없습니다"
    return a / b

def power(base: float, exponent: float = 2) -> float:
    """거듭제곱을 계산합니다. 기본값은 제곱입니다."""
    return base ** exponent

def calculate(operation: str, a: float, b: float = None) -> None:
    """계산을 수행하고 결과를 출력합니다."""
    if operation == "+":
        result = add(a, b)
        print(f"{a} + {b} = {result}")
    elif operation == "-":
        result = subtract(a, b)
        print(f"{a} - {b} = {result}")
    elif operation == "*":
        result = multiply(a, b)
        print(f"{a} × {b} = {result}")
    elif operation == "/":
        result = divide(a, b)
        if isinstance(result, str):
            print(result)
        else:
            print(f"{a} ÷ {b} = {result}")
    elif operation == "**":
        if b is None:
            result = power(a)  # 기본값 사용 (제곱)
        else:
            result = power(a, b)
        print(f"{a} ^ {b if b is not None else 2} = {result}")
    else:
        print("지원하지 않는 연산입니다.")

def get_calculator_history(*calculations) -> None:
    """계산 기록을 출력합니다."""
    print("계산 기록:")
    for i, calc in enumerate(calculations, 1):
        print(f"  {i}. {calc}")

# 계산기 사용 예제
print("=== 파이썬 계산기 ===")
calculate("+", 10, 5)
calculate("-", 10, 5)
calculate("*", 10, 5)
calculate("/", 10, 5)
calculate("/", 10, 0)  # 0으로 나누기 오류
calculate("**", 2, 3)
calculate("**", 5)  # 기본값 사용 (제곱)

print()
get_calculator_history(
    "10 + 5 = 15",
    "10 - 5 = 5",
    "10 × 5 = 50",
    "2 ^ 3 = 8"
)
```

---

## 확장 과제: 과학 계산기 구현

더 발전된 계산기를 만들어보겠습니다. 이 과제는 함수의 고급 활용법을 보여줍니다.

```python title=scientific_calculator.py
import math
from typing import List, Callable, Dict

def factorial(n: int) -> int:
    """팩토리얼을 계산합니다."""
    if n < 0:
        raise ValueError("음수의 팩토리얼은 정의되지 않습니다")
    if n <= 1:
        return 1
    return n * factorial(n - 1)

def is_prime(n: int) -> bool:
    """소수인지 판별합니다."""
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

def fibonacci_sequence(n: int) -> List[int]:
    """피보나치 수열을 생성합니다."""
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    elif n == 2:
        return [0, 1]

    sequence = [0, 1]
    for i in range(2, n):
        sequence.append(sequence[i-1] + sequence[i-2])
    return sequence

def statistics_calculator(*numbers: float) -> Dict[str, float]:
    """숫자들의 통계를 계산합니다."""
    if not numbers:
        return {"error": "숫자가 입력되지 않았습니다"}

    sorted_nums = sorted(numbers)
    length = len(numbers)

    # 평균
    mean = sum(numbers) / length

    # 중앙값
    if length % 2 == 0:
        median = (sorted_nums[length//2 - 1] + sorted_nums[length//2]) / 2
    else:
        median = sorted_nums[length//2]

    # 분산과 표준편차
    variance = sum((x - mean) ** 2 for x in numbers) / length
    std_dev = math.sqrt(variance)

    return {
        "평균": round(mean, 2),
        "중앙값": median,
        "최솟값": min(numbers),
        "최댓값": max(numbers),
        "분산": round(variance, 2),
        "표준편차": round(std_dev, 2)
    }

# 함수를 딕셔너리로 관리
FUNCTIONS: Dict[str, Callable] = {
    "factorial": factorial,
    "is_prime": is_prime,
    "fibonacci": fibonacci_sequence,
    "stats": statistics_calculator
}

def run_scientific_calculator():
    """과학 계산기를 실행합니다."""
    print("=== 과학 계산기 ===")

    # 팩토리얼 계산
    print(f"5! = {factorial(5)}")

    # 소수 판별
    test_numbers = [17, 18, 19, 20]
    for num in test_numbers:
        print(f"{num}은 소수인가? {is_prime(num)}")

    # 피보나치 수열
    fib_seq = fibonacci_sequence(10)
    print(f"피보나치 수열 (10항): {fib_seq}")

    # 통계 계산
    scores = [85, 92, 78, 96, 88, 91, 83, 89, 94, 87]
    stats = statistics_calculator(*scores)
    print(f"성적 통계:")
    for key, value in stats.items():
        print(f"  {key}: {value}")

if __name__ == "__main__":
    run_scientific_calculator()
```

---

## 연습문제

1. **기본 함수 작성**: 사용자의 이름과 나이를 입력받아 소개하는 함수를 작성하세요.

2. **계산 함수**: 원의 반지름을 입력받아 둘레와 넓이를 계산하는 함수를 작성하세요.

3. **가변 인수 함수**: 여러 개의 숫자를 입력받아 최댓값과 최솟값을 반환하는 함수를 작성하세요.

4. **타입 힌트 활용**: 학생 정보(이름, 점수 리스트)를 입력받아 평균 점수와 등급을 반환하는 함수를 타입 힌트와 함께 작성하세요.

5. **종합 문제**: 간단한 비밀번호 생성기 함수를 작성하세요. 길이, 숫자 포함 여부, 특수문자 포함 여부를 매개변수로 받아야 합니다.

이제 여러분은 파이썬 함수의 핵심 개념들을 모두 학습했습니다. 함수는 프로그래밍에서 가장 중요한 개념 중 하나이므로, 다양한 예제를 만들어보며 연습해보세요!
