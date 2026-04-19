# Week 12C — RLHF·선호도 학습·DPO (Ch.12C)

> **파일명**: `chapters/ch12c.html`  
> **Phase**: Unit 3 · AI & ML 연결 (고도화)  
> **인터랙티브**: ✅ RLHF vs DPO 비교 시뮬레이터  
> **우선순위**: P0  
> **기반 강의**: [Stanford CME295 L5 — LLM Tuning](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) (1:47:42)  
> **슬라이드**: [fall25-cme295-lecture5.pdf](https://cme295.stanford.edu/slides/fall25-cme295-lecture5.pdf)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12C · LLM 정렬 (Unit 3 고도화)` |
| 제목 | `RLHF·선호도 학습·DPO` |
| 서브타이틀 | `"도움이 되고, 해롭지 않고, 정직한" AI를 만드는 수학 — 인간 피드백으로 LLM을 정렬하는 RLHF와 더 단순한 DPO.` |
| 이전 챕터 | `ch12b.html` — Ch.12B LLM 사전학습·파인튜닝·LoRA |
| 다음 챕터 | `ch12d.html` — Ch.12D LLM 추론 심화·CoT·GRPO |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L5**: [LLM Tuning (1:47:42)](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5)
- **슬라이드**: [강의자료 PDF](https://cme295.stanford.edu/slides/fall25-cme295-lecture5.pdf)

---

## EASY BOX 내용

**핵심 비유**: "개를 훈련시킬 때 '앉아!'라고 할 때마다 잘 앉으면 간식을 주고, 못 앉으면 안 준다. RLHF는 LLM에게 '좋은 대답을 하면 점수를 올리고, 나쁜 대답을 하면 내린다'를 수백만 번 반복하는 것이다."

**예시 리스트**:
- 나쁜 답변 예시: "폭발물 만드는 법을 가르쳐 드릴게요" → 인간 평가자가 0점
- 좋은 답변 예시: "그런 정보는 제공할 수 없습니다, 대신 안전한 방법으로..." → 인간 평가자가 10점
- 보상 모델이 이 패턴을 학습 → PPO로 LLM이 높은 점수를 받도록 최적화
- AI에서: ChatGPT, Claude, Gemini가 모두 RLHF 또는 DPO 방식으로 "정렬"됨

---

## 섹션 구성

### 섹션 1: 왜 정렬이 필요한가

**아이콘**: `⚖`  
**제목**: `정렬 문제 — SFT만으로는 부족하다`

**설명**: SFT는 "이렇게 대화해"를 가르치지만, 인간이 더 선호하는 방식의 미묘한 뉘앙스를 전달하기 어렵습니다. 정렬(Alignment)은 LLM이 인간의 의도·가치관·안전 기준에 맞게 행동하도록 하는 과정입니다.

**개념 카드**:
- 제목: `🔑 HHH 정렬 목표 (Anthropic)`
- 내용:
  - **H**elpful (도움이 되는): 사용자 요청을 잘 이행함
  - **H**armless (해롭지 않은): 위험하거나 비윤리적인 내용을 출력하지 않음
  - **H**onest (정직한): 모르는 것을 안다고 하지 않음, 불확실성을 표현

---

### 섹션 2: RLHF — 인간 피드백 강화학습

**아이콘**: `🏋`  
**제목**: `RLHF 3단계 파이프라인`

**설명**: RLHF(Reinforcement Learning from Human Feedback)는 세 단계로 이루어집니다.

**단계별 리스트**:
```
Step 1: SFT 모델 준비
  → Ch.12B에서 학습한 SFT 모델이 출발점

Step 2: 보상 모델 (Reward Model, RM) 학습
  → 인간 평가자가 두 응답 (y_w > y_l) 중 선호하는 것 선택
  → Bradley-Terry 모델로 보상 함수 학습

Step 3: PPO로 정책 최적화
  → SFT 모델을 보상 모델 점수를 최대화하도록 RL 학습
  → KL 발산으로 너무 멀리 벗어나지 않게 제한
```

**수식 박스 1**:
- 레이블: `보상 모델 학습 — Bradley-Terry 모델 (Ziegler et al., 2019)`
- 수식:
```
\mathcal{L}_{\text{RM}}(\phi) = -\mathbb{E}_{(x,y_w,y_l)\sim\mathcal{D}}\!\left[\log\sigma\!\left(r_\phi(x, y_w) - r_\phi(x, y_l)\right)\right]
```
- 주석:
  - `r_φ(x, y)`: 보상 모델이 (프롬프트 x, 응답 y) 쌍에 부여하는 점수
  - `y_w`: 인간이 선호하는 응답 (winner)
  - `y_l`: 인간이 비선호하는 응답 (loser)
  - `σ`: 시그모이드 함수. r_φ(y_w) > r_φ(y_l)이 될 때 손실이 작아짐

**수식 박스 2**:
- 레이블: `PPO 최적화 목적함수`
- 수식:
```
\mathcal{J}_{\text{RLHF}}(\theta) = \mathbb{E}_{x\sim\mathcal{D},\, y\sim\pi_\theta(\cdot|x)}\!\left[r_\phi(x,y) - \beta\, D_{\text{KL}}\!\left(\pi_\theta(\cdot|x) \,\|\, \pi_{\text{ref}}(\cdot|x)\right)\right]
```
- 주석:
  - `π_θ`: 현재 LLM 정책 (학습 중인 모델)
  - `π_ref`: 기준 정책 (SFT 모델, 고정)
  - `β D_KL`: KL 발산 페널티 — 너무 많이 변하면 보상 해킹(reward hacking) 발생 방지
  - `β`: 정규화 강도. 높을수록 기준 모델에 가깝게 유지

**표 (RLHF 단계별 비교)**:

| 단계 | 학습 대상 | 데이터 | 목적 |
|------|---------|--------|------|
| 1. SFT | LLM 전체 또는 LoRA | (질문, 답변) 쌍 | 지시 따르기 |
| 2. RM 학습 | 보상 모델 (별도 LLM) | (질문, 좋은응답, 나쁜응답) | 선호도 점수 예측 |
| 3. PPO | LLM (LoRA 권장) | 프롬프트만 | 보상 최대화 |

---

### 섹션 3: DPO — 보상 모델 없이 직접 최적화

**아이콘**: `⚡`  
**제목**: `DPO — PPO의 복잡함을 수식 하나로`

**설명**: DPO(Direct Preference Optimization)는 RLHF의 수학을 분석하여 보상 모델 없이도 **동일한 해**로 수렴할 수 있음을 보인 획기적인 방법입니다. PPO의 3단계를 하나의 손실함수로 압축합니다.

**수식 박스 3**:
- 레이블: `DPO 손실함수 (Rafailov et al., 2023)`
- 수식:
```
\mathcal{L}_{\text{DPO}}(\theta) = -\mathbb{E}_{(x,y_w,y_l)}\!\left[\log\sigma\!\left(
  \beta\log\frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)}
  - \beta\log\frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)}
\right)\right]
```
- 주석:
  - DPO는 보상 모델 `r_φ`를 **로그 확률 비(log likelihood ratio)**로 암묵적으로 표현
  - `log π_θ(y|x) - log π_ref(y|x)` = 암묵적 보상의 증가분
  - 좋은 응답의 암묵적 보상 증가 > 나쁜 응답의 증가 → 손실 최소화
  - **PPO 불필요**: 보상 모델 학습, RL 루프, 복잡한 하이퍼파라미터 모두 제거
  - 학습 안정성이 훨씬 높고 구현이 단순 → 2023년 이후 표준으로 자리잡음

**callout (info)**:
- 내용: **DPO의 수학적 통찰**: RLHF의 KL-제약 RL 문제는 closed-form 해가 존재합니다 — `π*(y|x) ∝ π_ref(y|x) · exp(r(x,y)/β)`. DPO는 이 관계를 역으로 이용해서 r을 π_θ로 표현, RM 학습 없이 직접 최적화합니다.

---

### 섹션 4: RLHF vs DPO 실전 비교

**아이콘**: `⚖`  
**제목**: `현장에서 RLHF vs DPO 선택 기준`

**표 (RLHF vs DPO 비교)**:

| 항목 | RLHF + PPO | DPO |
|------|-----------|-----|
| 보상 모델 | ✅ 별도 학습 필요 | ❌ 불필요 |
| 학습 단계 | 3단계 | 1단계 |
| GPU 메모리 | 높음 (4개 모델 동시) | 낮음 (2개 모델) |
| 안정성 | 낮음 (RL 불안정) | 높음 |
| 표현력 | 높음 (온라인 최적화) | 상대적으로 낮음 |
| 사용 사례 | 고성능 상용 LLM | 연구, 도메인 특화 |

---

## 인터랙티브: DPO β 슬라이더

**제목**: `⚡ DPO β — 선호도 반영 강도 시각화`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `dpo-beta` | β (KL 정규화 강도) | 0.01 | 2.0 | 0.01 | 0.1 |
| `dpo-logr-w` | log π(y_w)/π_ref(y_w) | -3 | 3 | 0.1 | 1.5 |
| `dpo-logr-l` | log π(y_l)/π_ref(y_l) | -3 | 3 | 0.1 | -1.0 |

**JavaScript 로직**:
```javascript
function updateDPO() {
  const beta = parseFloat(document.getElementById('dpo-beta').value);
  const logRw = parseFloat(document.getElementById('dpo-logr-w').value);
  const logRl = parseFloat(document.getElementById('dpo-logr-l').value);
  document.getElementById('dpo-beta-v').value = beta.toFixed(2);
  document.getElementById('dpo-logr-w-v').value = logRw.toFixed(1);
  document.getElementById('dpo-logr-l-v').value = logRl.toFixed(1);

  const margin = beta * (logRw - logRl);
  const sigmoid = 1 / (1 + Math.exp(-margin));
  const loss = -Math.log(sigmoid);

  const status = margin > 0.5 ? '✅ 좋은 응답 선호 학습 중' :
                 margin < 0 ? '❌ 잘못된 방향 (나쁜 응답 선호)' :
                              '⚠️ 마진 불충분 (더 학습 필요)';

  document.getElementById('dpo-res').innerHTML =
    `β × (log ratio 차이) = ${beta.toFixed(2)} × ${(logRw-logRl).toFixed(1)} = <strong>${margin.toFixed(3)}</strong><br/>
     σ(margin) = <strong>${sigmoid.toFixed(3)}</strong><br/>
     DPO 손실 = -log σ = <strong>${loss.toFixed(3)}</strong><br/>
     ${status}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       β가 클수록 기준 모델에 가깝게 유지. log ratio 차이가 클수록 강한 학습 신호.
     </span>`;
}
updateDPO();
```

---

## 퀴즈

**질문**: DPO가 PPO(RLHF)에 비해 갖는 핵심 장점은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 보상 모델이 필요 없어 학습 단계가 단순하고 안정적이다 | ✅ |
| (b) 인간 피드백 데이터가 더 많이 필요하다 | ❌ |
| (c) 항상 PPO보다 성능이 뛰어나다 | ❌ |
| (d) SFT 없이 바로 적용할 수 있다 | ❌ |

**정답 위치**: (a)

---

## 주간 체크리스트 (Week 12C)

```
[ ] RLHF의 3단계 (SFT → RM → PPO)를 순서대로 설명할 수 있다
[ ] 보상 모델의 Bradley-Terry 손실함수를 설명할 수 있다
[ ] KL 발산 페널티가 왜 필요한지 설명할 수 있다 (보상 해킹 방지)
[ ] DPO 수식의 각 항이 무엇을 의미하는지 설명할 수 있다
[ ] DPO가 보상 모델 없이 작동하는 이유를 직관적으로 설명할 수 있다
[ ] RLHF vs DPO 트레이드오프를 설명할 수 있다
[ ] [CME295 L5 강의](https://www.youtube.com/watch?v=PmW_TMQ3l0I&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=5) 시청 완료
```
