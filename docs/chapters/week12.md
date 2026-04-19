# Week 12~13 — 딥러닝과 LLM (Ch.12)

> **파일명**: `chapters/ch12.html`  
> **인터랙티브**: ✅ Temperature 슬라이더 + next-token 시뮬레이터  
> **우선순위**: P2

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12 · 딥러닝 & LLM (14주차)` |
| 제목 | `딥러닝과 언어 모델 (LLM)` |
| 서브타이틀 | `언어모델이 다음 단어를 확률로 고른다는 것. P(다음 단어 | 앞 문장들)이 LLM의 핵심입니다.` |
| 이전 챕터 | `ch11.html` — Ch.11 정보이론 |
| 다음 챕터 | `ch13.html` — Ch.13 Calibration & 불확실성 |

---

## EASY BOX 내용

**핵심 비유**: "문장을 읽고 '다음에 무슨 단어가 나올까?' 맞히는 게임을 엄청 많이 해서 똑똑해진 것이 LLM이다."

**예시 리스트**:
- "나는 학교에 ___" → "가다" 확률 70%, "다니다" 확률 20%, "오다" 10%
- LLM은 이 예측을 조 단위 문장 데이터로 학습
- AI에서: ChatGPT, Claude, LLaMA 등 모두 P(다음 토큰 | 이전 토큰들)를 학습한 모델

---

## 섹션 구성

### 섹션 1: 언어 모델의 확률 구조

**아이콘**: `📝`  
**제목**: `언어 모델 = 조건부 확률의 연쇄`

**설명**: 문장의 확률은 각 단어의 조건부 확률의 곱으로 표현됩니다.

**수식 박스 1**:
- 레이블: `문장 확률 — Autoregressive Factorization`
- 수식:
```
P(w_1, w_2, \ldots, w_T) = \prod_{t=1}^{T} P(w_t \mid w_1, \ldots, w_{t-1})
= \prod_{t=1}^{T} P(w_t \mid w_{<t})
```
- 주석:
  - `w_t`: t번째 토큰
  - `w_{<t}`: t 이전의 모든 토큰 (context)
  - 연쇄 법칙(Chain Rule)의 직접적인 응용
  - LLM 학습 = 이 조건부 확률들을 가장 잘 추정하는 파라미터 찾기

---

### 섹션 2: LLM 학습 목표 (NLL)

**아이콘**: `🎯`  
**제목**: `LLM 학습 목표 — 음의 로그우도 최소화`

**수식 박스 2**:
- 레이블: `언어 모델 손실함수 (NLL)`
- 수식:
```
\mathcal{L} = -\frac{1}{T}\sum_{t=1}^{T} \log P_\theta(w_t \mid w_{<t})
```
- 주석:
  - 음의 로그우도 (NLL) = 언어모델의 표준 손실함수
  - 이것을 최소화 = 각 위치에서 다음 토큰을 잘 맞추는 것 = MLE
  - **Perplexity**: `exp(NLL)` — 낮을수록 좋은 언어모델

---

### 섹션 3: Sampling 전략

**아이콘**: `🎰`  
**제목**: `토큰 샘플링 전략 — Temperature, Top-k, Top-p`

**설명**: LLM이 다음 토큰을 선택하는 방법을 제어하는 파라미터들입니다.

**수식 박스 3**:
- 레이블: `Temperature Scaling`
- 수식:
```
P(w_t = k \mid w_{<t}; \tau) = \frac{e^{z_k / \tau}}{\sum_{j} e^{z_j / \tau}}
```
- 주석:
  - `τ` (temperature): 출력 분포의 "날카로움" 조절
  - `τ → 0`: 항상 가장 높은 확률 토큰 선택 (greedy, 결정론적)
  - `τ = 1`: 원래 분포 그대로
  - `τ > 1`: 분포를 부드럽게 (더 다양한 출력, 창의적)

**표 (샘플링 전략 비교)**:

| 전략 | 설명 | 특징 | 사용 시 |
|------|------|------|---------|
| Greedy | 항상 확률 최대 토큰 | 빠르지만 반복적 | 정확한 사실 답변 |
| Temperature (τ) | 분포 날카로움 조절 | τ 높으면 창의적 | 창의적 생성 |
| Top-k | 상위 k개만 샘플링 | 품질/다양성 균형 | 일반적 대화 |
| Top-p (nucleus) | 누적확률 p까지만 | 동적 k 결정 | 고품질 생성 |

---

## 인터랙티브: Temperature 시뮬레이터

**제목**: `🔢 Temperature Sampling 시뮬레이터`

**설명**: 가상의 다음 토큰 후보 [가다, 오다, 먹다]의 logit을 설정하고, Temperature에 따라 확률이 어떻게 바뀌는지 확인하세요.

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `tmp-tau` | Temperature τ | 0.1 | 3.0 | 0.1 | 1.0 |
| `tmp-z1` | "가다" logit | -3 | 3 | 0.1 | 2.0 |
| `tmp-z2` | "오다" logit | -3 | 3 | 0.1 | 1.0 |
| `tmp-z3` | "먹다" logit | -3 | 3 | 0.1 | 0.5 |

**JavaScript**:
```javascript
function updateTemp() {
  const tau = parseFloat(document.getElementById('tmp-tau').value);
  const z1 = parseFloat(document.getElementById('tmp-z1').value);
  const z2 = parseFloat(document.getElementById('tmp-z2').value);
  const z3 = parseFloat(document.getElementById('tmp-z3').value);
  document.getElementById('tmp-tau-v').value = tau.toFixed(1);
  document.getElementById('tmp-z1-v').value = z1.toFixed(1);
  document.getElementById('tmp-z2-v').value = z2.toFixed(1);
  document.getElementById('tmp-z3-v').value = z3.toFixed(1);

  const zs = [z1, z2, z3].map(z => z / tau);
  const exps = zs.map(z => Math.exp(z));
  const sum = exps.reduce((a, b) => a + b, 0);
  const probs = exps.map(e => e / sum);
  const words = ['가다', '오다', '먹다'];

  document.getElementById('tmp-res').innerHTML =
    `P(가다)=<strong>${probs[0].toFixed(3)}</strong> | 
     P(오다)=<strong>${probs[1].toFixed(3)}</strong> | 
     P(먹다)=<strong>${probs[2].toFixed(3)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       τ=${tau.toFixed(1)}: ${tau<0.5?'결정론적(greedy)':tau>1.5?'창의적(diverse)':'균형잡힌'}
     </span>`;
}
updateTemp();
```

---

## 퀴즈

**질문**: LLM에서 Temperature τ = 0.1로 설정하면 어떻게 되는가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 더 다양하고 창의적인 답변이 나온다 | ❌ |
| (b) 항상 확률이 가장 높은 토큰을 선택한다 (greedy에 가까워진다) | ✅ |
| (c) 모든 토큰이 동일한 확률을 갖게 된다 | ❌ |
| (d) 토큰을 무작위로 선택한다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 14)

```
[ ] LLM이 P(다음 토큰 | 이전 토큰들)을 학습한다는 것을 안다
[ ] Autoregressive factorization을 설명할 수 있다
[ ] NLL 손실함수를 설명할 수 있다
[ ] Perplexity가 무엇인지 안다
[ ] Temperature가 출력 분포에 미치는 영향을 설명할 수 있다
[ ] Top-k와 Top-p의 차이를 설명할 수 있다
```
