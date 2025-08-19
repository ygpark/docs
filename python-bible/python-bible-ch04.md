---
title: '4. 문자열 기본 완전'
slug: python-string-complete-guide
description: '문자열 인덱싱, 슬라이싱, 메서드, 포맷팅까지 파이썬 문자열 처리 기법을 체계적으로 학습'
keywords: [파이썬 문자열, 문자열 슬라이싱, 문자열 메서드, 문자열 포맷팅, f-string, 문자열 처리]
sidebar_position: 4
---

# 4. 문자열

문자열은 프로그래밍에서 가장 자주 다루는 데이터 타입 중 하나입니다. 사용자의 이름, 이메일 주소, 메시지 등 우리가 일상에서 접하는 대부분의 정보가 문자열 형태로 처리됩니다. 파이썬은 문자열을 다루기 위한 강력하고 직관적인 기능들을 제공하는데, 이번 챕터에서는 문자열의 핵심 기능들을 체계적으로 배워보겠습니다. 문자열을 자유자재로 다룰 수 있게 되면, 데이터 처리와 텍스트 분석 프로그램을 만드는 기초 실력을 쌓을 수 있습니다.

## 학습 목표

이번 챕터를 완료하면 다음과 같은 능력을 갖추게 됩니다:

- 문자열의 특정 부분을 추출하고 조작할 수 있습니다
- 다양한 문자열 메서드를 활용해 실무에서 필요한 문자열 처리를 할 수 있습니다
- 현대적인 문자열 포맷팅 방법을 사용해 동적인 문자열을 생성할 수 있습니다
- 실제 텍스트 데이터를 분석하는 간단한 프로그램을 만들 수 있습니다

---

## 4-1 문자열 인덱싱과 슬라이싱

문자열은 문자들이 순서대로 나열된 시퀀스입니다. 파이썬에서는 각 문자에 번호(인덱스)를 매겨서 특정 문자나 문자 그룹에 접근할 수 있습니다. 이는 마치 책의 페이지 번호처럼 작동하여, 원하는 부분을 정확히 찾아낼 수 있게 해줍니다.

### 인덱싱 기초

```python title=indexing_basic.py
# 문자열 인덱싱 기본
text = "Python"
print(f"첫 번째 문자: {text[0]}")    # P
print(f"두 번째 문자: {text[1]}")    # y
print(f"마지막 문자: {text[-1]}")    # n
print(f"뒤에서 두 번째: {text[-2]}")  # o

# 인덱스 범위 확인
print(f"문자열 길이: {len(text)}")   # 6
# print(text[6])  # IndexError 발생!
```

### 슬라이싱으로 부분 문자열 추출

```python title=slicing_basic.py
# 슬라이싱 문법: [시작:끝:간격]
sentence = "Hello, Python World!"

print(f"처음 5글자: {sentence[:5]}")        # Hello
print(f"7번째부터 끝까지: {sentence[7:]}")   # Python World!
print(f"7~12번째: {sentence[7:13]}")        # Python
print(f"2칸씩 건너뛰기: {sentence[::2]}")    # Hlo yhnWrd

# 문자열 뒤집기
print(f"거꾸로: {sentence[::-1]}")           # !dlroW nohtyP ,olleH
```

---

## 4-2 문자열 메서드 활용

파이썬 문자열은 다양한 내장 메서드를 제공합니다. 이 메서드들을 잘 활용하면 복잡한 문자열 처리 작업도 간단하게 해결할 수 있습니다. 각 메서드는 특정 목적에 특화되어 있어, 상황에 맞는 메서드를 선택하는 것이 중요합니다.

### 검색, 분할, 결합 메서드

```python title=string_methods.py
# 검색 메서드들
email = "user@example.com"
print(f"@ 위치: {email.find('@')}")         # 4
print(f"@ 포함 여부: {'@' in email}")        # True
print(f".com으로 끝나는가: {email.endswith('.com')}")  # True

# 문자열 분할과 결합
csv_data = "사과,바나나,오렌지,포도"
fruits = csv_data.split(',')
print(f"과일 리스트: {fruits}")              # ['사과', '바나나', '오렌지', '포도']

# 리스트를 다시 문자열로 결합
result = ' | '.join(fruits)
print(f"결합 결과: {result}")                # 사과 | 바나나 | 오렌지 | 포도

# 공백 제거
messy_text = "  여백이 많은 텍스트  "
print(f"양쪽 공백 제거: '{messy_text.strip()}'")     # '여백이 많은 텍스트'
print(f"왼쪽 공백만: '{messy_text.lstrip()}'")       # '여백이 많은 텍스트  '
```

### 대소문자 변환 메서드

```python title=case_methods.py
# 대소문자 변환
text = "Hello Python World"
print(f"소문자: {text.lower()}")            # hello python world
print(f"대문자: {text.upper()}")            # HELLO PYTHON WORLD
print(f"첫글자 대문자: {text.capitalize()}")  # Hello python world
print(f"제목 형식: {text.title()}")         # Hello Python World

# 대소문자 뒤바꾸기
mixed = "PyThOn"
print(f"대소문자 반전: {mixed.swapcase()}")  # pYtHoN
```

### 문자열 검증 메서드

```python title=validation_methods.py
# 문자열 내용 검증
def validate_input(text):
    """입력된 텍스트의 특성을 분석합니다."""
    print(f"검사 대상: '{text}'")
    print(f"  숫자로만 구성: {text.isdigit()}")
    print(f"  알파벳으로만 구성: {text.isalpha()}")
    print(f"  알파벳+숫자 구성: {text.isalnum()}")
    print(f"  공백 포함 여부: {text.isspace()}")
    print("-" * 30)

# 다양한 입력 테스트
validate_input("12345")
validate_input("Hello")
validate_input("Hello123")
validate_input("   ")
```

---

## 4-3 문자열 포맷팅

동적으로 변하는 값을 문자열에 삽입하는 것은 프로그래밍에서 매우 중요한 작업입니다. 파이썬은 여러 가지 문자열 포맷팅 방법을 제공하며, 각각의 장단점이 있습니다. 현재는 f-string이 가장 권장되는 방법이지만, 기존 코드를 이해하기 위해 다른 방법들도 알아두는 것이 좋습니다.

### % 포맷팅 (레거시 방법)

```python title=percent_formatting.py
# % 포맷팅 (오래된 방법)
name = "김파이"
age = 25
score = 85.75

# 기본 사용법
print("이름: %s, 나이: %d세" % (name, age))
print("점수: %.1f점" % score)

# 순서 지정
print("%(name)s님의 점수는 %(score).2f점입니다." % {
    'name': name,
    'score': score
})
```

### .format() 메서드

```python title=format_method.py
# .format() 메서드
name = "이파이"
age = 30
salary = 3500000

# 위치 인수
print("{}님은 {}세이고, 연봉은 {}원입니다.".format(name, age, salary))

# 인덱스 지정
print("{0}님 안녕하세요! {0}님의 나이는 {1}세입니다.".format(name, age))

# 키워드 인수
print("{name}님의 월급은 {monthly:,}원입니다.".format(
    name=name,
    monthly=salary//12
))
```

### f-string (현대적 방법)

```python title=f_string.py
# f-string (Python 3.6+, 권장 방법)
name = "박파이"
age = 28
height = 175.5

# 기본 사용법
print(f"{name}님은 {age}세이고, 키는 {height}cm입니다.")

# 표현식 사용
print(f"내년에는 {age + 1}세가 됩니다.")
print(f"키를 미터로 표현하면 {height/100:.2f}m입니다.")

# 포맷 지정
price = 15000
print(f"가격: {price:,}원")           # 천단위 구분자
print(f"할인가: {price*0.8:.0f}원")    # 소수점 제거

# 날짜 포맷팅
from datetime import datetime
now = datetime.now()
print(f"현재 시간: {now:%Y년 %m월 %d일 %H시 %M분}")
```

---

## 실습: 텍스트 분석 프로그램

실제 텍스트 데이터를 분석하는 프로그램을 만들어 보겠습니다. 이 프로그램은 사용자가 입력한 텍스트의 다양한 통계 정보를 제공하며, 지금까지 배운 문자열 메서드들을 종합적으로 활용합니다.

```python title=text_analyzer.py
def analyze_text(text):
    """
    텍스트를 분석하여 다양한 통계 정보를 반환합니다.
    사용자가 입력한 글의 특성을 파악하는 데 유용합니다.
    """
    if not text.strip():
        return "분석할 텍스트가 없습니다."

    # 기본 통계
    total_chars = len(text)
    chars_no_space = len(text.replace(' ', ''))
    words = text.split()
    word_count = len(words)

    # 문장 개수 (간단한 추정)
    sentences = [s.strip() for s in text.replace('!', '.').replace('?', '.').split('.') if s.strip()]
    sentence_count = len(sentences)

    # 대소문자 분석
    upper_count = sum(1 for c in text if c.isupper())
    lower_count = sum(1 for c in text if c.islower())
    digit_count = sum(1 for c in text if c.isdigit())

    # 가장 긴 단어 찾기
    longest_word = max(words, key=len) if words else ""

    # 단어 빈도 분석 (간단한 버전)
    word_freq = {}
    for word in words:
        clean_word = word.lower().strip('.,!?')
        word_freq[clean_word] = word_freq.get(clean_word, 0) + 1

    # 가장 많이 사용된 단어
    most_common = max(word_freq.items(), key=lambda x: x[1]) if word_freq else ("", 0)

    # 결과 출력
    print("=" * 50)
    print("📊 텍스트 분석 결과")
    print("=" * 50)
    print(f"전체 문자 수: {total_chars:,}개")
    print(f"공백 제외 문자 수: {chars_no_space:,}개")
    print(f"단어 수: {word_count:,}개")
    print(f"문장 수: {sentence_count}개")
    print(f"평균 단어 길이: {chars_no_space/word_count:.1f}자" if word_count > 0 else "평균 단어 길이: 0자")
    print("-" * 50)
    print(f"대문자: {upper_count}개")
    print(f"소문자: {lower_count}개")
    print(f"숫자: {digit_count}개")
    print("-" * 50)
    print(f"가장 긴 단어: '{longest_word}' ({len(longest_word)}자)")
    print(f"가장 자주 사용된 단어: '{most_common[0]}' ({most_common[1]}회)")

    # 텍스트 특성 분석
    print("-" * 50)
    if upper_count > lower_count:
        print("🔍 특징: 대문자 사용이 많은 텍스트입니다.")
    elif digit_count > total_chars * 0.1:
        print("🔍 특징: 숫자가 많이 포함된 텍스트입니다.")
    elif word_count > 0 and chars_no_space / word_count > 6:
        print("🔍 특징: 긴 단어들이 많은 전문적인 텍스트입니다.")
    else:
        print("🔍 특징: 일반적인 형태의 텍스트입니다.")


def main():
    """메인 프로그램 실행"""
    print("🔤 텍스트 분석 프로그램")
    print("분석하고 싶은 텍스트를 입력하세요 (종료: 'quit')")
    print("-" * 50)

    while True:
        user_input = input("\n텍스트 입력: ").strip()

        if user_input.lower() == 'quit':
            print("프로그램을 종료합니다. 안녕히 가세요!")
            break

        if user_input:
            analyze_text(user_input)
        else:
            print("텍스트를 입력해주세요.")


# 프로그램 실행
if __name__ == "__main__":
    main()
```

---

## 연습문제

### 기본 문제

1. 문자열 "Hello, World!"에서 "World" 부분만 추출하는 코드를 작성하세요.
2. 사용자로부터 이메일 주소를 입력받아 "@" 기호 앞의 사용자명을 추출하는 프로그램을 작성하세요.
3. 문자열 "Python Programming"을 모든 단어의 첫 글자가 대문자가 되도록 변환하세요.

### 응용 문제

1. 사용자가 입력한 문장에서 가장 긴 단어와 가장 짧은 단어를 찾는 프로그램을 작성하세요.
2. 문자열에서 모든 공백을 제거하고, 대문자와 소문자의 개수를 각각 세는 함수를 만드세요.
3. 주민등록번호 형식(예: 123456-1234567)이 올바른지 검증하는 함수를 작성하세요. (숫자 6자리-숫자 7자리)

### 도전 문제

1. 문자열에서 가장 많이 등장하는 문자를 찾고, 그 빈도를 출력하는 프로그램을 작성하세요.
2. 사용자가 입력한 문장을 단어별로 분리하여, 각 단어를 역순으로 뒤집은 후 다시 연결하는 프로그램을 만드세요. (예: "Hello World" → "olleH dlroW")

---

## 다음 챕터 예고

다음 챕터에서는 파이썬의 강력한 컬렉션 자료형들을 배워보겠습니다. 리스트, 튜플, 딕셔너리, 집합 등을 활용하여 여러 개의 데이터를 효율적으로 관리하고 처리하는 방법을 익혀보세요. 문자열과 컬렉션을 함께 활용하면 더욱 복잡하고 실용적인 프로그램을 만들 수 있습니다!
