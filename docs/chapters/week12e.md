# Week 12E — RAG: 검색 증강 생성 (Ch.12E) ★ 핵심

> **파일명**: `chapters/ch12e.html`  
> **Phase**: Unit 3 · AI & ML 연결 (고도화)  
> **인터랙티브**: ✅ RAG 파이프라인 시뮬레이터  
> **우선순위**: P0 ★  
> **기반 강의**: [Stanford CME295 L7 — Agentic LLMs](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) (1:49:23)  
> **슬라이드**: [fall25-cme295-lecture7.pdf](https://cme295.stanford.edu/slides/fall25-cme295-lecture7.pdf)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12E · RAG (Unit 3 고도화) ★` |
| 제목 | `RAG — 검색 증강 생성` |
| 서브타이틀 | `LLM이 모르는 것을 찾아서 답하게 하는 법 — Retrieval-Augmented Generation의 수학과 아키텍처. 논문·기업 AI 시스템의 90%가 사용하는 핵심 기술.` |
| 이전 챕터 | `ch12d.html` — Ch.12D LLM 추론 심화·CoT |
| 다음 챕터 | `ch12f.html` — Ch.12F LLM 평가 방법론 |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L7**: [Agentic LLMs — RAG, Function Calling, Agents (1:49:23)](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7)
- **슬라이드**: [강의자료 PDF](https://cme295.stanford.edu/slides/fall25-cme295-lecture7.pdf)
- **RAG 원논문**: Lewis et al., 2020 — "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks"

---

## EASY BOX 내용

**핵심 비유**: "오픈북 시험과 클로즈북 시험의 차이다. LLM이 혼자 기억에 의존하면 클로즈북(오래된 정보, 환각 가능). RAG는 시험장에 교재를 가져와서 답을 찾아 쓰는 오픈북 시험이다."

**예시 리스트**:
- 클로즈북 LLM: "2026년 최신 논문에 대해 알려줘" → 학습 데이터에 없어서 환각(hallucination)
- RAG: 질문 → 벡터 DB 검색 → 관련 논문 조각 가져오기 → LLM이 보며 답변
- 기업 활용: 사내 문서 RAG, 법률 문서 RAG, 의료 기록 RAG
- AI에서: ChatGPT 웹 검색, Perplexity AI, NotebookLM 모두 RAG 방식

---

## 섹션 구성

### 섹션 1: RAG란 무엇인가

**아이콘**: `🔍`  
**제목**: `RAG 기본 파이프라인 — 검색 + 생성`

**설명**: RAG는 LLM의 두 가지 핵심 문제를 해결합니다: ①학습 데이터 이후 정보는 모름(지식 최신성), ②틀린 정보를 확신하며 생성(환각). 외부 문서를 검색해서 컨텍스트로 제공하면 두 문제가 크게 완화됩니다.

**단계별 리스트**:
```
Step 1: 문서 전처리 (오프라인)
  → 문서를 작은 조각(청크)으로 나눔
  → 각 청크를 임베딩 모델로 벡터 변환
  → 벡터 DB(FAISS, Pinecone 등)에 저장

Step 2: 검색 (실시간)
  → 사용자 질문을 동일한 임베딩 모델로 벡터 변환
  → 벡터 DB에서 가장 유사한 청크 Top-k개 검색

Step 3: 생성 (실시간)
  → 검색된 청크 + 질문을 LLM 프롬프트에 포함
  → LLM이 제공된 컨텍스트를 참고하여 답변 생성
```

**수식 박스 1**:
- 레이블: `RAG 생성 확률 (Lewis et al., 2020)`
- 수식:
```
P(y \mid x) = \sum_{z \in \text{top-}k(p_\eta(\cdot|x))} p_\eta(z \mid x) \cdot p_\theta(y \mid x, z)
```
- 주석:
  - `x`: 입력 질문
  - `z`: 검색된 문서 조각 (passage)
  - `p_η(z|x)`: 검색 모델이 질문 x에 대해 문서 z를 선택할 확률
  - `p_θ(y|x,z)`: LLM이 질문 x와 문서 z를 보고 답 y를 생성할 확률
  - 전체 확률: top-k 문서들에 대한 가중 합산

---

### 섹션 2: Dense Retrieval — 어떻게 관련 문서를 찾는가

**아이콘**: `🧲`  
**제목**: `Dense Retrieval — 벡터 유사도로 의미 검색`

**설명**: 전통적 키워드 검색(BM25)은 "단어가 겹치는지"만 봅니다. Dense Retrieval(DPR)은 임베딩 모델로 질문과 문서를 **같은 벡터 공간**에 표현하여 의미적 유사도로 검색합니다.

**수식 박스 2**:
- 레이블: `DPR 유사도 점수 (Karpukhin et al., 2020)`
- 수식:
```
s(q, p) = E_Q(q)^\top E_P(p)
\quad\text{검색:}\quad \hat{p} = \arg\max_{p \in \mathcal{C}} E_Q(q)^\top E_P(p)
```
- 주석:
  - `E_Q`: 질문 인코더 (예: BERT 기반), `E_P`: 문서 인코더
  - 코사인 유사도 대신 내적(dot product) 사용 → 정규화된 임베딩에서는 동일
  - `𝒞`: 전체 문서 코퍼스 (수백만~수억 개 문서)
  - MIPS (Maximum Inner Product Search): FAISS로 근사 검색 → O(log n)

**표 (BM25 vs Dense Retrieval 비교)**:

| 항목 | BM25 (희소 검색) | Dense Retrieval (밀집 검색) |
|------|---------------|--------------------------|
| 원리 | 키워드 매칭, TF-IDF | 임베딩 벡터 유사도 |
| 동의어 처리 | ❌ "자동차"≠"차" | ✅ 의미적 유사도 포착 |
| 학습 필요 | ❌ | ✅ (임베딩 모델 학습) |
| 속도 | 빠름 | 근사 검색 필요 (FAISS) |
| 현재 추세 | Hybrid (BM25+Dense) | Dense가 대세 |

---

### 섹션 3: Advanced RAG — 기본을 넘어서

**아이콘**: `🏗`  
**제목**: `Advanced RAG 기법 — 검색 품질 향상`

**설명**: Naive RAG는 "그냥 Top-k 검색 후 붙여넣기"입니다. Advanced RAG는 검색 전·중·후를 모두 개선합니다.

**개념 카드**:
- 제목: `🔑 Advanced RAG 핵심 기법`
- 내용:
  - **Query Rewriting**: 사용자 질문을 검색에 최적화된 형태로 변환
  - **HyDE** (Hypothetical Document Embeddings): 가상 답변을 먼저 생성 → 그 임베딩으로 검색
  - **Reranking**: Top-k 결과를 Cross-Encoder로 재순위화 (점수 더 정확)
  - **Chunk Size Tuning**: 청크가 너무 크면 노이즈, 너무 작으면 맥락 부족
  - **Hybrid Search**: BM25 + Dense Retrieval 점수를 RRF로 통합

**수식 박스 3**:
- 레이블: `HyDE — 가상 문서 임베딩 (Gao et al., 2022)`
- 수식:
```
\hat{p} = \arg\max_{p \in \mathcal{C}} E_P(p)^\top E_P(\hat{d})
\quad\text{where}\quad \hat{d} \sim \text{LLM}(x)
```
- 주석:
  - `x`: 사용자 질문, `d̂`: LLM이 생성한 가상 답변 문서
  - 질문 `x` 대신 **가상 답변 `d̂`**의 임베딩으로 검색
  - 가상 답변은 틀려도 됨 — 검색 공간에서 올바른 문서와 더 가까운 위치에 있음
  - 특히 전문 도메인(의학, 법률)에서 효과적

**수식 박스 4**:
- 레이블: `Reciprocal Rank Fusion (RRF) — Hybrid 검색 점수 통합`
- 수식:
```
\text{RRF}(d) = \sum_{r \in R} \frac{1}{k + \text{rank}_r(d)}
\quad (k = 60 \text{ 권장})
```
- 주석:
  - `R`: 여러 검색 방법의 집합 (BM25, Dense 등)
  - `rank_r(d)`: 방법 r에서 문서 d의 순위
  - k=60: 상위 순위의 영향을 완화하는 상수 (경험적 최적값)
  - 점수 스케일이 달라도 순위만 사용하므로 자연스럽게 통합됨

---

### 섹션 4: RAG 평가 — RAGAS

**아이콘**: `📊`  
**제목**: `RAGAS — RAG 시스템을 어떻게 평가하는가`

**설명**: RAG 시스템은 일반 LLM 평가와 다릅니다. 검색 품질과 생성 품질을 따로 평가해야 합니다. RAGAS(Es et al., 2023)는 RAG 특화 자동 평가 프레임워크입니다.

**표 (RAGAS 4대 메트릭)**:

| 메트릭 | 평가 대상 | 측정 내용 |
|--------|---------|---------|
| **Faithfulness** | 생성 | 답변이 검색된 문서에 근거하는가 (환각 탐지) |
| **Answer Relevancy** | 생성 | 답변이 질문에 관련성 있는가 |
| **Context Precision** | 검색 | 검색된 문서 중 실제 도움이 된 비율 |
| **Context Recall** | 검색 | 필요한 정보가 검색에 포함되었는가 |

**callout (warn)**:
- 내용: **환각(Hallucination)의 두 유형**: ①**Intrinsic**: 검색된 문서와 모순되는 내용 생성, ②**Extrinsic**: 검색 문서에 없는 내용을 지어냄. RAG는 특히 Intrinsic 환각을 Faithfulness 메트릭으로 감지합니다.

---

## 인터랙티브: RAG 파이프라인 시뮬레이터

**제목**: `🔍 RAG 청크 크기·Top-k 최적화 탐색기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `rag-chunk` | 청크 크기 (토큰) | 64 | 1024 | 64 | 256 |
| `rag-topk` | Top-k (검색 수) | 1 | 20 | 1 | 5 |
| `rag-overlap` | 오버랩 (%) | 0 | 50 | 5 | 20 |

**JavaScript 로직**:
```javascript
function updateRAG() {
  const chunk = parseInt(document.getElementById('rag-chunk').value);
  const topk = parseInt(document.getElementById('rag-topk').value);
  const overlap = parseInt(document.getElementById('rag-overlap').value);
  document.getElementById('rag-chunk-v').value = chunk;
  document.getElementById('rag-topk-v').value = topk;
  document.getElementById('rag-overlap-v').value = overlap;

  const contextTokens = chunk * topk;
  const overlapTokens = Math.round(chunk * overlap / 100);
  const effectiveChunk = chunk - overlapTokens;

  // 단순화된 품질 추정 모델
  const retrievalQuality = Math.min(1.0, 0.3 + topk * 0.07 + (chunk > 128 ? 0.1 : 0));
  const noiseRisk = Math.min(1.0, topk * 0.05 + (chunk > 512 ? 0.3 : 0));
  const contextWindow = contextTokens > 8192 ? '⚠️ 긴 컨텍스트 (LLM 부담)' : '✅ 적정';

  document.getElementById('rag-res').innerHTML =
    `LLM 컨텍스트 토큰: <strong>${contextTokens.toLocaleString()}</strong> ${contextWindow}<br/>
     유효 청크 크기: ${effectiveChunk} 토큰 (오버랩 ${overlapTokens} 토큰 제외)<br/>
     검색 품질 추정: ${'█'.repeat(Math.round(retrievalQuality * 10))}${'░'.repeat(10 - Math.round(retrievalQuality * 10))} ${(retrievalQuality * 100).toFixed(0)}%<br/>
     노이즈 위험도: ${'█'.repeat(Math.round(noiseRisk * 10))}${'░'.repeat(10 - Math.round(noiseRisk * 10))} ${(noiseRisk * 100).toFixed(0)}%<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       권장: 청크 128~512 토큰, 오버랩 10~20%, Top-k 3~7.<br/>
       청크가 너무 크면 노이즈 증가, 너무 작으면 맥락 부족.
     </span>`;
}
updateRAG();
```

---

## 퀴즈

**질문**: RAG에서 Faithfulness(충실도)가 낮다는 것은 무엇을 의미하는가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 검색된 문서가 질문과 관련이 없다 | ❌ |
| (b) LLM의 답변이 검색된 문서의 내용과 일치하지 않는 부분이 있다 (환각) | ✅ |
| (c) 필요한 정보가 검색에 포함되지 않았다 | ❌ |
| (d) Top-k 수가 너무 적다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 12E)

```
[ ] RAG 3단계 파이프라인 (전처리→검색→생성)을 설명할 수 있다
[ ] RAG 생성 확률 수식을 설명할 수 있다 (p_η, p_θ, top-k)
[ ] BM25와 Dense Retrieval의 차이를 설명할 수 있다
[ ] DPR 유사도 점수 수식을 쓸 수 있다
[ ] HyDE가 왜 검색 품질을 높이는지 설명할 수 있다
[ ] RAGAS 4대 메트릭 (Faithfulness, Answer Relevancy, Context Precision, Context Recall)을 설명할 수 있다
[ ] Hybrid Search에서 RRF를 사용하는 이유를 설명할 수 있다
[ ] [CME295 L7 강의](https://www.youtube.com/watch?v=h-7S6HNq0Vg&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=7) 시청 완료 (2회 이상 권장)
```
