---
title: '1. 파이썬 시작하기'
slug: python-installation-setup-guide
description: '파이썬 설치부터 첫 프로그램 실행까지. 개발 환경 구축과 기본 사용법을 배우는 파이썬 입문 첫 단계'
keywords:
  [
    파이썬 설치,
    Python 설치,
    파이썬 개발환경,
    VS Code,
    PyCharm,
    파이썬 IDE,
    Hello World,
    파이썬 첫 프로그램,
  ]
sidebar_position: 1
---

# 1. 파이썬 시작하기

**학습 목표**
이 챕터를 마치면 파이썬이 무엇인지 이해하고, 자신의 컴퓨터에 파이썬을 설치하여 첫 번째 프로그램을 실행할 수 있습니다. 또한 파이썬으로 할 수 있는 다양한 일들을 알아보고, 개발 환경을 구축하여 본격적인 프로그래밍 학습을 시작할 준비를 완료하게 됩니다.

---

## 1-1 파이썬이란?

프로그래밍 언어 중에서도 파이썬은 특별합니다. 1991년 네덜란드의 귀도 반 로썸이 만든 파이썬은 단순하면서도 강력한 언어로, 전 세계 개발자들이 가장 사랑하는 프로그래밍 언어 중 하나가 되었습니다. 파이썬이라는 이름도 재미있는데, 뱀과는 전혀 관련이 없고 영국의 코미디 그룹 '몬티 파이썬'에서 따온 것입니다.

파이썬은 **고급 프로그래밍 언어**입니다. 이는 인간이 이해하기 쉬운 문법을 가지고 있다는 뜻입니다. 다른 프로그래밍 언어들과 비교해보면 파이썬의 특별함을 더 잘 알 수 있습니다.

```python title=hello_comparison.py
# 파이썬으로 "Hello, World!" 출력하기
print("Hello, World!")

# 다른 언어들과 비교해보세요
# Java의 경우:
# public class HelloWorld {
#     public static void main(String[] args) {
#         System.out.println("Hello, World!");
#     }
# }

# C의 경우:
# #include <stdio.h>
# int main() {
#     printf("Hello, World!\n");
#     return 0;
# }
```

위 예제에서 보듯이 파이썬은 단 한 줄로 같은 작업을 할 수 있습니다. 이것이 바로 파이썬의 매력입니다.

---

## 1-2 파이썬의 특징과 장점

파이썬을 배우기 전에 파이썬만의 특별한 특징들을 알아보겠습니다. 이 특징들을 이해하면 왜 파이썬이 초보자에게 최적의 프로그래밍 언어인지 납득할 수 있을 것입니다.

### 간결하고 읽기 쉬운 문법

파이썬의 가장 큰 장점은 **가독성**입니다. 파이썬 코드는 마치 영어 문장을 읽는 것처럼 자연스럽습니다.

```python title=readable_syntax.py
# 파이썬의 직관적인 문법 예시
age = 25
name = "김철수"

if age >= 18:
    print(f"{name}님은 성인입니다.")
else:
    print(f"{name}님은 미성년자입니다.")

# 리스트의 모든 요소에 2를 곱하기
numbers = [1, 2, 3, 4, 5]
doubled = [num * 2 for num in numbers]
print(doubled)  # [2, 4, 6, 8, 10]
```

### 들여쓰기로 코드 구조 표현

파이썬은 중괄호({}) 대신 **들여쓰기**를 사용해서 코드의 구조를 나타냅니다. 이는 코드를 더 깔끔하고 일관성 있게 만들어줍니다.

### 동적 타입 시스템

변수를 선언할 때 타입을 미리 정해주지 않아도 됩니다. 파이썬이 자동으로 판단해줍니다.

```python title=dynamic_typing.py
# 타입을 명시하지 않아도 됩니다
message = "안녕하세요"    # 문자열
count = 42              # 정수
price = 3.14            # 실수
is_active = True        # 불린

print(type(message))    # <class 'str'>
print(type(count))      # <class 'int'>
print(type(price))      # <class 'float'>
print(type(is_active))  # <class 'bool'>
```

### 풍부한 라이브러리

파이썬은 "배터리 포함(batteries included)" 철학을 가지고 있습니다. 즉, 웬만한 기능은 표준 라이브러리에 이미 포함되어 있다는 뜻입니다.

---

## 1-3 파이썬으로 할 수 있는 일들

파이썬의 활용 범위는 정말 넓습니다. 현재 가장 인기 있는 분야들을 살펴보겠습니다. 파이썬을 배우면 이 모든 분야에 도전할 수 있는 기초를 다지게 됩니다.

### 웹 개발

- **Django, Flask** 같은 프레임워크로 웹사이트 구축
- 인스타그램, 유튜브 같은 대형 서비스도 파이썬으로 개발

### 데이터 분석 및 과학

- **pandas, NumPy** 라이브러리로 데이터 분석
- **matplotlib, seaborn**으로 데이터 시각화
- 연구소, 기업에서 데이터 분석에 필수 도구

### 인공지능 및 머신러닝

- **TensorFlow, PyTorch** 등으로 AI 모델 개발
- 현재 AI 붐의 중심에 파이썬이 있습니다

### 자동화 및 업무 효율화

- 반복적인 업무를 자동화하는 스크립트 작성
- 파일 정리, 이메일 발송, 데이터 수집 등

```python title=automation_example.py
# 간단한 자동화 예시: 파일 이름 일괄 변경
import os

def rename_files(folder_path, old_pattern, new_pattern):
    """폴더 내 파일들의 이름을 일괄 변경하는 함수"""
    for filename in os.listdir(folder_path):
        if old_pattern in filename:
            new_name = filename.replace(old_pattern, new_pattern)
            old_path = os.path.join(folder_path, filename)
            new_path = os.path.join(folder_path, new_name)
            os.rename(old_path, new_path)
            print(f"변경됨: {filename} → {new_name}")

# 사용 예시 (실제로는 경로를 정확히 입력해야 합니다)
# rename_files("/Users/Documents", "보고서_", "report_")
```

이 예제는 업무에서 자주 발생하는 파일 이름 일괄 변경 작업을 파이썬으로 자동화한 것입니다. 수백 개의 파일도 몇 초 만에 처리할 수 있습니다.

---

## 1-4 파이썬 설치하기

이제 본격적으로 파이썬을 설치해보겠습니다. 운영체제별로 설치 방법이 조금씩 다르므로 자신의 운영체제에 맞는 부분을 참고하세요.

### Windows에서 파이썬 설치

**방법 1: 공식 홈페이지에서 설치 (권장)**

1. [python.org](https://python.org) 접속
2. "Downloads" → "Windows" 클릭
3. 최신 버전 (Python 3.12.x) 다운로드
4. 다운받은 설치 파일 실행
5. **중요**: "Add Python to PATH" 체크박스 반드시 선택
6. "Install Now" 클릭

**방법 2: Microsoft Store에서 설치**

1. Microsoft Store 앱 실행
2. "Python" 검색
3. "Python 3.12" (최신 버전) 선택 후 설치

### macOS에서 파이썬 설치

**방법 1: 공식 홈페이지에서 설치 (권장)**

1. [python.org](https://python.org) 접속
2. "Downloads" → "macOS" 클릭
3. 최신 버전 다운로드 후 설치

**방법 2: Homebrew 사용 (개발자 추천)**

```bash
# Homebrew가 설치되어 있지 않다면 먼저 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 파이썬 설치
brew install python
```

### Linux에서 파이썬 설치

대부분의 리눅스 배포판에는 파이썬이 기본으로 설치되어 있습니다.

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install python3 python3-pip

# CentOS/RHEL
sudo yum install python3 python3-pip

# Fedora
sudo dnf install python3 python3-pip
```

### 설치 확인하기

터미널(명령 프롬프트)을 열고 다음 명령어를 입력해보세요:

```bash
python --version
# 또는
python3 --version
```

"Python 3.12.x" 같은 버전 정보가 나오면 성공적으로 설치된 것입니다.

---

## 1-5 개발 환경 설정

파이썬 코드를 작성하려면 코드 에디터가 필요합니다. 초보자에게 추천하는 두 가지 옵션을 소개하겠습니다. 좋은 개발 환경은 학습 효율을 크게 높여주므로 시간을 들여 제대로 설정하는 것이 중요합니다.

### IDLE (파이썬 기본 에디터)

IDLE은 파이썬과 함께 자동으로 설치되는 간단한 개발 환경입니다.

**IDLE 실행하기:**

- Windows: 시작 메뉴에서 "IDLE" 검색
- macOS: Spotlight에서 "IDLE" 검색
- Linux: 터미널에서 `idle3` 명령어 실행

**IDLE의 장점:**

- 별도 설치 불필요
- 간단하고 직관적인 인터페이스
- 초보자에게 적합

### Visual Studio Code (VS Code) - 권장

VS Code는 현재 가장 인기 있는 코드 에디터 중 하나입니다.

**VS Code 설치:**

1. [code.visualstudio.com](https://code.visualstudio.com) 접속
2. 운영체제에 맞는 버전 다운로드 및 설치

**파이썬 확장 설치:**

1. VS Code 실행
2. 왼쪽 사이드바의 확장(Extensions) 아이콘 클릭
3. "Python" 검색 후 Microsoft의 Python 확장 설치

**VS Code의 장점:**

- 강력한 자동완성 기능
- 디버깅 도구
- 터미널 통합
- 다양한 확장 프로그램

---

## 1-6 첫 번째 파이썬 프로그램 실행하기

드디어 첫 번째 파이썬 프로그램을 작성해보겠습니다. 프로그래밍의 전통적인 시작점인 "Hello, World!" 프로그램부터 시작해서 조금 더 재미있는 프로그램까지 단계별로 만들어보겠습니다.

### Hello, World! 프로그램

```python title=hello_world.py
# 첫 번째 파이썬 프로그램
print("Hello, World!")
print("파이썬 세계에 오신 것을 환영합니다!")
```

**실행 방법:**

**IDLE에서:**

1. 새 파일 생성 (File → New File)
2. 위 코드 입력
3. 파일 저장 (File → Save As → hello_world.py)
4. F5 키 또는 Run → Run Module

**VS Code에서:**

1. 새 파일 생성 후 hello_world.py로 저장
2. 위 코드 입력
3. Ctrl+F5 (또는 터미널에서 `python hello_world.py`)

### 대화형 프로그램 만들기

이제 좀 더 흥미로운 프로그램을 만들어보겠습니다. 사용자와 대화하는 프로그램입니다.

```python title=interactive_hello.py
# 사용자와 대화하는 프로그램
print("=== 파이썬 인사 프로그램 ===")

# 사용자로부터 이름 입력받기
name = input("안녕하세요! 이름이 무엇인가요? ")

# 사용자로부터 나이 입력받기
age = input("나이는 몇 살인가요? ")

# 개인화된 인사말 출력
print(f"\n안녕하세요, {name}님!")
print(f"{age}살이시군요.")
print(f"{name}님과 함께 파이썬을 배우게 되어 기쁩니다!")

# 간단한 계산 예시
print(f"\n재미있는 사실: {name}님은 약 {int(age) * 365}일 정도 살아오셨네요!")
```

이 프로그램은 사용자의 이름과 나이를 입력받아 개인화된 메시지를 보여줍니다. `input()` 함수로 사용자 입력을 받고, f-string을 사용해 변수를 문자열에 삽입하는 방법을 보여줍니다.

### 간단한 계산기 만들기

마지막으로 실용적인 계산기 프로그램을 만들어보겠습니다.

```python title=simple_calculator.py
# 간단한 계산기 프로그램
print("=== 간단한 계산기 ===")

# 사용자로부터 두 숫자 입력받기
num1 = float(input("첫 번째 숫자를 입력하세요: "))
num2 = float(input("두 번째 숫자를 입력하세요: "))

# 계산 수행
addition = num1 + num2
subtraction = num1 - num2
multiplication = num1 * num2
division = num1 / num2

# 결과 출력
print(f"\n=== 계산 결과 ===")
print(f"{num1} + {num2} = {addition}")
print(f"{num1} - {num2} = {subtraction}")
print(f"{num1} × {num2} = {multiplication}")
print(f"{num1} ÷ {num2} = {division:.2f}")

print("\n계산이 완료되었습니다!")
```

이 예제에서는 `float()` 함수를 사용해 문자열을 실수로 변환하고, 기본적인 산술 연산을 수행합니다. `:.2f`는 소수점 둘째 자리까지만 표시하는 포맷팅입니다.

---

## 연습문제

1. **자기소개 프로그램 만들기**
   - 이름, 나이, 사는 곳, 취미를 입력받아 자기소개문을 출력하는 프로그램을 작성하세요.

2. **원의 넓이 계산기**
   - 반지름을 입력받아 원의 넓이를 계산하는 프로그램을 작성하세요.
   - 공식: 넓이 = 3.14159 × 반지름²

3. **온도 변환기**
   - 섭씨 온도를 입력받아 화씨 온도로 변환하는 프로그램을 작성하세요.
   - 공식: 화씨 = (섭씨 × 9/5) + 32

**도전 과제:**
여러 개의 숫자를 입력받아 평균을 계산하는 프로그램을 만들어보세요. (힌트: 쉼표로 구분된 숫자들을 입력받아 처리)

---

**다음 챕터 예고**

다음 챕터에서는 파이썬의 기본 문법을 본격적으로 학습합니다. 변수, 주석, 들여쓰기 등 파이썬 프로그래밍의 기초를 탄탄히 다져보겠습니다. 지금까지 만든 프로그램들이 어떻게 동작하는지 더 자세히 이해하게 될 것입니다!
