# Week 14-B — 논문 5-question 해석 프레임워크 (Ch.14B)

> **파일명**: `chapters/ch14b.html`
> **위치**: Phase 1 보강 챕터 — Ch.14 (논문읽기·쓰기) 실전 확장
> **인터랙티브**: ✅ 5-question 인터랙티브 워크시트 (입력 → 분석 결과)
> **우선순위**: P0 (90일 플랜 Day 71~90 핵심 갭)
> **선행 조건**: `ch14.html` (논문읽기·쓰기) 완료

---

## 이 챕터가 필요한 이유 (갭 분석)

Ch.14는 논문의 **구조**를 가르친다 (각 섹션의 역할, 기호 읽는 법).
90일 플랜 Day 71~90은 **실제 논문을 분해해서 이해하는 훈련**이 필요하다.

| Ch.14에서 하는 것 | 이 챕터에서 하는 것 |
|-----------------|------------------|
| 논문의 6개 섹션 구조 설명 | 실제 Abstract로 5-question 답하기 |
| 기호와 표기법 읽는 법 | 핵심 수식 → 코드 연결 실습 |
| "논문은 이렇게 생겼다" | "이 논문이 말하는 것은 이것이다" |
| 지식 습득 | 분석 능력 훈련 |

**이 챕터 이후 할 수 있는 것**:
임의의 ML 논문 Abstract를 보고 5개 질문에 답변을 작성 가능

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.14B · 논문 실전 (Phase 1 보강 · 90일 Day 71~90)` |
| 제목 | `논문 5-question 해석 프레임워크` |
| 서브타이틀 | `논문을 끝까지 읽는 것이 아닙니다. 5개 질문에 답하는 것이 목표입니다. 이 프레임워크로 어떤 논문이든 15분 안에 핵심을 파악합니다.` |
| 이전 챕터 | `ch14.html` — Ch.14 논문 읽기·쓰기 |
| 다음 챕터 | Phase 2 시작 (Ch.15 수열과 극한의 엄밀한 정의) |

---

## EASY BOX 내용

**핵심 비유**: "영화를 3시간 다 보지 않아도, 5가지를 알면 그 영화를 이해한다 — 장르, 주인공의 문제, 해결 방법, 결말, 왜 재밌는지"

**예시 리스트**:
- Q1. 문제가 무엇인가? → "기존 번역기는 긴 문장을 잘 못 번역한다"
- Q2. 기존 방법은? → "RNN으로 순서대로 처리했다"
- Q3. 무엇이 개선? → "Attention으로 전체 문장을 동시에 본다"
- Q4. 어떻게 구현? → "Self-attention + Multi-head + Positional encoding"
- Q5. 왜 좋아졌나? → "BLEU score 28.4 → 41.0, 학습 속도 10배 빠름"
- AI에서: 이것이 "Attention is All You Need" 논문이다. 5개 답변으로 핵심 파악 완료

---

## 섹션 구성

### 섹션 1: 5-question 프레임워크 — 정의

**아이콘**: `❓`
**제목**: `5-question 프레임워크 — 논문 분해의 5가지 열쇠`

**설명**: 모든 ML 논문은 같은 논리 구조를 따릅니다. "문제가 있다 → 기존 방법으로는 부족하다 → 우리는 이렇게 했다 → 이렇게 구현했다 → 성능이 좋아졌다". 이 5단계를 질문으로 바꾸면 어떤 논문이든 분해할 수 있습니다.

**표 (5-question 프레임워크)**:

| # | 질문 | 논문의 어디에 있나 | 답변 형식 |
|---|------|----------------|---------|
| Q1 | **문제는 무엇인가?** | Abstract 1~2번째 문장, Introduction | "기존 ~가 ~문제가 있다" |
| Q2 | **기존 방법은 무엇인가?** | Related Work, Introduction | "~방법이 있었지만 ~이 한계다" |
| Q3 | **무엇이 개선되었는가?** | Abstract 마지막 문장, Conclusion | "우리는 ~를 제안한다. 기여: ①②③" |
| Q4 | **어떻게 구현했는가?** | Method, Algorithm 박스 | 핵심 수식/알고리즘 1~2개 |
| Q5 | **왜 성능이 좋아졌는가?** | Experiments, Table/Figure | "~데이터셋에서 ~% 향상" |

**callout (tip)**:
- 내용: **Q1~Q3은 Abstract에서 모두 찾을 수 있다.** 잘 쓴 논문일수록 Abstract 5~7문장 안에 이 세 가지가 다 들어있다. Q4는 Method, Q5는 Experiments에서 찾는다.

---

### 섹션 2: Abstract 해석 실습

**아이콘**: `📄`
**제목**: `Abstract 5분 해석법 — 읽는 순서와 찾을 것`

**설명**: Abstract는 보통 5~8문장입니다. 각 문장이 Q1~Q3 중 어디에 해당하는지 찾으면 됩니다.

**표 (Abstract 문장 → Q 매핑)**:

| Abstract의 문장 위치 | 보통 담긴 내용 | 해당 Q |
|-------------------|-------------|-------|
| 1번째 문장 | 분야 배경 + 큰 문제 | Q1 배경 |
| 2~3번째 문장 | 구체적인 문제 / 기존 한계 | Q1 + Q2 |
| 4~5번째 문장 | 우리의 제안 (Contribution) | Q3 |
| 마지막 1~2 문장 | 실험 결과 요약 | Q5 |

**예시 Abstract 분석 (가상)**:

```
[원문]
"Large language models (LLMs) have achieved remarkable results on
many NLP benchmarks. However, fine-tuning these models requires
substantial computational resources, limiting their adoption.
We propose LoRA, a method that freezes pretrained model weights
and injects trainable rank decomposition matrices.
LoRA reduces trainable parameters by 10,000x with no inference latency."

[5-question 매핑]
Q1. 문제: LLM fine-tuning에 엄청난 계산 자원이 필요하다
Q2. 기존 방법: 전체 파라미터를 fine-tuning (implicit)
Q3. 개선: LoRA — 가중치를 고정하고 작은 행렬만 학습
Q4. 구현: rank decomposition matrices (r << d)
Q5. 성능: 파라미터 10,000배 감소, inference 지연 없음
```

**callout (info)**:
- 내용: 실제 논문 "LoRA: Low-Rank Adaptation of Large Language Models" (Hu et al., 2021)의 Abstract를 위와 같이 분해할 수 있다. 5개 답변을 쓰는 데 10분이면 충분하다.

---

### 섹션 3: 핵심 수식 → 코드 연결

**아이콘**: `⚙️`
**제목**: `Method 수식을 코드로 — 논문 재현의 시작`

**설명**: Q4(어떻게 구현했는가)에서 핵심 수식을 찾으면, 그것을 numpy/PyTorch로 구현해보는 것이 "논문 재현"의 시작입니다.

**표 (수식 → 코드 연결 패턴)**:

| 수식 패턴 | numpy 코드 | 논문 예시 |
|----------|-----------|---------|
| `ŷ = Xw` | `y_pred = X @ w` | 선형 모델 |
| `σ(z) = 1/(1+e⁻ᶻ)` | `1/(1+np.exp(-z))` | Logistic, Sigmoid |
| `softmax(z)ᵢ = eᶻⁱ/Σeᶻʲ` | `np.exp(z)/np.exp(z).sum()` | 다중분류 |
| `H(p,q) = -Σp log q` | `-np.sum(p * np.log(q))` | Cross-entropy |
| `KL(P‖Q) = Σp log(p/q)` | `np.sum(p * np.log(p/q))` | KL Divergence |

**callout (warn)**:
- 내용: 처음부터 논문의 모든 수식을 구현하려고 하지 마세요. 핵심 수식 **1개**만 구현해보는 것이 목표입니다. 나머지는 라이브러리가 처리합니다.

---

### 섹션 4: 실전 논문 선택 가이드

**아이콘**: `🎯`
**제목**: `90일 플랜 Day 71~90을 위한 논문 선택법`

**표 (난이도별 추천 논문)**:

| 단계 | 논문 | 이유 |
|------|------|------|
| Day 71~80 (입문용) | "A Neural Algorithm of Artistic Style" (Gatys et al., 2015) | 아이디어가 직관적, 수식 단순 |
| Day 71~80 (입문용) | "Word2Vec" (Mikolov et al., 2013) | 개념이 익숙하고 구현이 쉬움 |
| Day 81~90 (실전) | "BERT" (Devlin et al., 2018) | 현대 NLP의 기초, 구조가 명확 |
| Day 81~90 (실전) | "ResNet" (He et al., 2015) | 컴퓨터 비전 기초, Figure 직관적 |
| Day 81~90 (실전) | "Attention is All You Need" (Vaswani et al., 2017) | LLM의 출발점 |

**논문 찾는 방법**:
1. [arxiv.org](https://arxiv.org) 검색 — 분야 + "survey" 또는 "tutorial"
2. Papers With Code ([paperswithcode.com](https://paperswithcode.com)) — 코드 있는 논문 우선
3. Semantic Scholar — 인용수 높은 것 우선 (검증된 논문)

---

## 인터랙티브: 5-question 워크시트

**제목**: `🔢 5-question 프레임워크 실습 — Abstract를 붙여넣고 분해해보세요`

**파라미터**: 텍스트 입력 (슬라이더 없음, 텍스트 분석 시뮬레이션)

**JavaScript 로직**:
```javascript
function analyzeAbstract() {
  const text = document.getElementById('abstract-input').value.trim();

  if (!text) {
    document.getElementById('analysis-res').innerHTML =
      '⚠️ Abstract 텍스트를 입력해주세요.';
    return;
  }

  const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 10);
  const wordCount = text.split(/\s+/).length;

  // 키워드 기반 간단 분류
  const problemKeywords = ['problem', 'challenge', 'difficult', 'however', 'but', 'limitation', 'suffer'];
  const proposalKeywords = ['propose', 'present', 'introduce', 'our', 'we', 'novel', 'new method'];
  const resultKeywords = ['achieve', 'outperform', 'improve', 'state-of-the-art', 'sota', 'better', '%'];

  const hasProblem = sentences.some(s =>
    problemKeywords.some(k => s.toLowerCase().includes(k))
  );
  const hasProposal = sentences.some(s =>
    proposalKeywords.some(k => s.toLowerCase().includes(k))
  );
  const hasResult = sentences.some(s =>
    resultKeywords.some(k => s.toLowerCase().includes(k))
  );

  document.getElementById('analysis-res').innerHTML =
    `Abstract 분석 결과<br/>
     문장 수: ${sentences.length}개 | 단어 수: ${wordCount}개<br/><br/>
     <strong>자동 감지 (키워드 기반)</strong><br/>
     Q1. 문제 서술: ${hasProblem ? '✅ 감지됨 (however/but/limitation 등)' : '⚠️ 명확하지 않음 — 첫 2문장 확인'}<br/>
     Q3. 제안 방법: ${hasProposal ? '✅ 감지됨 (we propose/present 등)' : '⚠️ 명확하지 않음 — "we" 또는 "our" 찾기'}<br/>
     Q5. 실험 결과: ${hasResult ? '✅ 감지됨 (% 향상 또는 outperform)' : '⚠️ 명확하지 않음 — 숫자나 비교 표현 찾기'}<br/><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       자동 감지는 참고용입니다. 직접 5개 답변을 작성하세요.<br/>
       Q2 (기존 방법)와 Q4 (구현)는 Related Work와 Method 섹션에서 찾으세요.
     </span>`;
}
```

---

## 퀴즈

**질문**: 5-question 프레임워크에서 "Q4. 어떻게 구현했는가?"의 답은 주로 어느 섹션에서 찾나?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Abstract — 요약이 있으므로 | ❌ |
| (b) Introduction — 배경 설명에 있으므로 | ❌ |
| (c) Method (Methodology) — 구체적인 수식과 알고리즘이 있으므로 | ✅ |
| (d) Conclusion — 구현 요약이 있으므로 | ❌ |

**정답 위치**: (c)
**해설**: Method 섹션에 핵심 수식, Algorithm 박스, 모델 구조 Figure가 있다. Q4는 여기에서 핵심 수식 1~2개를 뽑으면 된다. Abstract에는 Q1~Q3만 있고 Q4의 세부 내용은 없다.

---

## 주간 체크리스트 (Week 14-B)

```
[ ] 5-question 프레임워크를 외우지 않고 보면서 사용할 수 있다
[ ] 가상 Abstract (위 LoRA 예시)에서 5개 답변 모두 찾기
[ ] 실제 논문 1편 선택 (추천: Word2Vec 또는 Style Transfer)
[ ] 선택한 논문의 Abstract를 5-question으로 분해
[ ] Method 섹션에서 핵심 수식 1개 찾기
[ ] 그 수식을 numpy 코드로 연결 시도 (완벽하지 않아도 됨)
[ ] 5개 답변을 글로 정리 (각 답변 1~3문장)

✅ 90일 플랜 완료 기준:
[ ] 위 5개 답변 문서를 저장하고 다른 사람에게 설명 가능한 수준
```

---

## 90일 플랜 최종 완료 점검표

이 챕터를 마치면 90일 목표 달성:

| 체크 | 90일 최종 기준 | 검증 방법 |
|------|-------------|---------|
| ☐ | 논문 Abstract 이해 | 임의 논문 → 3줄 한국어 요약 |
| ☐ | 핵심 수식 의미 설명 | 수식 1개 → "이것은 ~를 계산" |
| ☐ | 모델 구조 설명 (입력→출력) | Method Figure → 말로 설명 |
| ☐ | 간단 구현 | 수식 1개 → numpy 코드 |
| ☐ | **5-question 답변 작성** | 선택 논문 → 5개 답변 문서화 |
