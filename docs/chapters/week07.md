# Week 07~08 — 표본·통계량·MLE (Ch.7)

> **파일명**: `chapters/ch07.html`  
> **대응 섹션**: `c7` (기존 통계적 추론과 통합)  
> **인터랙티브**: ✅ MLE 시뮬레이터 + 신뢰구간 계산기  
> **우선순위**: P1

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.7 · 통계 & 추정 (8~9주차)` |
| 제목 | `표본, 통계량, 최대우도추정` |
| 서브타이틀 | `일부를 보고 전체를 짐작하는 법. 딥러닝 학습의 수학적 기반인 MLE를 이해합니다.` |
| 이전 챕터 | `ch06.html` — Ch.6 기대값과 분산 |
| 다음 챕터 | `ch08.html` — Ch.8 MAP & Bias-Variance |

---

## EASY BOX 내용

**핵심 비유**: "발자국을 보고 '누가 지나갔을까?' 추리하는 것이 MLE다. 가장 그럴듯한 설명을 찾는다."

**예시 리스트**:
- 표본: 반 친구 3명 키만 재봐도 반 전체를 짐작할 수 있다 (하지만 완전히 정확하지는 않다)
- 통계량: 표본 평균, 표본 분산 — 데이터로부터 계산한 값
- AI에서: 딥러닝 학습 = MLE (최대우도추정) — 데이터를 가장 잘 설명하는 파라미터 찾기

---

## 섹션 구성

### 섹션 1: 표본과 모집단

**아이콘**: `👥`  
**제목**: `모집단과 표본 (Population & Sample)`

**표**:

| 용어 | 설명 | 기호 | 예시 |
|------|------|------|------|
| 모집단 | 관심 있는 전체 집합 | N (모수) | 한국 전체 성인의 키 |
| 표본 | 관측한 일부 | n (표본 크기) | 1000명의 키 |
| 모평균 | 모집단의 진짜 평균 | μ | 실제 평균 키 (모름) |
| 표본평균 | 표본으로 추정한 평균 | x̄ | 관측한 1000명의 평균 |

**수식 박스 1**:
- 레이블: `표본평균과 표본분산`
- 수식:
```
\bar{x} = \frac{1}{n}\sum_{i=1}^n x_i
\qquad
s^2 = \frac{1}{n-1}\sum_{i=1}^n (x_i - \bar{x})^2
```
- 주석: `n-1`로 나누는 이유: 불편 추정량(unbiased estimator) — 표본 분산이 모집단 분산을 평균적으로 맞게 추정

---

### 섹션 2: Likelihood와 MLE

**아이콘**: `🎯`  
**제목**: `우도 (Likelihood) & 최대우도추정 (MLE)`

**EASY BOX (섹션 내)**:
- 비유: 동전을 10번 던졌는데 9번 앞면이 나왔다. "이 동전은 공평하다(p=0.5)"는 설명보다 "이 동전은 앞면이 잘 나온다(p=0.9)"가 더 그럴듯하다. → 더 그럴듯한 설명 = likelihood가 높은 설명

**표 (Probability vs Likelihood)**:

| 구분 | 고정 | 변함 | 표기 |
|------|------|------|------|
| 확률 (Probability) | 파라미터 θ | 데이터 X | P(X | θ고정) |
| 우도 (Likelihood) | 데이터 X | 파라미터 θ | L(θ | X고정) |

**수식 박스 2**:
- 레이블: `MLE 목적함수`
- 수식:
```
\hat{\theta}_{MLE} = \argmax_\theta \prod_{i=1}^{n} p(x_i \mid \theta) 
= \argmax_\theta \sum_{i=1}^{n} \log p(x_i \mid \theta)
```
- 주석:
  - 곱 → 합(로그 변환)으로 수치 안정성 확보 (작은 확률을 여러 번 곱하면 underflow 발생)
  - 딥러닝의 Cross-Entropy 최소화 = 음의 로그우도(NLL) 최소화 = MLE

**개념 카드**:
- 제목: `🔑 딥러닝 학습 = MLE`
- 내용:
  - Cross-Entropy Loss = `-∑ y log ŷ` = 음의 로그우도
  - CE를 최소화 = 로그우도를 최대화 = MLE
  - 즉, **딥러닝 학습의 수학적 본질은 MLE다**

**callout (tip)**:
- 내용: MAP(Maximum A Posteriori) = MLE + prior. `argmax_θ [log P(D|θ) + log P(θ)]` — 정규화(L2 패널티)가 Gaussian prior와 동치

---

### 섹션 3: 신뢰구간과 가설 검정

**아이콘**: `📏`  
**제목**: `신뢰구간 (Confidence Interval)`

**수식 박스 3**:
- 레이블: `모평균의 95% 신뢰구간`
- 수식: `\bar{x} \pm 1.96 \cdot \frac{s}{\sqrt{n}}`
- 주석:
  - `s/√n`: 표준오차 (SE) — 표본평균의 표준편차
  - 1.96: 표준정규분포의 97.5% 분위수 (양쪽 합쳐 95%)
  - 의미: "동일 방법으로 100번 실험하면, 95번은 이 구간이 참 모평균을 포함"

---

## 인터랙티브 1: Bernoulli MLE 시뮬레이터

**제목**: `🔢 동전 던지기 MLE 시뮬레이터`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `mle-n` | 시도 횟수 | 1 | 100 | 1 | 20 |
| `mle-k` | 앞면 횟수 | 0 | 100 | 1 | 15 |

**JavaScript 로직**:
```javascript
function updateMLE() {
  const n = parseInt(document.getElementById('mle-n').value);
  const k = Math.min(parseInt(document.getElementById('mle-k').value), n);
  document.getElementById('mle-n-v').value = n;
  document.getElementById('mle-k-v').value = k;

  const mle = k / n;  // Bernoulli MLE: p̂ = k/n
  const logL = k * Math.log(mle + 1e-10) + (n - k) * Math.log(1 - mle + 1e-10);

  document.getElementById('mle-res').innerHTML =
    `MLE 추정치 p̂ = k/n = ${k}/${n} = <strong>${mle.toFixed(3)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">
      로그우도 log L = ${logL.toFixed(3)} | 
      "데이터를 가장 잘 설명하는 p는 ${(mle*100).toFixed(1)}%"
    </span>`;
}
updateMLE();
```

## 인터랙티브 2: 신뢰구간 계산기

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `ci-m` | 표본평균 x̄ | 0 | 100 | 1 | 50 |
| `ci-s` | 표준편차 s | 1 | 30 | 0.5 | 10 |
| `ci-n` | 표본 크기 n | 5 | 500 | 5 | 30 |

**JavaScript 로직**:
```javascript
function updateCI() {
  const m = parseFloat(document.getElementById('ci-m').value);
  const s = parseFloat(document.getElementById('ci-s').value);
  const n = parseInt(document.getElementById('ci-n').value);
  document.getElementById('ci-m-v').value = m;
  document.getElementById('ci-s-v').value = s;
  document.getElementById('ci-n-v').value = n;

  const se = s / Math.sqrt(n);
  const lo = (m - 1.96 * se).toFixed(3);
  const hi = (m + 1.96 * se).toFixed(3);

  document.getElementById('ci-res').innerHTML =
    `95% 신뢰구간: [<strong>${lo}</strong>, <strong>${hi}</strong>]<br/>
    <span style="font-size:.78rem;color:var(--muted)">
      SE=${se.toFixed(3)}, 오차한계 ±${(1.96*se).toFixed(3)}
    </span>`;
}
updateCI();
```

---

## 퀴즈

**질문**: 동전을 20번 던져 15번 앞면이 나왔을 때, Bernoulli MLE 추정치 p̂은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 0.5 (공평한 동전 가정) | ❌ |
| (b) 0.7 | ❌ |
| (c) 0.75 (15/20) | ✅ |
| (d) 1.0 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 7~8)

```
[ ] 모집단과 표본의 차이를 설명할 수 있다
[ ] 표본평균과 표본분산을 계산할 수 있다
[ ] likelihood와 probability의 차이를 설명할 수 있다
[ ] MLE가 무엇인지 직관적으로 설명할 수 있다
[ ] log-likelihood를 사용하는 이유를 설명할 수 있다
[ ] 딥러닝 CE loss가 MLE임을 설명할 수 있다
[ ] 95% 신뢰구간의 의미를 정확히 설명할 수 있다
```
