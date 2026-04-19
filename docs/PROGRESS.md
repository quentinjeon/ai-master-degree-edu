# PROGRESS — 챕터 개발 상태 추적

> **⚠️ 작업 시작 전 반드시 `docs/LOCKS.md`를 먼저 확인할 것**  
> **상태값**: `🆓 TODO` | `🔒 IN_PROGRESS` | `✅ DONE` | `❌ QA_FAIL` | `🏆 QA_PASS`  
> **규칙**: 자기 챕터 행의 상태와 잠금 에이전트 컬럼만 수정. 다른 행 수정 금지.

---

## 챕터 상태 한눈에 보기

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.01 집합과 함수 | `chapters/ch01.html` | 🏆 QA_PASS | Chapter-Agent-A | 2026-04-19 | ✅ |
| 🔓 | Ch.01A 로그와 지수함수 | `chapters/ch00a.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.01B 수열과 Σ 표기 | `chapters/ch00b.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.01C 경우의 수·조합 | `chapters/ch00c.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.02 미분과 최적화 | `chapters/ch02.html` | 🏆 QA_PASS | Chapter-Agent-B | 2026-04-19 | ✅ |
| 🔓 | Ch.02B 적분 기초 | `chapters/ch02b.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.03 선형대수·행렬미분 | `chapters/ch03.html` | 🏆 QA_PASS | Chapter-Agent-C + Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.03B 선형회귀·최소제곱법 | `chapters/ch03b.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.04 확률론·베이즈·독립성 | `chapters/ch04.html` | 🏆 QA_PASS | Chapter-Agent-D + Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.05 확률변수·분포 | `chapters/ch05.html` | 🏆 QA_PASS | Chapter-Agent-E | 2026-04-19 | ✅ |
| 🔓 | Ch.06 기대값·분산 | `chapters/ch06.html` | 🏆 QA_PASS | Chapter-Agent-F | 2026-04-19 | ✅ |
| 🔓 | Ch.07 표본·MLE | `chapters/ch07.html` | 🏆 QA_PASS | Chapter-Agent-G | 2026-04-19 | ✅ |
| 🔓 | Ch.08 MAP·Bias-Variance | `chapters/ch08.html` | 🏆 QA_PASS | Chapter-Agent-H | 2026-04-19 | ✅ |
| 🔓 | Ch.09 ML연결1·Logistic | `chapters/ch09.html` | 🏆 QA_PASS | Chapter-Agent-I | 2026-04-19 | ✅ |
| 🔓 | Ch.09B 모델 평가지표 | `chapters/ch09b.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.10 ML연결2·Softmax | `chapters/ch10.html` | 🏆 QA_PASS | Chapter-Agent-J | 2026-04-19 | ✅ |
| 🔓 | Ch.11 정보이론·Jensen | `chapters/ch11.html` | 🏆 QA_PASS | Chapter-Agent-K + Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.12 딥러닝·LLM | `chapters/ch12.html` | 🏆 QA_PASS | Chapter-Agent-L | 2026-04-19 | ✅ |
| 🔓 | Ch.13 Calibration | `chapters/ch13.html` | 🏆 QA_PASS | Chapter-Agent-M | 2026-04-19 | ✅ |
| 🔓 | Ch.14 논문읽기·쓰기 | `chapters/ch14.html` | 🏆 QA_PASS | Chapter-Agent-N | 2026-04-19 | ✅ |

> **락 열 의미**: 🔒 = 현재 작업 중 (건드리지 말 것) | 🔓 = 작업 가능

---

## 상태 전환 흐름

```
🆓 TODO
  │  에이전트 배정됨
  ▼
🔒 IN_PROGRESS  ←─── 락 열도 🔒로 변경 + LOCKS.md 업데이트
  │  HTML 완성
  ▼
✅ DONE         ←─── 락 열을 🔓로 복원 + LOCKS.md 업데이트
  │  QA Agent 검증
  ├──(실패)──▶ ❌ QA_FAIL ──▶ 수정 후 다시 ✅ DONE
  └──(통과)──▶ 🏆 QA_PASS ──▶ Integration Agent 통합 대상
```

---

## 웨이브 실행 계획

### Wave 1 — P0 기초 (4개 병렬)
> 선행 조건: 없음

| 에이전트 | 챕터 | 출력 파일 |
|---------|------|---------|
| Chapter-Agent-A | Ch.01 집합과 함수 | `chapters/ch01.html` |
| Chapter-Agent-B | Ch.02 미분과 최적화 | `chapters/ch02.html` |
| Chapter-Agent-C | Ch.03 선형대수 | `chapters/ch03.html` |
| Chapter-Agent-D | Ch.04 확률론·베이즈 | `chapters/ch04.html` |

### Wave 1.5 — 브릿지 챕터 (신규, 병렬)
> 선행 조건: Wave 1 완료 후

| 에이전트 | 챕터 | 출력 파일 |
|---------|------|---------|
| Main-Agent | Ch.01A 로그와 지수함수 | `chapters/ch00a.html` |
| Main-Agent | Ch.01B 수열과 Σ 표기 | `chapters/ch00b.html` |
| Main-Agent | Ch.01C 경우의 수·조합 | `chapters/ch00c.html` |
| Main-Agent | Ch.02B 적분 기초 | `chapters/ch02b.html` |
| Main-Agent | Ch.03B 선형회귀·최소제곱법 | `chapters/ch03b.html` |
| Main-Agent | Ch.09B 모델 평가지표 | `chapters/ch09b.html` |

### Wave 2 — P1 통계·추정 (4개 병렬)
> 선행 조건: Wave 1 전체 QA_PASS 후

| 에이전트 | 챕터 | 출력 파일 |
|---------|------|---------|
| Chapter-Agent-E | Ch.05 확률변수·분포 | `chapters/ch05.html` |
| Chapter-Agent-F | Ch.06 기대값·분산 | `chapters/ch06.html` |
| Chapter-Agent-G | Ch.07 표본·MLE | `chapters/ch07.html` |
| Chapter-Agent-H | Ch.08 MAP·Bias-Variance | `chapters/ch08.html` |

### Wave 3 — P2 ML·응용 (6개 병렬)
> 선행 조건: Wave 2 전체 QA_PASS 후

| 에이전트 | 챕터 | 출력 파일 |
|---------|------|---------|
| Chapter-Agent-I | Ch.09 ML연결1 | `chapters/ch09.html` |
| Chapter-Agent-J | Ch.10 ML연결2 | `chapters/ch10.html` |
| Chapter-Agent-K | Ch.11 정보이론 | `chapters/ch11.html` |
| Chapter-Agent-L | Ch.12 딥러닝·LLM | `chapters/ch12.html` |
| Chapter-Agent-M | Ch.13 Calibration | `chapters/ch13.html` |
| Chapter-Agent-N | Ch.14 논문읽기 | `chapters/ch14.html` |

### Wave 4 — QA + 통합
> 선행 조건: 모든 챕터 ✅ DONE 확인 후

| 에이전트 | 담당 | 출력 |
|---------|------|-----|
| QA-Agent | 모든 챕터 검증 | `docs/QA_REPORT.md` |
| Integration-Agent | 전체 통합 | `index.html` |

---

## QA 결과

_QA Agent가 기록. 챕터 에이전트는 이 섹션 수정 금지._

| 챕터 | 결과 | 실패 항목 |
|------|:----:|---------|
| Ch.01 집합과 함수 | ✅ PASS | — |
| Ch.02 미분과 최적화 | ✅ PASS | — |
| Ch.03 선형대수 기초 | ✅ PASS | — |
| Ch.04 확률론·베이즈 | ✅ PASS | NAV ch03b 연결 + EASY BOX 아이콘 수정 (2026-04-19) |
| Ch.01A 로그와 지수함수 | ✅ PASS | — |
| Ch.01B 수열과 Σ 표기 | ✅ PASS | — |
| Ch.01C 경우의 수·조합 | — | 신규 추가 (2026-04-19) — QA 대기 |
| Ch.02B 적분 기초 | ✅ PASS | — |
| Ch.03B 선형회귀·최소제곱법 | ✅ PASS | — |
| Ch.05 확률변수·분포 | ✅ PASS | — |
| Ch.06 기대값·분산 | ✅ PASS | — |
| Ch.07 표본·MLE | ✅ PASS | — |
| Ch.08 MAP·Bias-Variance | ✅ PASS | — |
| Ch.09 AI가 확률로 배우는 법 | ❌ FAIL | [2] EASY BOX p 태그 내 strong 누락 |
| Ch.09B 모델 평가지표 | — | 신규 추가 (2026-04-19) — QA 대기 |
| Ch.10 Softmax와 다중분류 | ❌ FAIL | [2] EASY BOX p 태그 내 strong 누락 |
| Ch.11 정보이론 | ✅ PASS | Jensen's inequality 섹션 추가 (2026-04-19) |
| Ch.12 딥러닝·LLM | ✅ PASS | — |
| Ch.13 Calibration·불확실성 | ✅ PASS | — |
| Ch.14 논문 읽기·쓰기 | ✅ PASS | — |

---

## 업데이트 규칙 요약

| 시점 | PROGRESS.md 변경 | LOCKS.md 변경 |
|------|----------------|-------------|
| 작업 시작 | 내 행: 상태→🔒 IN_PROGRESS, 에이전트→내 이름 | 내 파일: 🔓→🔒 LOCKED + 이름 + 시각 |
| 작업 완료 | 내 행: 상태→✅ DONE, 완료일→날짜 | 내 파일: 🔒→🔓 FREE |
| QA 통과 | 내 행: QA→🏆 QA_PASS | — |
| QA 실패 | 내 행: 상태→❌ QA_FAIL, QA→실패 항목 | — |
| 수정 재시작 | 내 행: 상태→🔒 IN_PROGRESS | 내 파일: 🔓→🔒 LOCKED |
