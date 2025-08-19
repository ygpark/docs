---
title: '20. 미니 프로젝트 1: 텍스트 기반 게임'
slug: python-project-text-game
description: '파이썬 기초 문법을 종합한 던전 탐험 어드벤처 게임 제작. 객체지향 설계와 테스트 코드 작성 경험'
keywords: [파이썬 프로젝트, 텍스트 게임, 던전 게임, 객체지향 설계, 미니 프로젝트, 게임 개발]
sidebar_position: 20
---

# 20. 미니 프로젝트 1: 텍스트 기반 게임

지금까지 배운 파이썬 기초 문법들을 모두 활용해 실제 동작하는 게임을 만들어볼 차례입니다. 이번 프로젝트에서는 조건문, 반복문, 함수, 클래스, 파일 처리 등 지금까지 학습한 모든 요소를 종합적으로 활용하게 됩니다. 텍스트 기반 게임은 복잡한 그래픽 없이도 충분히 재미있고 교육적인 프로그램을 만들 수 있는 훌륭한 주제입니다. 완성된 게임은 여러분만의 창의적인 아이디어를 추가해 확장할 수 있는 기반이 될 것입니다.

## 20-1 프로젝트 개요

### 학습 목표

- 지금까지 배운 파이썬 문법을 종합적으로 활용하기
- 프로젝트 기획부터 완성까지의 개발 과정 경험하기
- 객체지향 설계 원칙을 실제 프로젝트에 적용하기
- 테스트 코드 작성의 중요성 이해하기
- GitHub를 활용한 프로젝트 관리 방법 익히기

### 만들어볼 게임: "모험가의 던전 탐험"

이번에 만들 게임은 플레이어가 던전을 탐험하며 몬스터와 전투하고 보물을 찾는 텍스트 기반 RPG 게임입니다. 간단해 보이지만 게임의 핵심 요소들을 모두 포함하고 있어 프로그래밍 학습에 매우 적합합니다.

**게임의 핵심 기능:**

- 캐릭터 생성 및 스탯 관리
- 던전 탐험 및 랜덤 이벤트
- 몬스터와의 전투 시스템
- 아이템 획득 및 인벤토리 관리
- 게임 진행 상황 저장/불러오기

## 20-2 게임 설계하기

프로그래밍에서 가장 중요한 단계는 설계입니다. 코딩을 시작하기 전에 게임의 전체 구조를 명확히 해야 합니다.

### 게임 흐름도

```
시작 → 캐릭터 생성 → 메인 메뉴
                      ↓
        던전 탐험 ← 게임 메뉴 → 인벤토리 확인
           ↓                     ↓
        랜덤 이벤트              게임 저장
    (몬스터/보물/휴식)              ↓
           ↓                   게임 종료
        전투 시스템
           ↓
    경험치/아이템 획득
           ↓
        메인 메뉴로 복귀
```

### 필요한 클래스 설계

```python title=game_design.py
# 게임에 필요한 주요 클래스들
class Character:
    """플레이어 캐릭터 클래스"""
    # 속성: 이름, 체력, 공격력, 방어력, 경험치, 레벨
    # 메서드: 공격, 방어, 레벨업, 상태 표시

class Monster:
    """몬스터 클래스"""
    # 속성: 이름, 체력, 공격력, 경험치 보상
    # 메서드: 공격, 상태 표시

class Item:
    """아이템 클래스"""
    # 속성: 이름, 타입, 효과값
    # 메서드: 사용 효과

class Game:
    """게임 전체를 관리하는 클래스"""
    # 속성: 플레이어, 현재 위치, 게임 상태
    # 메서드: 게임 시작, 던전 탐험, 전투, 저장/불러오기
```

## 20-3 단계별 구현

### 1단계: 기본 클래스 구현

먼저 게임의 핵심이 되는 캐릭터와 몬스터 클래스를 만들어보겠습니다.

```python title=character.py
import random
from typing import List, Dict, Any

class Character:
    """플레이어 캐릭터를 나타내는 클래스"""

    def __init__(self, name: str):
        self.name = name
        self.level = 1
        self.hp = 100
        self.max_hp = 100
        self.attack = 15
        self.defense = 5
        self.exp = 0
        self.exp_to_next = 100
        self.inventory: List[str] = []

    def attack_monster(self, monster: 'Monster') -> int:
        """몬스터를 공격하고 데미지를 반환"""
        damage = random.randint(self.attack - 3, self.attack + 3)
        actual_damage = max(1, damage - monster.defense)
        monster.hp -= actual_damage
        return actual_damage

    def take_damage(self, damage: int) -> None:
        """데미지를 받아 체력 감소"""
        actual_damage = max(1, damage - self.defense)
        self.hp -= actual_damage
        if self.hp < 0:
            self.hp = 0

    def gain_exp(self, exp: int) -> bool:
        """경험치 획득 및 레벨업 체크"""
        self.exp += exp
        if self.exp >= self.exp_to_next:
            return self.level_up()
        return False

    def level_up(self) -> bool:
        """레벨업 처리"""
        self.level += 1
        self.exp -= self.exp_to_next
        self.exp_to_next = int(self.exp_to_next * 1.5)

        # 스탯 증가
        hp_increase = random.randint(15, 25)
        self.max_hp += hp_increase
        self.hp = self.max_hp  # 레벨업 시 체력 완전 회복
        self.attack += random.randint(3, 7)
        self.defense += random.randint(1, 3)

        return True

    def is_alive(self) -> bool:
        """생존 여부 확인"""
        return self.hp > 0

    def get_status(self) -> str:
        """현재 상태를 문자열로 반환"""
        return f"""
=== {self.name}의 상태 ===
레벨: {self.level}
체력: {self.hp}/{self.max_hp}
공격력: {self.attack}
방어력: {self.defense}
경험치: {self.exp}/{self.exp_to_next}
"""

class Monster:
    """몬스터를 나타내는 클래스"""

    def __init__(self, name: str, hp: int, attack: int, defense: int, exp_reward: int):
        self.name = name
        self.hp = hp
        self.max_hp = hp
        self.attack = attack
        self.defense = defense
        self.exp_reward = exp_reward

    def attack_character(self, character: Character) -> int:
        """캐릭터를 공격하고 데미지를 반환"""
        damage = random.randint(self.attack - 2, self.attack + 2)
        character.take_damage(damage)
        return max(1, damage - character.defense)

    def is_alive(self) -> bool:
        """생존 여부 확인"""
        return self.hp > 0

    def get_status(self) -> str:
        """현재 상태를 문자열로 반환"""
        return f"{self.name} - 체력: {self.hp}/{self.max_hp}"
```

### 2단계: 게임 시스템 구현

이제 실제 게임을 진행하는 메인 클래스를 만들어보겠습니다.

```python title=game.py
import random
import json
from character import Character, Monster

class Game:
    """게임 전체를 관리하는 클래스"""

    def __init__(self):
        self.player = None
        self.current_floor = 1
        self.game_running = True

        # 몬스터 정보 정의
        self.monster_data = {
            1: [("슬라임", 30, 8, 1, 20), ("고블린", 45, 12, 2, 35)],
            2: [("오크", 60, 15, 3, 50), ("스켈레톤", 50, 18, 2, 45)],
            3: [("드래곤", 120, 25, 5, 100)]
        }

    def start_game(self) -> None:
        """게임 시작"""
        print("=== 모험가의 던전 탐험에 오신 것을 환영합니다! ===")
        print("1. 새 게임 시작")
        print("2. 게임 불러오기")

        choice = input("선택하세요 (1-2): ")

        if choice == "1":
            self.create_character()
        elif choice == "2":
            if not self.load_game():
                print("저장된 게임이 없습니다. 새 게임을 시작합니다.")
                self.create_character()

        self.main_menu()

    def create_character(self) -> None:
        """새 캐릭터 생성"""
        name = input("캐릭터의 이름을 입력하세요: ")
        self.player = Character(name)
        print(f"{name} 모험가가 생성되었습니다!")
        print(self.player.get_status())

    def main_menu(self) -> None:
        """메인 메뉴"""
        while self.game_running and self.player.is_alive():
            print("\n=== 메인 메뉴 ===")
            print("1. 던전 탐험")
            print("2. 상태 확인")
            print("3. 게임 저장")
            print("4. 게임 종료")

            choice = input("선택하세요 (1-4): ")

            if choice == "1":
                self.explore_dungeon()
            elif choice == "2":
                print(self.player.get_status())
            elif choice == "3":
                self.save_game()
            elif choice == "4":
                self.game_running = False
                print("게임을 종료합니다. 좋은 하루 되세요!")

    def explore_dungeon(self) -> None:
        """던전 탐험"""
        print(f"\n=== {self.current_floor}층 던전 탐험 ===")

        # 랜덤 이벤트 발생
        event = random.choice(["monster", "treasure", "rest", "monster"])

        if event == "monster":
            self.encounter_monster()
        elif event == "treasure":
            self.find_treasure()
        elif event == "rest":
            self.rest_area()

    def encounter_monster(self) -> None:
        """몬스터 조우"""
        floor_monsters = self.monster_data.get(min(self.current_floor, 3), self.monster_data[1])
        monster_info = random.choice(floor_monsters)
        monster = Monster(*monster_info)

        print(f"\n야생의 {monster.name}이(가) 나타났다!")
        print(monster.get_status())

        # 전투 시작
        self.battle(monster)

    def battle(self, monster: Monster) -> None:
        """전투 시스템"""
        print("\n=== 전투 시작! ===")

        while self.player.is_alive() and monster.is_alive():
            print(f"\n{self.player.name}: {self.player.hp}/{self.player.max_hp} HP")
            print(f"{monster.name}: {monster.hp}/{monster.max_hp} HP")
            print("\n1. 공격")
            print("2. 도망가기")

            choice = input("행동을 선택하세요 (1-2): ")

            if choice == "1":
                # 플레이어 공격
                damage = self.player.attack_monster(monster)
                print(f"{self.player.name}이(가) {monster.name}에게 {damage}의 데미지를 입혔습니다!")

                if not monster.is_alive():
                    print(f"{monster.name}을(를) 처치했습니다!")
                    gained_exp = monster.exp_reward
                    print(f"경험치 {gained_exp}를 획득했습니다!")

                    if self.player.gain_exp(gained_exp):
                        print(f"레벨 업! 현재 레벨: {self.player.level}")
                        print("모든 능력치가 상승했습니다!")
                    break

                # 몬스터 공격
                damage = monster.attack_character(self.player)
                print(f"{monster.name}이(가) {self.player.name}에게 {damage}의 데미지를 입혔습니다!")

            elif choice == "2":
                if random.random() < 0.7:  # 70% 확률로 도망 성공
                    print("성공적으로 도망쳤습니다!")
                    return
                else:
                    print("도망치지 못했습니다!")
                    damage = monster.attack_character(self.player)
                    print(f"{monster.name}이(가) {self.player.name}에게 {damage}의 데미지를 입혔습니다!")

        if not self.player.is_alive():
            print(f"\n{self.player.name}이(가) 쓰러졌습니다...")
            print("게임 오버!")
            self.game_running = False

    def find_treasure(self) -> None:
        """보물 발견"""
        treasures = ["체력 물약", "공격력 증진 물약", "경험치 보주"]
        treasure = random.choice(treasures)

        print(f"\n보물상자를 발견했습니다! '{treasure}'를 획득했습니다!")

        if treasure == "체력 물약":
            heal = random.randint(20, 40)
            self.player.hp = min(self.player.max_hp, self.player.hp + heal)
            print(f"체력이 {heal} 회복되었습니다!")
        elif treasure == "공격력 증진 물약":
            self.player.attack += 2
            print("공격력이 2 증가했습니다!")
        elif treasure == "경험치 보주":
            exp = random.randint(30, 50)
            if self.player.gain_exp(exp):
                print(f"경험치 {exp}를 획득했습니다! 레벨 업!")
            else:
                print(f"경험치 {exp}를 획득했습니다!")

    def rest_area(self) -> None:
        """휴식 공간"""
        print("\n평화로운 휴식 공간을 발견했습니다.")
        heal = random.randint(15, 30)
        self.player.hp = min(self.player.max_hp, self.player.hp + heal)
        print(f"체력이 {heal} 회복되었습니다!")

        if random.random() < 0.3:  # 30% 확률로 다음 층으로
            self.current_floor += 1
            print(f"다음 층으로 향하는 계단을 발견했습니다! 현재 {self.current_floor}층")

    def save_game(self) -> None:
        """게임 저장"""
        save_data = {
            "name": self.player.name,
            "level": self.player.level,
            "hp": self.player.hp,
            "max_hp": self.player.max_hp,
            "attack": self.player.attack,
            "defense": self.player.defense,
            "exp": self.player.exp,
            "exp_to_next": self.player.exp_to_next,
            "current_floor": self.current_floor
        }

        try:
            with open("savefile.json", "w", encoding="utf-8") as f:
                json.dump(save_data, f, ensure_ascii=False, indent=2)
            print("게임이 저장되었습니다!")
        except Exception as e:
            print(f"저장 중 오류가 발생했습니다: {e}")

    def load_game(self) -> bool:
        """게임 불러오기"""
        try:
            with open("savefile.json", "r", encoding="utf-8") as f:
                save_data = json.load(f)

            self.player = Character(save_data["name"])
            self.player.level = save_data["level"]
            self.player.hp = save_data["hp"]
            self.player.max_hp = save_data["max_hp"]
            self.player.attack = save_data["attack"]
            self.player.defense = save_data["defense"]
            self.player.exp = save_data["exp"]
            self.player.exp_to_next = save_data["exp_to_next"]
            self.current_floor = save_data["current_floor"]

            print("게임을 성공적으로 불러왔습니다!")
            print(self.player.get_status())
            return True

        except FileNotFoundError:
            return False
        except Exception as e:
            print(f"불러오기 중 오류가 발생했습니다: {e}")
            return False

# 게임 실행
if __name__ == "__main__":
    game = Game()
    game.start_game()
```

## 20-4 코드 개선하기

### 메인 실행 파일 생성

```python title=main.py
"""
던전 탐험 게임 메인 실행 파일
"""

from game import Game

def main():
    """메인 함수"""
    try:
        game = Game()
        game.start_game()
    except KeyboardInterrupt:
        print("\n\n게임이 중단되었습니다.")
    except Exception as e:
        print(f"게임 실행 중 오류가 발생했습니다: {e}")

if __name__ == "__main__":
    main()
```

## 20-5 테스트 코드 작성하기

테스트 코드는 우리가 만든 게임이 올바르게 동작하는지 확인하는 중요한 도구입니다.

```python title=test_character.py
"""
캐릭터 클래스 테스트
"""

import pytest
from character import Character, Monster

def test_character_creation():
    """캐릭터 생성 테스트"""
    player = Character("테스트용사")
    assert player.name == "테스트용사"
    assert player.level == 1
    assert player.hp == 100
    assert player.is_alive() == True

def test_character_attack():
    """캐릭터 공격 테스트"""
    player = Character("용사")
    monster = Monster("슬라임", 30, 8, 1, 20)

    initial_monster_hp = monster.hp
    damage = player.attack_monster(monster)

    assert damage > 0
    assert monster.hp < initial_monster_hp

def test_character_level_up():
    """레벨업 테스트"""
    player = Character("용사")
    initial_level = player.level
    initial_attack = player.attack

    # 충분한 경험치 획득
    result = player.gain_exp(150)

    assert result == True  # 레벨업 발생
    assert player.level == initial_level + 1
    assert player.attack > initial_attack

def test_monster_creation():
    """몬스터 생성 테스트"""
    monster = Monster("고블린", 45, 12, 2, 35)
    assert monster.name == "고블린"
    assert monster.hp == 45
    assert monster.is_alive() == True

if __name__ == "__main__":
    # 간단한 테스트 실행
    test_character_creation()
    test_character_attack()
    test_character_level_up()
    test_monster_creation()
    print("모든 테스트가 통과했습니다!")
```

## GitHub 연동 방법

### 1. 프로젝트 구조 정리

```
dungeon_game/
├── main.py              # 메인 실행 파일
├── game.py              # 게임 메인 클래스
├── character.py         # 캐릭터, 몬스터 클래스
├── test_character.py    # 테스트 파일
├── README.md           # 프로젝트 설명
├── requirements.txt    # 필요한 라이브러리
└── .gitignore         # Git 무시 파일
```

### 2. README.md 작성

````markdown title=README.md
# 던전 탐험 게임

파이썬으로 만든 텍스트 기반 RPG 게임입니다.

## 게임 특징

- 캐릭터 생성 및 레벨업 시스템
- 다양한 몬스터와의 전투
- 랜덤 이벤트 (보물, 휴식)
- 게임 저장/불러오기 기능

## 실행 방법

```bash
python main.py
```
````

## 테스트 실행

```bash
python test_character.py
```

## 개발 환경

- Python 3.8+
- 외부 라이브러리 없음 (순수 파이썬)

````

### 3. .gitignore 파일

```gitignore title=.gitignore
# 파이썬 관련
__pycache__/
*.pyc
*.pyo
*.pyd

# 게임 저장 파일
savefile.json

# IDE 관련
.vscode/
.idea/

# 운영체제 관련
.DS_Store
Thumbs.db
````

### 4. Git 저장소 초기화 및 업로드

```bash
# Git 저장소 초기화
git init

# 파일 추가
git add .

# 첫 번째 커밋
git commit -m "던전 탐험 게임 초기 버전 완성"

# GitHub 원격 저장소 연결 (본인의 저장소 URL로 변경)
git remote add origin https://github.com/your-username/dungeon-game.git

# GitHub에 업로드
git push -u origin main
```

이제 여러분만의 첫 번째 파이썬 게임 프로젝트가 완성되었습니다! 이 게임을 기반으로 새로운 몬스터, 아이템, 스킬 시스템 등을 추가해 더욱 풍성한 게임으로 발전시켜보세요. 프로그래밍의 진정한 재미는 바로 이런 창작 과정에서 나온다는 것을 기억하시기 바랍니다.
