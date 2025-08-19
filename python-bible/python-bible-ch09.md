---
title: '9. 모듈과 패키지'
slug: python-modules-packages
description: '모듈 생성과 사용, 패키지 구조, 작준 라이브러리 활용법과 pip를 이용한 외부 패키지 관리'
keywords: [파이썬 모듈, 모듈 import, 파이썬 패키지, 작준 라이브러리, pip, 모듈 사용법]
sidebar_position: 9
---

# 9. 모듈과 패키지

프로그램이 커질수록 모든 코드를 하나의 파일에 담는 것은 비효율적입니다. 코드를 기능별로 나누어 관리하고, 다른 프로그램에서도 재사용할 수 있도록 만드는 것이 바로 모듈과 패키지의 역할입니다. 이번 장에서는 파이썬의 모듈 시스템을 이해하고, 직접 모듈을 만들어 사용하는 방법을 배워보겠습니다. 또한 파이썬이 기본으로 제공하는 표준 라이브러리들을 활용하여 더 강력한 프로그램을 만드는 방법도 함께 알아보겠습니다.

## 학습 목표

이번 장을 완료하면 다음과 같은 능력을 갖게 됩니다:

- 모듈의 개념을 이해하고 직접 만들어 사용할 수 있습니다
- 패키지 구조를 설계하고 구현할 수 있습니다
- 파이썬 표준 라이브러리를 활용한 실용적인 프로그램을 작성할 수 있습니다
- pip를 이용한 외부 패키지 관리 기초를 익힐 수 있습니다

---

## 9-1 모듈이란?

모듈은 함수, 변수, 클래스 등을 담고 있는 파이썬 파일입니다. 모듈을 사용하면 코드를 논리적으로 구성하고, 재사용성을 높일 수 있습니다. 또한 코드의 유지보수가 쉬워지고, 다른 개발자와의 협업도 원활해집니다.

### 모듈의 기본 개념

모듈은 `.py` 확장자를 가진 파이썬 파일 그 자체입니다. 예를 들어, `calculator.py`라는 파일을 만들면 이것이 바로 `calculator` 모듈이 됩니다.

```python title=calculator.py
# 계산 관련 함수들을 모아둔 모듈

def add(a, b):
    """두 수를 더합니다."""
    return a + b

def subtract(a, b):
    """두 수를 뺍니다."""
    return a - b

def multiply(a, b):
    """두 수를 곱합니다."""
    return a * b

def divide(a, b):
    """두 수를 나눕니다."""
    if b == 0:
        return "0으로 나눌 수 없습니다"
    return a / b

# 모듈 상수
PI = 3.14159
VERSION = "1.0.0"
```

### 모듈 사용하기

모듈을 사용하는 방법은 여러 가지가 있습니다:

```python title=main.py
# 1. 전체 모듈 import
import calculator

result1 = calculator.add(10, 5)
print(f"10 + 5 = {result1}")
print(f"모듈 버전: {calculator.VERSION}")

# 2. 특정 함수만 import
from calculator import add, multiply

result2 = add(20, 30)
result3 = multiply(4, 5)
print(f"20 + 30 = {result2}")
print(f"4 × 5 = {result3}")

# 3. 별칭 사용
import calculator as calc

result4 = calc.divide(100, 4)
print(f"100 ÷ 4 = {result4}")

# 4. 모든 것을 import (권장하지 않음)
# from calculator import *
```

---

## 9-2 모듈 만들기와 사용하기

실제 프로젝트에서 유용한 모듈을 만들어보겠습니다. 여기서는 날짜와 시간을 다루는 유틸리티 모듈을 예제로 사용하겠습니다.

### 실용적인 모듈 만들기

```python title=date_utils.py
"""
날짜와 시간 관련 유틸리티 함수들
"""
from datetime import datetime, timedelta

def get_current_time():
    """현재 시간을 문자열로 반환합니다."""
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

def get_days_until(target_date_str):
    """목표 날짜까지 남은 일수를 계산합니다.

    Args:
        target_date_str (str): YYYY-MM-DD 형식의 날짜 문자열

    Returns:
        int: 남은 일수 (과거면 음수)
    """
    target_date = datetime.strptime(target_date_str, "%Y-%m-%d")
    current_date = datetime.now().date()
    diff = target_date.date() - current_date
    return diff.days

def format_duration(seconds):
    """초를 시:분:초 형식으로 변환합니다."""
    hours = seconds // 3600
    minutes = (seconds % 3600) // 60
    seconds = seconds % 60
    return f"{hours:02d}:{minutes:02d}:{seconds:02d}"

# 모듈 수준 변수
WEEKDAYS = ["월", "화", "수", "목", "금", "토", "일"]

def get_weekday_korean():
    """오늘의 요일을 한글로 반환합니다."""
    today = datetime.now().weekday()
    return WEEKDAYS[today]

if __name__ == "__main__":
    # 모듈을 직접 실행했을 때의 테스트 코드
    print("날짜 유틸리티 모듈 테스트")
    print(f"현재 시간: {get_current_time()}")
    print(f"오늘은 {get_weekday_korean()}요일입니다")
    print(f"2025-12-25까지 {get_days_until('2025-12-25')}일 남았습니다")
```

### 모듈 사용 예제

```python title=test_date_utils.py
from date_utils import get_current_time, get_days_until, format_duration

# 현재 시간 확인
print(f"지금은 {get_current_time()}입니다")

# 크리스마스까지 남은 일수
christmas_days = get_days_until("2025-12-25")
if christmas_days > 0:
    print(f"크리스마스까지 {christmas_days}일 남았습니다!")
elif christmas_days == 0:
    print("오늘이 크리스마스입니다!")
else:
    print(f"크리스마스가 {abs(christmas_days)}일 전에 지났습니다.")

# 시간 포맷팅
duration = format_duration(3661)  # 1시간 1분 1초
print(f"3661초는 {duration}입니다")
```

---

## 9-3 패키지 개념과 실제 구조

패키지는 관련된 모듈들을 폴더로 그룹화한 것입니다. 복잡한 프로젝트에서 코드를 체계적으로 구성하는 데 필수적입니다.

### 패키지의 기본 구조

```
my_package/
    __init__.py          # 패키지 초기화 파일
    __main__.py          # 패키지를 실행 가능하게 만드는 파일
    core/
        __init__.py
        calculator.py
        date_utils.py
    utils/
        __init__.py
        file_handler.py
        validator.py
    tests/
        __init__.py
        test_calculator.py
```

### `__init__.py`의 역할과 활용

`__init__.py` 파일은 폴더를 파이썬 패키지로 인식하게 만들고, 패키지 초기화 코드를 담습니다.

```python title=my_package/__init__.py
"""
My Package - 개인 유틸리티 패키지
버전: 1.0.0
"""

__version__ = "1.0.0"
__author__ = "Your Name"

# 주요 기능들을 패키지 레벨에서 바로 사용할 수 있게 설정
from .core.calculator import add, subtract, multiply, divide
from .core.date_utils import get_current_time, get_days_until

# 패키지가 import될 때 실행되는 초기화 코드
print(f"My Package v{__version__} 로드 완료")

# __all__을 정의하여 from my_package import *에서 가져올 것들 지정
__all__ = ['add', 'subtract', 'multiply', 'divide', 'get_current_time', 'get_days_until']
```

### `__main__.py`로 실행 가능한 패키지 만들기

```python title=my_package/__main__.py
"""
패키지를 python -m my_package로 실행했을 때 동작하는 코드
"""

from .core.calculator import add, subtract, multiply, divide
from .core.date_utils import get_current_time, get_weekday_korean

def main():
    print("=== My Package 데모 ===")
    print(f"현재 시간: {get_current_time()}")
    print(f"오늘은 {get_weekday_korean()}요일")

    # 간단한 계산기 데모
    print("\n계산기 기능:")
    a, b = 10, 3
    print(f"{a} + {b} = {add(a, b)}")
    print(f"{a} - {b} = {subtract(a, b)}")
    print(f"{a} × {b} = {multiply(a, b)}")
    print(f"{a} ÷ {b} = {divide(a, b)}")

if __name__ == "__main__":
    main()
```

### 상대 임포트와 절대 임포트

```python title=my_package/utils/validator.py
# 절대 임포트 (권장)
from my_package.core.calculator import add

# 상대 임포트 (패키지 내부에서만 사용)
from ..core.calculator import subtract
from .file_handler import read_file

def validate_calculation(a, b, operation):
    """계산 입력값의 유효성을 검증합니다."""
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        return False, "숫자만 입력 가능합니다"

    if operation == "divide" and b == 0:
        return False, "0으로 나눌 수 없습니다"

    return True, "유효한 입력입니다"
```

---

## 9-4 표준 라이브러리 활용

파이썬은 다양한 표준 라이브러리를 제공합니다. 가장 자주 사용되는 라이브러리들을 살펴보겠습니다.

### math 모듈

```python title=math_examples.py
import math

# 기본 수학 함수들
print(f"제곱근: {math.sqrt(16)}")
print(f"거듭제곱: {math.pow(2, 3)}")
print(f"반올림: {math.ceil(4.2)}, {math.floor(4.8)}")

# 삼각함수
angle = math.pi / 4  # 45도
print(f"sin(45°) = {math.sin(angle):.4f}")
print(f"cos(45°) = {math.cos(angle):.4f}")

# 상수들
print(f"π = {math.pi}")
print(f"e = {math.e}")
```

### random 모듈

```python title=random_examples.py
import random

# 기본 랜덤 함수들
print(f"0~1 사이 실수: {random.random()}")
print(f"1~10 사이 정수: {random.randint(1, 10)}")
print(f"1~100 사이 실수: {random.uniform(1, 100)}")

# 리스트에서 랜덤 선택
colors = ["빨강", "파랑", "초록", "노랑", "보라"]
print(f"랜덤 색상: {random.choice(colors)}")

# 리스트 섞기
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(f"섞인 숫자: {numbers}")

# 복수 선택 (중복 없음)
print(f"랜덤 2개 색상: {random.sample(colors, 2)}")
```

### datetime 모듈

```python title=datetime_examples.py
from datetime import datetime, date, time, timedelta

# 현재 날짜와 시간
now = datetime.now()
print(f"현재: {now}")
print(f"날짜만: {now.date()}")
print(f"시간만: {now.time()}")

# 특정 날짜 생성
birthday = datetime(2000, 12, 25, 15, 30, 0)
print(f"생일: {birthday}")

# 날짜 계산
one_week_later = now + timedelta(days=7)
print(f"일주일 후: {one_week_later}")

# 날짜 포맷팅
formatted = now.strftime("%Y년 %m월 %d일 %H시 %M분")
print(f"한국어 형식: {formatted}")
```

### pathlib 모듈 (현대적 경로 처리)

```python title=pathlib_examples.py
from pathlib import Path

# 현재 디렉토리
current_dir = Path.cwd()
print(f"현재 디렉토리: {current_dir}")

# 파일 경로 만들기
file_path = current_dir / "data" / "sample.txt"
print(f"파일 경로: {file_path}")

# 파일 존재 여부 확인
if file_path.exists():
    print("파일이 존재합니다")
    print(f"파일 크기: {file_path.stat().st_size} bytes")
else:
    print("파일이 존재하지 않습니다")

# 디렉토리 생성
data_dir = current_dir / "output"
data_dir.mkdir(exist_ok=True)  # 이미 있어도 오류 안 남

# 파일 목록 가져오기
python_files = list(current_dir.glob("*.py"))
print(f"파이썬 파일들: {[f.name for f in python_files]}")
```

---

## 9-5 pip 패키지 관리 기초

pip는 파이썬 패키지를 설치하고 관리하는 도구입니다. 외부 라이브러리를 사용할 때 필수적입니다.

### 기본 pip 명령어

```bash
# 패키지 설치
pip install requests

# 특정 버전 설치
pip install pandas==1.5.3

# 설치된 패키지 목록 확인
pip list

# 패키지 정보 확인
pip show requests

# 패키지 업그레이드
pip install --upgrade requests

# 패키지 제거
pip uninstall requests

# requirements.txt로 일괄 설치
pip install -r requirements.txt

# 현재 환경의 패키지 목록을 파일로 저장
pip freeze > requirements.txt
```

### requirements.txt 예제

```text title=requirements.txt
requests==2.31.0
pandas>=1.5.0
matplotlib==3.7.1
beautifulsoup4>=4.12.0
pytest>=7.0.0
```

---

## 실습: 로또 번호 생성기

다양한 표준 라이브러리를 활용하여 실용적인 로또 번호 생성기를 만들어보겠습니다. 이 프로그램은 랜덤한 로또 번호를 생성하고, 결과를 파일로 저장하는 기능을 제공합니다.

```python title=lotto_generator.py
"""
로또 번호 생성기 모듈
"""
import random
from datetime import datetime
from pathlib import Path

class LottoGenerator:
    """로또 번호 생성 및 관리 클래스"""

    def __init__(self):
        self.min_number = 1
        self.max_number = 45
        self.numbers_count = 6
        self.history = []

    def generate_numbers(self):
        """로또 번호 6개를 생성합니다."""
        numbers = random.sample(range(self.min_number, self.max_number + 1),
                               self.numbers_count)
        numbers.sort()

        # 생성 시간과 함께 히스토리에 저장
        entry = {
            'numbers': numbers,
            'timestamp': datetime.now(),
            'formatted_time': datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        }
        self.history.append(entry)

        return numbers

    def generate_multiple(self, count):
        """여러 세트의 로또 번호를 생성합니다."""
        results = []
        for _ in range(count):
            results.append(self.generate_numbers())
        return results

    def save_to_file(self, filename=None):
        """생성된 번호들을 파일로 저장합니다."""
        if not self.history:
            print("저장할 번호가 없습니다.")
            return False

        if filename is None:
            # 현재 날짜로 파일명 생성
            today = datetime.now().strftime("%Y%m%d")
            filename = f"lotto_numbers_{today}.txt"

        try:
            # pathlib 사용하여 파일 저장
            file_path = Path(filename)

            with open(file_path, 'w', encoding='utf-8') as f:
                f.write("=" * 50 + "\n")
                f.write("로또 번호 생성 결과\n")
                f.write("=" * 50 + "\n\n")

                for i, entry in enumerate(self.history, 1):
                    numbers_str = " - ".join(map(str, entry['numbers']))
                    f.write(f"{i:2d}. [{numbers_str}] "
                           f"({entry['formatted_time']})\n")

                f.write(f"\n총 {len(self.history)}세트 생성됨\n")

            print(f"파일 저장 완료: {file_path.absolute()}")
            return True

        except Exception as e:
            print(f"파일 저장 실패: {e}")
            return False

    def get_statistics(self):
        """번호 출현 통계를 분석합니다."""
        if not self.history:
            return {}

        # 각 번호별 출현 횟수 계산
        number_count = {}
        for entry in self.history:
            for number in entry['numbers']:
                number_count[number] = number_count.get(number, 0) + 1

        # 가장 많이/적게 나온 번호 찾기
        if number_count:
            most_frequent = max(number_count.items(), key=lambda x: x[1])
            least_frequent = min(number_count.items(), key=lambda x: x[1])

            return {
                'total_sets': len(self.history),
                'number_frequency': number_count,
                'most_frequent': most_frequent,
                'least_frequent': least_frequent
            }

        return {}

def main():
    """메인 실행 함수"""
    print("🎰 로또 번호 생성기에 오신 것을 환영합니다!")

    generator = LottoGenerator()

    while True:
        print("\n" + "=" * 40)
        print("1. 로또 번호 1세트 생성")
        print("2. 로또 번호 여러 세트 생성")
        print("3. 생성 히스토리 보기")
        print("4. 통계 보기")
        print("5. 파일로 저장")
        print("6. 종료")
        print("=" * 40)

        choice = input("선택하세요 (1-6): ").strip()

        if choice == "1":
            numbers = generator.generate_numbers()
            print(f"\n🎯 생성된 번호: {' - '.join(map(str, numbers))}")

        elif choice == "2":
            try:
                count = int(input("생성할 세트 수를 입력하세요: "))
                if count <= 0:
                    print("1 이상의 숫자를 입력해주세요.")
                    continue

                results = generator.generate_multiple(count)
                print(f"\n🎯 {count}세트 생성 완료:")
                for i, numbers in enumerate(results, 1):
                    print(f"{i:2d}. {' - '.join(map(str, numbers))}")

            except ValueError:
                print("올바른 숫자를 입력해주세요.")

        elif choice == "3":
            if not generator.history:
                print("\n생성된 번호가 없습니다.")
            else:
                print(f"\n📋 생성 히스토리 (총 {len(generator.history)}세트):")
                for i, entry in enumerate(generator.history, 1):
                    numbers_str = ' - '.join(map(str, entry['numbers']))
                    print(f"{i:2d}. [{numbers_str}] ({entry['formatted_time']})")

        elif choice == "4":
            stats = generator.get_statistics()
            if not stats:
                print("\n통계를 표시할 데이터가 없습니다.")
            else:
                print(f"\n📊 통계 정보 (총 {stats['total_sets']}세트)")
                print(f"가장 많이 나온 번호: {stats['most_frequent'][0]} "
                      f"({stats['most_frequent'][1]}번)")
                print(f"가장 적게 나온 번호: {stats['least_frequent'][0]} "
                      f"({stats['least_frequent'][1]}번)")

        elif choice == "5":
            if generator.save_to_file():
                print("저장이 완료되었습니다!")

        elif choice == "6":
            print("\n프로그램을 종료합니다. 행운을 빕니다! 🍀")
            break

        else:
            print("올바른 번호를 선택해주세요.")

if __name__ == "__main__":
    main()
```

---

## 확장 과제: 개인 유틸리티 패키지 만들기

앞서 배운 내용을 종합하여 자신만의 유틸리티 패키지를 만들어보세요. 다음과 같은 구조로 패키지를 설계해보겠습니다:

```
personal_utils/
    __init__.py
    __main__.py
    text/
        __init__.py
        analyzer.py      # 텍스트 분석 기능
        formatter.py     # 텍스트 포맷팅 기능
    file/
        __init__.py
        manager.py       # 파일 관리 기능
        backup.py        # 백업 기능
    math/
        __init__.py
        calculator.py    # 고급 계산 기능
        statistics.py    # 통계 기능
```

이 패키지에는 일상적으로 사용할 수 있는 다양한 유틸리티 함수들을 포함시켜보세요. 예를 들어:

- 텍스트 분석: 단어 수 세기, 문자 빈도 분석
- 파일 관리: 폴더 정리, 중복 파일 찾기
- 계산기: 복리 계산, 단위 변환
- 백업: 중요 파일 자동 백업

---

## 연습문제

### 문제 1: 온도 변환 모듈

섭씨, 화씨, 켈빈 온도를 서로 변환하는 모듈을 작성하세요.

### 문제 2: 패스워드 생성기 패키지

다양한 옵션(길이, 특수문자 포함 여부 등)을 가진 패스워드 생성기 패키지를 만드세요.

### 문제 3: 날짜 계산기

두 날짜 사이의 차이를 계산하고, 특정 날짜에서 N일 후의 날짜를 구하는 모듈을 작성하세요.

### 문제 4: 파일 정리 도구

지정된 폴더의 파일들을 확장자별로 분류하여 정리하는 패키지를 만드세요.

---

이번 장에서는 파이썬의 모듈과 패키지 시스템을 이해하고, 코드를 체계적으로 구성하는 방법을 배웠습니다. 모듈화는 프로그래밍에서 매우 중요한 개념이므로, 앞으로 프로젝트를 진행할 때 항상 코드의 재사용성과 유지보수성을 고려하여 적절히 모듈을 나누는 습관을 기르시기 바랍니다.
