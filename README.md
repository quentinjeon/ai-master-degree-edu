# 📐 논문을 위한 확률·수학·AI 인터랙티브 교재

> **"초등학생도 이해할 수 있는 비유 + 논문 수준의 수식"을 같은 페이지에서**

[![Phase](https://img.shields.io/badge/Phase-1·2·3%20완료-brightgreen)](#전체-로드맵)
[![Chapters](https://img.shields.io/badge/챕터-36개-blue)](#챕터-목록)
[![QA](https://img.shields.io/badge/QA_PASS-36%2F36-brightgreen)](#qa-현황)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](#라이선스)

---

## 미리보기

<table>
<tr>
<td width="50%">
<img src="assets/screenshots/00a_log.png" alt="Ch.00A 로그와 지수함수"/>
<p align="center"><b>Ch.00A</b> 로그와 지수함수</p>
</td>
<td width="50%">
<img src="assets/screenshots/01_sets.png" alt="Ch.01 집합과 함수"/>
<p align="center"><b>Ch.01</b> 집합과 함수</p>
</td>
</tr>
<tr>
<td width="50%">
<img src="assets/screenshots/02_diff.png" alt="Ch.02 미분과 최적화"/>
<p align="center"><b>Ch.02</b> 미분과 최적화 (경사하강법 시뮬레이터 포함)</p>
</td>
<td width="50%">
<img src="assets/screenshots/04_bayes.png" alt="Ch.04 확률론·베이즈"/>
<p align="center"><b>Ch.04</b> 확률론 기초 & 베이즈 정리</p>
</td>
</tr>
<tr>
<td width="50%">
<img src="assets/screenshots/05_dist.png" alt="Ch.05 확률변수·분포"/>
<p align="center"><b>Ch.05</b> 확률변수 & 확률분포</p>
</td>
<td width="50%">
<img src="assets/screenshots/07_mle.png" alt="Ch.07 표본·MLE"/>
<p align="center"><b>Ch.07</b> 표본 & 최대우도추정 (MLE)</p>
</td>
</tr>
<tr>
<td width="50%">
<img src="assets/screenshots/11_info.png" alt="Ch.11 정보이론"/>
<p align="center"><b>Ch.11</b> 정보이론 (Entropy & KL Divergence)</p>
</td>
<td width="50%">
<img src="assets/screenshots/12_llm.png" alt="Ch.12 딥러닝·LLM"/>
<p align="center"><b>Ch.12</b> 딥러닝 & LLM (Attention/Transformer)</p>
</td>
</tr>
</table>

---

## 개요

이 프로젝트는 **AI 논문을 읽고 쓰기 위해 필요한 기초 수학·확률·통계·정보이론**을 단계별로 익히는 인터랙티브 HTML 교재입니다.

수학 배경 없는 학습자가 SCI 국제학술지 논문을 스스로 읽고 쓰는 연구자로 성장하는 것을 최종 목표로 하며, 총 4개의 Phase에 걸쳐 약 50주(12개월) 커리큘럼으로 구성됩니다.

### 핵심 철학

모든 개념은 아래 **3개 층위**를 동시에 제공합니다:

| 층위 | 목표 독자 | 설명 방식 |
|------|-----------|-----------|
| **Layer 1** | 초등학생 | 일상 비유, 예시 리스트, 그림 언어 |
| **Layer 2** | 학부생 | 직관적 해설, 개념 카드, 표 |
| **Layer 3** | 대학원생/연구자 | 수학 공식 (MathJax), 논문 기호, AI 연결 |

---

## 대상 사용자

- 논문을 처음 쓰려는 대학원 입학 준비생
- AI/ML을 공부하지만 수학 배경이 부족한 개발자
- 확률·통계를 실무 AI와 연결하고 싶은 모든 학습자
- 수식을 이해하지 못한 채 딥러닝 코드만 쓰고 있는 엔지니어

---

## 전체 로드맵

```
Phase 1 │ 18챕터 │ 입문 브리지     → "논문 수식이 무섭지 않다"        ✅ 완료
Phase 2 │ 12챕터 │ 수학 심화       → "증명을 따라갈 수 있다"           ✅ 완료
Phase 3 │ 10챕터 │ ML 이론         → "논문 Contribution을 이해한다"    ✅ 완료
Phase 4 │  8주   │ 연구 실전       → "SCI 논문을 스스로 쓴다"           🔜 예정
────────────────────────────────────────────────────────────────────
합계    │ 약 50주 │ 약 12개월
```

| Phase | 졸업 기준 | 읽을 수 있는 논문 수준 |
|-------|----------|----------------------|
| Phase 1 ✅ | NIPS/ICML 수식의 50% 이상 읽기 가능 | Distill.pub 아티클, 딥러닝 입문 |
| Phase 2 ✅ | NeurIPS Theorem 섹션 혼자 검증 가능 | "Attention is All You Need" 수식 절반 이상 |
| Phase 3 ✅ | 논문 Contribution을 수식 수준에서 비판 가능 | NeurIPS/ICML 메인 트랙 논문 대부분 |
| Phase 4 🔜 | SCI 논문 초고 완성, 리뷰어 역할 가능 | arXiv 제출 및 학술대회 투고 |

---

## 챕터 목록

### Unit 0 — 사전 준비 (Phase 1)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch00a.html` | 로그와 지수함수 | log, e^x, 자연로그, AI 손실함수 연결 | 🏆 PASS |
| `ch00b.html` | 수열과 Σ 표기 | 수열, Σ 표기, 평균·합 표현 | 🏆 PASS |
| `ch00c.html` | 경우의 수·조합 | 순열, 조합, nCr, 확률 연결 | 🏆 PASS |

### Unit 1 — 기초 수학 (Phase 1)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch01.html` | 집합과 함수 | 집합 기호, f: X→Y, ∀, ∃ | 🏆 PASS |
| `ch02.html` | 미분과 최적화 | f'(x), 경사하강법, 학습률 | 🏆 PASS |
| `ch02b.html` | 적분 기초 | 정적분, 기댓값과 적분 연결 | 🏆 PASS |
| `ch02d.html` | 경사하강법 실습 | SGD, momentum, 시뮬레이터 | 🏆 PASS |
| `ch03.html` | 선형대수 기초 | 벡터, 행렬 곱, 내적 | 🏆 PASS |
| `ch03b.html` | 선형회귀·최소제곱법 | OLS, 정규방정식, RSS | 🏆 PASS |

### Unit 2 — 확률 & 통계 (Phase 1)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch04.html` | 확률론·베이즈 | P(A\|B), prior/posterior | 🏆 PASS |
| `ch05.html` | 확률변수·분포 | 정규분포, 베르누이, 포아송 | 🏆 PASS |
| `ch06.html` | 기대값·분산 | E[X], Var[X], 공분산 | 🏆 PASS |
| `ch07.html` | 표본·MLE | 최대우도추정, 표본분포 | 🏆 PASS |
| `ch08.html` | MAP·Bias-Variance | 정규화, 과적합 | 🏆 PASS |

### Unit 3 — AI & ML 연결 (Phase 1)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch09.html` | Logistic 회귀 | sigmoid, cross-entropy | 🏆 PASS |
| `ch09b.html` | 모델 평가지표 | Precision, Recall, F1, AUC | 🏆 PASS |
| `ch10.html` | Softmax·다중분류 | softmax, multinomial | 🏆 PASS |
| `ch11.html` | 정보이론 | entropy, KL divergence | 🏆 PASS |
| `ch12.html` | 딥러닝·LLM | attention, transformer | 🏆 PASS |
| `ch13.html` | Calibration | ECE, temperature scaling | 🏆 PASS |
| `ch14.html` | 논문 읽기·쓰기 | 논문 구조, 기호 해독 | 🏆 PASS |
| `ch14b.html` | 논문 5-question 프레임워크 | 비판적 독해, 재현 체크리스트 | 🏆 PASS |

### Unit 4 — 수학 심화 Lv.2 (Phase 2)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch15.html` | 수열과 극한 | ε-δ 논증, Cauchy 수열, 완비성 | 🏆 PASS |
| `ch16.html` | 연속성과 미분 | Lipschitz 연속, 미분가능성 | 🏆 PASS |
| `ch17.html` | 적분과 급수·오차 경계 | Taylor 급수, Remainder 추정 | 🏆 PASS |
| `ch18.html` | 볼록 집합·볼록 함수 | Convexity, Jensen 부등식 | 🏆 PASS |
| `ch19.html` | Lagrangian·KKT | 제약 최적화, KKT 조건 | 🏆 PASS |
| `ch20.html` | 수렴 속도 분석 | O(1/T), O(1/√T), 수렴 보장 | 🏆 PASS |
| `ch21.html` | σ-대수·확률공간 | 측도론, 가측 공간 | 🏆 PASS |
| `ch22.html` | 확률변수의 수렴 | a.s., L², 분포 수렴, CLT | 🏆 PASS |
| `ch23.html` | 기대값·Fubini | 측도론적 기대값, Fubini 정리 | 🏆 PASS |
| `ch24.html` | 고유값 분해 | EVD, PCA 연결 | 🏆 PASS |
| `ch25.html` | SVD·LoRA | 특이값 분해, LoRA 행렬 근사 | 🏆 PASS |
| `ch26.html` | 행렬 미분·역전파 | Jacobian, Chain Rule, Backprop | 🏆 PASS |

### Unit 5 — ML 이론 Lv.3 (Phase 3)

| 파일 | 챕터명 | 핵심 개념 | QA |
|------|--------|----------|----|
| `ch27.html` | 집중 부등식 | Markov, Chebyshev, Hoeffding | 🏆 PASS |
| `ch28.html` | PAC 학습·표본 복잡도 | PAC Learning, Sample Complexity | 🏆 PASS |
| `ch29.html` | VC 차원·Rademacher | VC Dimension, Rademacher 복잡도 | 🏆 PASS |
| `ch30.html` | 딥러닝 일반화 (Double Descent) | Double Descent, 암묵적 정규화 | 🏆 PASS |
| `ch31.html` | Transformer Attention | Self-Attention, Softmax 안정성 | 🏆 PASS |
| `ch32.html` | 정보 기하학·Fisher | Fisher Information, Natural Gradient | 🏆 PASS |
| `ch33.html` | 마팅게일·확률 과정 | Martingale, Optional Stopping | 🏆 PASS |
| `ch34.html` | 고급 최적화 (Adam·NGD) | Adam, AdaGrad, Natural Gradient | 🏆 PASS |
| `ch35.html` | 논문 비판 독해 | 반례 검토, Proof 검증 | 🏆 PASS |
| `ch36.html` | 연구 설계·실험 방법론 | Ablation Study, 통계 검정 | 🏆 PASS |

---

## QA 현황

| 상태 | 챕터 수 | 유닛 |
|------|--------|------|
| 🏆 QA_PASS | **36 / 36** | Unit 0~5 전체 |
| 🔜 개발 예정 | — | Phase 4 (연구 실전) |

---

## 프로젝트 구조

```
percent_ml/
├── index.html                    ← 메인 진입점 (사이드바 통합, Unit 0~5)
├── chapters/                     ← 챕터별 독립 HTML (51개)
│   ├── ch00a.html ~ ch00f.html   ← Unit 0: 사전 준비
│   ├── ch01.html ~ ch03b.html    ← Unit 1: 기초 수학
│   ├── ch04.html ~ ch08.html     ← Unit 2: 확률 & 통계
│   ├── ch09.html ~ ch14b.html    ← Unit 3: AI & ML 연결
│   ├── ch15.html ~ ch26.html     ← Unit 4: 수학 심화 (Phase 2)
│   └── ch27.html ~ ch36.html     ← Unit 5: ML 이론 (Phase 3)
└── docs/                         ← 개발 문서 (에이전트용)
    ├── PRD.md                    ← 전체 요구사항 명세
    ├── ROADMAP.md                ← Phase 1~4 전체 로드맵
    ├── DESIGN_SYSTEM.md          ← CSS/UI 컴포넌트 규칙
    ├── CHAPTER_TEMPLATE.md       ← 챕터 HTML 빈 템플릿
    ├── PROGRESS.md               ← 챕터별 개발 진행 상황
    ├── QA_REPORT.md              ← QA 검증 리포트 전체
    ├── LOCKS.md                  ← 동시 작업 충돌 방지 락
    └── chapters/                 ← 챕터별 콘텐츠 명세 (week00a.md ~ week36.md)
```

---

## 챕터 구성 원칙

모든 챕터는 아래 **7개 필수 블록**을 순서대로 포함합니다:

```
[1] HERO          — 챕터 번호·카테고리·주차 뱃지 + 제목 + 학습 목적 한 줄
[2] EASY BOX      — 🧒 초등학생 비유 (일상 사례 + AI 연결)
[3] MAIN CONTENT  — 핵심 개념 설명 (직관 위주, 표/개념카드 포함)
[4] FORMULA BOX   — MathJax 수식 + 한국어 기호 주석 (.fn)
[5] INTERACTIVE   — 실시간 파라미터 조작 계산기·시뮬레이터
[6] QUIZ          — 4지선다 확인 문제 (즉시 피드백)
[7] NAV BUTTONS   — 이전/다음 챕터 이동 버튼
```

---

## 기술 스택

| 항목 | 선택 | 이유 |
|------|------|------|
| 마크업 | Vanilla HTML5 | 의존성 제로, 어디서나 열림 |
| 스타일 | CSS Variables (다크 테마) | 디자인 토큰 기반, 유지보수 용이 |
| 수식 렌더링 | [MathJax 3](https://www.mathjax.org/) | LaTeX 문법 그대로, 논문 수준 수식 |
| 인터랙티브 | Vanilla JavaScript | 빌드 없이 브라우저에서 즉시 실행 |
| 저장 | localStorage | 퀴즈 체크 상태 자동 저장 |

### 디자인 토큰 (색상)

```css
--bg:       #0f1117   /* 전체 배경             */
--surface:  #1a1d27   /* 사이드바 배경         */
--surface2: #22263a   /* 카드 배경             */
--accent:   #6c8aff   /* Unit 0·1 (파란보라)   */
--accent2:  #a78bfa   /* Unit 1 (연보라)       */
--accent3:  #34d399   /* 정답·성공 (초록)      */
--accent4:  #f59e0b   /* Unit 3 (노랑)         */
--accent5:  #f87171   /* Unit 4 (분홍/빨강)    */
            #38bdf8   /* Unit 5 (하늘색)       */
--text:     #e2e8f0   /* 본문 텍스트           */
--muted:    #94a3b8   /* 보조 텍스트           */
```

---

## 빠른 시작

별도 빌드 과정 없이 브라우저로 바로 열 수 있습니다.

```bash
# 저장소 클론
git clone https://github.com/quentinjeon/ai-master-degree-edu.git
cd ai-master-degree-edu

# 브라우저로 열기 (macOS)
open index.html

# 브라우저로 열기 (Linux)
xdg-open index.html

# 또는 로컬 서버 (Python 3)
python -m http.server 8080
# → http://localhost:8080 접속
```

> **권장 브라우저**: Chrome 120+ / Firefox 121+ / Safari 17+  
> MathJax 수식 렌더링에 JavaScript가 활성화되어 있어야 합니다.

---

## 기여 방법

현재 이 프로젝트는 **개인 연구 학습 프로젝트**로 운영되고 있습니다.

오류 발견 및 제안 사항은 아래 연락처로 알려주세요.

---

## 개발자

| 항목 | 내용 |
|------|------|
| **이름** | 전용섭 (Jeon Yong-seop) |
| **소속** | 서울대학교 공학전문대학원 산업AI 트랙 |
| **이메일** | [zerotoanother@snu.ac.kr](mailto:zerotoanother@snu.ac.kr) |
| **GitHub** | [@quentinjeon](https://github.com/quentinjeon) |

---

## 라이선스

이 프로젝트는 [MIT License](LICENSE) 하에 공개됩니다.

교육 목적의 자유로운 활용을 환영합니다. 단, 출처 표기를 부탁드립니다.

---

<div align="center">

**수학 배경 제로 → SCI 논문 제출까지**  
*Phase 1~3 완료 (36챕터) — Phase 4 연구 실전 준비 중*

서울대학교 공학전문대학원 산업AI 트랙 | 전용섭

</div>
