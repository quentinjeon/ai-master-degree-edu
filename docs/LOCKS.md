# LOCKS — 파일 잠금 현황판

> **에이전트 필수 규칙**
> 1. 작업 **시작 전**: 이 파일을 읽고 🔒 LOCKED 파일은 건드리지 않는다
> 2. 작업 **시작 시**: 자기 파일을 🔒 LOCKED로 바꾼다 (이름 + 시각 기록)
> 3. 작업 **완료 시**: 자기 파일을 🔓 FREE로 되돌린다
>
> **스테일 락 기준**: 잠금 후 4시간 이상 변화 없으면 Orchestrator가 FREE로 초기화

---

## 챕터 파일 락 현황

| 파일 | 상태 | 잠금 에이전트 | 잠금 시각 |
|------|:----:|------------|---------|
| `chapters/ch00a.html` | 🔓 FREE | — | — |
| `chapters/ch00b.html` | 🔓 FREE | — | — |
| `chapters/ch00c.html` | 🔓 FREE | — | — |
| `chapters/ch01.html` | 🔓 FREE | — | — |
| `chapters/ch02.html` | 🔓 FREE | — | — |
| `chapters/ch02b.html` | 🔓 FREE | — | — |
| `chapters/ch03.html` | 🔓 FREE | — | — |
| `chapters/ch03b.html` | 🔓 FREE | — | — |
| `chapters/ch04.html` | 🔓 FREE | — | — |
| `chapters/ch05.html` | 🔓 FREE | — | — |
| `chapters/ch06.html` | 🔓 FREE | — | — |
| `chapters/ch07.html` | 🔓 FREE | — | — |
| `chapters/ch08.html` | 🔓 FREE | — | — |
| `chapters/ch09.html` | 🔓 FREE | — | — |
| `chapters/ch09b.html` | 🔓 FREE | — | — |
| `chapters/ch10.html` | 🔓 FREE | — | — |
| `chapters/ch11.html` | 🔓 FREE | — | — |
| `chapters/ch12.html` | 🔓 FREE | — | — |
| `chapters/ch13.html` | 🔓 FREE | — | — |
| `chapters/ch14.html` | 🔓 FREE | — | — |

---

## 공유 파일 락 현황

> 공유 파일은 에이전트 역할별로만 쓰기 가능. 나머지는 읽기 전용.

| 파일 | 쓰기 권한 에이전트 | 현재 상태 |
|------|----------------|---------|
| `index.html` | Integration-Agent만 | 🔓 FREE |
| `docs/PROGRESS.md` | Orchestrator (전체) / 각 에이전트 (자기 행) | 🔓 FREE |
| `docs/QA_REPORT.md` | QA-Agent만 | 🔓 FREE |
| `docs/PRD.md` | ❌ 누구도 수정 불가 | 🔒 READ-ONLY |
| `docs/DESIGN_SYSTEM.md` | ❌ 누구도 수정 불가 | 🔒 READ-ONLY |
| `docs/CHAPTER_TEMPLATE.md` | ❌ 누구도 수정 불가 | 🔒 READ-ONLY |

---

## 락 업데이트 방법

### 작업 시작 시 (🔓 → 🔒)

해당 행을 이렇게 바꾼다:
```
| `chapters/ch03.html` | 🔓 FREE | — | — |
→
| `chapters/ch03.html` | 🔒 LOCKED | Chapter-Agent-C | 2026-04-19 14:30 |
```

### 작업 완료 시 (🔒 → 🔓)

해당 행을 이렇게 되돌린다:
```
| `chapters/ch03.html` | 🔒 LOCKED | Chapter-Agent-C | 2026-04-19 14:30 |
→
| `chapters/ch03.html` | 🔓 FREE | — | — |
```

---

## 충돌 감지 규칙

에이전트가 작업 시작 전 이 파일을 읽었을 때:

```
if 파일 상태 == 🔒 LOCKED:
    → 해당 파일 작업 중단
    → Orchestrator에게 "ch0N은 이미 Agent-X가 작업 중" 보고
    → 다른 FREE 파일 선택

if 파일 상태 == 🔓 FREE:
    → 즉시 LOCKED로 업데이트
    → 작업 시작
```
