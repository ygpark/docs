---
title: '13. 파일 입출력'
slug: python-file-input-output
description: '텍스트 파일, CSV, JSON 처리와 pathlib를 활용한 현대적인 파일 입출력 방법 완전 마스터'
keywords: [파이썬 파일 입출력, with문, CSV 처리, JSON 데이터, pathlib, 파일 읽기 쓰기]
sidebar_position: 13
---

# 13. 파일 입출력

파일 입출력은 프로그래밍에서 가장 기본적이면서도 중요한 기능 중 하나입니다. 우리가 작성한 데이터를 파일에 저장하고, 저장된 데이터를 다시 읽어오는 작업은 거의 모든 프로그램에서 필요한 기능이죠. 이번 챕터에서는 파이썬에서 다양한 형태의 파일을 읽고 쓰는 방법을 배워보겠습니다. 텍스트 파일부터 CSV, JSON 파일까지 실무에서 자주 사용하는 파일 형식들을 다뤄보고, 현대적인 파일 처리 방법인 pathlib 모듈도 함께 살펴보겠습니다.

## 학습 목표

이번 챕터를 마치면 다음과 같은 것들을 할 수 있게 됩니다:

- 텍스트 파일을 안전하게 읽고 쓸 수 있습니다
- with문을 활용한 효율적인 파일 처리 방법을 이해합니다
- CSV 파일을 처리하여 표 형태의 데이터를 다룰 수 있습니다
- JSON 형식의 데이터를 파이썬 객체와 상호 변환할 수 있습니다
- pathlib를 활용한 현대적인 파일 경로 처리 방법을 익힙니다

---

## 13-1 파일 읽기와 쓰기

파이썬에서 파일을 다루는 것은 생각보다 간단합니다. 기본적으로 `open()` 함수를 사용하여 파일을 열고, 읽거나 쓴 후에 `close()` 함수로 파일을 닫아주면 됩니다. 파일을 열 때는 어떤 목적으로 파일을 여는지(읽기, 쓰기, 추가 등)를 모드로 지정해야 합니다. 또한 텍스트 파일의 경우 한글이 깨지지 않도록 인코딩을 지정하는 것이 중요합니다.

### 파일 쓰기

먼저 파일에 데이터를 저장하는 방법부터 알아보겠습니다.

```python title=write_file.py
# 기본적인 파일 쓰기
file = open('diary.txt', 'w', encoding='utf-8')
file.write('오늘은 파이썬 파일 입출력을 배웠다.\n')
file.write('정말 재미있었다!\n')
file.close()  # 반드시 파일을 닫아야 함

print("파일이 생성되었습니다!")
```

### 파일 읽기

이제 저장된 파일을 읽어보겠습니다.

```python title=read_file.py
# 파일 전체 읽기
file = open('diary.txt', 'r', encoding='utf-8')
content = file.read()
print(content)
file.close()

print("-" * 30)

# 한 줄씩 읽기
file = open('diary.txt', 'r', encoding='utf-8')
for line in file:
    print(f"읽은 줄: {line.strip()}")  # strip()으로 줄바꿈 문자 제거
file.close()
```

### 파일 모드 종류

```python title=file_modes.py
# 다양한 파일 모드 예제

# 'w' - 쓰기 모드 (기존 내용 덮어쓰기)
with open('test.txt', 'w', encoding='utf-8') as f:
    f.write('첫 번째 내용\n')

# 'a' - 추가 모드 (기존 내용에 추가)
with open('test.txt', 'a', encoding='utf-8') as f:
    f.write('두 번째 내용\n')

# 'r' - 읽기 모드
with open('test.txt', 'r', encoding='utf-8') as f:
    print("파일 내용:")
    print(f.read())
```

---

## 13-2 with문 활용

파일을 다룰 때 가장 중요한 것은 사용 후 반드시 파일을 닫아주는 것입니다. 하지만 프로그램에서 오류가 발생하면 `close()` 함수가 실행되지 않을 수 있습니다. 이런 문제를 해결하기 위해 파이썬에서는 `with문`을 제공합니다. with문을 사용하면 블록을 벗어날 때 자동으로 파일이 닫히므로 안전하고 깔끔한 코드를 작성할 수 있습니다.

```python title=with_statement.py
# with문을 사용한 안전한 파일 처리

# 파일 쓰기
with open('student_info.txt', 'w', encoding='utf-8') as f:
    f.write('이름: 김파이썬\n')
    f.write('나이: 20\n')
    f.write('전공: 컴퓨터공학\n')
# 여기서 자동으로 파일이 닫힘

# 파일 읽기
with open('student_info.txt', 'r', encoding='utf-8') as f:
    lines = f.readlines()  # 모든 줄을 리스트로 읽기

for i, line in enumerate(lines, 1):
    print(f"{i}번째 줄: {line.strip()}")

# 여러 파일을 동시에 처리
with open('input.txt', 'r', encoding='utf-8') as input_file, \
     open('output.txt', 'w', encoding='utf-8') as output_file:

    data = input_file.read()
    output_file.write(f"처리된 데이터: {data.upper()}")
```

---

## 13-3 CSV 파일 다루기

CSV(Comma-Separated Values) 파일은 표 형태의 데이터를 저장하는 가장 일반적인 형식입니다. 엑셀 파일보다 용량이 작고 다양한 프로그램에서 지원하기 때문에 데이터 교환 시 자주 사용됩니다. 파이썬의 csv 모듈을 사용하면 CSV 파일을 쉽게 읽고 쓸 수 있습니다. 특히 학생 성적, 상품 목록, 고객 정보 등의 테이블 데이터를 처리할 때 매우 유용합니다.

### CSV 파일 쓰기

```python title=write_csv.py
import csv

# 학생 성적 데이터 생성
students = [
    ['이름', '국어', '영어', '수학'],
    ['김철수', 85, 90, 78],
    ['이영희', 92, 88, 95],
    ['박민수', 78, 85, 82],
    ['정수진', 96, 92, 89]
]

# CSV 파일로 저장
with open('scores.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(students)

print("성적 CSV 파일이 생성되었습니다!")

# 딕셔너리를 이용한 CSV 쓰기
students_dict = [
    {'이름': '최지호', '국어': 88, '영어': 91, '수학': 86},
    {'이름': '한소영', '국어': 94, '영어': 87, '수학': 93}
]

with open('scores_dict.csv', 'w', newline='', encoding='utf-8') as f:
    fieldnames = ['이름', '국어', '영어', '수학']
    writer = csv.DictWriter(f, fieldnames=fieldnames)

    writer.writeheader()  # 헤더 쓰기
    writer.writerows(students_dict)
```

### CSV 파일 읽기

```python title=read_csv.py
import csv

print("=== CSV 파일 읽기 ===")

# 기본 CSV 읽기
with open('scores.csv', 'r', encoding='utf-8') as f:
    reader = csv.reader(f)

    for row_num, row in enumerate(reader):
        if row_num == 0:
            print(f"헤더: {row}")
        else:
            name, korean, english, math = row
            total = int(korean) + int(english) + int(math)
            average = total / 3
            print(f"{name}: 총점 {total}, 평균 {average:.1f}")

print("\n=== 딕셔너리로 CSV 읽기 ===")

# 딕셔너리를 이용한 CSV 읽기
with open('scores.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)

    for row in reader:
        name = row['이름']
        korean = int(row['국어'])
        english = int(row['영어'])
        math = int(row['수학'])

        total = korean + english + math
        print(f"{name}: 총점 {total}")
```

---

## 13-4 JSON 파일 다루기

JSON(JavaScript Object Notation)은 웹 개발에서 데이터를 주고받을 때 가장 많이 사용되는 형식입니다. 파이썬의 딕셔너리와 리스트 구조와 매우 유사하여 파이썬에서 다루기 쉽고, 사람이 읽기에도 편합니다. API 통신, 설정 파일, 데이터 저장 등 다양한 용도로 활용됩니다. json 모듈을 사용하면 파이썬 객체를 JSON 문자열로 변환하거나 그 반대로 변환할 수 있습니다.

### JSON 파일 쓰기

```python title=write_json.py
import json

# 학생 정보 딕셔너리
student_data = {
    "학교": "파이썬고등학교",
    "학년": 2,
    "학생들": [
        {
            "이름": "김코딩",
            "나이": 17,
            "성적": {
                "국어": 85,
                "영어": 90,
                "수학": 78
            },
            "취미": ["독서", "게임", "영화감상"]
        },
        {
            "이름": "이프로그램",
            "나이": 16,
            "성적": {
                "국어": 92,
                "영어": 88,
                "수학": 95
            },
            "취미": ["음악", "그림", "코딩"]
        }
    ]
}

# JSON 파일로 저장
with open('students.json', 'w', encoding='utf-8') as f:
    json.dump(student_data, f, ensure_ascii=False, indent=2)

print("학생 정보 JSON 파일이 생성되었습니다!")

# JSON 문자열로 변환 (파일이 아닌 문자열)
json_string = json.dumps(student_data, ensure_ascii=False, indent=2)
print("JSON 문자열 예시:")
print(json_string[:200] + "...")
```

### JSON 파일 읽기

```python title=read_json.py
import json

print("=== JSON 파일 읽기 ===")

# JSON 파일 읽기
with open('students.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

print(f"학교: {data['학교']}")
print(f"학년: {data['학년']}학년")
print()

# 학생 정보 출력
for student in data['학생들']:
    name = student['이름']
    age = student['나이']
    scores = student['성적']
    hobbies = student['취미']

    # 평균 점수 계산
    total_score = sum(scores.values())
    average = total_score / len(scores)

    print(f"이름: {name} ({age}세)")
    print(f"성적: {scores}")
    print(f"평균: {average:.1f}점")
    print(f"취미: {', '.join(hobbies)}")
    print("-" * 30)
```

---

## 13-5 pathlib를 활용한 현대적 파일 처리

파이썬 3.4부터 도입된 pathlib 모듈은 파일 경로를 객체지향적으로 다룰 수 있게 해주는 현대적인 도구입니다. 기존의 os.path 모듈보다 직관적이고 사용하기 쉽습니다. 경로 조작, 파일 존재 여부 확인, 파일 정보 조회 등의 작업을 더 간단하고 안전하게 수행할 수 있습니다. 특히 크로스 플랫폼 개발에서 경로 구분자 문제를 자동으로 해결해주므로 매우 유용합니다.

```python title=pathlib_example.py
from pathlib import Path
import json
import csv

print("=== pathlib를 활용한 파일 처리 ===")

# 현재 디렉토리 정보
current_dir = Path.cwd()
print(f"현재 디렉토리: {current_dir}")

# 파일 경로 생성 (운영체제에 상관없이 작동)
data_dir = Path("data")
json_file = data_dir / "config.json"
csv_file = data_dir / "users.csv"

# 디렉토리 생성 (없으면 생성)
data_dir.mkdir(exist_ok=True)

# JSON 파일 생성 및 쓰기
config_data = {
    "앱_이름": "나의 첫 파이썬 앱",
    "버전": "1.0.0",
    "설정": {
        "언어": "한국어",
        "테마": "다크모드",
        "자동저장": True
    }
}

# pathlib의 write_text 메서드 사용
json_file.write_text(
    json.dumps(config_data, ensure_ascii=False, indent=2),
    encoding='utf-8'
)

# CSV 파일 생성
users_data = [
    ["사용자ID", "이름", "이메일"],
    ["user001", "김철수", "kim@example.com"],
    ["user002", "이영희", "lee@example.com"],
    ["user003", "박민수", "park@example.com"]
]

with csv_file.open('w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(users_data)

# 파일 정보 확인
print(f"\n=== 파일 정보 ===")
for file_path in [json_file, csv_file]:
    if file_path.exists():
        print(f"파일명: {file_path.name}")
        print(f"전체 경로: {file_path.absolute()}")
        print(f"파일 크기: {file_path.stat().st_size} bytes")
        print(f"확장자: {file_path.suffix}")
        print("-" * 40)

# 디렉토리 내 파일 목록
print("=== data 디렉토리 파일 목록 ===")
for file in data_dir.iterdir():
    if file.is_file():
        print(f"📄 {file.name}")
    elif file.is_dir():
        print(f"📁 {file.name}")

# 파일 읽기
print("\n=== 설정 파일 내용 ===")
if json_file.exists():
    config_content = json_file.read_text(encoding='utf-8')
    config = json.loads(config_content)
    print(f"앱 이름: {config['앱_이름']}")
    print(f"버전: {config['버전']}")
    print(f"언어 설정: {config['설정']['언어']}")
```

---

## 실습: 일기장 프로그램

지금까지 배운 파일 입출력 기능을 활용하여 간단한 일기장 프로그램을 만들어보겠습니다. 이 프로그램은 사용자가 일기를 작성하고, 저장하고, 이전 일기를 조회할 수 있는 기능을 제공합니다.

사용자가 직접 일기를 작성하고 관리할 수 있는 프로그램으로, 실생활에서 바로 활용할 수 있습니다. JSON 형식으로 데이터를 저장하여 구조화된 정보 관리가 가능하며, pathlib를 사용하여 안전한 파일 처리를 구현합니다.

```python title=diary_program.py
from pathlib import Path
import json
from datetime import datetime

class DiaryManager:
    def __init__(self):
        self.diary_dir = Path("my_diary")
        self.diary_file = self.diary_dir / "diary_entries.json"
        self.setup_diary()

    def setup_diary(self):
        """일기장 초기 설정"""
        self.diary_dir.mkdir(exist_ok=True)

        if not self.diary_file.exists():
            # 빈 일기장 파일 생성
            self.diary_file.write_text(
                json.dumps([], ensure_ascii=False, indent=2),
                encoding='utf-8'
            )

    def load_entries(self):
        """일기 항목들 불러오기"""
        content = self.diary_file.read_text(encoding='utf-8')
        return json.loads(content)

    def save_entries(self, entries):
        """일기 항목들 저장하기"""
        self.diary_file.write_text(
            json.dumps(entries, ensure_ascii=False, indent=2),
            encoding='utf-8'
        )

    def write_entry(self):
        """새 일기 작성"""
        print("\n=== 새 일기 작성 ===")
        title = input("일기 제목: ")
        print("일기 내용을 입력하세요 (엔터 두 번으로 완료):")

        content_lines = []
        while True:
            line = input()
            if line == "":
                break
            content_lines.append(line)

        content = "\n".join(content_lines)

        # 새 일기 항목 생성
        new_entry = {
            "날짜": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "제목": title,
            "내용": content
        }

        # 기존 일기들 불러와서 새 일기 추가
        entries = self.load_entries()
        entries.append(new_entry)
        self.save_entries(entries)

        print("일기가 저장되었습니다!")

    def view_entries(self):
        """일기 목록 보기"""
        entries = self.load_entries()

        if not entries:
            print("아직 작성된 일기가 없습니다.")
            return

        print(f"\n=== 일기 목록 (총 {len(entries)}개) ===")
        for i, entry in enumerate(entries, 1):
            print(f"{i}. {entry['제목']} - {entry['날짜']}")

        # 특정 일기 보기
        try:
            choice = int(input("\n읽을 일기 번호 (0: 돌아가기): "))
            if choice == 0:
                return
            elif 1 <= choice <= len(entries):
                entry = entries[choice - 1]
                print(f"\n=== {entry['제목']} ===")
                print(f"작성일: {entry['날짜']}")
                print(f"내용:\n{entry['내용']}")
            else:
                print("잘못된 번호입니다.")
        except ValueError:
            print("숫자를 입력해주세요.")

    def run(self):
        """일기장 프로그램 실행"""
        print("🗒️  나의 일기장에 오신 것을 환영합니다!")

        while True:
            print("\n" + "="*40)
            print("1. 새 일기 작성")
            print("2. 일기 목록 보기")
            print("3. 종료")
            print("="*40)

            choice = input("선택하세요: ")

            if choice == "1":
                self.write_entry()
            elif choice == "2":
                self.view_entries()
            elif choice == "3":
                print("일기장을 종료합니다. 안녕히 가세요!")
                break
            else:
                print("올바른 번호를 선택해주세요.")

# 프로그램 실행
if __name__ == "__main__":
    diary = DiaryManager()
    diary.run()
```

## 확장 과제: 백업 기능이 있는 메모장

더 고급 기능을 원한다면 다음과 같은 기능들을 추가해보세요:

1. **백업 기능**: 일정 개수 이상의 일기가 저장되면 자동으로 백업 파일 생성
2. **검색 기능**: 제목이나 내용으로 일기 검색
3. **태그 기능**: 일기에 태그를 달아서 분류
4. **통계 기능**: 월별 작성 횟수, 가장 많이 사용한 단어 등

---

## 연습문제

1. **파일 복사 프로그램**: 하나의 텍스트 파일을 읽어서 다른 이름으로 저장하는 프로그램을 작성하세요.

2. **단어 개수 세기**: 텍스트 파일을 읽어서 각 단어가 몇 번 나타나는지 세는 프로그램을 만들어보세요.

3. **CSV 성적 관리**: CSV 파일로 학생 성적을 관리하는 프로그램을 작성하세요. 학생 추가, 성적 수정, 평균 계산 기능을 포함해야 합니다.

4. **JSON 설정 관리자**: 프로그램 설정을 JSON 파일로 저장하고 불러오는 기능을 구현해보세요.

5. **파일 정리 도구**: pathlib를 사용하여 특정 폴더의 파일들을 확장자별로 분류하여 정리하는 프로그램을 만들어보세요.

이번 챕터에서 배운 파일 입출력 기능은 거의 모든 프로그램에서 사용되는 핵심 기능입니다. 다음 챕터에서는 프로그램에서 발생할 수 있는 오류들을 안전하게 처리하는 예외 처리에 대해 알아보겠습니다!
