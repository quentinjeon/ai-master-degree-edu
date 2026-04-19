# Week 12D — LLM 추론 심화·CoT·GRPO (Ch.12D)

> **파일명**: `chapters/ch12d.html`  
> **Phase**: Unit 3 · AI & ML 연결 (고도화)  
> **인터랙티브**: ✅ Self-Consistency 다수결 시뮬레이터  
> **우선순위**: P1  
> **기반 강의**:  
> - [Stanford CME295 L3 — LLMs](https://www.youtube.com/watch?v=Q5baLehv5So&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=3) (1:48:44) — CoT, Prompting  
> - [Stanford CME295 L6 — LLM Reasoning](https://www.youtube.com/watch?v=k5Fh-UgTuCo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=6) (1:47:10) — GRPO, Scaling  

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12D · LLM 추론 (Unit 3 고도화)` |
| 제목 | `LLM 추론 심화 — CoT·Self-Consistency·GRPO` |
| 서브타이틀 | `"생각하고 나서 답하는" AI — CoT가 LLM의 수학 실력을 2배로 높이는 원리, 그리고 DeepSeek-R1을 만든 GRPO.` |
| 이전 챕터 | `ch12c.html` — Ch.12C RLHF·선호도 학습·DPO |
| 다음 챕터 | `ch12e.html` — Ch.12E RAG: 검색 증강 생성 |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L3**: [LLMs — Prompting, CoT (1:48:44)](https://www.youtube.com/watch?v=Q5baLehv5So&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=3)
- **Stanford CME295 L6**: [LLM Reasoning — GRPO, Scaling (1:47:10)](https://www.youtube.com/watch?v=k5Fh-UgTuCo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=6)

---

## EASY BOX 내용

**핵심 비유**: "수학 시험에서 답만 적는 학생보다 풀이 과정을 쓰는 학생이 더 정확하다. CoT는 LLM에게 '답 전에 풀이 과정부터 쓰게' 하는 방법이다."

**예시 리스트**:
- 직접 답변: "17×24 = ?" → 오류 가능성 높음
- CoT 적용: "17×24 = 17×20 + 17×4 = 340 + 68 = 408" → 단계적 검증으로 정확도 상승
- Self-Consistency: 같은 문제를 10번 풀어 가장 많이 나온 답을 선택 (다수결)
- AI에서: GPT-o1, o3, DeepSeek-R1 등 추론 모델이 CoT + GRPO 조합으로 탄생

---

## 섹션 구성

### 섹션 1: Chain of Thought (CoT)

**아이콘**: `🧠`  
**제목**: `Chain of Thought — 단계별로 생각하면 더 잘 맞춘다`

**설명**: CoT(Wei et al., 2022)는 LLM이 최종 답변 전에 **중간 추론 단계**를 생성하도록 유도하는 방법입니다. "단계별로 생각해보자"(Let's think step by step)라는 문구 하나로 수학·논리 문제 정확도를 크게 높입니다.

**수식 박스 1**:
- 레이블: `CoT 생성 확률`
- 수식:
```
P(a \mid x) = \sum_{r} P(a \mid x, r) \cdot P(r \mid x)
```
- 주석:
  - `x`: 입력 질문, `r`: 추론 체인 (reasoning chain), `a`: 최종 답
  - 직접 생성: `P(a|x)` — 중간 단계 없이 바로 답
  - CoT: `r`을 먼저 생성하고, 그 맥락에서 `a`를 예측 → 더 정확한 조건부 확률
  - 마치 조건부 확률의 주변화처럼 더 많은 정보를 활용

**표 (CoT 변형 기법 비교)**:

| 기법 | 방법 | 장점 |
|------|------|------|
| Zero-shot CoT | "Let's think step by step" 추가 | 예시 불필요 |
| Few-shot CoT | 풀이 과정 포함 예시 3~5개 제공 | 더 높은 정확도 |
| Self-Consistency | 다수결 투표 | 신뢰도 향상 |
| ToT (Tree of Thoughts) | 여러 추론 경로 탐색 | 복잡한 계획 문제 |

---

### 섹션 2: Self-Consistency

**아이콘**: `🗳`  
**제목**: `Self-Consistency — 다수결로 더 정확하게`

**설명**: Self-Consistency(Wang et al., 2022)는 동일한 프롬프트를 여러 번 샘플링하여 **가장 자주 나온 답을 선택**하는 방법입니다. 각 추론 경로는 다를 수 있지만, 올바른 답은 여러 경로에서 수렴합니다.

**수식 박스 2**:
- 레이블: `Self-Consistency 다수결`
- 수식:
```
\hat{a} = \arg\max_{a} \sum_{i=1}^{N} \mathbf{1}\!\left[a_i = a\right]
\quad \text{where each } a_i \sim P(\cdot \mid x, r_i),\; r_i \sim P(\cdot \mid x)
```
- 주석:
  - N번 샘플링하여 가장 많이 등장한 답 `â`를 최종 선택
  - N이 클수록 정확도 향상 (보통 N=40이면 수렴)
  - 단점: 추론 비용 N배 증가 (비용-정확도 트레이드오프)
  - 고성능이 필요할 때만 사용 (수학 올림피아드, 코딩 등)

---

### 섹션 3: GRPO — 강화학습 기반 추론 최적화

**아이콘**: `🚀`  
**제목**: `GRPO — DeepSeek-R1의 핵심 알고리즘`

**설명**: GRPO(Group Relative Policy Optimization, Shao et al., 2024)는 DeepSeek-R1을 만든 핵심 알고리즘입니다. 같은 질문에 대해 여러 응답을 생성하고, **그룹 내 상대적 보상**을 사용하여 PPO의 가치 함수(Value Network) 없이 학습합니다.

**수식 박스 3**:
- 레이블: `GRPO 목적함수 (Shao et al., 2024)`
- 수식:
```
J_{\text{GRPO}}(\theta) = \mathbb{E}_{q \sim \mathcal{D},\, \{o_i\}_{i=1}^G \sim \pi_{\theta_{\text{old}}}}
\left[\frac{1}{G}\sum_{i=1}^G \min\!\left(\rho_i A_i,\; \text{clip}(\rho_i, 1\pm\epsilon)A_i\right) - \beta D_{\text{KL}}\right]
```
- 주석:
  - `q`: 질문, `G`: 그룹 크기 (한 번에 생성하는 응답 수, 보통 8~16)
  - `o_i`: i번째 응답, `ρ_i = π_θ(o_i|q) / π_θold(o_i|q)`: 중요도 비율
  - `A_i`: 어드밴티지 — **그룹 내 상대적 보상**으로 계산 (아래 수식)
  - `clip(..., 1±ε)`: PPO 클리핑 — 너무 큰 업데이트 방지

**수식 박스 4**:
- 레이블: `GRPO 어드밴티지 계산`
- 수식:
```
A_i = \frac{r_i - \text{mean}(\{r_j\}_{j=1}^G)}{\text{std}(\{r_j\}_{j=1}^G)}
```
- 주석:
  - `r_i`: i번째 응답의 보상 (정답이면 +1, 오답이면 0 또는 -1)
  - 그룹 내 평균을 빼고 표준편차로 나눔 → 상대적 우열만 신호로 사용
  - 가치 함수(Critic Network) 학습이 불필요 → PPO보다 단순하고 안정적
  - DeepSeek-R1의 수학 추론 능력이 이 방법으로 크게 향상됨

**callout (info)**:
- 내용: **DeepSeek-R1과 GRPO**: DeepSeek-R1 (2025)은 GRPO를 사용하여 수학·코딩 문제에서 OpenAI o1에 근접하는 성능을 달성했습니다. 핵심은 "생각 과정(Thinking)"을 출력하도록 훈련 + 결과 기반 보상(정답=1, 오답=0)만 사용하는 단순한 GRPO.

---

### 섹션 4: Mixture of Experts (MoE)

**아이콘**: `🔀`  
**제목**: `MoE — 파라미터는 많지만 쓰는 건 일부만`

**설명**: MoE(Mixture of Experts)는 LLM의 FFN 레이어를 여러 "전문가" 네트워크로 교체하고, 각 토큰마다 일부 전문가만 활성화하는 구조입니다. Mistral-8×7B, GPT-4(추정) 등이 MoE 방식을 사용합니다.

**수식 박스 5**:
- 레이블: `MoE 라우팅`
- 수식:
```
\text{MoE}(x) = \sum_{i \in \text{top-}k} g_i(x) \cdot E_i(x)
\quad\text{where}\quad g(x) = \text{softmax}(W_g x)
```
- 주석:
  - `E_i(x)`: i번째 전문가 네트워크의 출력
  - `g_i(x)`: i번째 전문가의 게이팅 가중치 (상위 k개만 활성화)
  - Mistral-8×7B: 8개 전문가 중 2개 활성화 → 실제 연산량 = 13B 파라미터 수준, 용량 = 47B
  - GPT-4는 추정상 16개 전문가 × 111B 파라미터 MoE로 알려짐

---

## 인터랙티브: Self-Consistency 다수결 시뮬레이터

**제목**: `🗳 Self-Consistency — N번 샘플링의 효과`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `sc-n` | 샘플링 횟수 N | 1 | 40 | 1 | 10 |
| `sc-p` | 정답 확률 (단일 시도) | 0.1 | 0.9 | 0.05 | 0.5 |

**JavaScript 로직**:
```javascript
function updateSC() {
  const N = parseInt(document.getElementById('sc-n').value);
  const p = parseFloat(document.getElementById('sc-p').value);
  document.getElementById('sc-n-v').value = N;
  document.getElementById('sc-p-v').value = p.toFixed(2);

  // 다수결 정답 확률: P(다수결=정답) = P(정답 횟수 > N/2)
  let majority = 0;
  for (let k = Math.ceil(N / 2); k <= N; k++) {
    const binom = combinations(N, k);
    majority += binom * Math.pow(p, k) * Math.pow(1 - p, N - k);
  }

  document.getElementById('sc-res').innerHTML =
    `단일 시도 정답 확률: <strong>${(p * 100).toFixed(0)}%</strong><br/>
     N=${N}번 다수결 정답 확률: <strong style="color:var(--accent3)">${(majority * 100).toFixed(1)}%</strong><br/>
     향상: +${((majority - p) * 100).toFixed(1)}%p<br/>
     추론 비용: ${N}배<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       p=0.5이면 단순 다수결이지만, p>0.5이면 N↑로 급격히 향상됩니다.
     </span>`;
}

function combinations(n, k) {
  if (k === 0 || k === n) return 1;
  if (k > n - k) k = n - k;
  let result = 1;
  for (let i = 0; i < k; i++) {
    result *= (n - i);
    result /= (i + 1);
  }
  return result;
}
updateSC();
```

---

## 퀴즈

**질문**: GRPO가 PPO에 비해 갖는 핵심 장점은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 인간 피드백 없이도 작동한다 | ❌ |
| (b) 가치 함수(Critic Network) 없이 그룹 내 상대 보상으로 학습하여 더 단순하다 | ✅ |
| (c) 보상 모델을 사용하지 않는다 | ❌ |
| (d) 무조건 성능이 더 높다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 12D)

```
[ ] CoT가 LLM 정확도를 높이는 이유를 확률 관점에서 설명할 수 있다
[ ] Self-Consistency의 수식과 한계(비용)를 설명할 수 있다
[ ] GRPO 어드밴티지 계산 방법 (그룹 내 상대적 보상)을 설명할 수 있다
[ ] MoE가 "파라미터는 많고 연산은 적은" 이유를 설명할 수 있다
[ ] DeepSeek-R1의 학습 방법을 간략히 설명할 수 있다
[ ] [CME295 L3 강의](https://www.youtube.com/watch?v=Q5baLehv5So&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=3) + [L6 강의](https://www.youtube.com/watch?v=k5Fh-UgTuCo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=6) 시청 완료
```
