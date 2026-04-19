# QA REPORT — 2026-04-19

> QA Agent 작성. Chapter Agent는 이 파일을 수정하지 않는다.

---

## Ch.01A — 로그와 지수함수 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (지수함수, 로그함수, AI에서의 로그·지수) |
| [4] FORMULA BOX | ✅ | .fb 5개 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] + oninput="updateLG()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(d번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch01.html, .btn-p → ch00b.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용. 템플릿 수준 색상값은 이전 QA와 동일 기준으로 허용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.01B — 수열과 Σ·Π 표기 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (Σ 표기, 수열, Π 표기) |
| [4] FORMULA BOX | ✅ | .fb 4개 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 3개 + oninput="updateSQ()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(a번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch00a.html, .btn-p → ch02.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.02B — 적분 기초 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (정적분, 확률분포와 적분, 연속형 기대값) |
| [4] FORMULA BOX | ✅ | .fb 4개 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateIT()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(c번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch02.html, .btn-p → ch03.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.03B — 선형회귀와 최소제곱법 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 4개 (선형회귀 모델, MSE 손실함수, 최소제곱법, 딥러닝으로의 확장) |
| [4] FORMULA BOX | ✅ | .fb 4개 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateLR()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(d번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch03.html, .btn-p → ch04.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.05 — 확률변수와 확률분포 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | `<section class="hero">` 형태. .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (확률변수, AI 주요 확률분포, 정규분포) |
| [4] FORMULA BOX | ✅ | .fb 2개 (정규분포 PDF, 다변량 정규분포) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 3개 + oninput="updateND()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(b번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch04.html, .btn-p → ch06.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용. .qo.wrong에 var(--accent5) 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.06 — 기대값과 분산 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (기대값, 분산과 표준편차, 공분산 & 상관관계) |
| [4] FORMULA BOX | ✅ | .fb 4개 (기대값, 선형성, 분산, 공분산) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateEV()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(c번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch05.html, .btn-p → ch07.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.07 — 표본, 통계량, 최대우도추정 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (모집단과 표본, 우도 & MLE, 신뢰구간) |
| [4] FORMULA BOX | ✅ | .fb 3개 (표본평균·분산, MLE 목적함수, 95% 신뢰구간) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 2개 (MLE 시뮬레이터, 신뢰구간 계산기), 모두 input[type=range] + oninput 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(c번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch06.html, .btn-p → ch08.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.08 — MAP, Bias-Variance Tradeoff, 가설검정 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (MAP 추정, Bias-Variance Tradeoff, 통계적 가설 검정) |
| [4] FORMULA BOX | ✅ | .fb 2개 (MAP vs MLE, Bias-Variance 분해) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateBV()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(c번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch07.html, .btn-p → ch09.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.09 — AI가 확률로 배우는 법 (2026-04-19)

**결과**: FAIL ❌

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | `<section class="hero">` 형태. .badge, h1, p 모두 존재 |
| [2] EASY BOX | ❌ | .et ✅, ul>li 3개 ✅, 마지막 li AI 연결 ✅, **그러나 `<p>` 태그 내부에 `<strong>` 없음** |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (지도학습 구조, 시그모이드 함수, 로지스틱 회귀) |
| [4] FORMULA BOX | ✅ | .fb 3개 (Sigmoid, Logistic Regression, BCE Loss) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 2개, 모두 input[type=range] + oninput 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(B번), .qfb 존재 |
| [7] NAV | ✅ | `<nav class="nb">`, .btn-s → ch08.html, .btn-p → ch10.html |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**:
- [2] EASY BOX의 `<p>` 태그에 핵심 비유를 `<strong>`으로 감싸야 함. 현재: `<p>AI는 '이 답이 맞을 가능성이 얼마나 큰가?'...` → 수정: `<p><strong>"AI는 '이 답이 맞을 가능성이 얼마나 큰가?'를 배우는 기계다."</strong></p>` 형태로 변경

---

## Ch.10 — Softmax와 다중분류 (2026-04-19)

**결과**: FAIL ❌

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | `<section class="hero">` 형태. .badge, h1, p 모두 존재 |
| [2] EASY BOX | ❌ | .et ✅, ul>li 3개(li 내부에는 `<strong>`) ✅, 마지막 li AI 연결 ✅, **그러나 `<p>` 태그 자체에 `<strong>` 없음** |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (Softmax 함수, 다중분류 Cross-Entropy, Naive Bayes 분류기) |
| [4] FORMULA BOX | ✅ | .fb 3개 (Softmax, Categorical CE, Naive Bayes) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 3개 + oninput="updateSM()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(b번), .qfb 존재 |
| [7] NAV | ✅ | `<nav class="nb">`, .btn-s → ch09.html, .btn-p → ch11.html |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**:
- [2] EASY BOX의 `<p>` 태그에 핵심 비유를 `<strong>`으로 감싸야 함. 현재: `<p>고양이, 강아지, 토끼 중 하나를...` → 수정: `<p><strong>"고양이, 강아지, 토끼 중 하나를 골라야 할 때, AI는 각각의 가능성을 계산해서 가장 큰 것을 고른다."</strong></p>` 형태로 변경

---

## Ch.11 — 정보이론: Entropy, Cross-Entropy, KL Divergence (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (엔트로피, 교차 엔트로피, KL 발산) |
| [4] FORMULA BOX | ✅ | .fb 3개 (Shannon Entropy, Cross-Entropy, KL Divergence) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateEnt()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(b번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch10.html, .btn-p → ch12.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.12 — 딥러닝과 언어 모델 (LLM) (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (언어 모델=조건부 확률, LLM 학습 목표, 토큰 샘플링 전략) |
| [4] FORMULA BOX | ✅ | .fb 3개 (문장 확률, NLL 손실, Temperature Scaling) 모두 .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 4개 + oninput="updateTemp()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(b번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch11.html, .btn-p → ch13.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.13 — 불확실성과 캘리브레이션 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 3개 (불확실성의 두 종류, 캘리브레이션, 임계값 결정) |
| [4] FORMULA BOX | ✅ | .fb 1개 (완벽한 Calibration 조건) .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | ✅ | .calc 존재, input[type=range] 2개 + oninput="updateCal()" 연결 |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(b번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch12.html, .btn-p → ch14.html (실제 파일명) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## Ch.14 — 논문 읽기와 쓰기 (2026-04-19)

**결과**: PASS ✅

| 항목 | 결과 | 비고 |
|------|------|------|
| [1] HERO | ✅ | .badge, h1, p 모두 존재. PLACEHOLDER 없음 |
| [2] EASY BOX | ✅ | .et, p(strong 포함), ul>li 3개. 마지막 li AI 연결 |
| [3] MAIN CONTENT | ✅ | h2.st 4개 (논문 구조, 수학 표기 읽기, 논문 읽기 전략, 연구문제 수학적 표현) |
| [4] FORMULA BOX | ✅ | .fb 1개 (연구문제의 수학적 표현) .fl + 수식 + .fn 완비 |
| [5] INTERACTIVE | N/A | 논문 읽기·쓰기 챕터 특성상 수치 계산기 불필요. .calc 블록 없음 (적절) |
| [6] QUIZ | ✅ | .qo 4개, ans(this,true) 1개(c번), .qfb 존재 |
| [7] NAV | ✅ | .btn-s → ch13.html, .btn-p → ../index.html (마지막 챕터이므로 홈으로 연결 — 적절) |
| [8] CSS | ✅ | :root 변수 사용 |
| [9] MathJax | ✅ | \( \) 인라인, \[ \] 블록 모두 사용 |

**수정 요청**: 없음

---

## 종합 결과 요약

| 챕터 | 파일 | 결과 | 실패 항목 |
|------|------|:----:|---------|
| Ch.01A 로그와 지수함수 | ch00a.html | ✅ PASS | — |
| Ch.01B 수열과 Σ 표기 | ch00b.html | ✅ PASS | — |
| Ch.02B 적분 기초 | ch02b.html | ✅ PASS | — |
| Ch.03B 선형회귀·최소제곱법 | ch03b.html | ✅ PASS | — |
| Ch.05 확률변수·분포 | ch05.html | ✅ PASS | — |
| Ch.06 기대값·분산 | ch06.html | ✅ PASS | — |
| Ch.07 표본·MLE | ch07.html | ✅ PASS | — |
| Ch.08 MAP·Bias-Variance | ch08.html | ✅ PASS | — |
| Ch.09 AI가 확률로 배우는 법 | ch09.html | ❌ FAIL | [2] EASY BOX p 태그 strong 누락 |
| Ch.10 Softmax와 다중분류 | ch10.html | ❌ FAIL | [2] EASY BOX p 태그 strong 누락 |
| Ch.11 정보이론 | ch11.html | ✅ PASS | — |
| Ch.12 딥러닝·LLM | ch12.html | ✅ PASS | — |
| Ch.13 Calibration·불확실성 | ch13.html | ✅ PASS | — |
| Ch.14 논문 읽기·쓰기 | ch14.html | ✅ PASS | — |

**총계**: PASS 12개 / FAIL 2개 (ch09, ch10)
