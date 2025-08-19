---
title: '2. 파이썬 기본 문법 - 변수, 주석, 입출력'
slug: python-basic-syntax-variables-comments
description: '파이썬의 변수, 주석, 들여쓰기, input/print 함수 등 핵심 기본 문법을 실습과 함께 완벽 마스터'
keywords:
  [
    파이썬 변수,
    Python 주석,
    파이썬 들여쓰기,
    input 함수,
    print 함수,
    파이썬 기본 문법,
    파이썬 입출력,
  ]
sidebar_position: 2
---

# 2. 파이썬 기본 문법

## 학습 목표

이 챕터를 완료하면 다음과 같은 능력을 갖게 됩니다:

- 변수를 생성하고 값을 저장하는 방법을 이해합니다
- 파이썬에서 주석을 작성하여 코드를 문서화할 수 있습니다
- 들여쓰기의 중요성을 이해하고 올바르게 적용할 수 있습니다
- input()과 print() 함수를 활용하여 사용자와 상호작용하는 프로그램을 만들 수 있습니다
- 배운 내용을 종합하여 간단한 계산기 프로그램을 직접 구현할 수 있습니다

---

## 2-1 변수와 상수

파이썬에서 변수는 데이터를 저장하는 상자와 같습니다. 프로그래밍을 할 때 다양한 정보를 임시로 기억해두었다가 나중에 사용해야 하는 경우가 많은데, 이때 변수를 활용합니다. 파이썬의 변수는 다른 언어와 달리 매우 유연하여 같은 변수에 숫자, 문자, 리스트 등 다양한 종류의 데이터를 저장할 수 있습니다.

### 변수 생성하기

```python title=variables_basic.py
# 변수 생성과 값 할당
name = "김파이썬"
age = 25
height = 175.5
is_student = True

# 변수 값 출력
print(name)
print(age)
print(height)
print(is_student)
```

### 변수명 규칙

```python title=variable_naming.py
# 올바른 변수명
user_name = "홍길동"
total_price = 50000
student_count = 30
is_valid = True

# 잘못된 변수명 (오류 발생)
# 2name = "잘못됨"      # 숫자로 시작할 수 없음
# user-name = "잘못됨"  # 하이픈 사용할 수 없음
# class = "잘못됨"      # 예약어 사용할 수 없음

# 추천하는 변수명 스타일 (스네이크 케이스)
first_name = "철수"
last_name = "김"
full_name = first_name + " " + last_name
print(f"안녕하세요, {full_name}님!")
```

### 상수 개념

```python title=constants.py
# 파이썬에는 진짜 상수는 없지만, 관례적으로 대문자로 작성
PI = 3.14159
MAX_STUDENTS = 100
COMPANY_NAME = "파이썬 회사"

# 원의 넓이 계산
radius = 5
area = PI * radius * radius
print(f"반지름 {radius}인 원의 넓이: {area}")
```

---

## 2-2 주석 작성하기

주석은 코드의 설명서 역할을 합니다. 나중에 자신이 작성한 코드를 다시 봤을 때 "이게 뭔지 기억이 안 난다"는 상황을 방지하고, 다른 사람이 코드를 이해하기 쉽게 도와줍니다. 좋은 주석은 '무엇을' 하는지보다 '왜' 하는지를 설명하는 것이 중요합니다.

### 한 줄 주석

```python title=single_line_comments.py
# 이것은 한 줄 주석입니다
print("안녕하세요!")  # 코드 끝에도 주석을 달 수 있습니다

# 할인율 계산 (왜 이 값을 사용하는지 설명)
discount_rate = 0.15  # 15% 할인 (신규 고객 혜택)
original_price = 10000
final_price = original_price * (1 - discount_rate)

print(f"원가: {original_price}원")
print(f"할인가: {final_price}원")
```

### 여러 줄 주석

```python title=multi_line_comments.py
"""
이것은 여러 줄 주석입니다.
복잡한 함수나 클래스를 설명할 때 사용합니다.
프로그램의 전체적인 목적을 설명하기도 합니다.
"""

def calculate_tax(income):
    """
    소득세 계산 함수

    Args:
        income (int): 연봉 (원)

    Returns:
        int: 계산된 소득세 (원)
    """
    # 간단한 세율 적용 (실제 세법과는 다름)
    tax_rate = 0.1  # 10% 세율
    return income * tax_rate

# 함수 사용 예제
annual_income = 30000000
tax = calculate_tax(annual_income)
print(f"연봉 {annual_income:,}원의 소득세: {tax:,}원")
```

---

## 2-3 들여쓰기의 중요성

파이썬의 가장 독특한 특징 중 하나는 들여쓰기로 코드 블록을 구분한다는 것입니다. 다른 언어에서 중괄호 {}를 사용하는 것과 달리, 파이썬은 들여쓰기 자체가 문법의 일부입니다. 이는 코드를 더 읽기 쉽게 만들어주지만, 처음에는 주의 깊게 관리해야 합니다.

### 올바른 들여쓰기

```python title=indentation_correct.py
# 조건문에서의 들여쓰기
score = 85

if score >= 90:
    print("A등급입니다!")
    print("훌륭해요!")
elif score >= 80:
    print("B등급입니다!")
    print("잘했어요!")
else:
    print("더 노력하세요!")

print("성적 확인이 끝났습니다.")  # 들여쓰기 없음 (if문 밖)
```

### 중첩된 들여쓰기

```python title=nested_indentation.py
# 중첩된 조건문에서의 들여쓰기
weather = "비"
temperature = 15

if weather == "비":
    print("우산을 챙기세요!")
    if temperature < 10:
        print("추우니까 두꺼운 옷도 입으세요!")
    else:
        print("비만 조심하면 됩니다.")
else:
    print("좋은 날씨네요!")

# 반복문에서의 들여쓰기
print("\n구구단 3단:")
for i in range(1, 6):
    result = 3 * i
    print(f"3 × {i} = {result}")
```

---

## 2-4 입력과 출력 (input, print)

프로그램이 사용자와 소통하려면 입력과 출력 기능이 필요합니다. print() 함수로 정보를 보여주고, input() 함수로 사용자의 입력을 받아올 수 있습니다. 이 두 함수를 잘 활용하면 사용자와 상호작용하는 프로그램을 만들 수 있습니다.

### 기본 출력 (print)

```python title=print_examples.py
# 기본 출력
print("안녕하세요!")
print("파이썬을 배워봅시다!")

# 여러 값 동시 출력
name = "김코딩"
age = 20
print("이름:", name, "나이:", age)

# 구분자 변경
print("사과", "바나나", "오렌지", sep=" | ")
print("첫 번째 줄", end=" ")
print("같은 줄에 이어서")

# f-string 사용 (추천 방법)
price = 1500
quantity = 3
total = price * quantity
print(f"단가: {price}원, 수량: {quantity}개")
print(f"총액: {total:,}원")  # 천 단위 콤마 표시
```

### 기본 입력 (input)

```python title=input_examples.py
# 기본 입력받기
name = input("이름을 입력하세요: ")
print(f"안녕하세요, {name}님!")

# 숫자 입력받기 (형변환 필요)
age = input("나이를 입력하세요: ")
age_number = int(age)  # 문자열을 정수로 변환
print(f"당신은 {age_number}세이군요!")

# 더 간단하게 한 번에
height = float(input("키를 입력하세요 (cm): "))
print(f"당신의 키는 {height}cm입니다.")

# 여러 값 한 번에 입력받기
print("좋아하는 과일 3개를 공백으로 구분해서 입력하세요:")
fruits = input().split()  # 공백으로 나누어 리스트로 만들기
print(f"입력한 과일들: {fruits}")
```

---

## 2-5 간단한 계산기 만들기

지금까지 배운 변수, 입출력, 주석을 모두 활용하여 실용적인 계산기 프로그램을 만들어보겠습니다. 이 프로젝트를 통해 사용자 입력을 받아 처리하고 결과를 보여주는 완전한 프로그램의 구조를 이해할 수 있습니다.

```python title=simple_calculator.py
"""
간단한 계산기 프로그램
사용자로부터 두 숫자와 연산자를 입력받아 계산 결과를 보여줍니다.
"""

# 프로그램 시작 메시지
print("=" * 30)
print("    간단한 계산기 v1.0")
print("=" * 30)

# 첫 번째 숫자 입력
print("\n첫 번째 숫자를 입력하세요.")
num1 = float(input("숫자 1: "))

# 연산자 입력
print("\n연산자를 입력하세요 (+, -, *, /)")
operator = input("연산자: ")

# 두 번째 숫자 입력
print("\n두 번째 숫자를 입력하세요.")
num2 = float(input("숫자 2: "))

# 계산 수행
print(f"\n계산 중: {num1} {operator} {num2}")

if operator == "+":
    result = num1 + num2
    print(f"결과: {result}")
elif operator == "-":
    result = num1 - num2
    print(f"결과: {result}")
elif operator == "*":
    result = num1 * num2
    print(f"결과: {result}")
elif operator == "/":
    if num2 != 0:  # 0으로 나누기 방지
        result = num1 / num2
        print(f"결과: {result}")
    else:
        print("오류: 0으로 나눌 수 없습니다!")
else:
    print("오류: 지원하지 않는 연산자입니다!")

print("\n계산기를 사용해 주셔서 감사합니다!")
```

### 개선된 계산기

```python title=advanced_calculator.py
"""
개선된 계산기 프로그램
- 여러 번 계산 가능
- 이전 결과 활용 가능
- 더 나은 사용자 경험
"""

print("고급 계산기에 오신 것을 환영합니다!")
print("'quit' 또는 'q'를 입력하면 종료됩니다.\n")

previous_result = None  # 이전 결과 저장

while True:
    # 첫 번째 숫자 입력 (이전 결과 활용 가능)
    if previous_result is not None:
        use_previous = input(f"이전 결과 {previous_result}를 사용하시겠습니까? (y/n): ")
        if use_previous.lower() == 'y':
            num1 = previous_result
        else:
            num1_input = input("첫 번째 숫자: ")
            if num1_input.lower() in ['quit', 'q']:
                break
            num1 = float(num1_input)
    else:
        num1_input = input("첫 번째 숫자: ")
        if num1_input.lower() in ['quit', 'q']:
            break
        num1 = float(num1_input)

    # 연산자 입력
    operator = input("연산자 (+, -, *, /): ")
    if operator.lower() in ['quit', 'q']:
        break

    # 두 번째 숫자 입력
    num2_input = input("두 번째 숫자: ")
    if num2_input.lower() in ['quit', 'q']:
        break
    num2 = float(num2_input)

    # 계산 및 결과 출력
    if operator == "+":
        result = num1 + num2
    elif operator == "-":
        result = num1 - num2
    elif operator == "*":
        result = num1 * num2
    elif operator == "/":
        if num2 != 0:
            result = num1 / num2
        else:
            print("오류: 0으로 나눌 수 없습니다!\n")
            continue
    else:
        print("오류: 올바른 연산자를 입력하세요!\n")
        continue

    print(f"결과: {num1} {operator} {num2} = {result}\n")
    previous_result = result  # 결과를 다음 계산에서 사용할 수 있도록 저장

print("계산기를 종료합니다. 안녕히 가세요!")
```

---

## 연습문제

### 문제 1: 개인정보 입력 프로그램

사용자로부터 이름, 나이, 취미를 입력받아 자기소개 문장을 만드는 프로그램을 작성하세요.

### 문제 2: 단위 변환기

섭씨온도를 입력받아 화씨온도로 변환하는 프로그램을 작성하세요.
(공식: 화씨 = 섭씨 × 9/5 + 32)

### 문제 3: 할인 계산기

상품 가격과 할인율을 입력받아 최종 가격을 계산하는 프로그램을 작성하세요.

### 문제 4: BMI 계산기

키(cm)와 몸무게(kg)를 입력받아 BMI를 계산하고, BMI 수치에 따른 건강 상태를 출력하는 프로그램을 작성하세요.

---

## 챕터 요약

이 챕터에서는 파이썬 프로그래밍의 기본 문법을 배웠습니다:

- **변수와 상수**: 데이터를 저장하고 관리하는 방법
- **주석**: 코드를 문서화하여 가독성을 높이는 방법
- **들여쓰기**: 파이썬만의 독특한 코드 블록 구분 방법
- **입출력**: input()과 print()를 활용한 사용자와의 상호작용
- **실습 프로젝트**: 배운 내용을 종합한 계산기 프로그램 구현

다음 챕터에서는 파이썬의 기본 자료형에 대해 자세히 알아보겠습니다. 숫자, 문자열, 불린형 등 다양한 데이터 타입을 다루는 방법을 배우게 될 것입니다!
