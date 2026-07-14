# Plan: Bowling Kata — Agentic Engineering TDD (2단계)

## 목표 (Goal)

`game.py`에 `Game` 클래스를 처음부터 구현한다.

- `roll(pins: int) -> None`: 투구마다 호출. `pins`는 쓰러뜨린 핀 수.
- `score() -> int`: 게임 종료 후 총점을 반환.

범위 제외 (kata 단순화):
- 투구/프레임 유효성 검증 없음
- 중간 프레임 점수 조회 없음

## 파일 위치

```
game.py
tests/
  test_game.py
pytest.ini   (pythonpath = . 설정)
```

## 구현 예정 테스트 케이스 (RED에서 정의, GREEN에서 통과시킬 것)

1. `test_gutter_game_scores_zero` — 20회 모두 0핀 → 0점
2. `test_all_ones_scores_20` — 20회 모두 1핀 → 20점
3. `test_one_spare_adds_next_roll_as_bonus` — 5,5(스페어),3,이후 0×17 → 16점
4. `test_one_strike_adds_next_two_rolls_as_bonus` — 10(스트라이크),3,4,이후 0×16 → 24점
5. `test_perfect_game_scores_300` — 12회 스트라이크(10번 프레임 보너스 2회 포함) → 300점

## 구현 접근 방식

프레임 포인터(`i`) 기반 순회:
- 각 프레임마다 `rolls[i]`가 스트라이크(10)인지, 두 투구 합이 스페어(10)인지 확인
- 스트라이크: `10 + rolls[i+1] + rolls[i+2]` 더하고 `i += 1`
- 스페어: `10 + rolls[i+2]` 더하고 `i += 2`
- 일반: `rolls[i] + rolls[i+1]` 더하고 `i += 2`
- 10프레임 반복. 10번 프레임의 보너스 투구는 별도 특수 처리 없이 위 로직으로 자연 처리됨.

## 커밋 계획

- 이 문서(Plan.md) 승인 후 → **RED 커밋** (Plan.md만 커밋)
- GREEN 구현 후 테스트 통과 확인 + 사람 리뷰(Plan 범위 이탈 여부, 리팩토링 필요 여부) 완료 → **GREEN 커밋** (game.py + tests 커밋)

## 리뷰 요청 사항

아래 사항에 대해 승인 또는 수정 의견 부탁드립니다.
- 테스트 케이스 5개가 충분한지 (누락된 엣지 케이스 있는지)
- 구현 접근 방식(프레임 포인터 방식)이 적절한지
- 파일 위치가 적절한지
