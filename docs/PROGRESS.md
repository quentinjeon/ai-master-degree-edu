# PROGRESS — 챕터 개발 상태 추적

> **⚠️ 작업 시작 전 반드시 `docs/LOCKS.md`를 먼저 확인할 것**  
> **상태값**: `🆓 TODO` | `🔒 IN_PROGRESS` | `✅ DONE` | `❌ QA_FAIL` | `🏆 QA_PASS`  
> **규칙**: 자기 챕터 행의 상태와 잠금 에이전트 컬럼만 수정. 다른 행 수정 금지.

---

## 🎯 레벨 학습 목표 프레임워크

> 자세한 정의는 `docs/LEVEL_FRAMEWORK.md` 참조  
> Level 0 커리큘럼 전체: `docs/LEVEL0_CURRICULUM.md`

| 레벨 | 이름 | 완성 기준 | 해당 Phase |
|:----:|------|----------|-----------|
| ⚪ **레벨 0** | 사전 준비 | AI를 이해하는 **사고 구조**를 만든다 | 1년 커리큘럼 (Scratch→Python→미니프로젝트) 📋 |
| 🟢 **레벨 1** | 입문 | SCI급 AI 논문을 **읽고 이해**할 수 있다 | Phase 1 (Ch.00a ~ Ch.14) ✅ 완료 |
| 🔵 **레벨 2** | 중급 | SCI급을 완전히 이해하고 **논리적 질문**을 던질 수 있다 | Phase 2 (Ch.15 ~ Ch.26) ✅ 완료 |
| 🔴 **레벨 3** | 고급 | SCI급 논문을 **직접 설계하고 쓴다** (Stanford/MIT 수준) | Phase 3 (Ch.27~Ch.36) ✅ QA 완료 |

---

## 🟢 레벨 1 (입문) — Phase 1 챕터 상태 (완료)

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

> **🟢 레벨 1 (입문) Phase 1 — 전 챕터 QA_PASS 완료 (2026-04-19)**

---

## 📋 90일 집중 플랜 — 신규 챕터 명세

> 90일 플랜 PRD 전체: `docs/PLAN_90DAY.md`  
> Phase 1 커버율: 이론 90% ✅ / 실습 40% ❌ / 논문 실전 30% ❌

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.02D 경사하강법 numpy 구현 실습 | `chapters/ch02d.html` | 🏆 QA_PASS | Chapter-Agent-P02D | 2026-04-19 | ✅ |
| 🔓 | Ch.14B 논문 5-question 해석 프레임워크 | `chapters/ch14b.html` | 🏆 QA_PASS | Chapter-Agent-P14B | 2026-04-19 | ✅ |

> **갭 해결**: ①경사하강법 코드 구현 체크리스트 (Day 51~60) ②논문 실전 해석 훈련 (Day 71~90)

---

## ⚪ 레벨 0 (사전 준비) — 브릿지 챕터 명세

> Level 0 커리큘럼(1년) 완료 후 Level 1 진입 전 필수 브릿지  
> 명세 파일: `docs/chapters/week00d.md`, `week00e.md`, `week00f.md`

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.00D Python 데이터 과학 도구 | `chapters/ch00d.html` | 🏆 QA_PASS | Chapter-Agent-P00D | 2026-04-19 | ✅ |
| 🔓 | Ch.00E 수학 브릿지 | `chapters/ch00e.html` | 🏆 QA_PASS | Chapter-Agent-P00E | 2026-04-19 | ✅ |
| 🔓 | Ch.00F ML 실습 입문 | `chapters/ch00f.html` | 🏆 QA_PASS | Chapter-Agent-P00F | 2026-04-19 | ✅ |

> **갭 해결**: Level 0 커리큘럼에서 누락된 ①Python 데이터 과학 도구 ②수학 기호 체계 ③ML 실습 경험을 보완

---

## 🔵 레벨 2 (중급) — Phase 2 챕터 상태

> 목표: SCI급 논문을 완전히 이해하고 논리적 질문을 던질 수 있는 수준  
> 자세한 레벨 정의: `docs/LEVEL_FRAMEWORK.md`

### Phase 2-A · 해석학 기초

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.15 수열과 극한의 엄밀한 정의 | `chapters/ch15.html` | 🏆 QA_PASS | Main-Agent | 2026-04-19 | ✅ |
| 🔓 | Ch.16 연속성과 미분의 엄밀한 정의 | `chapters/ch16.html` | 🏆 QA_PASS | Chapter-Agent-P16 | 2026-04-19 | ✅ |
| 🔓 | Ch.17 적분과 급수 — 오차 경계와 근사 | `chapters/ch17.html` | 🏆 QA_PASS | Chapter-Agent-P17 | 2026-04-19 | ✅ |

### Phase 2-B · 볼록 최적화

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.18 볼록 집합·볼록 함수 | `chapters/ch18.html` | 🏆 QA_PASS | Chapter-Agent-P18 | 2026-04-19 | ✅ |
| 🔓 | Ch.19 Lagrangian·KKT 조건 | `chapters/ch19.html` | 🏆 QA_PASS | Chapter-Agent-P19 | 2026-04-19 | ✅ |
| 🔓 | Ch.20 수렴 속도 분석 | `chapters/ch20.html` | 🏆 QA_PASS | Chapter-Agent-P20 | 2026-04-19 | ✅ |

### Phase 2-C · 측도 기반 확률론

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.21 σ-대수·확률공간 | `chapters/ch21.html` | 🏆 QA_PASS | Chapter-Agent-P21 | 2026-04-19 | ✅ |
| 🔓 | Ch.22 확률변수의 수렴 (a.s./확률/분포) | `chapters/ch22.html` | 🏆 QA_PASS | Chapter-Agent-P22 | 2026-04-19 | ✅ |
| 🔓 | Ch.23 기대값의 측도론적 정의·Fubini | `chapters/ch23.html` | 🏆 QA_PASS | Chapter-Agent-P23 | 2026-04-19 | ✅ |

### Phase 2-D · 선형대수 심화

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.24 고유값 분해·스펙트럼 이론 | `chapters/ch24.html` | 🏆 QA_PASS | Chapter-Agent-P24 | 2026-04-19 | ✅ |
| 🔓 | Ch.25 SVD 완전 유도·LoRA | `chapters/ch25.html` | 🏆 QA_PASS | Chapter-Agent-P25 | 2026-04-19 | ✅ |
| 🔓 | Ch.26 행렬 미분·역전파 수학 | `chapters/ch26.html` | 🏆 QA_PASS | Chapter-Agent-P26 | 2026-04-19 | ✅ |

> **락 열 의미**: 🔒 = 현재 작업 중 (건드리지 말 것) | 🔓 = 작업 가능

---

## 🔴 레벨 3 (고급) — Phase 3 챕터 상태 (Stanford AI 석사급)

> 목표: SCI급 논문을 **직접 설계하고 쓰는** 수준 — 이론·실험·재현·연구 설계까지 가능
> 기준: Stanford CS229/231n/224n/230 + NeurIPS/ICML 논문 비판·확장 가능

### Phase 3-A · 통계적 학습 이론

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.27 집중 부등식 (Markov·Hoeffding·Bernstein) | `chapters/ch27.html` | 🏆 QA_PASS | Chapter-Agent-P27 | 2026-04-19 | ✅ |
| 🔓 | Ch.28 PAC 학습 이론·표본 복잡도 | `chapters/ch28.html` | 🏆 QA_PASS | Chapter-Agent-P28 | 2026-04-19 | ✅ |
| 🔓 | Ch.29 VC 차원·Rademacher 복잡도 | `chapters/ch29.html` | 🏆 QA_PASS | Chapter-Agent-P29 | 2026-04-19 | ✅ |
| 🔓 | Ch.30 딥러닝 일반화 미스터리 (Double Descent·NTK) | `chapters/ch30.html` | 🏆 QA_PASS | Chapter-Agent-P30 | 2026-04-19 | ✅ |

### Phase 3-B · 딥러닝 이론 심화

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.31 Transformer Attention 완전 유도 | `chapters/ch31.html` | 🏆 QA_PASS | Chapter-Agent-P31 | 2026-04-19 | ✅ |
| 🔓 | Ch.32 정보 기하학·Fisher Information | `chapters/ch32.html` | 🏆 QA_PASS | Chapter-Agent-P32 | 2026-04-19 | ✅ |
| 🔓 | Ch.33 마팅게일·확률 과정 | `chapters/ch33.html` | 🏆 QA_PASS | Chapter-Agent-P33 | 2026-04-19 | ✅ |
| 🔓 | Ch.34 고급 최적화 이론 (Adam·Natural Gradient) | `chapters/ch34.html` | 🏆 QA_PASS | Chapter-Agent-C34 | 2026-04-19 | ✅ |

### Phase 3-C · 연구 방법론

| 락 | 챕터 | 출력 파일 | 상태 | 담당 에이전트 | 완료일 | QA |
|:--:|------|---------|:----:|------------|:------:|:--:|
| 🔓 | Ch.35 논문 비판 독해 방법론 | `chapters/ch35.html` | 🏆 QA_PASS | Chapter-Agent-P35 | 2026-04-19 | ✅ |
| 🔓 | Ch.36 연구 설계·실험 방법론 | `chapters/ch36.html` | 🏆 QA_PASS | Chapter-Agent-P36 | 2026-04-19 | ✅ |
| 🔓 | Ch.37 선행연구 구조화·문헌 지도 | `chapters/ch37.html` | 🆓 TODO | — | — | — |
| 🔓 | Ch.38 데이터 수집·품질·윤리 관리 | `chapters/ch38.html` | 🆓 TODO | — | — | — |
| 🔓 | Ch.39 방법론 설계 + 논문 글쓰기 IMRaD | `chapters/ch39.html` | 🆓 TODO | — | — | — |
| 🔓 | Ch.40 리뷰 대응·Response Letter·재투고 | `chapters/ch40.html` | 🆓 TODO | — | — | — |

> **🔴 레벨 3 (고급) Phase 3 — Ch.27~Ch.36 QA_PASS 완료 (2026-04-19)** | Ch.37~Ch.40 파일 미생성 (TODO)

---

## 🟢 레벨 1 (입문) — Phase 1 챕터 상태 (완료)

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
| Ch.01C 경우의 수·조합 | ✅ PASS | — |
| Ch.02B 적분 기초 | ✅ PASS | — |
| Ch.03B 선형회귀·최소제곱법 | ✅ PASS | — |
| Ch.05 확률변수·분포 | ✅ PASS | — |
| Ch.06 기대값·분산 | ✅ PASS | — |
| Ch.07 표본·MLE | ✅ PASS | — |
| Ch.08 MAP·Bias-Variance | ✅ PASS | — |
| Ch.09 AI가 확률로 배우는 법 | ✅ PASS | EASY BOX p>strong 수정 완료 (2026-04-19) |
| Ch.09B 모델 평가지표 | ✅ PASS | — |
| Ch.10 Softmax와 다중분류 | ✅ PASS | EASY BOX p>strong 수정 완료 (2026-04-19) |
| Ch.11 정보이론 | ✅ PASS | Jensen's inequality 섹션 추가 (2026-04-19) |
| Ch.12 딥러닝·LLM | ✅ PASS | — |
| Ch.13 Calibration·불확실성 | ✅ PASS | — |
| Ch.14 논문 읽기·쓰기 | ✅ PASS | — |
| Ch.15 수열과 극한의 엄밀한 정의 | ✅ PASS | — |
| Ch.16 연속성과 미분의 엄밀한 정의 | ✅ PASS | — |
| Ch.17 적분과 급수·오차 경계와 근사 | ✅ PASS | — |
| Ch.18 볼록 집합·볼록 함수 | ✅ PASS | — |
| Ch.19 Lagrangian·KKT 조건 | ✅ PASS | — |
| Ch.20 수렴 속도 분석 | ✅ PASS | — |
| Ch.21 σ-대수·확률공간 | ✅ PASS | — |
| Ch.22 확률변수의 수렴 | ✅ PASS | — |
| Ch.23 기대값의 측도론적 정의·Fubini | ✅ PASS | — |
| Ch.24 고유값 분해·스펙트럼 이론 | ✅ PASS | — |
| Ch.25 SVD 완전 유도·LoRA | ✅ PASS | CSS 경미한 하드코딩 (허용) |
| Ch.26 행렬 미분·역전파 수학 | ✅ PASS | — |
| Ch.27 집중 부등식 | ✅ PASS | — |
| Ch.28 PAC 학습 이론·표본 복잡도 | ✅ PASS | PROGRESS TODO였으나 완성 파일 확인 |
| Ch.29 VC 차원·Rademacher 복잡도 | ✅ PASS | — |
| Ch.30 딥러닝 일반화 미스터리 | ✅ PASS | — |
| Ch.31 Transformer Attention 완전 유도 | ✅ PASS | — |
| Ch.32 정보 기하학·Fisher Information | ✅ PASS | CSS 경미한 하드코딩 (허용) |
| Ch.33 마팅게일·확률 과정 | ✅ PASS | PROGRESS TODO였으나 완성 파일 확인 |
| Ch.34 고급 최적화 이론 | ✅ PASS | — |
| Ch.35 논문 비판 독해 방법론 | ✅ PASS | PROGRESS TODO였으나 완성 파일 확인 |
| Ch.36 연구 설계·실험 방법론 | ✅ PASS | — |

> **전체 QA 누계 (2026-04-19 기준)**: ✅ PASS 36개 / ❌ FAIL 0개 (수정 완료 포함)

---

## 업데이트 규칙 요약

| 시점 | PROGRESS.md 변경 | LOCKS.md 변경 |
|------|----------------|-------------|
| 작업 시작 | 내 행: 상태→🔒 IN_PROGRESS, 에이전트→내 이름 | 내 파일: 🔓→🔒 LOCKED + 이름 + 시각 |
| 작업 완료 | 내 행: 상태→✅ DONE, 완료일→날짜 | 내 파일: 🔒→🔓 FREE |
| QA 통과 | 내 행: QA→🏆 QA_PASS | — |
| QA 실패 | 내 행: 상태→❌ QA_FAIL, QA→실패 항목 | — |
| 수정 재시작 | 내 행: 상태→🔒 IN_PROGRESS | 내 파일: 🔓→🔒 LOCKED |

---

## 🔗 통합 완료 (Integration Agent)

- **날짜**: 2026-04-19
- **담당**: Main-Agent (Integration)
- **작업 내용**:
  - `index.html` 사이드바에 Unit 4 (Ch.15~Ch.26) 추가 ✅
  - `index.html` 사이드바에 Unit 5 (Ch.27~Ch.36) 추가 ✅
  - `index.html` 사이드바에 Ch.02D, Ch.14B 추가 ✅
  - 홈 페이지 학습 경로 카드 Unit 4/5 추가 ✅
  - CSS 신규 유닛 색상 클래스 추가 (`.nu-adv`, `.nu-res`, `.pu-adv`, `.pu-res`) ✅
  - JS `TOTAL` 값 23 → 47 업데이트 ✅
  - 사이드바 타이틀 "Phase 1" → "Phase 1~3" 업데이트 ✅
- **연결된 챕터**: Unit 0 (3) + Unit 1 (6) + Unit 2 (5) + Unit 3 (8) + Unit 4 (12) + Unit 5 (10) = 44챕터
- **index.html 업데이트**: ✅ 완료
