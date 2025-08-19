---
title: '17. 테스팅 기초'
slug: python-testing-basics
description: 'pytest를 활용한 단위 테스트 작성과 테스트 주도 개발(TDD) 기초로 코드 품질 향상시키기'
keywords: [파이썬 테스트, pytest, 단위 테스트, TDD, 코드 품질, 테스트 코드 작성]
sidebar_position: 17
---

# 17. 테스팅 기초

## 학습 목표

이 챕터를 학습한 후, 여러분은 파이썬에서 테스트 코드를 작성하는 방법을 이해하고, pytest를 활용한 단위 테스트 작성과 테스트 주도 개발의 기본 개념을 적용할 수 있게 됩니다. 또한 코드의 품질을 높이고 버그를 예방하는 테스팅의 중요성을 깨닫게 될 것입니다.

---

## 17-1 테스트의 중요성

프로그래밍을 하다 보면 "내 코드가 정말 제대로 작동하고 있을까?"라는 의문이 들 때가 있습니다. 특히 코드가 복잡해질수록 예상치 못한 버그가 발생하거나, 새로운 기능을 추가했을 때 기존 기능이 망가지는 경우가 생깁니다. 이런 문제들을 해결하고 코드의 신뢰성을 높이는 것이 바로 테스팅입니다. 테스트 코드는 우리가 작성한 함수나 클래스가 예상대로 동작하는지 자동으로 확인해주는 코드입니다.

### 테스트가 필요한 이유

1. **버그 예방**: 코드를 작성하면서 동시에 테스트를 만들면 버그를 일찍 발견할 수 있습니다
2. **코드 신뢰성**: 테스트가 통과하면 코드가 의도한 대로 작동한다는 확신을 가질 수 있습니다
3. **리팩토링 안전성**: 코드를 개선할 때 기존 기능이 망가지지 않았는지 쉽게 확인할 수 있습니다
4. **문서화 효과**: 테스트 코드는 함수나 클래스의 사용법을 보여주는 살아있는 문서 역할을 합니다

### 테스트의 종류

```python title=test_example.py
# 간단한 계산기 함수
def add(a, b):
    """두 수를 더하는 함수"""
    return a + b

def divide(a, b):
    """두 수를 나누는 함수"""
    if b == 0:
        raise ValueError("0으로 나눌 수 없습니다")
    return a / b

# 수동 테스트 (비추천)
print(add(2, 3))  # 5가 나와야 함
print(divide(10, 2))  # 5.0이 나와야 함

# 자동 테스트 (추천)
def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_divide():
    assert divide(10, 2) == 5.0
    assert divide(7, 2) == 3.5

    # 예외 처리 테스트
    try:
        divide(5, 0)
        assert False, "예외가 발생해야 합니다"
    except ValueError:
        pass  # 예상된 예외이므로 테스트 통과
```

---

## 17-2 pytest 기초

pytest는 파이썬에서 가장 인기 있는 테스팅 프레임워크입니다. 간단한 문법으로 강력한 테스트를 작성할 수 있으며, 테스트 발견, 실행, 보고 기능을 자동으로 제공합니다. 내장된 unittest 모듈보다 사용하기 쉽고 더 많은 기능을 제공하므로 현업에서 널리 사용됩니다.

### pytest 설치와 기본 사용법

```bash
# pytest 설치
pip install pytest

# 또는 uv 사용시
uv add pytest --dev
```

### 첫 번째 pytest 테스트

```python title=test_calculator.py
# 테스트할 함수들
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

def is_even(number):
    return number % 2 == 0

# pytest 테스트 함수들 (함수명이 test_로 시작해야 함)
def test_add():
    """덧셈 함수 테스트"""
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_multiply():
    """곱셈 함수 테스트"""
    assert multiply(3, 4) == 12
    assert multiply(-2, 5) == -10
    assert multiply(0, 100) == 0

def test_is_even():
    """짝수 판별 함수 테스트"""
    assert is_even(2) == True
    assert is_even(3) == False
    assert is_even(0) == True
    assert is_even(-4) == True

# 여러 케이스를 한 번에 테스트
def test_add_multiple_cases():
    test_cases = [
        (1, 2, 3),
        (0, 0, 0),
        (-1, 1, 0),
        (100, -50, 50)
    ]

    for a, b, expected in test_cases:
        assert add(a, b) == expected
```

### pytest 실행하기

```bash
# 현재 디렉토리의 모든 테스트 실행
pytest

# 특정 파일의 테스트만 실행
pytest test_calculator.py

# 상세한 출력으로 실행
pytest -v

# 특정 테스트 함수만 실행
pytest test_calculator.py::test_add
```

---

## 17-3 단위 테스트 작성하기

단위 테스트는 프로그램의 가장 작은 단위(보통 함수나 메서드)가 올바르게 작동하는지 확인하는 테스트입니다. 좋은 단위 테스트는 독립적이고, 반복 가능하며, 빠르게 실행되어야 합니다. 각 테스트는 하나의 특정한 기능이나 시나리오에 집중해야 하며, 다른 테스트에 의존하지 않아야 합니다.

### 좋은 테스트 작성 방법

```python title=test_string_utils.py
# 테스트할 문자열 유틸리티 함수들
def reverse_string(text):
    """문자열을 뒤집는 함수"""
    return text[::-1]

def count_words(text):
    """문자열의 단어 개수를 세는 함수"""
    if not text.strip():
        return 0
    return len(text.split())

def capitalize_words(text):
    """각 단어의 첫 글자를 대문자로 만드는 함수"""
    return ' '.join(word.capitalize() for word in text.split())

# 테스트 코드들
def test_reverse_string():
    """문자열 뒤집기 테스트"""
    # 기본 케이스
    assert reverse_string("hello") == "olleh"
    assert reverse_string("python") == "nohtyp"

    # 특수 케이스
    assert reverse_string("") == ""
    assert reverse_string("a") == "a"
    assert reverse_string("12321") == "12321"  # 회문

def test_count_words():
    """단어 개수 세기 테스트"""
    # 정상적인 케이스
    assert count_words("hello world") == 2
    assert count_words("python is awesome") == 3

    # 경계 케이스
    assert count_words("") == 0
    assert count_words("   ") == 0  # 공백만 있는 경우
    assert count_words("hello") == 1  # 단어 하나
    assert count_words("  hello   world  ") == 2  # 앞뒤 공백

def test_capitalize_words():
    """단어 첫 글자 대문자화 테스트"""
    assert capitalize_words("hello world") == "Hello World"
    assert capitalize_words("python is fun") == "Python Is Fun"
    assert capitalize_words("") == ""
    assert capitalize_words("a") == "A"

# 예외 처리 테스트
import pytest

def test_divide_by_zero():
    """0으로 나누기 예외 테스트"""
    def divide(a, b):
        if b == 0:
            raise ValueError("Cannot divide by zero")
        return a / b

    # 예외가 발생하는지 테스트
    with pytest.raises(ValueError):
        divide(10, 0)

    # 예외 메시지까지 확인
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(5, 0)
```

### 테스트 픽스처 활용

```python title=test_with_fixtures.py
import pytest

# 테스트에서 공통으로 사용할 데이터나 객체를 준비하는 픽스처
@pytest.fixture
def sample_list():
    """테스트용 샘플 리스트"""
    return [1, 2, 3, 4, 5]

@pytest.fixture
def sample_dict():
    """테스트용 샘플 딕셔너리"""
    return {"name": "홍길동", "age": 25, "city": "서울"}

# 픽스처를 사용하는 테스트들
def test_list_operations(sample_list):
    """리스트 연산 테스트"""
    assert len(sample_list) == 5
    assert sum(sample_list) == 15
    assert max(sample_list) == 5

    # 리스트 수정 테스트
    sample_list.append(6)
    assert len(sample_list) == 6

def test_dict_operations(sample_dict):
    """딕셔너리 연산 테스트"""
    assert sample_dict["name"] == "홍길동"
    assert sample_dict["age"] == 25

    # 새 키 추가
    sample_dict["job"] = "개발자"
    assert "job" in sample_dict
```

---

## 17-4 테스트 주도 개발 (TDD) 맛보기

테스트 주도 개발(Test-Driven Development, TDD)은 코드를 작성하기 전에 먼저 테스트를 작성하는 개발 방법론입니다. "빨간색(실패) → 초록색(성공) → 리팩토링" 사이클을 반복하며 개발을 진행합니다. 이 방법을 사용하면 더 안전하고 깔끔한 코드를 작성할 수 있으며, 요구사항을 명확히 이해하게 됩니다.

### TDD 사이클 체험하기

간단한 은행 계좌 클래스를 TDD 방식으로 만들어보겠습니다.

```python title=test_bank_account.py
import pytest

# 1단계: 실패하는 테스트 작성 (빨간색)
def test_account_creation():
    """계좌 생성 테스트"""
    account = BankAccount("홍길동", 10000)
    assert account.owner == "홍길동"
    assert account.balance == 10000

def test_deposit():
    """입금 테스트"""
    account = BankAccount("김철수", 5000)
    account.deposit(3000)
    assert account.balance == 8000

def test_withdraw():
    """출금 테스트"""
    account = BankAccount("이영희", 10000)
    result = account.withdraw(3000)
    assert result == True
    assert account.balance == 7000

def test_withdraw_insufficient_funds():
    """잔액 부족시 출금 테스트"""
    account = BankAccount("박민수", 5000)
    result = account.withdraw(10000)
    assert result == False
    assert account.balance == 5000  # 잔액 변화 없음

def test_get_balance():
    """잔액 조회 테스트"""
    account = BankAccount("최지훈", 15000)
    assert account.get_balance() == 15000

# 2단계: 테스트를 통과시키는 최소한의 코드 작성 (초록색)
class BankAccount:
    def __init__(self, owner, initial_balance):
        self.owner = owner
        self.balance = initial_balance

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount

    def withdraw(self, amount):
        if amount > 0 and self.balance >= amount:
            self.balance -= amount
            return True
        return False

    def get_balance(self):
        return self.balance

# 3단계: 코드 개선 (리팩토링)
class BankAccount:
    def __init__(self, owner, initial_balance=0):
        if initial_balance < 0:
            raise ValueError("초기 잔액은 0 이상이어야 합니다")
        self.owner = owner
        self.balance = initial_balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("입금액은 0보다 커야 합니다")
        self.balance += amount
        return self.balance

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("출금액은 0보다 커야 합니다")
        if self.balance < amount:
            return False
        self.balance -= amount
        return True

    def get_balance(self):
        return self.balance

    def __str__(self):
        return f"계좌주: {self.owner}, 잔액: {self.balance:,}원"

# 개선된 코드에 대한 추가 테스트
def test_invalid_initial_balance():
    """잘못된 초기 잔액 테스트"""
    with pytest.raises(ValueError):
        BankAccount("테스트", -1000)

def test_invalid_deposit():
    """잘못된 입금 테스트"""
    account = BankAccount("테스트", 1000)
    with pytest.raises(ValueError):
        account.deposit(-500)
    with pytest.raises(ValueError):
        account.deposit(0)

def test_invalid_withdraw():
    """잘못된 출금 테스트"""
    account = BankAccount("테스트", 1000)
    with pytest.raises(ValueError):
        account.withdraw(-500)
    with pytest.raises(ValueError):
        account.withdraw(0)
```

### TDD의 장점

1. **요구사항 명확화**: 테스트를 먼저 작성하면서 기능의 요구사항을 명확히 이해하게 됩니다
2. **과도한 설계 방지**: 테스트를 통과하는 최소한의 코드만 작성하므로 불필요한 기능을 만들지 않습니다
3. **높은 테스트 커버리지**: 모든 코드가 테스트와 함께 작성되므로 자연스럽게 높은 테스트 커버리지를 달성합니다
4. **리팩토링 안전성**: 언제든지 테스트를 실행해서 기능이 제대로 작동하는지 확인할 수 있습니다

---

## 실습: 계산기 함수 테스트 작성하기

앞서 배운 내용을 바탕으로 실제 계산기 함수들을 만들고 테스트를 작성해보겠습니다. 이 실습을 통해 테스트 작성의 실제 과정을 체험하고, 다양한 테스트 케이스를 고려하는 방법을 익힐 수 있습니다.

```python title=calculator.py
"""
간단한 계산기 모듈
"""
import math

def add(a, b):
    """두 수를 더하는 함수"""
    return a + b

def subtract(a, b):
    """두 수를 빼는 함수"""
    return a - b

def multiply(a, b):
    """두 수를 곱하는 함수"""
    return a * b

def divide(a, b):
    """두 수를 나누는 함수"""
    if b == 0:
        raise ZeroDivisionError("0으로 나눌 수 없습니다")
    return a / b

def power(base, exponent):
    """거듭제곱 계산 함수"""
    return base ** exponent

def square_root(number):
    """제곱근 계산 함수"""
    if number < 0:
        raise ValueError("음수의 제곱근은 계산할 수 없습니다")
    return math.sqrt(number)

def factorial(n):
    """팩토리얼 계산 함수"""
    if not isinstance(n, int):
        raise TypeError("정수만 입력 가능합니다")
    if n < 0:
        raise ValueError("음수의 팩토리얼은 계산할 수 없습니다")
    if n == 0 or n == 1:
        return 1
    return n * factorial(n - 1)
```

```python title=test_calculator.py
"""
계산기 모듈 테스트
"""
import pytest
import math
from calculator import add, subtract, multiply, divide, power, square_root, factorial

class TestBasicOperations:
    """기본 사칙연산 테스트"""

    def test_add(self):
        """덧셈 테스트"""
        assert add(2, 3) == 5
        assert add(-1, 1) == 0
        assert add(0, 0) == 0
        assert add(-5, -3) == -8
        assert add(1.5, 2.5) == 4.0

    def test_subtract(self):
        """뺄셈 테스트"""
        assert subtract(5, 3) == 2
        assert subtract(1, 1) == 0
        assert subtract(-1, -1) == 0
        assert subtract(10, -5) == 15
        assert subtract(3.7, 1.2) == pytest.approx(2.5)

    def test_multiply(self):
        """곱셈 테스트"""
        assert multiply(3, 4) == 12
        assert multiply(-2, 5) == -10
        assert multiply(0, 100) == 0
        assert multiply(-3, -4) == 12
        assert multiply(2.5, 4) == 10.0

    def test_divide(self):
        """나눗셈 테스트"""
        assert divide(10, 2) == 5.0
        assert divide(7, 2) == 3.5
        assert divide(-8, 4) == -2.0
        assert divide(0, 5) == 0.0

        # 0으로 나누기 예외 테스트
        with pytest.raises(ZeroDivisionError):
            divide(5, 0)

class TestAdvancedOperations:
    """고급 연산 테스트"""

    def test_power(self):
        """거듭제곱 테스트"""
        assert power(2, 3) == 8
        assert power(5, 0) == 1
        assert power(4, 0.5) == 2.0
        assert power(-2, 3) == -8
        assert power(10, -2) == 0.01

    def test_square_root(self):
        """제곱근 테스트"""
        assert square_root(4) == 2.0
        assert square_root(9) == 3.0
        assert square_root(0) == 0.0
        assert square_root(2) == pytest.approx(1.414, rel=1e-3)

        # 음수 제곱근 예외 테스트
        with pytest.raises(ValueError):
            square_root(-1)

    def test_factorial(self):
        """팩토리얼 테스트"""
        assert factorial(0) == 1
        assert factorial(1) == 1
        assert factorial(5) == 120
        assert factorial(10) == 3628800

        # 예외 테스트
        with pytest.raises(ValueError):
            factorial(-1)

        with pytest.raises(TypeError):
            factorial(3.14)

        with pytest.raises(TypeError):
            factorial("5")

class TestEdgeCases:
    """경계 케이스 테스트"""

    def test_large_numbers(self):
        """큰 수 계산 테스트"""
        large_num = 10**10
        assert add(large_num, 1) == large_num + 1
        assert multiply(large_num, 0) == 0

    def test_floating_point_precision(self):
        """부동소수점 정밀도 테스트"""
        # pytest.approx를 사용한 근사값 비교
        result = add(0.1, 0.2)
        assert result == pytest.approx(0.3)

        result = divide(1, 3)
        assert multiply(result, 3) == pytest.approx(1.0)

# 매개화된 테스트 (여러 입력값을 한 번에 테스트)
@pytest.mark.parametrize("a,b,expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, 1, 0),
    (100, -50, 50),
    (3.14, 2.86, 6.0)
])
def test_add_parametrized(a, b, expected):
    """매개화된 덧셈 테스트"""
    assert add(a, b) == pytest.approx(expected)

@pytest.mark.parametrize("base,exp,expected", [
    (2, 3, 8),
    (5, 0, 1),
    (1, 100, 1),
    (-2, 2, 4),
    (4, 0.5, 2)
])
def test_power_parametrized(base, exp, expected):
    """매개화된 거듭제곱 테스트"""
    assert power(base, exp) == pytest.approx(expected)
```

### 테스트 실행 및 결과 확인

```bash
# 모든 테스트 실행
pytest test_calculator.py -v

# 특정 클래스의 테스트만 실행
pytest test_calculator.py::TestBasicOperations -v

# 코드 커버리지 확인 (pytest-cov 설치 필요)
pip install pytest-cov
pytest test_calculator.py --cov=calculator --cov-report=html
```

---

## 연습문제

1. **문자열 검증 함수 테스트**: 이메일 주소가 유효한지 검사하는 함수를 만들고 다양한 케이스에 대한 테스트를 작성해보세요.

2. **리스트 유틸리티 테스트**: 리스트에서 중복을 제거하는 함수, 리스트를 정렬하는 함수 등을 만들고 테스트를 작성해보세요.

3. **TDD 실습**: 간단한 도서관 시스템의 `Book` 클래스를 TDD 방식으로 만들어보세요. 책 정보 저장, 대출/반납 기능을 포함해야 합니다.

4. **예외 처리 테스트**: 파일을 읽는 함수를 만들고, 파일이 존재하지 않는 경우나 권한이 없는 경우 등의 예외 상황에 대한 테스트를 작성해보세요.

테스팅은 처음에는 번거로워 보일 수 있지만, 습관이 되면 코드의 품질을 크게 향상시키고 개발 과정에서 큰 도움이 됩니다. 작은 함수부터 시작해서 점진적으로 테스트 작성 습관을 기르시기 바랍니다!
