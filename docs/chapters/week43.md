# Week 43 — RAG 수학 기반·Dense Retrieval 이론 (Ch.43)

> **파일명**: `chapters/ch43.html`  
> **Phase**: Phase 3-D · LLM 심화 이론 (Unit 5 확장)  
> **인터랙티브**: ✅ FAISS 근사 검색 오차 시뮬레이터  
> **우선순위**: P2  
> **전제 챕터**: Ch.12E (RAG 기초), Ch.24 (고유값), Ch.25 (SVD·LoRA)  
> **기반 강의**: [Stanford CME295 L7](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.43 · RAG 이론 심화 (Phase 3-D)` |
| 제목 | `RAG 수학 기반 — Dense Retrieval·FAISS·임베딩 기하학` |
| 서브타이틀 | `벡터 유사도 검색의 수학적 기반. MIPS 문제, FAISS 근사 알고리즘, DPR 학습 이론, Modular RAG 아키텍처 — 논문을 직접 구현하는 수준으로.` |
| 이전 챕터 | `ch42.html` — Ch.42 효과크기와 결과 해석 |
| 다음 챕터 | `ch44.html` — Ch.44 LLM Alignment 이론 |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L7**: [Agentic LLMs — Advanced RAG (1:49:23)](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7)
- **핵심 논문**: 
  - Karpukhin et al., 2020 — DPR (Dense Passage Retrieval)
  - Johnson et al., 2019 — FAISS
  - Gao et al., 2022 — HyDE
  - Es et al., 2023 — RAGAS

---

## EASY BOX 내용

**핵심 비유**: "수천만 권의 책 중에서 내 질문과 가장 관련 있는 단락을 1초 안에 찾는 문제다. 모든 책을 다 읽을 수는 없으니, 책마다 '핵심 특징 벡터'를 미리 만들어 놓고, 내 질문과 각도가 비슷한 벡터를 빠르게 찾는다 — 이것이 MIPS 문제이고, FAISS가 이를 해결한다."

**예시 리스트**:
- 완전 검색(Brute Force): 1억 개 문서 × 768차원 코사인 계산 → 수 분 소요
- FAISS IVF: 문서를 군집(클러스터)으로 나누고 관련 군집만 검색 → 수 밀리초
- HNSW: 계층적 그래프로 탐색 → 더 빠르지만 메모리 더 필요
- AI에서: OpenAI 임베딩 API + FAISS로 수백만 문서 RAG 구축 실제 사례

---

## 섹션 구성

### 섹션 1: 임베딩 공간의 기하학

**아이콘**: `📐`  
**제목**: `벡터 공간에서 의미는 어디에 있는가`

**설명**: 임베딩 모델은 텍스트를 고차원 벡터로 변환합니다. 잘 학습된 임베딩 공간에서는 의미적으로 유사한 텍스트가 기하학적으로 가까운 위치에 있습니다. 이 공간의 성질을 이해해야 검색 품질을 높일 수 있습니다.

**수식 박스 1**:
- 레이블: `코사인 유사도와 내적의 관계`
- 수식:
```
\cos\theta(u, v) = \frac{u \cdot v}{\|u\| \|v\|}
\quad\Leftrightarrow\quad u \cdot v \quad\text{(정규화된 경우 동일)}
```
- 주석:
  - `‖u‖ = ‖v‖ = 1` (L2 정규화)이면 코사인 유사도 = 내적
  - DPR 등 대부분의 검색 모델은 L2 정규화된 임베딩 사용
  - 코사인 유사도: 방향 유사도 (크기 무관)
  - 유클리드 거리: `‖u-v‖² = 2 - 2cosθ` (정규화된 경우) → 동치

**수식 박스 2**:
- 레이블: `임베딩 공간의 차원의 저주 (Curse of Dimensionality)`
- 수식:
```
\mathbb{E}\!\left[\frac{\max_{i} d(q, p_i) - \min_{i} d(q, p_i)}{\min_{i} d(q, p_i)}\right] \xrightarrow{d\to\infty} 0
```
- 주석:
  - 고차원 공간에서 모든 점 사이의 거리가 점점 비슷해짐
  - 768차원 임베딩: 여전히 차별적 신호 존재, 하지만 주의 필요
  - 해결: 차원 축소 (PCA), 이분법적 인덱스 (IVF), 근사 검색

---

### 섹션 2: MIPS와 FAISS

**아이콘**: `⚡`  
**제목**: `MIPS — 수억 벡터에서 가장 가까운 것 찾기`

**설명**: MIPS(Maximum Inner Product Search)는 쿼리 벡터와 내적이 가장 큰 벡터를 찾는 문제입니다. Exact MIPS는 O(n·d)이므로 수억 개 문서에는 비실용적입니다. FAISS는 두 가지 근사 전략을 제공합니다.

**수식 박스 3**:
- 레이블: `IVF (Inverted File Index) — 군집 기반 근사 검색`
- 수식:
```
\hat{p} = \arg\max_{p \in \mathcal{C}_{k^*}} E_Q(q)^\top E_P(p)
\quad\text{where}\quad k^* = \arg\min_{k} \|E_Q(q) - c_k\|_2
```
- 주석:
  - `c_k`: k번째 군집의 중심 벡터 (k-means로 사전 계산)
  - 쿼리와 가장 가까운 nprobe개 군집 내에서만 검색
  - nprobe↑ → 정확도↑, 속도↓ (trade-off)
  - 전체 검색 대비 nprobe/nlist 비율만큼 빠름

**수식 박스 4**:
- 레이블: `PQ (Product Quantization) — 벡터 압축`
- 수식:
```
E_P(p) \approx [q_1, q_2, \ldots, q_M]
\quad\text{where}\quad q_m = \arg\min_{c \in \mathcal{C}_m} \|E_P(p)_{d_m} - c\|_2
```
- 주석:
  - 768차원 벡터를 M개 서브공간으로 분할 (예: M=8 → 각 96차원)
  - 각 서브공간에서 k-means → 코드북 `𝒞_m`
  - 원본 768×4 byte = 3072B → PQ 압축 후 8×1 byte = 8B (384배 압축!)
  - 속도↑↑, 메모리↓↓, 정확도 약간 손실

**표 (FAISS 인덱스 비교)**:

| 인덱스 | 방법 | 메모리 | 속도 | 정확도 | 사용 시 |
|--------|------|--------|------|--------|---------|
| Flat | 완전 탐색 | 높음 | 느림 | 100% | < 100만 문서 |
| IVF-Flat | 군집 기반 | 높음 | 빠름 | 95%+ | 수백만 문서 |
| IVF-PQ | 군집+압축 | 매우 낮음 | 매우 빠름 | 85~95% | 수억 문서 |
| HNSW | 그래프 기반 | 높음 | 매우 빠름 | 95%+ | 실시간 서비스 |

---

### 섹션 3: DPR 학습 이론

**아이콘**: `🎯`  
**제목**: `DPR 학습 — 올바른 임베딩 공간 만들기`

**설명**: Dense Passage Retrieval(DPR)은 질문 인코더와 문서 인코더를 **대조 학습(Contrastive Learning)**으로 훈련합니다. 같은 의미의 (질문, 문서) 쌍은 가깝게, 다른 의미는 멀게 만듭니다.

**수식 박스 5**:
- 레이블: `DPR 학습 손실 — In-Batch Negative Sampling`
- 수식:
```
\mathcal{L}_{\text{DPR}} = -\log\frac{e^{s(q_i, p_i^+)}}{e^{s(q_i, p_i^+)} + \sum_{j \neq i} e^{s(q_i, p_j^-)}}
```
- 주석:
  - `s(q, p) = E_Q(q)ᵀ E_P(p)`: 유사도 점수
  - `p_i⁺`: 질문 `q_i`에 대한 정답 문서 (positive)
  - `p_j⁻`: 다른 질문의 정답 문서 (in-batch negatives) — 같은 배치의 나머지 문서들
  - 배치 크기 B이면 B-1개의 negative 자동으로 생성 → 효율적
  - Hard Negative Mining: 모델이 헷갈리는 어려운 negative 추가 → 성능↑

**callout (tip)**:
- 내용: **최신 임베딩 모델**: OpenAI text-embedding-3-large (3072차원), Voyage AI, Cohere Embed v3 등이 DPR의 원리를 대규모 데이터로 확장한 것입니다. 이 모델들의 내부 학습 손실도 동일한 대조 학습 형태입니다.

---

### 섹션 4: Modular RAG 아키텍처

**아이콘**: `🏗`  
**제목**: `Modular RAG — 현대 RAG 시스템의 구조`

**설명**: 기본 RAG에서 발전한 Modular RAG는 각 모듈을 독립적으로 교체·튜닝할 수 있는 구조입니다. 논문에서 "RAG 시스템"이라 하면 대부분 이 구조를 의미합니다.

**단계별 리스트**:
```
모듈 1: Query Transformation
  → Rewriting (검색 최적화)
  → Decomposition (복잡 질문 분해)
  → HyDE (가상 문서 생성)

모듈 2: Retrieval
  → Dense (DPR)
  → Sparse (BM25)
  → Hybrid (RRF 통합)
  → Multi-step (반복 검색)

모듈 3: Post-Retrieval
  → Reranking (Cross-Encoder)
  → Context Compression (긴 문서 압축)
  → Filtering (관련성 임계값)

모듈 4: Generation
  → Prompt 구성 (검색 결과 + 질문)
  → LLM 생성 (출처 포함)
  → Self-RAG (검색 여부 자체 결정)
```

**수식 박스 6**:
- 레이블: `Self-RAG — 검색 여부 자체 결정 (Asai et al., 2023)`
- 수식:
```
P(y, r \mid x) = P(r \mid x) \cdot P(y \mid x, r, d^*)
\quad\text{where}\quad d^* = \arg\max_{d} P(d \mid x) \cdot P(\text{isrel} \mid x, d)
```
- 주석:
  - `r`: 검색 토큰 (Retrieve / No-Retrieve 중 모델이 결정)
  - 항상 검색하는 Naive RAG 대신, 검색이 필요할 때만 검색
  - `P(isrel | x, d)`: 검색된 문서가 관련있는지 모델이 스스로 판단
  - 4개 특수 토큰: `[Retrieve]`, `[IsRel]`, `[IsSup]`, `[IsUse]`

---

## 인터랙티브: FAISS 근사 검색 Trade-off 탐색기

**제목**: `⚡ IVF nprobe — 속도 vs 정확도 Trade-off`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `faiss-nlist` | 군집 수 (nlist) | 16 | 4096 | 16 | 256 |
| `faiss-nprobe` | 탐색 군집 수 (nprobe) | 1 | 256 | 1 | 8 |
| `faiss-n` | 총 문서 수 (만 개) | 1 | 1000 | 10 | 100 |

**JavaScript 로직**:
```javascript
function updateFAISS() {
  const nlist = parseInt(document.getElementById('faiss-nlist').value);
  const nprobe = Math.min(parseInt(document.getElementById('faiss-nprobe').value), nlist);
  const n = parseInt(document.getElementById('faiss-n').value) * 10000;
  document.getElementById('faiss-nlist-v').value = nlist;
  document.getElementById('faiss-nprobe-v').value = nprobe;
  document.getElementById('faiss-n-v').value = (n / 10000).toFixed(0);

  const docsPerCluster = Math.round(n / nlist);
  const searchRatio = nprobe / nlist;
  const speedup = Math.max(1, nlist / nprobe);
  // 정확도 추정: nprobe가 높을수록 정확도↑ (시그모이드형)
  const recall = 1 - Math.exp(-3 * searchRatio);

  document.getElementById('faiss-res').innerHTML =
    `군집당 평균 문서 수: <strong>${docsPerCluster.toLocaleString()}</strong><br/>
     검색 비율: ${nprobe}/${nlist} = <strong>${(searchRatio*100).toFixed(1)}%</strong><br/>
     완전 탐색 대비 속도: <strong style="color:var(--accent3)">${speedup.toFixed(0)}×</strong> 빠름<br/>
     추정 Recall@10: <strong>${(recall*100).toFixed(1)}%</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       nprobe = nlist이면 완전 탐색과 동일 (정확하지만 느림).<br/>
       권장: nprobe = nlist × 0.03~0.1 (3~10%만 탐색)
     </span>`;
}
updateFAISS();
```

---

## 퀴즈

**질문**: DPR 학습에서 In-Batch Negative Sampling을 사용하는 주된 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 정답 문서를 더 많이 포함시키기 위해 | ❌ |
| (b) 배치 내 다른 질문의 문서를 부정 예시로 자동 활용해 추가 데이터 없이 대조 학습 효율을 높이기 위해 | ✅ |
| (c) FAISS 인덱스 구축 속도를 높이기 위해 | ❌ |
| (d) 임베딩 차원을 줄이기 위해 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 43)

```
[ ] 코사인 유사도와 내적의 동치 조건을 설명할 수 있다
[ ] MIPS 문제를 정의하고 Brute Force의 복잡도를 계산할 수 있다
[ ] IVF와 PQ가 각각 어떻게 검색을 가속하는지 설명할 수 있다
[ ] DPR 학습 손실함수에서 In-Batch Negative의 의미를 설명할 수 있다
[ ] Modular RAG의 4개 모듈을 설명할 수 있다
[ ] Self-RAG가 기존 RAG와 다른 점을 설명할 수 있다
[ ] [CME295 L7 강의](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) 재시청 (고급 기법 중심)
```
