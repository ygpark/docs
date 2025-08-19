---
title: '14. 예외 처리'
slug: python-exception-handling
description: 'try/except/finally문, 예외 타입, 사용자 정의 예외까지 안정적인 파이썬 프로그램 작성법'
keywords: [파이썬 예외처리, try except, 예외 타입, finally, 사용자정의예외, 오류 처리]
sidebar_position: 14
---

# 14. 예외 처리

프로그램을 개발하다 보면 예상치 못한 상황들이 발생합니다. 사용자가 잘못된 값을 입력하거나, 파일이 존재하지 않거나, 네트워크 연결이 끊어지는 등의 문제들이죠. 이런 상황들을 우아하게 처리하는 것이 바로 예외 처리입니다. 파이썬의 예외 처리 메커니즘을 익히면 더 안정적이고 사용자 친화적인 프로그램을 만들 수 있습니다. 이번 챕터에서는 예외의 기본 개념부터 실무에서 활용할 수 있는 고급 기법까지 차근차근 살펴보겠습니다.

## 14-1 오류와 예외

프로그래밍에서 발생하는 문제들을 크게 문법 오류와 런타임 오류로 나눌 수 있습니다. 문법 오류는 코드를 실행하기 전에 발견되는 오류이고, 런타임 오류는 프로그램이 실행되는 중에 발생하는 오류입니다. 파이썬에서는 런타임 오류를 '예외'라고 부르며, 이를 적절히 처리하지 않으면 프로그램이 갑자기 종료됩니다.

```python title=basic_errors.py
# 문법 오류 (SyntaxError)
# print("Hello World"  # 괄호가 닫히지 않음

# 런타임 오류 (예외)
number = 10
# result = number / 0  # ZeroDivisionError 발생

# 존재하지 않는 파일 열기
# file = open("not_exist.txt")  # FileNotFoundError 발생

# 리스트 인덱스 오류
numbers = [1, 2, 3]
# print(numbers[5])  # IndexError 발생

# 타입 오류
text = "hello"
# result = text + 5  # TypeError 발생
```

### 주요 예외 유형들

```python title=exception_types.py
# ValueError: 잘못된 값
try:
    number = int("hello")
except ValueError as e:
    print(f"값 오류: {e}")

# KeyError: 존재하지 않는 키
person = {"name": "김파이", "age": 25}
try:
    height = person["height"]
except KeyError as e:
    print(f"키 오류: {e}")

# AttributeError: 존재하지 않는 속성
try:
    result = "hello".non_exist_method()
except AttributeError as e:
    print(f"속성 오류: {e}")

# IndexError: 인덱스 범위 초과
numbers = [1, 2, 3]
try:
    print(numbers[10])
except IndexError as e:
    print(f"인덱스 오류: {e}")
```

## 14-2 try-except문

예외가 발생할 가능성이 있는 코드를 try 블록에 작성하고, 예외가 발생했을 때 실행할 코드를 except 블록에 작성합니다. 이를 통해 프로그램이 갑자기 종료되는 것을 방지할 수 있습니다.

```python title=try_except_basic.py
def safe_divide(a, b):
    """안전한 나눗셈 함수"""
    try:
        result = a / b
        return result
    except ZeroDivisionError:
        print("0으로 나눌 수 없습니다!")
        return None

# 사용 예제
print(safe_divide(10, 2))   # 5.0
print(safe_divide(10, 0))   # 0으로 나눌 수 없습니다! None

def get_user_age():
    """사용자 나이 입력받기"""
    while True:
        try:
            age = int(input("나이를 입력하세요: "))
            if age < 0:
                print("나이는 0 이상이어야 합니다.")
                continue
            return age
        except ValueError:
            print("숫자를 입력해주세요!")

# age = get_user_age()
# print(f"입력된 나이: {age}")
```

### 여러 예외 처리하기

```python title=multiple_exceptions.py
def process_data(data, index):
    """데이터 처리 함수"""
    try:
        # 여러 종류의 예외가 발생할 수 있는 코드
        value = data[index]
        result = 100 / value
        return result
    except IndexError:
        print("인덱스가 범위를 벗어났습니다.")
    except ZeroDivisionError:
        print("0으로 나눌 수 없습니다.")
    except TypeError:
        print("잘못된 타입입니다.")
    except (ValueError, KeyError) as e:
        # 여러 예외를 한 번에 처리
        print(f"값 또는 키 오류가 발생했습니다: {e}")
    except Exception as e:
        # 모든 예외를 잡는 일반적인 처리
        print(f"예상치 못한 오류가 발생했습니다: {e}")

# 테스트
numbers = [1, 2, 0, 4]
process_data(numbers, 0)   # 정상 실행
process_data(numbers, 10)  # IndexError
process_data(numbers, 2)   # ZeroDivisionError
```

## 14-3 else와 finally

try-except문에는 else와 finally 블록을 추가할 수 있습니다. else는 예외가 발생하지 않았을 때 실행되고, finally는 예외 발생 여부와 관계없이 항상 실행됩니다.

```python title=else_finally.py
def read_file_safely(filename):
    """파일을 안전하게 읽는 함수"""
    file = None
    try:
        print(f"{filename} 파일을 열려고 시도합니다...")
        file = open(filename, 'r', encoding='utf-8')
        content = file.read()
        print("파일을 성공적으로 읽었습니다.")
        return content
    except FileNotFoundError:
        print(f"파일을 찾을 수 없습니다: {filename}")
        return None
    except PermissionError:
        print(f"파일에 접근할 권한이 없습니다: {filename}")
        return None
    else:
        # 예외가 발생하지 않았을 때만 실행
        print("파일 읽기가 완료되었습니다.")
    finally:
        # 항상 실행되는 코드
        if file:
            file.close()
            print("파일을 닫았습니다.")
        print("파일 처리 작업이 종료되었습니다.")

# 사용 예제
content = read_file_safely("example.txt")
```

### 리소스 관리 예제

```python title=resource_management.py
import time

def connect_to_database():
    """데이터베이스 연결 시뮬레이션"""
    connection = None
    try:
        print("데이터베이스에 연결 중...")
        time.sleep(1)  # 연결 시뮬레이션
        connection = "DB_CONNECTION_OBJECT"

        # 데이터베이스 작업 시뮬레이션
        print("데이터를 조회 중...")
        # raise Exception("네트워크 오류 발생!")  # 의도적 오류

        data = ["데이터1", "데이터2", "데이터3"]
        return data

    except Exception as e:
        print(f"데이터베이스 작업 중 오류 발생: {e}")
        return None
    else:
        print("데이터베이스 작업이 성공적으로 완료되었습니다.")
    finally:
        if connection:
            print("데이터베이스 연결을 종료합니다.")
        print("리소스 정리가 완료되었습니다.")

# 테스트
result = connect_to_database()
print(f"결과: {result}")
```

## 14-4 예외 발생시키기 (raise)

특정 조건에서 의도적으로 예외를 발생시킬 수 있습니다. 이는 프로그램의 논리적 흐름을 제어하거나 잘못된 사용을 방지하는 데 유용합니다.

```python title=raise_exceptions.py
def validate_age(age):
    """나이 유효성 검사"""
    if not isinstance(age, int):
        raise TypeError("나이는 정수여야 합니다.")

    if age < 0:
        raise ValueError("나이는 0 이상이어야 합니다.")

    if age > 150:
        raise ValueError("나이는 150 이하여야 합니다.")

    return True

def create_account(name, age):
    """계정 생성 함수"""
    try:
        # 입력값 검증
        if not name or not name.strip():
            raise ValueError("이름은 비워둘 수 없습니다.")

        validate_age(age)

        print(f"계정이 생성되었습니다: {name} ({age}세)")
        return {"name": name, "age": age}

    except (TypeError, ValueError) as e:
        print(f"계정 생성 실패: {e}")
        return None

# 테스트
create_account("김파이", 25)      # 성공
create_account("", 25)           # 이름 오류
create_account("이파이", -5)      # 나이 오류
create_account("박파이", "20")    # 타입 오류
```

### assert문 활용

```python title=assert_example.py
def calculate_average(scores):
    """점수 평균 계산"""
    # 개발 중 디버깅을 위한 assert문
    assert isinstance(scores, list), "점수는 리스트여야 합니다."
    assert len(scores) > 0, "점수 리스트가 비어있습니다."
    assert all(isinstance(score, (int, float)) for score in scores), "모든 점수는 숫자여야 합니다."

    return sum(scores) / len(scores)

# 테스트
try:
    print(calculate_average([85, 90, 78]))  # 정상
    print(calculate_average([]))           # AssertionError
except AssertionError as e:
    print(f"검증 실패: {e}")
```

## 14-5 사용자 정의 예외

특별한 상황을 나타내기 위해 자신만의 예외 클래스를 만들 수 있습니다. 이를 통해 더 명확하고 의미 있는 예외 처리가 가능합니다.

```python title=custom_exceptions.py
class BankAccountError(Exception):
    """은행 계좌 관련 기본 예외"""
    pass

class InsufficientFundsError(BankAccountError):
    """잔액 부족 예외"""
    def __init__(self, current_balance, requested_amount):
        self.current_balance = current_balance
        self.requested_amount = requested_amount
        message = f"잔액 부족: 현재 잔액 {current_balance}원, 요청 금액 {requested_amount}원"
        super().__init__(message)

class InvalidAmountError(BankAccountError):
    """잘못된 금액 예외"""
    pass

class BankAccount:
    """은행 계좌 클래스"""
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self.balance = initial_balance

    def deposit(self, amount):
        """입금"""
        if amount <= 0:
            raise InvalidAmountError("입금액은 0보다 커야 합니다.")

        self.balance += amount
        print(f"{amount}원이 입금되었습니다. 현재 잔액: {self.balance}원")

    def withdraw(self, amount):
        """출금"""
        if amount <= 0:
            raise InvalidAmountError("출금액은 0보다 커야 합니다.")

        if amount > self.balance:
            raise InsufficientFundsError(self.balance, amount)

        self.balance -= amount
        print(f"{amount}원이 출금되었습니다. 현재 잔액: {self.balance}원")

# 사용 예제
def bank_transaction_demo():
    """은행 거래 데모"""
    account = BankAccount("123-456-789", 10000)

    try:
        account.deposit(5000)      # 성공
        account.withdraw(3000)     # 성공
        account.withdraw(20000)    # 잔액 부족 오류
    except InsufficientFundsError as e:
        print(f"거래 실패: {e}")
        print(f"부족한 금액: {e.requested_amount - e.current_balance}원")
    except InvalidAmountError as e:
        print(f"입력 오류: {e}")
    except BankAccountError as e:
        print(f"계좌 오류: {e}")

bank_transaction_demo()
```

## 실습: 안전한 계산기 프로그램

실제 예외 처리를 활용한 계산기 프로그램을 만들어보겠습니다. 이 예제는 다양한 입력 오류를 처리하고 사용자 친화적인 인터페이스를 제공합니다.

```python title=safe_calculator.py
class CalculatorError(Exception):
    """계산기 오류 기본 클래스"""
    pass

class DivisionByZeroError(CalculatorError):
    """0으로 나누기 오류"""
    pass

class InvalidOperationError(CalculatorError):
    """잘못된 연산 오류"""
    pass

class SafeCalculator:
    """안전한 계산기 클래스"""

    def __init__(self):
        self.operations = {
            '+': self.add,
            '-': self.subtract,
            '*': self.multiply,
            '/': self.divide,
            '**': self.power,
            '%': self.modulo
        }

    def add(self, a, b):
        return a + b

    def subtract(self, a, b):
        return a - b

    def multiply(self, a, b):
        return a * b

    def divide(self, a, b):
        if b == 0:
            raise DivisionByZeroError("0으로 나눌 수 없습니다.")
        return a / b

    def power(self, a, b):
        try:
            return a ** b
        except OverflowError:
            raise CalculatorError("결과가 너무 큽니다.")

    def modulo(self, a, b):
        if b == 0:
            raise DivisionByZeroError("0으로 나눌 수 없습니다.")
        return a % b

    def calculate(self, expression):
        """수식 계산"""
        try:
            # 간단한 수식 파싱 (실제로는 더 정교한 파서가 필요)
            parts = expression.replace(' ', '').split()
            if len(parts) == 1:
                # 단일 숫자
                return float(parts[0])

            # 연산자 찾기
            operator = None
            for op in ['**', '+', '-', '*', '/', '%']:
                if op in expression:
                    operator = op
                    break

            if not operator:
                raise InvalidOperationError("지원하지 않는 연산입니다.")

            # 피연산자 분리
            operands = expression.split(operator)
            if len(operands) != 2:
                raise InvalidOperationError("잘못된 수식 형식입니다.")

            num1 = float(operands[0].strip())
            num2 = float(operands[1].strip())

            # 계산 실행
            operation_func = self.operations.get(operator)
            if not operation_func:
                raise InvalidOperationError(f"지원하지 않는 연산자: {operator}")

            return operation_func(num1, num2)

        except ValueError:
            raise CalculatorError("잘못된 숫자 형식입니다.")

def main():
    """메인 함수"""
    calculator = SafeCalculator()
    print("=== 안전한 계산기 ===")
    print("사용법: 숫자 연산자 숫자 (예: 10 + 5)")
    print("지원 연산자: +, -, *, /, **, %")
    print("종료하려면 'quit' 입력")

    while True:
        try:
            user_input = input("\n수식을 입력하세요: ").strip()

            if user_input.lower() == 'quit':
                print("계산기를 종료합니다.")
                break

            if not user_input:
                print("수식을 입력해주세요.")
                continue

            result = calculator.calculate(user_input)
            print(f"결과: {result}")

        except DivisionByZeroError as e:
            print(f"수학 오류: {e}")
        except InvalidOperationError as e:
            print(f"연산 오류: {e}")
        except CalculatorError as e:
            print(f"계산기 오류: {e}")
        except KeyboardInterrupt:
            print("\n\n계산기를 종료합니다.")
            break
        except Exception as e:
            print(f"예상치 못한 오류가 발생했습니다: {e}")

if __name__ == "__main__":
    main()
```

이번 챕터에서는 파이썬의 예외 처리 메커니즘을 자세히 살펴보았습니다. try-except문의 기본 사용법부터 사용자 정의 예외까지, 예외 처리의 모든 측면을 다뤘습니다. 예외 처리는 단순히 오류를 피하는 것이 아니라, 프로그램을 더 견고하고 사용자 친화적으로 만드는 중요한 기법입니다. 실제 프로젝트에서는 예상 가능한 모든 예외 상황을 고려하여 적절한 처리 방법을 구현하는 것이 중요합니다.

## 연습문제

1. 사용자로부터 두 개의 숫자를 입력받아 나눗셈을 수행하는 프로그램을 작성하세요. 0으로 나누기 오류와 잘못된 입력을 처리하세요.

2. 파일을 읽어서 숫자들의 합을 계산하는 프로그램을 작성하세요. 파일이 존재하지 않거나 올바르지 않은 형식일 때의 예외를 처리하세요.

3. 학생의 성적을 관리하는 클래스를 만들고, 성적 범위 검증을 위한 사용자 정의 예외를 구현하세요.

4. ATM 기계를 시뮬레이션하는 프로그램을 작성하세요. 잔액 부족, 잘못된 비밀번호 등의 상황을 예외로 처리하세요.
