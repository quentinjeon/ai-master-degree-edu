# Week 08~10 — MAP, Bias-Variance, 가설검정 (Ch.8)

> **파일명**: `chapters/ch08.html`  
> **인터랙티브**: ✅ Bias-Variance 시각화  
> **우선순위**: P1

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.8 · 통계 & 추정 (10주차)` |
| 제목 | `MAP, Bias-Variance Tradeoff, 가설검정` |
| 서브타이틀 | `너무 단순한 모델도, 너무 복잡한 모델도 위험합니다. 좋은 추정이 무엇인지 배웁니다.` |
| 이전 챕터 | `ch07.html` — Ch.7 표본과 MLE |
| 다음 챕터 | `ch09.html` — Ch.9 ML 연결 1 |

---

## EASY BOX 내용

**핵심 비유**: "너무 단순한 모델은 패턴을 못 보고(underfitting), 너무 복잡한 모델은 노이즈까지 외운다(overfitting). 딱 적당한 복잡도를 찾는 것이 Bias-Variance Tradeoff다."

**예시 리스트**:
- 과소적합: 시험을 준비 못 해서 모든 문제에 "2번"이라고 쓰는 것 (너무 단순)
- 과적합: 작년 시험 문제를 통째로 외워서 올해 새 문제는 못 푸는 것 (너무 복잡)
- AI에서: 적절한 모델 크기, 정규화, 드롭아웃 → 좋은 일반화 (generalization)

---

## 섹션 구성

### 섹션 1: MAP (Maximum A Posteriori)

**아이콘**: `🎯`  
**제목**: `MAP 추정 (MLE + Prior)`

**설명**: MAP는 MLE에 사전 지식(prior)을 추가한 추정법입니다.

**수식 박스 1**:
- 레이블: `MAP vs MLE`
- 수식:
```
\hat{\theta}_{MLE} = \argmax_\theta \log P(\mathcal{D} \mid \theta)
\quad \text{vs} \quad
\hat{\theta}_{MAP} = \argmax_\theta \left[ \log P(\mathcal{D} \mid \theta) + \log P(\theta) \right]
```
- 주석:
  - MLE: 데이터만 봄. 데이터가 적으면 과적합 위험
  - MAP: 데이터 + prior. prior가 정규화(regularization) 역할
  - Gaussian prior P(θ) → L2 정규화와 수학적으로 동치

**개념 카드**:
- 제목: `🔑 정규화 = MAP의 prior`
- 내용:
  - L2 정규화: `loss + λ‖θ‖²` = Gaussian prior N(0, 1/λ) 가정
  - L1 정규화: `loss + λ‖θ‖₁` = Laplace prior 가정
  - 즉, **정규화는 베이지안 해석에서 prior를 추가하는 것**

---

### 섹션 2: Bias-Variance Tradeoff

**아이콘**: `⚖️`  
**제목**: `Bias-Variance Tradeoff`

**설명**: 모델의 예측 오류는 bias²과 variance의 합으로 분해됩니다.

**수식 박스 2**:
- 레이블: `Bias-Variance 분해`
- 수식:
```
\mathbb{E}[(\hat{f}(x) - y)^2] = 
\underbrace{\text{Bias}[\hat{f}(x)]^2}_{\text{과소적합}} + 
\underbrace{\text{Var}[\hat{f}(x)]}_{\text{과적합}} + 
\underbrace{\sigma^2_\epsilon}_{\text{줄일 수 없는 노이즈}}
```
- 주석:
  - Bias: 모델이 평균적으로 얼마나 틀리는지 (너무 단순한 모델 → 큰 bias)
  - Variance: 훈련 데이터에 따라 예측이 얼마나 흔들리는지 (너무 복잡한 모델 → 큰 variance)
  - `σ²_ε`: 데이터 자체의 노이즈 (어떤 모델도 줄일 수 없음)

**표 (Bias vs Variance)**:

| | Bias 큼 | Variance 큼 |
|--|---------|------------|
| 모델 복잡도 | 너무 단순 | 너무 복잡 |
| 상태 | 과소적합 (underfitting) | 과적합 (overfitting) |
| 해결책 | 모델 복잡도 증가, 특성 추가 | 정규화, 드롭아웃, 더 많은 데이터 |
| 예시 | 선형 모델로 비선형 패턴 학습 | 100차 다항식으로 10개 데이터 학습 |

---

### 섹션 3: 가설 검정 (Hypothesis Testing)

**아이콘**: `🔬`  
**제목**: `통계적 가설 검정`

**설명**: 논문에서 "이 방법이 정말 더 좋은가?"를 통계적으로 검증하는 방법입니다.

**단계별 설명**:
1. **귀무가설 H₀ 설정**: "두 방법 사이에 차이가 없다"
2. **대립가설 H₁ 설정**: "새로운 방법이 더 좋다"
3. **검정 통계량 계산**: 데이터로부터 t-통계량, z-통계량 등 계산
4. **p-value 계산**: p < α (보통 0.05)이면 귀무가설 기각

**개념 카드**:
- 제목: `🔑 p-value의 정확한 의미`
- 내용: p-value = "귀무가설이 사실이라고 가정했을 때, 현재 관측된 데이터(또는 더 극단적인 결과)가 나올 확률"
- p = 0.03: H₀이 사실이면 이런 데이터가 나올 확률이 3% → H₀ 기각

**callout (warn)**:
- 제목: `흔한 오해`
- 내용:
  - p-value는 "가설이 옳을 확률"이 아님
  - p < 0.05가 실용적 중요성을 보장하지 않음 (효과 크기, Cohen's d 함께 보기)
  - 표본이 크면 의미 없는 차이도 p < 0.05 가능

---

## 인터랙티브: Bias-Variance 시각화

**제목**: `🔢 Bias-Variance 트레이드오프 시뮬레이터`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `bv-complexity` | 모델 복잡도 | 1 | 10 | 1 | 3 |
| `bv-noise` | 데이터 노이즈 | 0 | 5 | 0.1 | 1 |

**JavaScript 로직**:
```javascript
function updateBV() {
  const c = parseInt(document.getElementById('bv-complexity').value);
  const noise = parseFloat(document.getElementById('bv-noise').value);
  document.getElementById('bv-c-v').value = c;
  document.getElementById('bv-n-v').value = noise.toFixed(1);

  // 단순화된 bias-variance 추정 (모의값)
  const bias2 = Math.max(0, 5 - c) ** 1.5;
  const variance = (c * 0.8) ** 1.5;
  const irreducible = noise ** 2;
  const totalError = bias2 + variance + irreducible;

  document.getElementById('bv-res').innerHTML =
    `총 오류 = <strong>${totalError.toFixed(2)}</strong><br/>
    Bias² = ${bias2.toFixed(2)} | Variance = ${variance.toFixed(2)} | 노이즈 = ${irreducible.toFixed(2)}<br/>
    <span style="font-size:.78rem;color:var(--muted)">
      ${c <= 3 ? '⚠️ 과소적합 위험 (bias 큼)' : c >= 8 ? '⚠️ 과적합 위험 (variance 큼)' : '✅ 균형 잡힌 복잡도'}
    </span>`;
}
updateBV();
```

---

## 퀴즈

**질문**: L2 정규화(`loss + λ‖θ‖²`)는 MAP 관점에서 어떤 prior를 가정하는가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Laplace prior | ❌ |
| (b) Bernoulli prior | ❌ |
| (c) Gaussian prior N(0, 1/λ) | ✅ |
| (d) Uniform prior | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 9~10)

```
[ ] MAP와 MLE의 차이를 설명할 수 있다
[ ] 정규화가 prior와 동치임을 설명할 수 있다
[ ] bias(편향)를 설명할 수 있다
[ ] variance(분산)를 설명할 수 있다
[ ] bias-variance tradeoff를 과소/과적합으로 설명할 수 있다
[ ] p-value를 과장 없이 정확하게 설명할 수 있다
[ ] 가설 검정 4단계를 순서대로 말할 수 있다
```
