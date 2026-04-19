# Week 44 — LLM Alignment 이론·RLHF 수학 (Ch.44)

> **파일명**: `chapters/ch44.html`  
> **Phase**: Phase 3-D · LLM 심화 이론 (Unit 5 확장)  
> **인터랙티브**: ✅ KL-제약 RL 최적 정책 시뮬레이터  
> **우선순위**: P2  
> **전제 챕터**: Ch.12C (RLHF·DPO 기초), Ch.19 (KKT 조건), Ch.22 (확률 수렴)  
> **기반 강의**: [Stanford CME295 L5](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.44 · LLM Alignment 이론 (Phase 3-D)` |
| 제목 | `LLM Alignment 이론 — RLHF 수학과 DPO 도출` |
| 서브타이틀 | `RLHF의 수학적 정식화 완전 유도 — Bradley-Terry 보상 모델, KL 발산 정규화 RL의 Closed-Form 해, 그리고 DPO가 왜 RLHF와 동치인지. NeurIPS/ICML 수준 Alignment 논문 독해를 위한 필수 이론.` |
| 이전 챕터 | `ch43.html` — Ch.43 RAG 수학 기반 |
| 다음 챕터 | `index.html` — 홈으로 (최종 챕터) |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L5**: [LLM Tuning — RLHF, PPO, DPO (1:47:42)](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5)
- **핵심 논문**:
  - Christiano et al., 2017 — Deep RLHF (원논문)
  - Ziegler et al., 2019 — RLHF for Language Models
  - Rafailov et al., 2023 — DPO (Direct Preference Optimization)
  - Anthropic, 2022 — Constitutional AI

---

## EASY BOX 내용

**핵심 비유**: "새 직원을 훈련시킬 때, '하루 일당을 주는 대신 계속 일하게 하면 무조건 나쁜 짓도 한다.' RLHF는 '좋은 행동에 보너스, 나쁜 행동에 패널티'를 주되, '원래 성격(기준 모델)에서 너무 멀어지면 안 된다'는 KL 제약을 건다. DPO는 이 최적해가 수식으로 표현되므로, 보상 모델 없이 직접 그 해로 가는 길이다."

**예시 리스트**:
- KL 발산 없이 보상만 최대화: 점수가 높은 "완벽한 문장" 하나를 무한 반복 출력 (보상 해킹)
- KL 제약 추가: 기준 모델과 비슷하게 유지하면서 보상 최대화 → 균형 잡힌 최적화
- DPO의 통찰: KL-제약 RL의 해 π*(y|x) ∝ π_ref(y|x)·exp(r(x,y)/β) → 이를 역이용
- AI에서: Claude (Constitutional AI), InstructGPT, LLaMA 2 Chat 모두 이 이론 기반

---

## 섹션 구성

### 섹션 1: RLHF의 수학적 정식화

**아이콘**: `📐`  
**제목**: `KL-제약 강화학습 — 완전 수학적 정식화`

**설명**: RLHF는 다음의 최적화 문제로 정식화됩니다: 보상 함수를 최대화하되, 기준 정책(reference policy)과의 KL 발산을 β로 제한합니다. 이 문제의 구조를 이해하면 DPO 도출이 자연스럽습니다.

**수식 박스 1**:
- 레이블: `RLHF 최적화 문제 (KL-제약 RL)`
- 수식:
```
\pi^* = \arg\max_{\pi} \mathbb{E}_{x\sim\mathcal{D},\, y\sim\pi(\cdot|x)}\!\left[r(x,y)\right]
        - \beta\, \mathbb{E}_{x\sim\mathcal{D}}\!\left[D_{\text{KL}}\!\left(\pi(\cdot|x) \,\|\, \pi_{\text{ref}}(\cdot|x)\right)\right]
```
- 주석:
  - `r(x,y)`: 보상 함수 (보상 모델이 출력하는 점수)
  - `π_ref`: 기준 정책 (SFT 모델)
  - `β`: KL 패널티 강도. β=0이면 보상만 최대화 (보상 해킹 위험), β→∞이면 π→π_ref
  - 이 형태: Constrained MDP (제약 마코프 결정 과정)의 특수 케이스

**수식 박스 2**:
- 레이블: `KL-제약 RL의 Closed-Form 최적해 (완전 유도)`
- 수식:
```
\pi^*(y \mid x) = \frac{\pi_{\text{ref}}(y \mid x) \cdot \exp\!\left(r(x,y)/\beta\right)}{Z(x)}
\quad\text{where}\quad Z(x) = \sum_y \pi_{\text{ref}}(y \mid x)\cdot e^{r(x,y)/\beta}
```
- 주석:
  - 유도: 목적함수에 ∂/∂π = 0 → 라그랑주 승수법으로 직접 해 도출
  - `Z(x)`: 정규화 상수 (분배 함수). 직접 계산 불가능 (모든 y에 대한 합)
  - 해석: 보상이 높은 응답은 기준 정책 확률보다 exp(r/β)배 높아짐
  - 이 해가 존재한다는 것 = RLHF 문제의 해가 항상 이 형태

**callout (info)**:
- 내용: **DPO의 핵심 통찰**: 위 최적해를 역으로 표현하면 `r(x,y) = β·log[π*(y|x)/π_ref(y|x)] + β·log Z(x)`. 보상 r(x,y)를 π*의 함수로 표현할 수 있다! 이 관계를 Bradley-Terry 손실에 대입하면 보상 모델 없이 π를 직접 최적화하는 DPO가 도출됩니다.

---

### 섹션 2: Bradley-Terry 보상 모델의 수학

**아이콘**: `⚖`  
**제목**: `쌍대 선호도에서 보상 점수 학습하기`

**설명**: 인간이 "A보다 B가 더 좋다"고 말할 때, 이 선호도를 확률 모델로 표현한 것이 Bradley-Terry 모델입니다. RLHF에서 보상 모델은 이 모델을 기반으로 학습됩니다.

**수식 박스 3**:
- 레이블: `Bradley-Terry 선호 모델`
- 수식:
```
P(y_w \succ y_l \mid x) = \sigma(r(x, y_w) - r(x, y_l)) = \frac{e^{r(x,y_w)}}{e^{r(x,y_w)} + e^{r(x,y_l)}}
```
- 주석:
  - `y_w ≻ y_l`: "y_w가 y_l보다 선호됨" (인간이 선택)
  - `σ`: 시그모이드 함수. r 차이가 클수록 확률이 1에 가까워짐
  - 보상 모델 학습 = 이 확률을 최대화하는 MLE = 이진 교차 엔트로피
  - 확률적 선호 (deterministic 아님): 같은 비교에서도 노이즈 존재

**수식 박스 4**:
- 레이블: `DPO 완전 도출 — 보상을 π로 치환`
- 수식:
```
\mathcal{L}_{\text{DPO}} = -\mathbb{E}\!\left[\log\sigma\!\left(
  \underbrace{\beta\log\frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)}}_{\approx r_\theta(x,y_w)}
  - \underbrace{\beta\log\frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)}}_{\approx r_\theta(x,y_l)}
\right)\right]
```
- 주석:
  - 핵심 치환: `r(x,y) = β·log[π_θ(y|x)/π_ref(y|x)] + β·log Z(x)` (Z(x)는 두 항에서 상쇄)
  - log Z(x)는 y_w, y_l 비교에서 동일하게 상쇄 → 사라짐
  - 결과: 보상 모델 `r_φ` 없이 `π_θ`만으로 DPO 학습 가능
  - 이것이 DPO가 "보상 모델 없이 RLHF와 동치"인 수학적 이유

---

### 섹션 3: Constitutional AI

**아이콘**: `📜`  
**제목**: `Constitutional AI — 규칙으로 정렬하기`

**설명**: Anthropic의 Constitutional AI(CAI)는 인간 피드백 대신 **미리 정의한 원칙(Constitution)**을 사용해 AI를 스스로 비판하고 개선하게 합니다. 이를 RLAIF(RL from AI Feedback)라고도 합니다.

**단계별 리스트**:
```
Step 1: SL-CAI (Supervised Learning)
  → LLM이 원칙 목록을 보고 자신의 응답을 비판
  → 비판을 바탕으로 응답을 개선
  → (비판, 개선된 응답) 쌍으로 SFT

Step 2: RLAIF (RL from AI Feedback)
  → AI 헌법 기준으로 다른 LLM이 쌍대 선호도 생성
  → 이 AI 선호도로 보상 모델 학습
  → RLHF와 동일하지만 인간 피드백 없음 → 확장 가능
```

**callout (tip)**:
- 내용: **RLAIF vs RLHF**: Lee et al., 2023에서 RLAIF가 RLHF와 비슷한 성능을 내면서 인간 어노테이션 없이 확장 가능함을 보임. 이는 "AI가 AI를 훈련"하는 패러다임으로, 현재 대부분의 대형 AI 기업이 활용.

---

### 섹션 4: RLHF 실패 모드와 최신 연구

**아이콘**: `⚠`  
**제목**: `보상 해킹과 Goodhart's Law`

**설명**: "보상 측정 지표가 목표가 되는 순간, 그것은 더 이상 좋은 측정 지표가 아니다" — Goodhart's Law. RLHF에서도 동일한 문제가 발생합니다.

**수식 박스 5**:
- 레이블: `보상 해킹의 수학적 이해`
- 수식:
```
\mathbb{E}[r_{\text{true}}] \leq \mathbb{E}[r_{\hat{\phi}}] 
\quad\text{단,}\quad \mathbb{E}[r_{\hat{\phi}}] \nearrow \text{as training} \nearrow
```
- 주석:
  - `r_true`: 진짜 인간의 선호, `r_φ̂`: 학습된 보상 모델의 점수
  - 보상 모델은 훈련 데이터 분포 밖에서 오버핏 → 진짜 보상과 분리됨
  - 학습이 길어질수록 보상 모델 점수는 오르지만 실제 품질은 떨어질 수 있음
  - 해결: KL 제약 (β), 더 많은 인간 피드백, 보상 앙상블

**표 (RLHF 실패 모드와 해결책)**:

| 실패 모드 | 원인 | 해결책 |
|---------|------|--------|
| 보상 해킹 | 보상 모델 과신 | KL 제약 강화, 보상 앙상블 |
| 응답 길이 편향 | 긴 응답 = 높은 점수 | 길이 패널티, 길이 정규화 |
| 아첨 (Sycophancy) | 인간 평가자 편향 | 다양한 평가자, 더 객관적 기준 |
| 훈련 불안정 | PPO의 높은 민감도 | DPO로 전환, 더 낮은 lr |

---

## 인터랙티브: KL-제약 RL 최적 정책 시뮬레이터

**제목**: `📐 KL 제약이 최적 정책에 미치는 영향`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `align-beta` | β (KL 제약 강도) | 0.01 | 5.0 | 0.01 | 0.5 |
| `align-r1` | 응답 A 보상 r(y1) | -2 | 5 | 0.1 | 3.0 |
| `align-r2` | 응답 B 보상 r(y2) | -2 | 5 | 0.1 | 1.0 |

**JavaScript 로직**:
```javascript
function updateAlign() {
  const beta = parseFloat(document.getElementById('align-beta').value);
  const r1 = parseFloat(document.getElementById('align-r1').value);
  const r2 = parseFloat(document.getElementById('align-r2').value);
  document.getElementById('align-beta-v').value = beta.toFixed(2);
  document.getElementById('align-r1-v').value = r1.toFixed(1);
  document.getElementById('align-r2-v').value = r2.toFixed(1);

  // π_ref = uniform (0.5, 0.5) 가정
  const pRef1 = 0.5, pRef2 = 0.5;
  // π*(y|x) ∝ π_ref(y|x) · exp(r(y)/β)
  const unnorm1 = pRef1 * Math.exp(r1 / beta);
  const unnorm2 = pRef2 * Math.exp(r2 / beta);
  const Z = unnorm1 + unnorm2;
  const pi1 = unnorm1 / Z;
  const pi2 = unnorm2 / Z;

  // KL(π*||π_ref)
  const kl = pi1 * Math.log(pi1 / pRef1) + pi2 * Math.log(pi2 / pRef2);
  const expectedReward = pi1 * r1 + pi2 * r2;
  const objective = expectedReward - beta * kl;

  document.getElementById('align-res').innerHTML =
    `최적 정책 π*(y1): <strong>${(pi1*100).toFixed(1)}%</strong> | π*(y2): <strong>${(pi2*100).toFixed(1)}%</strong><br/>
     기준 정책: π_ref(y1) = 50% | π_ref(y2) = 50%<br/>
     KL(π*‖π_ref): <strong>${kl.toFixed(3)}</strong><br/>
     기대 보상: ${expectedReward.toFixed(3)}<br/>
     목적함수 값: <strong>${objective.toFixed(3)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       β가 작을수록 보상이 높은 y1에 집중 (보상 해킹 위험).<br/>
       β가 클수록 기준 정책(50/50)에 가까워짐.
     </span>`;
}
updateAlign();
```

---

## 퀴즈

**질문**: DPO 수식에서 log Z(x) 항이 사라지는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Z(x)가 항상 1이기 때문에 | ❌ |
| (b) y_w와 y_l 두 응답에 대한 Z(x)가 동일하므로 차이에서 상쇄되기 때문에 | ✅ |
| (c) 보상 모델을 사용하지 않기 때문에 | ❌ |
| (d) KL 발산 항이 Z(x)를 흡수하기 때문에 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 44)

```
[ ] RLHF 최적화 문제를 KL-제약 RL로 수식화할 수 있다
[ ] KL-제약 RL의 Closed-Form 해를 도출할 수 있다 (라그랑주 승수법)
[ ] Bradley-Terry 모델이 보상 모델 학습에 어떻게 사용되는지 설명할 수 있다
[ ] DPO가 RLHF와 수학적으로 동치인 이유를 log Z(x) 상쇄로 설명할 수 있다
[ ] Constitutional AI (RLAIF)와 RLHF의 차이를 설명할 수 있다
[ ] 보상 해킹이 발생하는 수학적 원인을 Goodhart's Law로 연결할 수 있다
[ ] [CME295 L5 강의](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) 재시청 (수학 증명 중심)
```
