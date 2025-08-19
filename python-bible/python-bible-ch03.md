---
title: '3. 기본 자료형 - 숫자, 문자열, 불린'
slug: python-basic-data-types
description: '파이썬의 핵심 자료형인 숫자형(정수, 실수), 문자열, 불린형의 특징과 활용법을 실습과 함께 완전 마스터'
keywords:
  [파이썬 자료형, 정수형 int, 실수형 float, 문자열 str, 불린형 bool, 자료형 변환, 파이썬 기본 타입]
sidebar_position: 3
---

# 3. 기본 자료형

프로그래밍에서 데이터를 다루는 것은 요리에서 재료를 다루는 것과 같습니다. 좋은 요리를 만들려면 각 재료의 특성을 알아야 하듯이, 효과적인 프로그램을 작성하려면 각 자료형의 특징과 용도를 정확히 이해해야 합니다. 파이썬은 다양한 기본 자료형을 제공하며, 각각은 서로 다른 종류의 데이터를 저장하고 처리하는 데 최적화되어 있습니다. 이번 챕터에서는 숫자형, 문자열, 불린형 등 파이썬의 핵심 자료형들을 살펴보고, 실제 프로그램에서 어떻게 활용하는지 배워보겠습니다.

## 학습 목표

- 파이썬의 기본 자료형(숫자형, 문자열, 불린형)의 특징과 용도를 이해한다
- 각 자료형의 기본 연산과 메서드를 활용할 수 있다
- 자료형 간 변환 방법을 익히고 적절한 상황에서 활용할 수 있다
- 실제 프로그램에서 적절한 자료형을 선택하여 사용할 수 있다

## 3-1 숫자형 (정수, 실수)

숫자는 프로그래밍에서 가장 기본적이면서도 중요한 데이터입니다. 계산, 측정, 카운팅 등 거의 모든 프로그램에서 숫자를 다루게 됩니다. 파이썬은 정수(int)와 실수(float) 두 가지 주요 숫자형을 제공하며, 각각의 특성을 이해하면 더 정확하고 효율적인 프로그램을 작성할 수 있습니다.

### 정수형 (int)

정수형은 소수점이 없는 숫자를 나타냅니다. 파이썬의 정수형은 크기 제한이 없어 매우 큰 수도 처리할 수 있습니다.

```python title=integer_example.py
# 정수 변수 선언
age = 25
year = 2025
population = 51000000

# 큰 수도 처리 가능
big_number = 123456789012345678901234567890
print(f"큰 수: {big_number}")

# 정수 연산
a = 10
b = 3
print(f"덧셈: {a + b}")      # 13
print(f"뺄셈: {a - b}")      # 7
print(f"곱셈: {a * b}")      # 30
print(f"나눗셈: {a / b}")    # 3.3333333333333335 (실수 결과)
print(f"정수 나눗셈: {a // b}")  # 3 (몫만)
print(f"나머지: {a % b}")    # 1
print(f"거듭제곱: {a ** b}") # 1000
```

### 실수형 (float)

실수형은 소수점이 있는 숫자를 나타냅니다. 과학적 표기법도 지원합니다.

```python title=float_example.py
# 실수 변수 선언
height = 175.5
weight = 70.2
pi = 3.14159

# 과학적 표기법
speed_of_light = 3.0e8  # 3.0 × 10^8
planck_constant = 6.626e-34  # 6.626 × 10^-34

print(f"빛의 속도: {speed_of_light}")
print(f"플랑크 상수: {planck_constant}")

# 실수 연산
x = 10.5
y = 3.2
print(f"실수 덧셈: {x + y}")  # 13.7
print(f"실수 곱셈: {x * y}")  # 33.6

# 반올림
print(f"반올림: {round(pi, 2)}")  # 3.14
```

### 진법 표현

파이썬은 다양한 진법으로 수를 표현할 수 있습니다.

```python title=number_base.py
# 다양한 진법
decimal = 42        # 10진법
binary = 0b101010   # 2진법 (0b 접두사)
octal = 0o52        # 8진법 (0o 접두사)
hexadecimal = 0x2A  # 16진법 (0x 접두사)

print(f"10진법: {decimal}")
print(f"2진법: {binary}")
print(f"8진법: {octal}")
print(f"16진법: {hexadecimal}")

# 진법 변환
number = 42
print(f"2진법으로: {bin(number)}")    # 0b101010
print(f"8진법으로: {oct(number)}")    # 0o52
print(f"16진법으로: {hex(number)}")   # 0x2a
```

## 3-2 문자열 기초

문자열은 텍스트 데이터를 다루는 핵심 자료형입니다. 사용자와의 소통, 파일 처리, 웹 개발 등 거의 모든 프로그래밍 영역에서 문자열을 활용합니다. 파이썬에서 문자열은 매우 강력하고 유연한 기능을 제공하므로, 기본 개념을 확실히 익히는 것이 중요합니다.

### 문자열 생성과 출력

문자열은 작은따옴표('')나 큰따옴표("")로 묶어서 생성할 수 있습니다.

```python title=string_creation.py
# 다양한 문자열 생성 방법
name = "김파이썬"
message = '안녕하세요!'
empty_string = ""

# 따옴표 포함하기
quote1 = "그는 '안녕하세요'라고 말했습니다."
quote2 = '그는 "안녕하세요"라고 말했습니다.'

# 여러 줄 문자열
multiline = """이것은
여러 줄에 걸친
문자열입니다."""

another_multiline = '''또 다른
여러 줄
문자열'''

print(name)
print(message)
print(quote1)
print(multiline)
```

### 이스케이프 시퀀스

특수 문자를 문자열에 포함하려면 이스케이프 시퀀스를 사용합니다.

```python title=escape_sequences.py
# 이스케이프 시퀀스 활용
newline_example = "첫 번째 줄\n두 번째 줄"
tab_example = "이름:\t김파이썬"
backslash_example = "파일 경로: C:\\Users\\Python"
quote_example = "그는 \"안녕하세요\"라고 말했습니다."

print(newline_example)
print(tab_example)
print(backslash_example)
print(quote_example)

# raw 문자열 (이스케이프 무시)
raw_string = r"C:\Users\Python\newfile.txt"
print(raw_string)
```

### 기본 문자열 연산

문자열도 덧셈과 곱셈 연산이 가능합니다.

```python title=string_operations.py
# 문자열 연결 (concatenation)
first_name = "김"
last_name = "파이썬"
full_name = first_name + last_name
print(f"이름: {full_name}")

# 문자열과 숫자를 함께 출력
age = 25
greeting = "안녕하세요! 저는 " + full_name + "이고, " + str(age) + "살입니다."
print(greeting)

# 문자열 반복
separator = "-" * 30
print(separator)
print("중요한 공지사항")
print(separator)

# 문자열 길이
message = "파이썬 프로그래밍"
print(f"문자열 길이: {len(message)}")

# 문자열 포함 여부 확인
sentence = "파이썬은 쉽고 강력한 프로그래밍 언어입니다."
print("파이썬" in sentence)      # True
print("자바" in sentence)        # False
print("어려운" not in sentence)  # True
```

## 3-3 불린형 (Boolean)

불린형은 참(True) 또는 거짓(False) 두 가지 값만을 가지는 자료형입니다. 조건문, 반복문, 논리 연산에서 핵심적인 역할을 합니다. 프로그램의 흐름을 제어하고 의사결정을 내리는 데 필수적인 자료형이므로, 불린 값이 어떻게 결정되는지 정확히 이해하는 것이 중요합니다.

```python title=boolean_basics.py
# 불린 변수
is_student = True
is_adult = False
has_license = True

print(f"학생인가요? {is_student}")
print(f"성인인가요? {is_adult}")

# 비교 연산의 결과는 불린
age = 20
print(f"성인 여부: {age >= 18}")        # True
print(f"미성년자 여부: {age < 18}")      # False

score = 85
print(f"합격 여부: {score >= 60}")       # True

# 논리 연산자
has_id = True
has_money = False

can_enter = has_id and has_money
print(f"입장 가능: {can_enter}")         # False

can_try = has_id or has_money
print(f"시도 가능: {can_try}")           # True

is_ready = not has_money
print(f"준비되지 않음: {is_ready}")      # True
```

### 다른 자료형의 불린 변환

파이썬에서는 모든 값이 불린 컨텍스트에서 참 또는 거짓으로 평가됩니다.

```python title=boolean_conversion.py
# 거짓으로 평가되는 값들
print(bool(0))          # False - 숫자 0
print(bool(0.0))        # False - 실수 0.0
print(bool(""))         # False - 빈 문자열
print(bool([]))         # False - 빈 리스트
print(bool({}))         # False - 빈 딕셔너리
print(bool(None))       # False - None

print("=" * 30)

# 참으로 평가되는 값들
print(bool(1))          # True - 0이 아닌 숫자
print(bool(-1))         # True - 음수도 참
print(bool("hello"))    # True - 비어있지 않은 문자열
print(bool([1, 2, 3]))  # True - 비어있지 않은 리스트
print(bool({"a": 1}))   # True - 비어있지 않은 딕셔너리

# 실제 활용 예시
username = input("사용자명을 입력하세요: ")
if username:  # 빈 문자열이 아니면 True
    print(f"환영합니다, {username}님!")
else:
    print("사용자명을 입력해주세요.")
```

## 3-4 자료형 변환

프로그래밍을 하다 보면 한 자료형을 다른 자료형으로 변환해야 하는 경우가 자주 있습니다. 예를 들어, 사용자로부터 입력받은 문자열을 숫자로 변환하거나, 계산 결과를 문자열로 바꿔 출력해야 할 때가 있습니다. 파이썬은 이러한 변환을 위한 다양한 내장 함수를 제공합니다.

```python title=type_conversion.py
# 문자열을 숫자로 변환
str_number = "123"
int_number = int(str_number)
print(f"문자열 '{str_number}'을 정수로: {int_number}")
print(f"타입: {type(int_number)}")

str_float = "45.67"
float_number = float(str_float)
print(f"문자열 '{str_float}'을 실수로: {float_number}")

# 숫자를 문자열로 변환
age = 25
age_str = str(age)
print(f"정수 {age}를 문자열로: '{age_str}'")

pi = 3.14159
pi_str = str(pi)
print(f"실수 {pi}를 문자열로: '{pi_str}'")

# 불린 변환
print(f"문자열 '1'을 불린으로: {bool('1')}")      # True
print(f"정수 0을 불린으로: {bool(0)}")             # False
print(f"정수 1을 불린으로: {bool(1)}")             # True

# 변환 시 주의사항
try:
    invalid_number = int("abc")  # 오류 발생!
except ValueError as e:
    print(f"변환 오류: {e}")

# 안전한 변환 방법
user_input = "123abc"
if user_input.isdigit():
    number = int(user_input)
    print(f"변환 성공: {number}")
else:
    print("유효한 숫자가 아닙니다.")

# 실수 변환 시 주의점
print(f"정수를 실수로: {float(42)}")        # 42.0
print(f"실수를 정수로: {int(3.9)}")         # 3 (소수점 버림)
print(f"반올림 후 정수로: {round(3.9)}")     # 4
```

### 자료형 확인하기

프로그램에서 변수의 자료형을 확인하는 것은 디버깅과 안전한 코딩에 중요합니다.

```python title=type_checking.py
# type() 함수로 자료형 확인
name = "김파이썬"
age = 25
height = 175.5
is_student = True

print(f"name의 타입: {type(name)}")           # <class 'str'>
print(f"age의 타입: {type(age)}")             # <class 'int'>
print(f"height의 타입: {type(height)}")       # <class 'float'>
print(f"is_student의 타입: {type(is_student)}")  # <class 'bool'>

# isinstance() 함수로 타입 검사
print(f"name이 문자열인가? {isinstance(name, str)}")      # True
print(f"age가 정수인가? {isinstance(age, int)}")          # True
print(f"height가 숫자인가? {isinstance(height, (int, float))}")  # True

# 실용적인 타입 검사 함수
def safe_divide(a, b):
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        return "숫자만 입력해주세요."
    if b == 0:
        return "0으로 나눌 수 없습니다."
    return a / b

print(safe_divide(10, 2))      # 5.0
print(safe_divide("10", 2))    # 숫자만 입력해주세요.
print(safe_divide(10, 0))      # 0으로 나눌 수 없습니다.
```

## 실습: 사용자 정보 입력받기 프로그램

지금까지 배운 기본 자료형들을 활용하여 실제 사용자 정보를 입력받고 처리하는 프로그램을 만들어보겠습니다. 이 실습을 통해 자료형 변환, 입력 검증, 그리고 사용자 친화적인 출력 방법을 익힐 수 있습니다.

```python title=user_info_program.py
def get_user_info():
    """사용자 정보를 입력받고 처리하는 프로그램"""

    print("=" * 40)
    print("      사용자 정보 입력 프로그램")
    print("=" * 40)

    # 이름 입력 (문자열)
    while True:
        name = input("이름을 입력하세요: ").strip()
        if name:  # 빈 문자열이 아닌 경우
            break
        print("이름을 반드시 입력해주세요!")

    # 나이 입력 (정수)
    while True:
        age_input = input("나이를 입력하세요: ").strip()
        if age_input.isdigit():
            age = int(age_input)
            if 0 < age < 150:  # 합리적인 나이 범위
                break
            else:
                print("올바른 나이를 입력해주세요. (1-149)")
        else:
            print("숫자만 입력해주세요.")

    # 키 입력 (실수)
    while True:
        try:
            height = float(input("키를 입력하세요 (cm): "))
            if 50 < height < 300:  # 합리적인 키 범위
                break
            else:
                print("올바른 키를 입력해주세요. (50-300cm)")
        except ValueError:
            print("올바른 숫자를 입력해주세요.")

    # 학생 여부 입력 (불린)
    while True:
        student_input = input("학생이신가요? (y/n): ").strip().lower()
        if student_input in ['y', 'yes', '예', 'ㅇ']:
            is_student = True
            break
        elif student_input in ['n', 'no', '아니오', 'ㄴ']:
            is_student = False
            break
        else:
            print("y(예) 또는 n(아니오)로 답해주세요.")

    # 결과 출력
    print("\n" + "=" * 40)
    print("         입력된 정보")
    print("=" * 40)
    print(f"이름: {name}")
    print(f"나이: {age}세")
    print(f"키: {height:.1f}cm")
    print(f"학생 여부: {'예' if is_student else '아니오'}")

    # 추가 정보 계산 및 출력
    birth_year = 2025 - age
    print(f"추정 출생년도: {birth_year}년")

    # BMI 계산을 위한 몸무게 입력 (선택사항)
    weight_input = input("\n몸무게를 입력하시겠습니까? (y/n): ").strip().lower()
    if weight_input in ['y', 'yes', '예', 'ㅇ']:
        while True:
            try:
                weight = float(input("몸무게를 입력하세요 (kg): "))
                if 20 < weight < 500:
                    # BMI 계산
                    height_m = height / 100  # cm를 m로 변환
                    bmi = weight / (height_m ** 2)
                    print(f"BMI: {bmi:.2f}")

                    # BMI 분류
                    if bmi < 18.5:
                        bmi_category = "저체중"
                    elif bmi < 23:
                        bmi_category = "정상"
                    elif bmi < 25:
                        bmi_category = "과체중"
                    else:
                        bmi_category = "비만"

                    print(f"BMI 분류: {bmi_category}")
                    break
                else:
                    print("올바른 몸무게를 입력해주세요. (20-500kg)")
            except ValueError:
                print("올바른 숫자를 입력해주세요.")

    print("\n프로그램을 종료합니다. 감사합니다!")

# 프로그램 실행
if __name__ == "__main__":
    get_user_info()
```

이 실습 프로그램에서는 다음과 같은 내용을 다뤘습니다:

- **문자열 처리**: 사용자 이름 입력과 공백 제거
- **정수 변환**: 나이 입력과 유효성 검사
- **실수 변환**: 키와 몸무게 입력 처리
- **불린 로직**: 학생 여부 판단과 조건문 활용
- **예외 처리**: 잘못된 입력에 대한 안전한 처리
- **계산과 포맷팅**: BMI 계산과 소수점 표시

## 연습문제

### 기본 문제

1. 두 개의 정수를 입력받아 사칙연산(+, -, \*, /, //, %, \*\*)의 결과를 모두 출력하는 프로그램을 작성하세요.

2. 문자열 3개를 입력받아 이들을 연결한 결과와 각 문자열의 길이를 출력하는 프로그램을 작성하세요.

3. 점수를 입력받아 60점 이상이면 "합격", 미만이면 "불합격"을 출력하는 프로그램을 작성하세요.

### 응용 문제

4. 원의 반지름을 입력받아 둘레와 넓이를 계산하는 프로그램을 작성하세요. (π = 3.14159 사용)

5. 초를 입력받아 "시:분:초" 형식으로 변환하여 출력하는 프로그램을 작성하세요.

6. 이름과 생년월일을 입력받아 현재 나이를 계산하고, 성인인지 미성년자인지 판단하는 프로그램을 작성하세요.

이번 챕터에서는 파이썬의 기본 자료형들을 배웠습니다. 다음 챕터에서는 문자열을 더 깊이 있게 다뤄보겠습니다!
