---
title: '6. 조건문으로 선택하기'
slug: python-conditional-statements
description: 'if, elif, else문과 논리 연산자를 활용한 조건문 완전 마스터. 실생활 문제 해결 예제 포함'
keywords: [파이썬 조건문, if문, elif문, else문, 논리 연산자, and or not, 조건 판단]
sidebar_position: 6
---

# 6. 조건문으로 선택하기

## 학습 목표

이번 챕터를 완료하면 다음을 할 수 있게 됩니다:

- if문의 기본 구조를 이해하고 활용할 수 있습니다
- 복잡한 조건을 논리 연산자로 표현할 수 있습니다
- 중첩 조건문으로 복잡한 판단 로직을 구현할 수 있습니다
- 실생활 문제를 조건문으로 해결할 수 있습니다

---

## 6-1 if문 기본 구조

프로그래밍에서 조건문은 특정 조건이 참일 때만 코드를 실행하게 해줍니다. 파이썬의 if문은 매우 직관적인 구조를 가지고 있어요.

### 기본 문법

```python title=basic_if.py
# 기본 if문 구조
if 조건:
    실행할 코드
```

조건이 참(True)이면 들여쓰기된 코드 블록이 실행됩니다. 파이썬에서는 들여쓰기가 매우 중요하다는 점을 기억하세요!

### 간단한 예제

```python title=simple_if_example.py
age = 20

if age >= 18:
    print("성인입니다.")
    print("투표할 수 있습니다.")

print("프로그램이 끝났습니다.")
```

위 코드에서 age가 18 이상이므로 조건이 참이 되어 들여쓰기된 두 줄이 모두 실행됩니다.

---

## 6-2 if-else문

if문만으로는 조건이 거짓일 때 할 일을 지정할 수 없습니다. else문을 사용하면 조건이 거짓일 때 실행할 코드를 지정할 수 있어요.

### 기본 문법

```python title=if_else_syntax.py
if 조건:
    조건이 참일 때 실행할 코드
else:
    조건이 거짓일 때 실행할 코드
```

### 실용적인 예제

나이에 따른 입장료를 계산하는 프로그램을 만들어봅시다. 영화관에서는 보통 성인과 청소년의 요금이 다르죠.

```python title=movie_ticket.py
age = int(input("나이를 입력하세요: "))

if age >= 19:
    price = 12000
    print(f"성인 요금: {price}원")
else:
    price = 8000
    print(f"청소년 요금: {price}원")

print(f"입장료는 {price}원입니다.")
```

---

## 6-3 if-elif-else문

여러 개의 조건을 순차적으로 확인해야 할 때는 elif(else if의 줄임말)를 사용합니다.

### 기본 문법

```python title=if_elif_else_syntax.py
if 첫_번째_조건:
    첫_번째_조건이_참일_때_실행
elif 두_번째_조건:
    두_번째_조건이_참일_때_실행
elif 세_번째_조건:
    세_번째_조건이_참일_때_실행
else:
    모든_조건이_거짓일_때_실행
```

### 성적 등급 시스템 예제

학교에서 자주 사용하는 성적 등급을 매기는 프로그램을 만들어보겠습니다.

```python title=grade_system.py
score = int(input("점수를 입력하세요 (0-100): "))

if score >= 90:
    grade = "A"
    comment = "우수합니다!"
elif score >= 80:
    grade = "B"
    comment = "잘했습니다!"
elif score >= 70:
    grade = "C"
    comment = "보통입니다."
elif score >= 60:
    grade = "D"
    comment = "조금 더 노력하세요."
else:
    grade = "F"
    comment = "재시험이 필요합니다."

print(f"점수: {score}점")
print(f"등급: {grade}")
print(f"평가: {comment}")
```

---

## 6-4 중첩 조건문

조건문 안에 또 다른 조건문을 넣을 수 있습니다. 이를 중첩 조건문이라고 하며, 복잡한 조건을 단계별로 판단할 때 유용합니다.

### 기본 구조

```python title=nested_if_syntax.py
if 외부_조건:
    if 내부_조건:
        내부_조건도_참일_때_실행
    else:
        내부_조건이_거짓일_때_실행
else:
    외부_조건이_거짓일_때_실행
```

### 놀이공원 탑승 제한 예제

놀이공원의 탑승 제한을 확인하는 프로그램입니다. 나이와 키를 모두 고려해야 합니다.

```python title=ride_restriction.py
age = int(input("나이를 입력하세요: "))
height = int(input("키를 입력하세요 (cm): "))

if age >= 12:
    if height >= 120:
        print("탑승 가능합니다!")
        print("안전한 여행 되세요!")
    else:
        print("키가 부족합니다.")
        print("키 120cm 이상이어야 합니다.")
else:
    print("나이가 부족합니다.")
    print("12세 이상만 탑승 가능합니다.")
```

---

## 6-5 논리 연산자 (and, or, not)

복잡한 조건을 표현할 때는 논리 연산자를 사용합니다. 여러 조건을 하나로 결합하거나 조건을 반대로 만들 수 있어요.

### 논리 연산자 종류

- `and`: 모든 조건이 참일 때 참
- `or`: 하나 이상의 조건이 참일 때 참
- `not`: 조건을 반대로 바꿈

```python title=logical_operators.py
# and 연산자 예제
age = 25
has_license = True

if age >= 18 and has_license:
    print("운전할 수 있습니다.")

# or 연산자 예제
weather = "비"
if weather == "비" or weather == "눈":
    print("우산을 가져가세요.")

# not 연산자 예제
is_weekend = False
if not is_weekend:
    print("평일입니다. 일어나세요!")
```

### 장학금 지급 조건 확인

대학교에서 장학금 지급 조건을 확인하는 프로그램을 만들어봅시다.

```python title=scholarship_check.py
gpa = float(input("학점을 입력하세요 (4.0 만점): "))
attendance = int(input("출석률을 입력하세요 (%): "))
volunteer_hours = int(input("봉사시간을 입력하세요 (시간): "))

# 성적 우수 장학금: 학점 3.5 이상 AND 출석률 90% 이상
if gpa >= 3.5 and attendance >= 90:
    print("성적 우수 장학금 대상입니다!")

# 봉사 장학금: 봉사시간 50시간 이상 OR 학점 3.0 이상
elif volunteer_hours >= 50 or gpa >= 3.0:
    print("봉사 장학금 대상입니다!")

# 기본 지원금: 출석률 80% 이상이고 학점이 2.0 미만이 아님
elif attendance >= 80 and not gpa < 2.0:
    print("기본 지원금 대상입니다.")

else:
    print("장학금 대상이 아닙니다.")
```

---

## 6-6 비교 연산자

조건문에서 값을 비교할 때 사용하는 연산자들을 알아봅시다.

### 비교 연산자 종류

```python title=comparison_operators.py
# 비교 연산자들
a = 10
b = 5

print(f"a == b: {a == b}")  # 같음 (False)
print(f"a != b: {a != b}")  # 다름 (True)
print(f"a > b: {a > b}")    # 큰지 (True)
print(f"a < b: {a < b}")    # 작은지 (False)
print(f"a >= b: {a >= b}")  # 크거나 같은지 (True)
print(f"a <= b: {a <= b}")  # 작거나 같은지 (False)

# 문자열 비교도 가능
name1 = "김철수"
name2 = "이영희"
print(f"name1 == name2: {name1 == name2}")  # False
```

### 비밀번호 검증 시스템

사용자의 비밀번호가 안전한지 확인하는 프로그램입니다.

```python title=password_validator.py
password = input("비밀번호를 입력하세요: ")
confirm_password = input("비밀번호를 다시 입력하세요: ")

# 비밀번호 일치 확인
if password != confirm_password:
    print("비밀번호가 일치하지 않습니다.")
else:
    # 비밀번호 강도 확인
    length = len(password)
    has_number = any(char.isdigit() for char in password)
    has_upper = any(char.isupper() for char in password)

    if length >= 8 and has_number and has_upper:
        print("강한 비밀번호입니다!")
    elif length >= 6:
        print("보통 비밀번호입니다.")
    else:
        print("약한 비밀번호입니다. 8자 이상 권장합니다.")
```

---

## 실습: 성적 등급 판정 프로그램

지금까지 배운 내용을 종합하여 종합적인 성적 관리 프로그램을 만들어보겠습니다. 이 프로그램은 여러 과목의 점수를 입력받아 평균을 계산하고 등급을 매기는 실용적인 예제입니다.

```python title=comprehensive_grade_system.py
print("=== 종합 성적 관리 시스템 ===")
print()

# 학생 정보 입력
student_name = input("학생 이름을 입력하세요: ")
student_id = input("학번을 입력하세요: ")

print(f"\n{student_name}님의 성적을 입력해주세요.")

# 과목별 점수 입력
korean = int(input("국어 점수 (0-100): "))
english = int(input("영어 점수 (0-100): "))
math = int(input("수학 점수 (0-100): "))
science = int(input("과학 점수 (0-100): "))

# 평균 계산
total = korean + english + math + science
average = total / 4

# 등급 판정
if average >= 90:
    grade = "A"
    comment = "우수"
elif average >= 80:
    grade = "B"
    comment = "양호"
elif average >= 70:
    grade = "C"
    comment = "보통"
elif average >= 60:
    grade = "D"
    comment = "미흡"
else:
    grade = "F"
    comment = "재평가 필요"

# 특별 판정 (과목별 낙제 확인)
has_failing = korean < 60 or english < 60 or math < 60 or science < 60

# 결과 출력
print("\n" + "="*40)
print("📊 성적표")
print("="*40)
print(f"학생명: {student_name} ({student_id})")
print(f"국어: {korean}점")
print(f"영어: {english}점")
print(f"수학: {math}점")
print(f"과학: {science}점")
print("-"*40)
print(f"총점: {total}점")
print(f"평균: {average:.1f}점")
print(f"등급: {grade}등급 ({comment})")

# 특별 메시지
if has_failing:
    print("⚠️  낙제 과목이 있습니다. 보충학습이 필요합니다.")
elif average >= 95:
    print("🏆 최우수상 후보입니다!")
elif average >= 85:
    print("⭐ 우수상 후보입니다!")

print("="*40)
```

---

## 확장 과제: 다중 조건 점수 분석기

더 복잡한 조건들을 다루는 확장 과제입니다. 여러 학생의 점수를 비교하고 순위를 매기는 프로그램을 만들어보세요.

```python title=multi_student_analyzer.py
print("=== 반 성적 분석기 ===")

# 학생 수 입력
num_students = int(input("학생 수를 입력하세요: "))
students = []

# 학생 정보 입력
for i in range(num_students):
    print(f"\n{i+1}번째 학생 정보:")
    name = input("이름: ")
    score = int(input("점수 (0-100): "))
    students.append({"name": name, "score": score})

# 분석 결과
print("\n" + "="*50)
print("📈 반 성적 분석 결과")
print("="*50)

# 평균 계산
total_score = sum(student["score"] for student in students)
class_average = total_score / num_students

print(f"반 평균: {class_average:.1f}점")

# 최고점, 최저점 찾기
highest = max(students, key=lambda x: x["score"])
lowest = min(students, key=lambda x: x["score"])

print(f"최고점: {highest['name']} ({highest['score']}점)")
print(f"최저점: {lowest['name']} ({lowest['score']}점)")

# 각 학생별 분석
print("\n📋 개인별 분석:")
for student in students:
    name = student["name"]
    score = student["score"]

    # 반 평균 대비 분석
    if score > class_average + 10:
        performance = "우수 (반 평균 초과)"
    elif score > class_average:
        performance = "양호 (반 평균 이상)"
    elif score > class_average - 10:
        performance = "보통 (반 평균 근처)"
    else:
        performance = "미흡 (반 평균 미달)"

    print(f"• {name}: {score}점 - {performance}")
```

---

## 연습문제

### 문제 1: 계절 판정기

월(1-12)을 입력받아서 해당하는 계절을 출력하는 프로그램을 작성하세요.

- 봄: 3, 4, 5월
- 여름: 6, 7, 8월
- 가을: 9, 10, 11월
- 겨울: 12, 1, 2월

### 문제 2: 택시 요금 계산기

거리와 시간대를 입력받아 택시 요금을 계산하는 프로그램을 작성하세요.

- 기본 요금: 3000원 (2km까지)
- 추가 거리: km당 1000원
- 심야 할증 (22시-06시): 20% 추가

### 문제 3: 윤년 판정기

연도를 입력받아서 윤년인지 평년인지 판정하는 프로그램을 작성하세요.
윤년 조건:

- 4로 나누어떨어지는 해는 윤년
- 단, 100으로 나누어떨어지는 해는 평년
- 단, 400으로 나누어떨어지는 해는 윤년

### 문제 4: BMI 계산 및 판정

키와 몸무게를 입력받아 BMI를 계산하고 건강 상태를 판정하는 프로그램을 작성하세요.

- BMI = 몸무게(kg) / (키(m))²
- 저체중: 18.5 미만
- 정상: 18.5-24.9
- 과체중: 25-29.9
- 비만: 30 이상

조건문은 프로그래밍의 핵심 중 하나입니다. 단순해 보이지만 복잡한 논리를 구현할 수 있는 강력한 도구예요. 다음 챕터에서는 같은 작업을 반복하는 반복문에 대해 배워보겠습니다!
