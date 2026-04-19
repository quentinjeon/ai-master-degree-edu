# Week 06 — 기대값과 분산 (Ch.6)

> **파일명**: `chapters/ch06.html`  
> **대응 섹션**: `c6`  
> **인터랙티브**: ✅ 기대값/분산 계산기  
> **우선순위**: P1

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.6 · 확률 & 통계 (7주차)` |
| 제목 | `기대값과 분산` |
| 서브타이틀 | `평균과 흔들림을 수학적으로 표현합니다. AI 학습 목표와 불확실성의 기초입니다.` |
| 이전 챕터 | `ch05.html` — Ch.5 확률분포 |
| 다음 챕터 | `ch07.html` — Ch.7 표본과 MLE |

---

## EASY BOX 내용

**핵심 비유**: "기대값은 '보통 어느 정도인가', 분산은 '얼마나 들쑥날쑥한가'다."

**예시 리스트**:
- 기대값: 동전을 1000번 던지면 앞면이 약 500번 → 기대값 0.5
- 분산: 학생 5명 키가 비슷하면 분산 작음, 한 명이 아주 작고 한 명이 아주 크면 분산 큼
- AI에서: AI 학습 = 기대 손실 E[L]을 최소화. 모델 예측의 불확실성 = 분산

---

## 섹션 구성

### 섹션 1: 기대값 (Expected Value)

**아이콘**: `⚖️`  
**제목**: `기대값 (Expected Value / Mean)`

**설명**: 확률변수 X의 기대값은 무한히 많은 실험을 반복했을 때 나타나는 평균값입니다.

**수식 박스 1**:
- 레이블: `기대값 공식`
- 수식:
```
\mathbb{E}[X] = \begin{cases}
\sum_x x \cdot P(X=x) & \text{(이산형)} \\
\int_{-\infty}^{\infty} x \cdot f(x) \, dx & \text{(연속형)}
\end{cases}
```
- 주석: `E[X]`: 기대값 연산자 (논문에서 E[] 또는 𝔼[] 사용), f(x): PDF

**수식 박스 2 (선형성)**:
- 레이블: `기대값의 선형성 (매우 중요)`
- 수식: `\mathbb{E}[aX + bY] = a\mathbb{E}[X] + b\mathbb{E}[Y]`
- 주석: X, Y가 독립이 아니어도 성립. 머신러닝에서 배치 손실 계산에 핵심적으로 사용

**개념 카드**:
- 제목: `🔑 AI에서의 기대값`
- 내용:
  - 학습 목표: `θ* = argmin_θ E[L(f_θ(x), y)]` — 기대 손실을 최소화
  - 배치 SGD: 미니배치로 기대 그레디언트를 근사

---

### 섹션 2: 분산 (Variance)

**아이콘**: `📏`  
**제목**: `분산과 표준편차 (Variance & Standard Deviation)`

**설명**: 분산은 확률변수의 값이 기대값으로부터 얼마나 흩어져 있는지를 측정합니다.

**수식 박스 3**:
- 레이블: `분산 공식`
- 수식: `\text{Var}(X) = \mathbb{E}[(X - \mathbb{E}[X])^2] = \mathbb{E}[X^2] - (\mathbb{E}[X])^2`
- 주석:
  - `E[X]`를 빼서 중심을 0으로 만든 후 제곱 → 항상 양수
  - 두 번째 등식: 계산에 편리한 형태 (분산의 공식)
  - 표준편차: `σ = √Var(X)` (원래 단위와 같음)

---

### 섹션 3: 공분산과 상관관계

**아이콘**: `↔️`  
**제목**: `공분산 (Covariance) & 상관관계 (Correlation)`

**설명**: 두 확률변수가 함께 어떻게 변하는지 측정합니다.

**수식 박스 4**:
- 레이블: `공분산과 상관계수`
- 수식:
```
\text{Cov}(X, Y) = \mathbb{E}[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])]
\qquad
\rho(X,Y) = \frac{\text{Cov}(X,Y)}{\sqrt{\text{Var}(X)\text{Var}(Y)}} \in [-1, 1]
```
- 주석:
  - `Cov > 0`: 함께 증가, `Cov < 0`: 반대로 움직임, `Cov = 0`: 독립 (단, 역은 성립 안 함)
  - `ρ`: 상관계수 (단위 없음, -1~1 범위)

**callout (warn)**:
- 제목: `상관관계 ≠ 인과관계`
- 내용: 아이스크림 판매량과 익사 사고 수는 상관관계가 높지만, 원인은 여름 기온. 논문에서 이 혼동을 조심해야 함.

---

## 인터랙티브: 기대값·분산 계산기

**제목**: `🔢 이산분포 기대값·분산 계산기`

**설명**: 주사위의 기대값과 분산을 직접 계산해보세요. 각 면의 확률을 조정할 수 있습니다.

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `ev-n` | 주사위 면 수 | 2 | 6 | 1 | 6 |
| `ev-bias` | 편향 (1번 면 가중치) | 1 | 10 | 1 | 1 |

**JavaScript 로직**:
```javascript
function updateEV() {
  const n = parseInt(document.getElementById('ev-n').value);
  const bias = parseInt(document.getElementById('ev-bias').value);
  document.getElementById('ev-n-v').value = n;
  document.getElementById('ev-bias-v').value = bias;

  // 확률 계산 (1번 면에 bias 적용)
  const weights = Array.from({length: n}, (_, i) => i === 0 ? bias : 1);
  const total = weights.reduce((a, b) => a + b, 0);
  const probs = weights.map(w => w / total);

  // 기대값
  const ev = probs.reduce((acc, p, i) => acc + p * (i + 1), 0);
  // 분산
  const ev2 = probs.reduce((acc, p, i) => acc + p * (i + 1) ** 2, 0);
  const variance = ev2 - ev ** 2;

  document.getElementById('ev-res').innerHTML =
    `기대값 E[X] = <strong>${ev.toFixed(3)}</strong> | 
     분산 Var(X) = <strong>${variance.toFixed(3)}</strong> | 
     표준편차 σ = <strong>${Math.sqrt(variance).toFixed(3)}</strong>`;
}
updateEV();
```

---

## 퀴즈

**질문**: `Var(X) = E[X²] - (E[X])²`에서 E[X] = 3, E[X²] = 13이면 Var(X)는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 6 | ❌ |
| (b) 10 | ❌ |
| (c) 4 | ✅ (13 - 3² = 13 - 9 = 4) |
| (d) 3 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 6~7)

```
[ ] 기대값 E[X]를 이산형/연속형 각각 설명할 수 있다
[ ] 기대값의 선형성을 설명할 수 있다
[ ] 분산 Var(X)를 두 가지 공식으로 계산할 수 있다
[ ] 표준편차와 분산의 차이를 설명할 수 있다
[ ] 공분산이 무엇을 측정하는지 설명할 수 있다
[ ] 상관관계와 인과관계를 구분할 수 있다
```
