# PRD: Unit 3 RAG·LLM 고도화 — 3대 Stanford 강좌 완전 통합 계획서

> **버전**: 2.0.0  
> **작성일**: 2026-04-19  
> **통합 강좌 3종**:
> 1. **Stanford CME295** — Transformers and Large Language Models (Autumn 2025) · [플레이리스트](https://www.youtube.com/playlist?list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy)
> 2. **Stanford CS229** — Machine Learning by Andrew Ng (Autumn 2018) · [플레이리스트](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU)
> 3. **Stanford CS336** — Language Modeling from Scratch (Spring 2026) · [플레이리스트](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)

---

## 1. 왜 이 세 강좌인가

| 강좌 | 성격 | 학습 목표 | 현재 커리큘럼과의 관계 |
|------|------|---------|---------------------|
| **CS229** (Andrew Ng) | 이론 기반 ML 전체 | 수학·통계·ML 알고리즘의 수식 수준 이해 | Unit 1~3 수학적 기반 강화 |
| **CME295** (Amidi brothers) | LLM·Transformer 최신 이론 | RAG·RLHF·추론·평가 실무 적용 | Unit 3 LLM 고도화 |
| **CS336** (Percy Liang·Tatsu Hashimoto) | LM 구현 심화 | 토크나이저→학습→스케일링→정렬 직접 구현 | Unit 5 구현 실전 강화 |

> **학습 철학**: CS229로 **왜(이론)**를 이해하고, CME295로 **무엇(최신 LLM)**을 알고, CS336로 **어떻게(구현)**를 익힌다.

---

## 2. 강좌별 전체 강의 목록 및 커리큘럼 매핑

### 2.1 Stanford CS229 — Machine Learning (Autumn 2018)

> **시작 영상**: [Lecture 1 — Introduction](https://youtu.be/jGwO_UgTS7I) | **강사**: Andrew Ng  
> **전체 플레이리스트**: [PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU)  
> **공식 사이트**: [cs229.stanford.edu](https://cs229.stanford.edu/syllabus-autumn2018.html)

| CS229 강의 | 주제 | 유튜브 | 대응 챕터 | 비고 |
|:---:|------|--------|---------|------|
| L1 | Introduction & Basic Concepts | [▶](https://youtu.be/jGwO_UgTS7I) | Ch.01~Ch.02 | 커리큘럼 개요 |
| L2 | Supervised Learning · Linear Regression | — | Ch.03B (선형회귀) | MLE 연결 |
| L3 | Weighted Least Squares · Logistic Regression · GLM | — | Ch.09 (Logistic) | |
| L4 | Perceptron · Exponential Family | — | Ch.05 (분포) | |
| L5 | GDA · Naive Bayes | — | Ch.04 (베이즈) | |
| L6 | Laplace Smoothing · SVM | — | Ch.04 + Ch.19 (KKT) | |
| L9 | Tree Ensembles | — | Ch.08 (B-V) | |
| L10 | Neural Networks: Basics | — | Ch.12 (딥러닝) | |
| L11 | Neural Networks: Training · Backprop | — | Ch.26 (행렬미분) | |
| L12 | Practical Advice for ML | — | Ch.08 (MAP) | |
| L13 | K-means · GMM · EM | — | Ch.05~Ch.06 | |
| L15 | PCA · ICA | — | Ch.24~Ch.25 (SVD) | |
| L16 | MDPs · Bellman Equations | — | Phase 3 기반 | |
| L17 | Value/Policy Iteration · LQR | — | Phase 3 참고 | |
| L18 | Q-Learning · Value Function Approximation | — | Phase 3 참고 | |
| L19 | Policy Search · REINFORCE | — | Ch.12C (RLHF 배경) | |

**CS229 우선 시청 권장 강의** (현재 커리큘럼과 직접 연결):

```
┌─────────────────────────────────────────────────────────────┐
│ 수학 기반 강화 (Unit 1~2 학습 시)                             │
│  L2  → Ch.03B 선형회귀 시청 전/후                             │
│  L3  → Ch.09 Logistic 시청 전/후                             │
│  L5  → Ch.04 베이즈 시청 전/후                               │
│                                                             │
│ ML 이론 강화 (Unit 3 학습 시)                                 │
│  L10 → Ch.12 딥러닝 시청 전/후                               │
│  L11 → Ch.26 역전파 수학 시청 전/후                           │
│  L13 → Ch.06 기대값·EM 시청 후                               │
│  L15 → Ch.24~25 SVD/PCA 시청 후                             │
│                                                             │
│ RLHF 배경 이해 (Unit 3 고도화 시)                             │
│  L16~L19 → Ch.12C RLHF 시청 전 (RL 기초)                    │
└─────────────────────────────────────────────────────────────┘
```

---

### 2.2 Stanford CME295 — Transformers and Large Language Models (Autumn 2025)

> **시작 영상**: [Lecture 1 — Transformer](https://www.youtube.com/watch?v=Ub3GoFaUcds&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=1) | **강사**: Afshine & Shervine Amidi  
> **전체 플레이리스트**: [PLoROMvodv4rOCXd21gf0CF4xr35yINeOy](https://www.youtube.com/playlist?list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy)  
> **공식 사이트**: [cme295.stanford.edu](https://cme295.stanford.edu/syllabus/)

| CME295 강의 | 제목 | 유튜브 | 슬라이드 | 대응 챕터 |
|:---:|------|--------|---------|---------|
| L1 | Transformer: NLP 배경·토크나이저·Attention | [▶ 1:41:58](https://www.youtube.com/watch?v=Ub3GoFaUcds&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=1) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture1.pdf) | Ch.12 + Ch.31 |
| L2 | Transformer 고급: MHA/MQA/GQA·RoPE·BERT | [▶ 1:47:19](https://www.youtube.com/watch?v=yT84Y5zCnaA&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=2) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture2.pdf) | Ch.31 |
| L3 | LLM: MoE·Context Length·Prompting·CoT | [▶ 1:48:44](https://www.youtube.com/watch?v=Q5baLehv5So&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=3) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture3.pdf) | **Ch.12D** |
| L4 | LLM 학습: Pretraining·양자화·SFT·LoRA | [▶ 1:47:27](https://www.youtube.com/watch?v=VlA_jt_3Qc4&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=4) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture4.pdf) | **Ch.12B** |
| L5 | LLM 정렬: RLHF·PPO·DPO | [▶ 1:47:42](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture5.pdf) | **Ch.12C** + Ch.44 |
| L6 | LLM 추론: Reasoning·GRPO·Scaling | [▶ 1:47:10](https://www.youtube.com/watch?v=k5Fh-UgTuCo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=6) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture6.pdf) | **Ch.12D** |
| L7 | Agentic LLMs: RAG·Advanced RAG·Agents | [▶ 1:49:23](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture7.pdf) | **Ch.12E** + Ch.43 |
| L8 | LLM 평가: LLM-as-a-Judge·편향 | [▶ 1:49:25](https://www.youtube.com/watch?v=8fNP4N46RRo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=8) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture8.pdf) | **Ch.12F** |
| L9 | 최신 트렌드·마무리 | [▶ 1:51:31](https://www.youtube.com/watch?v=Q86qzJ1K1Ss&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=9) | [📄](https://cme295.stanford.edu/slides/fall25-cme295-lecture9.pdf) | Ch.14 참고 |

---

### 2.3 Stanford CS336 — Language Modeling from Scratch (Spring 2026)

> **시작 영상**: [Lecture 1 — Overview, Tokenization](https://youtu.be/JuoVZkPBiKk) | **강사**: Percy Liang, Tatsu Hashimoto  
> **전체 플레이리스트**: [PLoROMvodv4rMqXOcazWaTUHhq-yembLCV](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)  
> **공식 사이트**: [cs336.stanford.edu](https://cs336.stanford.edu/)

| CS336 강의 | 날짜 | 제목 | 유튜브 | 대응 챕터 | 비고 |
|:---:|------|------|--------|---------|------|
| L1 | 3/30 | 개요·토크나이저 (BPE, SentencePiece) | [▶](https://youtu.be/JuoVZkPBiKk) | Ch.12 + **Ch.12B** | BPE 수학 |
| L2 | 4/1 | PyTorch·FLOPs·메모리·Arithmetic Intensity | — | Ch.41 (구현) | 시스템 |
| L3 | 4/6 | 아키텍처·하이퍼파라미터 | — | Ch.31 (Transformer) | |
| L4 | 4/8 | Attention 대안·MoE | — | Ch.12D (MoE) | |
| L5 | 4/13 | GPU·TPU 하드웨어 | — | Ch.41 참고 | 시스템 |
| L6 | 4/15 | 커널·Triton·XLA | — | Ch.41 구현 | FlashAttention |
| L7~L8 | 4/20~22 | 병렬화 (Tensor·Pipeline·Data) | — | **Ch.12B** 확장 | 분산 학습 |
| L9·L11 | 4/27~5/4 | Scaling Laws (Chinchilla) | — | **Ch.12B** | 토큰/파라미터 비율 |
| L10 | 4/29 | 추론 최적화 (KV-Cache·Speculative) | — | Ch.12 + **Ch.12D** | |
| L12 | 5/6 | 평가 (Perplexity·벤치마크) | — | **Ch.12F** | |
| L13~L14 | 5/11~13 | 데이터 (Common Crawl·필터링·중복제거) | — | **Ch.12B** + 신규 | |
| L15 | 5/18 | Alignment — RLHF·DPO | — | **Ch.12C** + Ch.44 | |
| L16 | 5/20 | Alignment — RL 알고리즘 (PPO·GRPO) | — | **Ch.12C** + **Ch.12D** | |
| L17 | 5/27 | Alignment — RL 시스템 | — | Ch.44 | |

**CS336 핵심 과제 매핑**:

| 과제 | 내용 | 대응 챕터 |
|------|------|---------|
| Assignment 1 | 토크나이저·Transformer·옵티마이저 구현 | Ch.12B + Ch.31 |
| Assignment 2 | FlashAttention·분산 학습 | Ch.41 + Ch.31 |
| Assignment 3 | Scaling Law 실험 | Ch.12B (스케일링) |
| Assignment 4 | Common Crawl 데이터 처리 | (신규 Ch.12G 제안) |
| Assignment 5 | SFT + RL 추론 학습 + DPO | Ch.12C + Ch.12D |

---

## 3. 3대 강좌 통합 학습 로드맵

### 3.1 단계별 강좌 연계 순서

```
[기초 수식 이해] ──────────────────────────────────────────────
  CS229 L1~L6   ←→   Unit 1 (Ch.01~Ch.03) + Unit 2 (Ch.04~Ch.08)
  "Andrew Ng의 수식 설명 = 이 챕터의 Layer 3"

[ML 알고리즘 연결] ─────────────────────────────────────────────
  CS229 L10~L15 ←→   Unit 3 (Ch.09~Ch.12) + Unit 4 (Ch.24~Ch.26)
  "신경망·역전파·PCA의 직관"

[LLM 이론] ─────────────────────────────────────────────────────
  CME295 L1~L4  ←→   Unit 3 고도화 (Ch.12B~Ch.12D)
  "Transformer·사전학습·파인튜닝·LoRA"

[RAG & 정렬] ───────────────────────────────────────────────────
  CME295 L5~L8  ←→   Unit 3 고도화 (Ch.12C~Ch.12F)
  CS229 L16~L19 ←→   Ch.12C (RLHF 배경으로 RL 기초)
  "RLHF·DPO·RAG·평가 실무"

[구현 심화] ────────────────────────────────────────────────────
  CS336 L1~L17  ←→   Unit 5 (Ch.41~Ch.44) + 신규
  "처음부터 LM 구현 — 토크나이저→학습→스케일링→정렬"
```

### 3.2 주별 병행 학습 계획 (추천)

| 주차 | 교재 챕터 | CS229 시청 | CME295 시청 | CS336 시청 |
|------|---------|----------|------------|-----------|
| **A주** | Ch.12 복습 | L10 (신경망) | L1 (Transformer) | L1 (개요·토크나이저) |
| **B주** | Ch.12B (사전학습·LoRA) | L2 (선형회귀) | L4 (LLM 학습) | L9 (Scaling Laws) |
| **C주** | Ch.12C (RLHF·DPO) | L16~L19 (RL) | L5 (정렬) | L15~L16 (Alignment) |
| **D주** | Ch.12D (추론·CoT) | — | L3 + L6 (추론) | L4 (MoE) + L10 (추론) |
| **E주** | Ch.12E (RAG) ★ | — | L7 (RAG) × 2회 | L13~14 (데이터) |
| **F주** | Ch.12F (평가) | — | L8 (평가) | L12 (평가) |
| **G주** | Ch.43 (RAG 심화) | L15 (PCA/SVD) | L7 재시청 | L6~8 (시스템) |
| **H주** | Ch.44 (RLHF 수학) | L19 (REINFORCE) | L5 재시청 | L17 (RL 시스템) |

---

## 4. 신규 챕터 설계 (전체)

### 4.1 Unit 3 확장 — AI & ML 연결 (8챕터 → 13챕터)

| 챕터 | 파일명 | 명세 | CME295 | CS336 | CS229 | 우선순위 |
|------|--------|------|--------|-------|-------|---------|
| Ch.12B LLM 사전학습·LoRA | `ch12b.html` | `week12b.md` ✅ | L4 | L1, L9 | L10~11 | P0 |
| Ch.12C RLHF·DPO | `ch12c.html` | `week12c.md` ✅ | L5 | L15~16 | L16~19 | P0 |
| Ch.12D LLM 추론·CoT·GRPO | `ch12d.html` | `week12d.md` ✅ | L3+L6 | L4, L10 | — | P1 |
| Ch.12E RAG ★ | `ch12e.html` | `week12e.md` ✅ | L7 | — | — | P0 |
| Ch.12F LLM 평가 | `ch12f.html` | `week12f.md` ✅ | L8 | L12 | — | P1 |

### 4.2 Unit 5 확장 — Phase 3-D (14챕터 → 17챕터)

| 챕터 | 파일명 | 명세 | CME295 | CS336 | CS229 | 우선순위 |
|------|--------|------|--------|-------|-------|---------|
| Ch.43 RAG 수학 기반 | `ch43.html` | `week43.md` ✅ | L7 | L13~14 | — | P2 |
| Ch.44 LLM Alignment 이론 | `ch44.html` | `week44.md` ✅ | L5 | L15~17 | L16~19 | P2 |
| **Ch.45 LM 구현 실습** | `ch45.html` | `week45.md` 🆕 | — | L1~6 | — | P2 |

> **Ch.45 신규 추가**: CS336 Assignment 1~2 내용 기반. 토크나이저(BPE) → Transformer 구현 → FlashAttention 원리 → 분산 학습. CS336 L1~L6 매핑.

---

## 5. CS229 상세 매핑 — 기존 챕터 업그레이드

### 5.1 기존 챕터에 CS229 링크 추가 계획

| 기존 챕터 | CS229 강의 | 추가 방법 |
|---------|----------|---------|
| Ch.03B 선형회귀·최소제곱법 | [L2 Supervised Learning](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.04 확률론·베이즈 | [L5 GDA·Naive Bayes](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.07 표본·MLE | [L2~L3 MLE 설명](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.09 Logistic 회귀 | [L3 Logistic Regression](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.12 딥러닝·LLM | [L10~L11 Neural Networks](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.24 고유값·스펙트럼 | [L15 PCA](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.25 SVD·LoRA | [L15 PCA·ICA](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |
| Ch.12C RLHF | [L16~L19 RL](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | `.callout.info` 블록 |

---

## 6. CS336 신규 챕터 — Ch.45 상세 명세

### Ch.45 — LM 구현 실습: 토크나이저부터 FlashAttention까지

**파일명**: `chapters/ch45.html`  
**명세**: `docs/chapters/week45.md` (신규 작성 필요)  
**Phase**: Unit 5 Phase 3-D  
**기반 강의**: CS336 L1~L6  
**전제**: Ch.31 (Transformer), Ch.41 (구현·재현성), Ch.26 (역전파)

| 섹션 | 내용 | CS336 강의 |
|------|------|-----------|
| 섹션 1 | BPE 토크나이저 수학 (byte-pair encoding 알고리즘) | L1 |
| 섹션 2 | Transformer FLOPs·메모리 계산 (Arithmetic Intensity) | L2 |
| 섹션 3 | FlashAttention 원리 (I/O 최적화, tiling) | L6 |
| 섹션 4 | Scaling Laws — Chinchilla 최적 비율 | L9 + L11 |
| 인터랙티브 | FLOPs 계산기 (d_model, n_layers, seq_len 슬라이더) | L2 |

**핵심 수식**:
```
BPE: merge(ab) → c  s.t. freq(ab) = max_{pair} freq(pair)
FLOPs/token ≈ 6ND  (N: 파라미터, D: 학습 토큰)
Chinchilla: N* = (C/6)^{0.5}, D* = (C/6)^{0.5} × 20
FlashAttention: O(N²d) → O(N²d/M) I/O  (M: SRAM 크기)
```

---

## 7. 전체 챕터 목록 및 강의 연결 완전판

### Unit 3 (AI & ML 연결) — 최종 13챕터

| # | 챕터 | CME295 | CS229 | CS336 |
|---|------|--------|-------|-------|
| 1 | Ch.09 Logistic 회귀 | — | [L3](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | — |
| 2 | Ch.09B 모델 평가지표 | — | L12 | — |
| 3 | Ch.10 Softmax·다중분류 | — | — | — |
| 4 | Ch.11 정보이론 | — | — | — |
| 5 | Ch.12 딥러닝·LLM 기초 | [L1](https://www.youtube.com/watch?v=Ub3GoFaUcds&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=1) | [L10~11](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | [L1](https://youtu.be/JuoVZkPBiKk) |
| 6 | **Ch.12B** LLM 사전학습·LoRA | [L4](https://www.youtube.com/watch?v=VlA_jt_3Qc4&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=4) | L11 | [L9](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 7 | **Ch.12C** RLHF·DPO | [L5](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) | [L16~19](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | [L15~16](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 8 | **Ch.12D** LLM 추론·CoT | [L3+L6](https://www.youtube.com/watch?v=Q5baLehv5So&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=3) | — | [L4, L10](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 9 | **Ch.12E** RAG ★ | [L7](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) | — | — |
| 10 | **Ch.12F** LLM 평가 | [L8](https://www.youtube.com/watch?v=8fNP4N46RRo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=8) | — | [L12](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 11 | Ch.13 Calibration | — | — | — |
| 12 | Ch.14 논문 읽기·쓰기 | [L9](https://www.youtube.com/watch?v=Q86qzJ1K1Ss&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=9) | — | — |
| 13 | Ch.14B 논문 5-question | — | — | — |

### Unit 5 (ML 이론 고급) — 최종 17챕터

| # | 챕터 | CME295 | CS229 | CS336 |
|---|------|--------|-------|-------|
| 1~14 | Ch.27~Ch.40 기존 | 참고 | 참고 | — |
| 15 | Ch.41 구현·재현성 | — | — | [L2, L5~L8](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 16 | Ch.42 효과크기·해석 | — | — | — |
| 17 | **Ch.43** RAG 수학 기반 | [L7](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) | — | [L13~14](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 18 | **Ch.44** LLM Alignment 이론 | [L5](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) | [L16~19](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) | [L15~17](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |
| 19 | **Ch.45** LM 구현 실습 🆕 | — | — | [L1~L6](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |

---

## 8. 개발 우선순위 및 에이전트 계획

### Wave 6 — Unit 3 확장 (CME295 기반, 5개 병렬)
```
Chapter-Agent-P12B  →  ch12b.html  [CME295 L4 + CS336 L9 + CS229 L10]
Chapter-Agent-P12C  →  ch12c.html  [CME295 L5 + CS336 L15~16 + CS229 L16~19]
Chapter-Agent-P12D  →  ch12d.html  [CME295 L3+L6 + CS336 L4/L10]
Chapter-Agent-P12E  →  ch12e.html  [CME295 L7] ★ 최우선
Chapter-Agent-P12F  →  ch12f.html  [CME295 L8 + CS336 L12]
```

### Wave 7 — Phase 3-D 확장 (3개 병렬)
```
Chapter-Agent-P43  →  ch43.html  [CME295 L7 + CS336 L13~14]
Chapter-Agent-P44  →  ch44.html  [CME295 L5 + CS336 L15~17 + CS229 L16~19]
Chapter-Agent-P45  →  ch45.html  [CS336 L1~L6] (신규)
```

### Wave 8 — 기존 챕터 CS229 링크 추가 (통합 에이전트)
```
Integration-Agent  →  ch03b / ch04 / ch07 / ch09 / ch12 / ch24 / ch25에
                       CS229 강의 링크를 .callout.info로 삽입
```

---

## 9. 품질 기준 (통합 버전)

### 9.1 기본 품질 기준 (기존 7블록)
- [ ] 7개 필수 블록 순서대로 존재
- [ ] EASY BOX에 일상 비유 포함
- [ ] 수식마다 `.fn` 한국어 주석

### 9.2 LLM·RAG 챕터 추가 기준
- [ ] 해당 CME295 강의 유튜브 링크가 `.callout.info`에 포함
- [ ] 해당 CS336 강의 유튜브 링크가 `.callout.info`에 포함 (해당 시)
- [ ] CS229 관련 강의 링크 포함 (해당 시)
- [ ] RAG 챕터: 파이프라인 단계가 `.steps` 블록으로 표현
- [ ] RLHF/DPO 챕터: RLHF vs DPO 비교 표 포함
- [ ] 수식에 **논문 출처** 주석 포함 (예: "Lewis et al., 2020")

### 9.3 CS336 구현 챕터 추가 기준
- [ ] 수식과 함께 **Python/NumPy 의사코드** 또는 설명 포함
- [ ] FLOPs·메모리 계산이 인터랙티브로 확인 가능
- [ ] 실제 구현 시 발생하는 **엣지 케이스** 설명

---

## 10. 전체 참고 자료

### 강좌 공식 사이트
| 강좌 | 사이트 | 플레이리스트 |
|------|--------|------------|
| Stanford CS229 (2018, Andrew Ng) | [cs229.stanford.edu](https://cs229.stanford.edu/syllabus-autumn2018.html) | [YouTube](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU) |
| Stanford CME295 (2025, Amidi) | [cme295.stanford.edu](https://cme295.stanford.edu/syllabus/) | [YouTube](https://www.youtube.com/playlist?list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy) |
| Stanford CS336 (2026, Liang+Hashimoto) | [cs336.stanford.edu](https://cs336.stanford.edu/) | [YouTube](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) |

### 핵심 논문 (챕터별)

| 챕터 | 논문 |
|------|------|
| Ch.12B | Hu et al., 2022 — LoRA; Dettmers et al., 2023 — QLoRA |
| Ch.12B | Hoffmann et al., 2022 — Chinchilla Scaling Laws |
| Ch.12C | Christiano et al., 2017 — RLHF; Rafailov et al., 2023 — DPO |
| Ch.12D | Wei et al., 2022 — Chain of Thought; Shao et al., 2024 — GRPO |
| Ch.12D | DeepSeek-AI, 2025 — DeepSeek-R1 |
| Ch.12E | Lewis et al., 2020 — RAG; Karpukhin et al., 2020 — DPR |
| Ch.12E | Es et al., 2023 — RAGAS; Gao et al., 2022 — HyDE |
| Ch.12F | Zheng et al., 2023 — Judging LLM-as-a-Judge |
| Ch.43 | Johnson et al., 2019 — FAISS; Asai et al., 2023 — Self-RAG |
| Ch.44 | Ziegler et al., 2019 — RLHF 원논문; Anthropic, 2022 — Constitutional AI |
| Ch.45 | Sennrich et al., 2016 — BPE; Dao et al., 2022 — FlashAttention |
| Ch.45 | Kaplan et al., 2020 — Neural LM Scaling Laws |

### Amidi Brothers 치트시트 (CME295와 동일 저자)
- [ML Cheatsheet](https://stanford.edu/~shervine/teaching/cs-229/cheatsheet-supervised-learning) — CS229 공식 보충 자료
- [Deep Learning Cheatsheet](https://stanford.edu/~shervine/teaching/cs-230/) — CS230 공식 보충 자료
