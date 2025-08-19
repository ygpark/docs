---
title: '5. 컬렉션 자료형'
slug: python-collection-data-types
description: '리스트, 튜플, 딕셔너리, 집합 등 파이썬 컬렉션 자료형의 특징과 활용법을 실무 예제로 완전 정복'
keywords:
  [파이썬 리스트, 파이썬 튜플, 파이썬 딕셔너리, 파이썬 집합, 컬렉션 자료형, list tuple dict set]
sidebar_position: 5
---

# 5. 컬렉션 자료형

## 학습 목표

이 챕터를 완료하면 다음과 같은 능력을 갖추게 됩니다:

- 리스트, 튜플, 딕셔너리, 집합의 특징과 차이점을 이해하고 상황에 맞게 선택할 수 있습니다
- 각 자료형의 핵심 메서드와 연산을 자유자재로 활용할 수 있습니다
- 실제 프로그래밍에서 데이터를 효율적으로 저장하고 관리하는 방법을 습득합니다
- 학생 성적 관리 프로그램을 통해 컬렉션 자료형을 종합적으로 활용할 수 있습니다

---

지금까지 우리는 숫자, 문자열, 불린 같은 기본 자료형을 다뤘습니다. 이제는 여러 개의 데이터를 하나로 묶어서 관리할 수 있는 컬렉션 자료형을 배워보겠습니다. 컬렉션 자료형은 실제 프로그래밍에서 가장 자주 사용되는 핵심 도구입니다. 학생들의 성적을 관리하거나, 쇼핑 목록을 만들거나, 사용자 정보를 저장할 때 모두 컬렉션 자료형을 사용합니다. 각각의 특징을 이해하고 상황에 맞는 자료형을 선택하는 것이 효율적인 프로그래밍의 첫걸음입니다.

## 5-1 리스트 기본 개념과 활용

리스트는 파이썬에서 가장 유연하고 자주 사용되는 컬렉션 자료형입니다. 여러 개의 값을 순서대로 저장할 수 있고, 언제든지 내용을 변경할 수 있습니다. 대괄호 []를 사용해서 만들며, 각 요소는 쉼표로 구분합니다.

```python title=list_basics.py
# 리스트 생성하기
fruits = ["사과", "바나나", "오렌지"]
numbers = [1, 2, 3, 4, 5]
mixed = ["홍길동", 25, True, 3.14]  # 다양한 자료형 혼합 가능

# 빈 리스트 생성
empty_list = []
empty_list2 = list()

print(fruits)  # ['사과', '바나나', '오렌지']
print(len(fruits))  # 3

# 리스트 인덱싱과 슬라이싱
print(fruits[0])     # 사과 (첫 번째 요소)
print(fruits[-1])    # 오렌지 (마지막 요소)
print(fruits[1:3])   # ['바나나', '오렌지']

# 리스트 수정하기
fruits[0] = "딸기"
print(fruits)  # ['딸기', '바나나', '오렌지']

# 요소 추가하기
fruits.append("포도")  # 맨 끝에 추가
print(fruits)  # ['딸기', '바나나', '오렌지', '포도']

fruits.insert(1, "키위")  # 인덱스 1에 삽입
print(fruits)  # ['딸기', '키위', '바나나', '오렌지', '포도']

# 요소 제거하기
fruits.remove("바나나")  # 값으로 제거
print(fruits)  # ['딸기', '키위', '오렌지', '포도']

popped = fruits.pop()  # 마지막 요소 제거하고 반환
print(f"제거된 과일: {popped}")  # 제거된 과일: 포도
print(fruits)  # ['딸기', '키위', '오렌지']

# 리스트 연산
numbers1 = [1, 2, 3]
numbers2 = [4, 5, 6]
combined = numbers1 + numbers2  # [1, 2, 3, 4, 5, 6]
repeated = numbers1 * 3  # [1, 2, 3, 1, 2, 3, 1, 2, 3]

# 유용한 리스트 메서드
scores = [85, 92, 78, 96, 88]
print(f"최고 점수: {max(scores)}")  # 96
print(f"최저 점수: {min(scores)}")  # 78
print(f"평균 점수: {sum(scores) / len(scores)}")  # 87.8

scores.sort()  # 오름차순 정렬
print(scores)  # [78, 85, 88, 92, 96]

scores.reverse()  # 순서 뒤집기
print(scores)  # [96, 92, 88, 85, 78]
```

## 5-2 튜플의 특징과 활용

튜플은 리스트와 비슷하지만 한 번 만들어지면 내용을 변경할 수 없는 불변 자료형입니다. 소괄호 ()를 사용해서 만들며, 변경되면 안 되는 데이터를 저장할 때 사용합니다. 좌표, 설정값, 함수의 여러 반환값 등에 주로 활용됩니다.

```python title=tuple_usage.py
# 튜플 생성하기
point = (3, 5)  # 좌표
person = ("김철수", 30, "서울")  # 이름, 나이, 주소
colors = ("빨강", "파랑", "노랑")

# 요소가 하나인 튜플은 쉼표를 붙여야 함
single = (42,)  # 또는 42,
print(type(single))  # <class 'tuple'>

# 튜플 언패킹 (매우 유용!)
x, y = point
print(f"x좌표: {x}, y좌표: {y}")  # x좌표: 3, y좌표: 5

name, age, city = person
print(f"{name}님은 {age}세이고 {city}에 살고 있습니다")

# 튜플 인덱싱과 슬라이싱 (리스트와 동일)
print(colors[0])    # 빨강
print(colors[-1])   # 노랑
print(colors[1:])   # ('파랑', '노랑')

# 튜플은 변경 불가능
# colors[0] = "초록"  # TypeError 발생!

# 튜플 메서드
numbers = (1, 2, 2, 3, 2, 4)
print(numbers.count(2))  # 3 (2의 개수)
print(numbers.index(3))  # 3 (3의 인덱스)

# 함수에서 여러 값 반환하기
def get_name_age():
    return "이영희", 25

name, age = get_name_age()
print(f"{name}, {age}세")  # 이영희, 25세

# 튜플을 활용한 변수 교환
a, b = 10, 20
a, b = b, a  # 간단한 변수 교환!
print(f"a: {a}, b: {b}")  # a: 20, b: 10
```

## 5-3 딕셔너리 완전 활용

딕셔너리는 키(key)와 값(value)의 쌍으로 데이터를 저장하는 자료형입니다. 중괄호 {}를 사용하며, 실제 사전처럼 단어(키)를 찾으면 뜻(값)을 얻을 수 있습니다. 데이터베이스의 레코드나 객체의 속성을 표현할 때 매우 유용합니다.

```python title=dictionary_mastery.py
# 딕셔너리 생성하기
student = {
    "이름": "박민수",
    "나이": 20,
    "학과": "컴퓨터공학",
    "성적": 3.8
}

# 빈 딕셔너리 생성
empty_dict = {}
empty_dict2 = dict()

# 값 접근하기
print(student["이름"])  # 박민수
print(student.get("나이"))  # 20
print(student.get("전화번호", "정보 없음"))  # 정보 없음 (기본값)

# 값 수정하고 추가하기
student["나이"] = 21  # 수정
student["전화번호"] = "010-1234-5678"  # 추가
print(student)

# 키 존재 확인
if "성적" in student:
    print(f"성적: {student['성적']}")

# 딕셔너리 메서드 활용
print("모든 키:", list(student.keys()))
print("모든 값:", list(student.values()))
print("키-값 쌍:", list(student.items()))

# 딕셔너리 순회하기
for key in student:
    print(f"{key}: {student[key]}")

# 더 깔끔한 순회 방법
for key, value in student.items():
    print(f"{key}: {value}")

# 딕셔너리 병합 (Python 3.9+)
contact = {"이메일": "minsu@example.com", "주소": "서울시 강남구"}
student.update(contact)  # 또는 student = student | contact

# 중첩 딕셔너리
class_info = {
    "1학년": {
        "학생수": 30,
        "담임": "김선생",
        "과목": ["국어", "영어", "수학"]
    },
    "2학년": {
        "학생수": 28,
        "담임": "이선생",
        "과목": ["국어", "영어", "수학", "과학"]
    }
}

print(class_info["1학년"]["담임"])  # 김선생
print(class_info["2학년"]["과목"][2])  # 수학

# 딕셔너리 컴프리헨션
numbers = [1, 2, 3, 4, 5]
squares = {num: num**2 for num in numbers}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

## 5-4 집합(Set)과 집합 연산

집합은 중복을 허용하지 않는 데이터의 모음입니다. 수학의 집합과 같은 개념으로, 합집합, 교집합, 차집합 같은 연산을 수행할 수 있습니다. 중복 제거나 멤버십 테스트에 매우 효율적입니다.

```python title=set_operations.py
# 집합 생성하기
fruits = {"사과", "바나나", "오렌지", "사과"}  # 중복 자동 제거
print(fruits)  # {'오렌지', '바나나', '사과'} (순서는 무작위)

numbers = set([1, 2, 3, 3, 4, 4, 5])
print(numbers)  # {1, 2, 3, 4, 5}

# 빈 집합 생성 (주의: {}는 딕셔너리!)
empty_set = set()

# 집합 연산
set_a = {1, 2, 3, 4, 5}
set_b = {4, 5, 6, 7, 8}

# 합집합 (union)
union = set_a | set_b  # 또는 set_a.union(set_b)
print(f"합집합: {union}")  # {1, 2, 3, 4, 5, 6, 7, 8}

# 교집합 (intersection)
intersection = set_a & set_b  # 또는 set_a.intersection(set_b)
print(f"교집합: {intersection}")  # {4, 5}

# 차집합 (difference)
difference = set_a - set_b  # 또는 set_a.difference(set_b)
print(f"차집합: {difference}")  # {1, 2, 3}

# 대칭 차집합 (symmetric difference)
sym_diff = set_a ^ set_b  # 또는 set_a.symmetric_difference(set_b)
print(f"대칭 차집합: {sym_diff}")  # {1, 2, 3, 6, 7, 8}

# 집합 메서드
colors = {"빨강", "파랑", "노랑"}
colors.add("초록")  # 요소 추가
colors.remove("빨강")  # 요소 제거 (없으면 에러)
colors.discard("보라")  # 요소 제거 (없어도 에러 없음)

# 멤버십 테스트 (매우 빠름!)
if "파랑" in colors:
    print("파랑색이 있습니다")

# 집합의 관계
small_set = {1, 2}
large_set = {1, 2, 3, 4, 5}

print(small_set.issubset(large_set))    # True (부분집합)
print(large_set.issuperset(small_set))  # True (상위집합)
print(small_set.isdisjoint({6, 7, 8}))  # True (교집합 없음)

# 실용적인 예제: 중복 제거
duplicate_list = [1, 2, 2, 3, 3, 3, 4, 5]
unique_list = list(set(duplicate_list))
print(f"중복 제거: {unique_list}")
```

## 5-5 자료형별 특징 비교

각 컬렉션 자료형의 특징을 비교해보고 언제 어떤 것을 사용해야 하는지 알아보겠습니다. 올바른 자료형 선택은 프로그램의 성능과 가독성에 큰 영향을 미칩니다.

```python title=collection_comparison.py
# 특징 비교 예제
data = "파이썬은 재미있고 파이썬은 유용하다"

# 1. 리스트: 순서 있음, 변경 가능, 중복 허용
word_list = data.split()
print(f"리스트: {word_list}")
print(f"첫 번째 단어: {word_list[0]}")
word_list.append("정말로")  # 수정 가능
print(f"수정 후: {word_list}")

# 2. 튜플: 순서 있음, 변경 불가능, 중복 허용
word_tuple = tuple(word_list)
print(f"튜플: {word_tuple}")
print(f"첫 번째 단어: {word_tuple[0]}")
# word_tuple[0] = "Java"  # 에러! 변경 불가능

# 3. 집합: 순서 없음, 변경 가능, 중복 불허
word_set = set(word_list)
print(f"집합: {word_set}")  # 중복된 '파이썬은' 하나만 남음
word_set.add("멋지다")  # 수정 가능
print(f"수정 후: {word_set}")

# 4. 딕셔너리: 키-값 쌍, 변경 가능, 키 중복 불허
word_count = {}
for word in word_list:
    word_count[word] = word_count.get(word, 0) + 1
print(f"단어 빈도: {word_count}")

# 성능 비교 (간단한 예제)
import time

# 큰 데이터에서 멤버십 테스트
large_list = list(range(10000))
large_set = set(range(10000))

# 리스트에서 검색 (느림)
start = time.time()
9999 in large_list
list_time = time.time() - start

# 집합에서 검색 (빠름)
start = time.time()
9999 in large_set
set_time = time.time() - start

print(f"리스트 검색 시간: {list_time:.6f}초")
print(f"집합 검색 시간: {set_time:.6f}초")
print(f"집합이 약 {list_time/set_time:.0f}배 빠름")

# 언제 무엇을 사용할까?
print("\n=== 자료형 선택 가이드 ===")
print("리스트: 순서가 중요하고 중복이 필요하며 내용을 자주 변경하는 경우")
print("튜플: 순서가 중요하지만 한 번 정하면 변경하지 않는 데이터")
print("딕셔너리: 키를 통해 값을 빠르게 찾아야 하는 경우")
print("집합: 중복을 제거하거나 집합 연산이 필요한 경우")
```

## 실습: 학생 성적 관리 프로그램

지금까지 배운 모든 컬렉션 자료형을 활용해서 종합적인 학생 성적 관리 프로그램을 만들어보겠습니다. 이 프로그램은 실제 교육 현장에서 사용할 수 있을 정도로 완성도 있게 구현해보겠습니다.

```python title=student_grade_manager.py
# 학생 성적 관리 프로그램
class GradeManager:
    def __init__(self):
        # 딕셔너리로 학생 정보 저장
        self.students = {}
        # 과목 목록 (집합으로 중복 방지)
        self.subjects = {"국어", "영어", "수학", "과학", "사회"}

    def add_student(self, student_id, name):
        """새 학생 추가"""
        if student_id not in self.students:
            self.students[student_id] = {
                "이름": name,
                "성적": {},  # 과목별 성적
                "출석일": []  # 출석한 날짜들
            }
            print(f"{name} 학생이 추가되었습니다.")
        else:
            print("이미 존재하는 학번입니다.")

    def add_grade(self, student_id, subject, score):
        """성적 추가"""
        if student_id in self.students and subject in self.subjects:
            if 0 <= score <= 100:
                self.students[student_id]["성적"][subject] = score
                print(f"{subject} 성적이 추가되었습니다.")
            else:
                print("성적은 0-100 사이여야 합니다.")
        else:
            print("존재하지 않는 학생이거나 과목입니다.")

    def get_student_average(self, student_id):
        """학생 평균 점수 계산"""
        if student_id in self.students:
            grades = self.students[student_id]["성적"]
            if grades:
                return sum(grades.values()) / len(grades)
            else:
                return 0
        return None

    def get_subject_average(self, subject):
        """과목별 전체 평균 계산"""
        if subject not in self.subjects:
            return None

        scores = []
        for student in self.students.values():
            if subject in student["성적"]:
                scores.append(student["성적"][subject])

        return sum(scores) / len(scores) if scores else 0

    def get_top_students(self, n=3):
        """상위 n명 학생 반환"""
        student_averages = []
        for student_id, student_info in self.students.items():
            avg = self.get_student_average(student_id)
            if avg > 0:
                student_averages.append((student_id, student_info["이름"], avg))

        # 평균 점수로 정렬 (내림차순)
        student_averages.sort(key=lambda x: x[2], reverse=True)
        return student_averages[:n]

    def print_report(self):
        """전체 성적 보고서 출력"""
        print("\n" + "="*50)
        print("           학생 성적 관리 시스템 보고서")
        print("="*50)

        # 전체 통계
        total_students = len(self.students)
        print(f"총 학생 수: {total_students}명")

        # 과목별 평균
        print("\n[ 과목별 평균 점수 ]")
        for subject in sorted(self.subjects):
            avg = self.get_subject_average(subject)
            print(f"{subject}: {avg:.1f}점")

        # 상위 학생들
        print("\n[ 상위 3명 ]")
        top_students = self.get_top_students(3)
        for i, (student_id, name, avg) in enumerate(top_students, 1):
            print(f"{i}등: {name} ({student_id}) - 평균 {avg:.1f}점")

        # 개별 학생 상세 정보
        print("\n[ 학생별 상세 성적 ]")
        for student_id, student_info in self.students.items():
            name = student_info["이름"]
            grades = student_info["성적"]
            avg = self.get_student_average(student_id)

            print(f"\n{name} ({student_id}) - 평균: {avg:.1f}점")
            for subject in sorted(self.subjects):
                score = grades.get(subject, "미응시")
                print(f"  {subject}: {score}")

# 프로그램 사용 예제
def main():
    # 성적 관리자 생성
    manager = GradeManager()

    # 학생 추가
    students_data = [
        ("2024001", "김철수"),
        ("2024002", "이영희"),
        ("2024003", "박민수"),
        ("2024004", "최지원"),
        ("2024005", "정다은")
    ]

    for student_id, name in students_data:
        manager.add_student(student_id, name)

    # 성적 데이터 (학번, 과목, 점수)
    grades_data = [
        ("2024001", "국어", 85), ("2024001", "영어", 92), ("2024001", "수학", 78),
        ("2024002", "국어", 94), ("2024002", "영어", 88), ("2024002", "수학", 96),
        ("2024003", "국어", 76), ("2024003", "영어", 84), ("2024003", "수학", 82),
        ("2024004", "국어", 90), ("2024004", "영어", 95), ("2024004", "수학", 89),
        ("2024005", "국어", 88), ("2024005", "영어", 86), ("2024005", "수학", 91),
    ]

    # 성적 입력
    for student_id, subject, score in grades_data:
        manager.add_grade(student_id, subject, score)

    # 보고서 출력
    manager.print_report()

    # 추가 분석
    print("\n[ 추가 분석 ]")
    math_avg = manager.get_subject_average("수학")
    print(f"수학 과목 평균: {math_avg:.1f}점")

    # 수학 90점 이상 학생 찾기
    high_math_students = []
    for student_id, student_info in manager.students.items():
        math_score = student_info["성적"].get("수학", 0)
        if math_score >= 90:
            high_math_students.append((student_info["이름"], math_score))

    print("수학 90점 이상 학생:")
    for name, score in high_math_students:
        print(f"  {name}: {score}점")

if __name__ == "__main__":
    main()
```

## 연습문제

1. **쇼핑 카트 프로그램**: 딕셔너리를 사용해서 상품명을 키로, (가격, 수량)을 값으로 하는 쇼핑 카트를 만들어보세요. 상품 추가, 수량 변경, 총액 계산 기능을 구현해보세요.

2. **텍스트 분석기**: 문장을 입력받아서 단어별 빈도수를 딕셔너리로 저장하고, 가장 많이 사용된 단어 3개를 찾는 프로그램을 작성해보세요.

3. **학급 관리 시스템**: 여러 학급의 정보를 중첩 딕셔너리로 저장하고, 학급별 평균 점수, 전체 평균 점수를 계산하는 프로그램을 만들어보세요.

4. **집합 연산 문제**: 두 개의 과목을 수강하는 학생 명단이 있을 때, 둘 다 수강하는 학생, 한 과목만 수강하는 학생을 찾는 프로그램을 집합 연산으로 구현해보세요.

이번 챕터에서는 파이썬의 핵심 컬렉션 자료형들을 모두 배웠습니다. 각각의 특징을 이해하고 상황에 맞게 활용할 수 있다면, 더욱 효율적이고 읽기 쉬운 코드를 작성할 수 있을 것입니다. 다음 챕터에서는 이런 데이터들을 다루는 조건문에 대해서 알아보겠습니다!
