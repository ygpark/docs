---
title: '7. 반복문으로 반복하기'
slug: python-loops-iteration
description: 'for문, while문, range 함수, break/continue, 리스트 컴프리헨션까지 파이썬 반복문 완전 정복'
keywords:
  [파이썬 반복문, for문, while문, range 함수, break continue, 리스트 컴프리헨션, 중첩 반복문]
sidebar_position: 7
---

# 7. 반복문으로 반복하기

프로그래밍에서 반복은 매우 중요한 개념입니다. 같은 작업을 여러 번 수행해야 할 때, 코드를 반복해서 작성하는 것은 비효율적이죠. 예를 들어 "안녕하세요"를 100번 출력하고 싶다면, print문을 100번 작성할 수는 없습니다. 이때 반복문이 필요합니다. 반복문을 사용하면 코드 몇 줄로 복잡한 반복 작업을 간단하게 처리할 수 있습니다. 이번 장에서는 파이썬의 두 가지 주요 반복문인 for문과 while문을 배우고, 반복을 제어하는 방법과 더 효율적인 코드 작성법까지 알아보겠습니다.

## 학습 목표

- for문과 while문의 차이점을 이해하고 상황에 맞게 선택할 수 있다
- range() 함수를 활용하여 다양한 반복 패턴을 구현할 수 있다
- break와 continue를 사용하여 반복 흐름을 제어할 수 있다
- 중첩 반복문으로 복잡한 패턴을 출력할 수 있다
- 리스트 컴프리헨션으로 간결한 코드를 작성할 수 있다

---

## 7-1 for문 기본 구조

for문은 정해진 범위나 컬렉션의 각 요소에 대해 반복 작업을 수행할 때 사용합니다. 반복 횟수가 명확하거나 리스트, 문자열 같은 시퀀스 데이터를 처리할 때 매우 유용합니다.

### 기본 문법

```python
for 변수 in 반복가능한객체:
    실행할 코드
```

### 리스트와 함께 사용하기

```python title=for_basic.py
# 리스트의 각 요소 출력하기
fruits = ['사과', '바나나', '오렌지', '포도']
for fruit in fruits:
    print(f"저는 {fruit}를 좋아해요!")

print("과일 소개가 끝났습니다.")
```

### 문자열과 함께 사용하기

```python title=for_string.py
# 문자열의 각 문자 출력하기
message = "안녕하세요"
for char in message:
    print(f"문자: {char}")

# 문자열의 문자 개수 세기
text = "Python"
count = 0
for char in text:
    count += 1
print(f"'{text}'의 문자 개수: {count}")
```

---

## 7-2 range() 함수 활용

range() 함수는 연속된 숫자를 생성하는 파이썬의 내장 함수입니다. for문과 함께 사용하면 지정된 횟수만큼 반복하거나 특정 패턴의 숫자를 생성할 수 있어서 매우 유용합니다.

### range() 함수의 세 가지 형태

```python title=range_examples.py
# 1. range(끝값) - 0부터 끝값-1까지
print("range(5):")
for i in range(5):
    print(i, end=' ')
print()

# 2. range(시작값, 끝값) - 시작값부터 끝값-1까지
print("range(2, 8):")
for i in range(2, 8):
    print(i, end=' ')
print()

# 3. range(시작값, 끝값, 증가값) - 증가값만큼 증가
print("range(0, 10, 2):")
for i in range(0, 10, 2):
    print(i, end=' ')
print()

# 감소하는 range
print("range(10, 0, -2):")
for i in range(10, 0, -2):
    print(i, end=' ')
print()
```

### 실용적인 range() 활용

```python title=range_practical.py
# 1부터 10까지의 합 구하기
total = 0
for i in range(1, 11):
    total += i
print(f"1부터 10까지의 합: {total}")

# 리스트의 인덱스와 값을 함께 출력하기
colors = ['빨강', '파랑', '노랑', '초록']
for i in range(len(colors)):
    print(f"{i+1}번째 색깔: {colors[i]}")

# enumerate() 함수를 사용한 더 간단한 방법
print("\nenumerate() 사용:")
for index, color in enumerate(colors, 1):
    print(f"{index}번째 색깔: {color}")
```

---

## 7-3 while문 기본 구조

while문은 조건이 참인 동안 계속 반복을 수행합니다. 반복 횟수가 미리 정해지지 않았거나, 특정 조건이 만족될 때까지 반복해야 할 때 사용합니다.

### 기본 문법

```python
while 조건:
    실행할 코드
    # 조건을 변경하는 코드가 있어야 함
```

### while문 기본 예제

```python title=while_basic.py
# 1부터 5까지 출력하기
count = 1
while count <= 5:
    print(f"현재 카운트: {count}")
    count += 1  # 조건을 변경하는 중요한 부분!

print("반복 완료!")

# 사용자 입력을 받아 처리하기
print("\n숫자 맞추기 게임")
secret_number = 7
guess = 0

while guess != secret_number:
    guess = int(input("1부터 10까지 숫자를 맞춰보세요: "))
    if guess < secret_number:
        print("더 큰 수를 입력하세요!")
    elif guess > secret_number:
        print("더 작은 수를 입력하세요!")
    else:
        print("정답입니다!")
```

### 무한 루프 주의사항

```python title=infinite_loop_warning.py
# 위험한 무한 루프 예제 (실행하지 마세요!)
# count = 1
# while count <= 5:
#     print(count)
#     # count += 1이 없으면 무한 루프!

# 안전한 while문 작성법
count = 1
max_iterations = 1000  # 안전장치

while count <= 5 and max_iterations > 0:
    print(f"안전한 카운트: {count}")
    count += 1
    max_iterations -= 1
```

---

## 7-4 break와 continue

반복문을 실행하다가 특정 조건에서 반복을 중단하거나 건너뛰어야 할 때가 있습니다. 이때 break와 continue 키워드를 사용합니다.

### break: 반복문 완전히 종료

```python title=break_example.py
# 특정 조건에서 반복 중단하기
print("break 예제:")
for i in range(1, 11):
    if i == 6:
        print("6에 도달했으므로 반복을 중단합니다.")
        break
    print(f"현재 숫자: {i}")

print("반복문 종료\n")

# 사용자 입력 프로그램에서 break 활용
print("간단한 계산기 (quit 입력시 종료)")
while True:
    user_input = input("계산할 숫자를 입력하세요 (quit으로 종료): ")
    if user_input.lower() == 'quit':
        print("계산기를 종료합니다.")
        break

    try:
        number = float(user_input)
        result = number * 2
        print(f"{number} × 2 = {result}")
    except ValueError:
        print("올바른 숫자를 입력해주세요.")
```

### continue: 현재 반복 건너뛰기

```python title=continue_example.py
# 짝수만 출력하기 (홀수는 건너뛰기)
print("continue 예제 - 짝수만 출력:")
for i in range(1, 11):
    if i % 2 == 1:  # 홀수라면
        continue    # 아래 코드를 건너뛰고 다음 반복으로
    print(f"짝수: {i}")

print()

# 리스트에서 특정 조건의 요소만 처리하기
numbers = [1, -2, 3, -4, 5, -6, 7, -8, 9, -10]
positive_sum = 0

print("양수만 더하기:")
for num in numbers:
    if num < 0:
        continue  # 음수는 건너뛰기
    print(f"양수 발견: {num}")
    positive_sum += num

print(f"양수들의 합: {positive_sum}")
```

---

## 7-5 중첩 반복문

반복문 안에 또 다른 반복문을 넣는 것을 중첩 반복문이라고 합니다. 2차원 데이터를 처리하거나 복잡한 패턴을 만들 때 유용합니다.

### 기본 중첩 반복문

```python title=nested_loops.py
# 기본 중첩 for문
print("중첩 반복문 기본 예제:")
for i in range(1, 4):
    print(f"외부 루프: {i}")
    for j in range(1, 4):
        print(f"  내부 루프: {j}")
    print()

# 구구단 만들기
print("구구단 (2단부터 4단까지):")
for dan in range(2, 5):
    print(f"\n[{dan}단]")
    for num in range(1, 10):
        result = dan * num
        print(f"{dan} × {num} = {result}")
```

### 패턴 출력하기

```python title=pattern_output.py
# 별표 패턴 출력하기
print("별표 삼각형 패턴:")
for i in range(1, 6):
    for j in range(i):
        print("*", end="")
    print()  # 줄바꿈

print()

# 숫자 피라미드 패턴
print("숫자 피라미드:")
for i in range(1, 6):
    # 공백 출력
    for space in range(6-i):
        print(" ", end="")
    # 숫자 출력
    for num in range(1, i+1):
        print(num, end=" ")
    print()  # 줄바꿈

print()

# 체스판 패턴 (5x5)
print("체스판 패턴:")
for i in range(5):
    for j in range(5):
        if (i + j) % 2 == 0:
            print("■", end=" ")
        else:
            print("□", end=" ")
    print()  # 줄바꿈
```

---

## 7-6 리스트 컴프리헨션

리스트 컴프리헨션은 기존의 리스트나 반복 가능한 객체를 기반으로 새로운 리스트를 간결하게 만드는 파이썬의 강력한 기능입니다. 반복문과 조건문을 한 줄로 표현할 수 있어 코드가 훨씬 깔끔해집니다.

### 기본 리스트 컴프리헨션

```python title=list_comprehension.py
# 기본 for문으로 제곱수 리스트 만들기
squares_traditional = []
for i in range(1, 6):
    squares_traditional.append(i ** 2)
print(f"전통적인 방법: {squares_traditional}")

# 리스트 컴프리헨션으로 같은 결과
squares_comprehension = [i ** 2 for i in range(1, 6)]
print(f"리스트 컴프리헨션: {squares_comprehension}")

# 문자열 리스트 처리
fruits = ['apple', 'banana', 'cherry', 'date']
upper_fruits = [fruit.upper() for fruit in fruits]
print(f"대문자 변환: {upper_fruits}")

# 문자열 길이 리스트
fruit_lengths = [len(fruit) for fruit in fruits]
print(f"문자열 길이: {fruit_lengths}")
```

### 조건이 있는 리스트 컴프리헨션

```python title=conditional_comprehension.py
# 짝수만 선택하기
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 전통적인 방법
even_traditional = []
for num in numbers:
    if num % 2 == 0:
        even_traditional.append(num)
print(f"전통적인 방법 (짝수): {even_traditional}")

# 리스트 컴프리헨션
even_comprehension = [num for num in numbers if num % 2 == 0]
print(f"리스트 컴프리헨션 (짝수): {even_comprehension}")

# 조건에 따라 다른 값 할당
# 짝수는 제곱, 홀수는 원래 값
modified_numbers = [num ** 2 if num % 2 == 0 else num for num in range(1, 11)]
print(f"조건부 변환: {modified_numbers}")

# 문자열 필터링
words = ['python', 'java', 'javascript', 'go', 'rust']
long_words = [word for word in words if len(word) > 4]
print(f"5글자 이상 단어: {long_words}")
```

---

## 실습: 구구단 출력 프로그램

학습한 반복문 개념들을 종합하여 사용자가 원하는 구구단을 출력하는 프로그램을 만들어보겠습니다. 이 프로그램은 사용자 입력 처리, 반복문, 조건문을 모두 활용하는 실용적인 예제입니다.

```python title=multiplication_table.py
def print_single_table(dan):
    """단일 구구단을 출력하는 함수"""
    print(f"\n=== {dan}단 ===")
    for i in range(1, 10):
        result = dan * i
        print(f"{dan} × {i} = {result:2d}")

def print_multiple_tables(start_dan, end_dan):
    """여러 구구단을 출력하는 함수"""
    print(f"\n=== {start_dan}단 ~ {end_dan}단 ===")

    # 각 줄에 여러 단을 나란히 출력
    for i in range(1, 10):
        line = ""
        for dan in range(start_dan, end_dan + 1):
            result = dan * i
            line += f"{dan} × {i} = {result:2d}    "
        print(line)

def main():
    """메인 프로그램"""
    print("구구단 출력 프로그램")
    print("-" * 30)

    while True:
        print("\n메뉴를 선택하세요:")
        print("1. 특정 단 출력")
        print("2. 범위 단 출력")
        print("3. 전체 구구단 출력")
        print("4. 종료")

        choice = input("선택 (1-4): ").strip()

        if choice == '1':
            try:
                dan = int(input("출력할 단을 입력하세요 (2-9): "))
                if 2 <= dan <= 9:
                    print_single_table(dan)
                else:
                    print("2부터 9까지의 숫자를 입력해주세요.")
            except ValueError:
                print("올바른 숫자를 입력해주세요.")

        elif choice == '2':
            try:
                start = int(input("시작 단을 입력하세요 (2-9): "))
                end = int(input("끝 단을 입력하세요 (2-9): "))

                if 2 <= start <= 9 and 2 <= end <= 9 and start <= end:
                    print_multiple_tables(start, end)
                else:
                    print("올바른 범위를 입력해주세요. (시작 ≤ 끝, 2-9 범위)")
            except ValueError:
                print("올바른 숫자를 입력해주세요.")

        elif choice == '3':
            print_multiple_tables(2, 9)

        elif choice == '4':
            print("프로그램을 종료합니다.")
            break

        else:
            print("1부터 4까지의 숫자를 입력해주세요.")

# 프로그램 실행
if __name__ == "__main__":
    main()
```

---

## 확장 과제: 패턴 출력 프로그램

다양한 패턴을 출력하는 프로그램을 만들어보겠습니다. 중첩 반복문과 조건문을 활용한 창의적인 패턴들을 구현해보세요.

```python title=pattern_generator.py
def print_triangle_pattern():
    """삼각형 패턴들"""
    print("1. 직각삼각형 (별표)")
    for i in range(1, 6):
        print("*" * i)

    print("\n2. 직각삼각형 (숫자)")
    for i in range(1, 6):
        for j in range(1, i + 1):
            print(j, end="")
        print()

    print("\n3. 역삼각형")
    for i in range(5, 0, -1):
        print("*" * i)

def print_pyramid_patterns():
    """피라미드 패턴들"""
    height = 5

    print("1. 별표 피라미드")
    for i in range(1, height + 1):
        # 공백 출력
        print(" " * (height - i), end="")
        # 별표 출력
        print("*" * (2 * i - 1))

    print("\n2. 숫자 피라미드")
    for i in range(1, height + 1):
        # 공백 출력
        print(" " * (height - i), end="")
        # 증가하는 숫자
        for j in range(1, i + 1):
            print(j, end="")
        # 감소하는 숫자
        for j in range(i - 1, 0, -1):
            print(j, end="")
        print()

def print_diamond_pattern():
    """다이아몬드 패턴"""
    print("다이아몬드 패턴")
    size = 5

    # 상단 부분 (피라미드)
    for i in range(1, size + 1):
        print(" " * (size - i) + "*" * (2 * i - 1))

    # 하단 부분 (역피라미드)
    for i in range(size - 1, 0, -1):
        print(" " * (size - i) + "*" * (2 * i - 1))

def print_checkerboard():
    """체스판 패턴"""
    print("체스판 패턴 (8x8)")
    for i in range(8):
        for j in range(8):
            if (i + j) % 2 == 0:
                print("■", end=" ")
            else:
                print("□", end=" ")
        print()

def main():
    """패턴 출력 메인 프로그램"""
    patterns = {
        '1': ('삼각형 패턴', print_triangle_pattern),
        '2': ('피라미드 패턴', print_pyramid_patterns),
        '3': ('다이아몬드 패턴', print_diamond_pattern),
        '4': ('체스판 패턴', print_checkerboard)
    }

    while True:
        print("\n" + "="*40)
        print("패턴 출력 프로그램")
        print("="*40)

        for key, (name, _) in patterns.items():
            print(f"{key}. {name}")
        print("5. 종료")

        choice = input("\n원하는 패턴을 선택하세요 (1-5): ").strip()

        if choice in patterns:
            name, func = patterns[choice]
            print(f"\n{name}")
            print("-" * 20)
            func()
        elif choice == '5':
            print("프로그램을 종료합니다.")
            break
        else:
            print("올바른 선택지를 입력해주세요.")

if __name__ == "__main__":
    main()
```

---

## 연습문제

### 기본 문제

1. **합계 계산기**: 1부터 100까지의 홀수의 합을 구하는 프로그램을 작성하세요.

2. **팩토리얼 계산**: 사용자가 입력한 숫자의 팩토리얼을 계산하는 프로그램을 작성하세요.

3. **별표 계단**: 다음과 같은 패턴을 출력하는 프로그램을 작성하세요.
   ```
   *
   **
   ***
   ****
   *****
   ```

### 중급 문제

4. **소수 찾기**: 2부터 50까지의 숫자 중에서 소수를 찾아 출력하는 프로그램을 작성하세요.

5. **리스트 컴프리헨션 연습**: 1부터 20까지의 숫자 중에서 3의 배수만 선택하여 각각을 제곱한 결과를 리스트로 만드세요.

6. **단어 분석**: 문자열 리스트에서 특정 문자로 시작하는 단어들만 선택하여 길이순으로 정렬하는 프로그램을 작성하세요.

### 도전 문제

7. **숫자 피라미드**: 다음과 같은 숫자 피라미드를 출력하는 프로그램을 작성하세요.

   ```
       1
      121
     12321
    1234321
   123454321
   ```

8. **로또 번호 생성기**: 1부터 45까지의 숫자 중에서 중복되지 않는 6개의 숫자를 선택하는 프로그램을 작성하세요. (random 모듈 사용)

이번 장에서는 파이썬의 반복문을 완전히 마스터했습니다. for문과 while문의 차이점을 이해하고, 각각을 언제 사용해야 하는지 알게 되었으며, break와 continue로 반복의 흐름을 제어하는 방법도 배웠습니다. 특히 리스트 컴프리헨션은 파이썬다운 코드를 작성하는 데 매우 중요한 기능이니 꼭 연습해보세요. 다음 장에서는 이러한 반복문과 조건문을 활용하여 더 복잡한 프로그램을 만들어보겠습니다.
