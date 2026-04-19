# ROADMAP — 논문을 쓰는 연구자로 가는 전체 로드맵

> **작성일**: 2026-04-19  
> **목표**: 수학 배경 없는 학습자 → SCI 논문을 스스로 읽고 쓰는 연구자  
> **구성**: Phase 1 (현재 완성) + Phase 2~4 (개발 예정)

---

## 전체 구조 한눈에 보기

```
Phase 1 │ 18챕터 │ 입문 브리지 (보강 포함) → "논문 수식이 무섭지 않다"
Phase 2 │ 12주   │ 수학 심화               → "증명을 따라갈 수 있다"
Phase 3 │ 12주   │ ML 이론                 → "논문 Contribution을 이해한다"
Phase 4 │  8주   │ 연구 실전               → "SCI 논문을 스스로 쓴다"
──────────────────────────────────────────────────────────
합계    │ 약 50주 │ 약 12개월
```

---

## Phase 1 — 입문 브리지 (진행 중, 18챕터)

> **목표**: AI 논문을 처음 펼쳤을 때 수식과 기호를 두려워하지 않는다.  
> **수준**: 고등학교 수학 → 학부 2학년 수준  
> **상태**: 16개 QA_PASS / 2개 QA_FAIL 수정 중 / 통합 대기

### Phase 1 챕터 목록 (현재 확정 18개)

| 순서 | 파일 | 챕터명 | 핵심 개념 | QA 상태 | 비고 |
|------|------|--------|----------|:-------:|------|
| 0-A | `ch00a.html` | 로그와 지수함수 | log, e^x, 자연로그, AI 손실함수 연결 | 🏆 PASS | 보강 챕터 |
| 0-B | `ch00b.html` | 수열과 Σ 표기 | 수열, Σ 표기, 평균·합 표현 | 🏆 PASS | 보강 챕터 |
| 1 | `ch01.html` | 집합과 함수 | 집합 기호, f: X→Y, ∀, ∃ | 🏆 PASS | 핵심 |
| 2 | `ch02.html` | 미분과 최적화 | f'(x), 경사하강법, 학습률 | 🏆 PASS | 핵심 |
| 2-B | `ch02b.html` | 적분 기초 | 정적분, 기댓값과 적분 연결 | 🏆 PASS | 보강 챕터 |
| 3 | `ch03.html` | 선형대수 기초 | 벡터, 행렬 곱, 내적 | 🏆 PASS | 핵심 |
| 3-B | `ch03b.html` | 선형회귀·최소제곱법 | OLS, 정규방정식, RSS | 🏆 PASS | 보강 챕터 |
| 4 | `ch04.html` | 확률론·베이즈 | P(A\|B), prior/posterior | 🏆 PASS | 핵심 |
| 5 | `ch05.html` | 확률변수·분포 | 정규분포, 베르누이, 포아송 | 🏆 PASS | 핵심 |
| 6 | `ch06.html` | 기대값·분산 | E[X], Var[X], 공분산 | 🏆 PASS | 핵심 |
| 7 | `ch07.html` | 표본·MLE | 최대우도추정, 표본분포 | 🏆 PASS | 핵심 |
| 8 | `ch08.html` | MAP·Bias-Variance | 정규화, 과적합 | 🏆 PASS | 핵심 |
| 9 | `ch09.html` | ML연결1·Logistic | sigmoid, cross-entropy | ❌ FAIL | EASY BOX 수정 중 |
| 10 | `ch10.html` | ML연결2·Softmax | softmax, multinomial | ❌ FAIL | EASY BOX 수정 중 |
| 11 | `ch11.html` | 정보이론 | entropy, KL divergence | 🏆 PASS | 핵심 |
| 12 | `ch12.html` | 딥러닝·LLM | attention, transformer | 🏆 PASS | 핵심 |
| 13 | `ch13.html` | Calibration | ECE, temperature scaling | 🏆 PASS | 핵심 |
| 14 | `ch14.html` | 논문읽기·쓰기 | 논문 구조, 기호 해독 | 🏆 PASS | 핵심 |

### 보강 챕터 추가 배경

원래 14주 계획에서 **4개 보강 챕터가 자동 추가**됨:

| 보강 챕터 | 추가 이유 |
|----------|----------|
| Ch.00A 로그·지수 | Ch.01 이전에 로그 개념 없으면 MLE·entropy 이해 불가 |
| Ch.00B 수열·Σ | 평균·기댓값 표기 이해를 위한 사전 준비 |
| Ch.02B 적분 | 연속 확률분포의 PDF·기댓값 계산에 필수 |
| Ch.03B 선형회귀 | 행렬·최적화 개념의 실전 적용 사례 |

### Phase 1에서 추가될 가능성 있는 챕터 (다른 에이전트 작업 중)

> 아래는 확정 전 — 다른 에이전트가 작업 완료 시 위 목록에 추가

| 예상 파일 | 예상 내용 | 추가 위치 |
|----------|----------|----------|
| `ch04b.html` | 독립성·조건부독립 | Ch.04 이후 |
| `ch05b.html` | CLT·대수의 법칙 | Ch.05 이후 |
| `ch08b.html` | 교차검증·정규화 기법 | Ch.08 이후 |
| `ch12b.html` | 역전파 수학 | Ch.12 이후 |

### Phase 1 통합 조건 (index.html)

```
✅ 조건 1: 모든 현재 챕터 QA_PASS (ch09, ch10 수정 완료 후)
✅ 조건 2: 다른 에이전트의 보강 작업 완료 확인
✅ 조건 3: 전체 챕터 NAV 링크 순서 재검토
→ Integration Agent 실행
```

**Phase 1 졸업 기준**: NIPS/ICML 논문을 펼쳤을 때 수식의 50% 이상 읽을 수 있다.

---

## Phase 2 — 수학 심화 (개발 예정, 12주)

> **목표**: 논문의 Theorem, Proof, Lemma를 따라갈 수 있다.  
> **수준**: 학부 3~4학년 / 대학원 1학기  
> **선행 조건**: Phase 1 전체 완료

### 2-A. 해석학 기초 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 1 | 수열과 극한의 엄밀한 정의 | ε-δ 논증, Cauchy 수열 | 수렴 증명 읽기 |
| 2 | 연속성과 미분의 엄밀한 정의 | Lipschitz 연속, 미분가능성 조건 | Gradient Lipschitz 조건 |
| 3 | 적분과 급수 | Riemann 적분, 테일러 급수, 오차 경계 | 근사 오차 분석 |

### 2-B. 볼록 최적화 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 4 | 볼록 집합·볼록 함수 | 볼록성 정의, Jensen's inequality | Loss 함수 분석 |
| 5 | Lagrangian·KKT 조건 | 등호/부등호 제약, dual problem | SVM, 정규화 유도 |
| 6 | 수렴 속도 분석 | Gradient descent convergence, O(1/T) | 논문 complexity 읽기 |

### 2-C. 측도 기반 확률론 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 7 | σ-대수·확률공간 | Borel σ-algebra, 측도 | 확률론 증명 기반 |
| 8 | 확률변수의 수렴 | a.s., in probability, in distribution | CLT, LLN 증명 |
| 9 | 기대값의 측도론적 정의 | Lebesgue 적분, Fubini 정리 | 연속 분포 계산 |

### 2-D. 선형대수 심화 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 10 | 고유값 분해·스펙트럼 이론 | 대칭행렬, PSD, Rayleigh quotient | PCA, 공분산 분석 |
| 11 | SVD 완전 유도 | 저랭크 근사, Eckart-Young 정리 | LoRA, 차원 축소 |
| 12 | 행렬 미분 | ∂(Ax)/∂x, ∂(xᵀAx)/∂x, 체인룰 | Backprop 행렬 버전 |

**Phase 2 졸업 기준**: NeurIPS 논문의 Theorem 섹션을 혼자서 검증할 수 있다.

---

## Phase 3 — ML 이론 (개발 예정, 12주)

> **목표**: 논문의 Contribution과 Novelty를 정확히 이해하고 비판적으로 평가한다.  
> **수준**: 대학원 1~2학기 (Stanford CS229, CS231n 수준)  
> **선행 조건**: Phase 2 전체 완료

### 3-A. 통계적 학습 이론 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 1 | PAC Learning | 샘플 복잡도, 일반화 이론 | Generalization bound 읽기 |
| 2 | VC Dimension | 성장함수, Shattering | 모델 복잡도 이론 |
| 3 | Rademacher 복잡도 | 현대적 일반화 이론, Margin bound | 딥러닝 이론 논문 |

### 3-B. 고전 ML 이론 심화 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 4 | Kernel Methods | RKHS, Mercer 정리, kernel SVM | 커널 논문 읽기 |
| 5 | 가우시안 프로세스 | GP 회귀, 베이즈 최적화 | Hyperparameter tuning |
| 6 | EM Algorithm | 기대값 최대화, GMM, HMM | Latent variable model |

### 3-C. 딥러닝 이론 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 7 | Universal Approximation | 표현력 이론, 깊이 vs 넓이 | 아키텍처 선택 근거 |
| 8 | Optimization 이론 | Loss landscape, 안장점, BatchNorm 이론 | 학습 안정성 분석 |
| 9 | Attention·Transformer 이론 | Self-attention 복잡도, positional encoding | LLM 논문 기반 |

### 3-D. 확률 생성 모델 (3주)

| 주차 | 챕터 | 핵심 내용 | 논문 연결 |
|------|------|----------|----------|
| 10 | VAE — Variational Autoencoder | ELBO 유도, reparameterization trick | 생성 모델 논문 |
| 11 | Flow 모델·GAN 이론 | Normalizing flow, JS divergence, Wasserstein | 생성 모델 심화 |
| 12 | Diffusion Models | DDPM, score matching, SDE | 최신 생성 모델 논문 |

**Phase 3 졸업 기준**: NeurIPS/ICML 논문을 읽고 "왜 이 방법이 기존 것보다 나은가"를 수식 수준에서 설명할 수 있다.

---

## Phase 4 — 연구 실전 (개발 예정, 8주)

> **목표**: SCI 논문을 처음부터 끝까지 스스로 쓰고 제출한다.  
> **수준**: 대학원 2학기 ~ 논문 제출 단계  
> **선행 조건**: Phase 3 전체 완료

### 4-A. 논문 해부 (2주)

| 주차 | 주제 | 실습 |
|------|------|------|
| 1 | 논문 구조 분해 | Abstract/Intro/Related Work/Method/Exp/Conclusion 각 섹션의 역할과 분량 |
| 2 | 유명 논문 50편 구조 분석 | "Attention is All You Need", "BERT", "LoRA" 등을 섹션별로 해부 |

### 4-B. 실험 설계 (2주)

| 주차 | 주제 | 실습 |
|------|------|------|
| 3 | Ablation Study 설계 | 변인 통제, 기여도 분리, 베이스라인 선정 |
| 4 | 통계적 유의성 검증 | p-value, confidence interval, paired t-test를 논문에 적용 |

### 4-C. 논문 쓰기 실전 (3주)

| 주차 | 주제 | 실습 |
|------|------|------|
| 5 | 관련 연구 (Related Work) 작성법 | 연구 흐름 지도 그리기, 비교 표 작성 |
| 6 | Method 섹션 작성법 | 수식 → 알고리즘 → 그림 순서 설계, 기호 일관성 |
| 7 | Introduction·Abstract 작성 | Problem → Gap → Contribution → Result 구조 |

### 4-D. 리뷰어 관점 훈련 (1주)

| 주차 | 주제 | 실습 |
|------|------|------|
| 8 | OpenReview 분석 | NeurIPS reject 논문 리뷰 읽기, Rebuttal 작성 연습 |

**Phase 4 졸업 기준**: arXiv에 논문을 올리거나 학술대회에 제출할 수 있는 수준의 초고 완성.

---

## 각 Phase 졸업 시 읽을 수 있는 논문 수준

| Phase | 읽을 수 있는 논문 예시 |
|-------|----------------------|
| Phase 1 졸업 | 딥러닝 입문 블로그 수준, Distill.pub 아티클 |
| Phase 2 졸업 | "Attention is All You Need" 수식 절반 이상 |
| Phase 3 졸업 | NeurIPS/ICML 메인 트랙 논문 대부분 |
| Phase 4 졸업 | SCI 논문 제출 가능, 리뷰어 역할 가능 |

---

## 개발 우선순위 및 일정

| 단계 | 개발 시작 조건 | 예상 콘텐츠 분량 |
|------|--------------|----------------|
| Phase 1 (현재) | — | HTML 14개 챕터 |
| Phase 2 | Phase 1 전체 QA_PASS | HTML 12개 챕터 + 증명 인터랙티브 |
| Phase 3 | Phase 2 전체 완료 | HTML 12개 챕터 + 코드 실습 (Python/Jupyter) |
| Phase 4 | Phase 3 전체 완료 | HTML 8개 챕터 + 논문 작성 워크시트 |

---

## Phase 1의 위치 재정의

> Phase 1은 "이 코스만 들으면 논문을 쓸 수 있다"가 아니라  
> **"이 코스를 들으면 Phase 2~4를 두려움 없이 시작할 수 있다"** 는 브리지다.  
>
> 초등학생 비유 → 대학원 수식으로 이어지는 **언어 번역 코스**.

---

_이 문서는 Phase 2~4 개발 시작 시 각 챕터별 weekNN.md 명세로 분리한다._
