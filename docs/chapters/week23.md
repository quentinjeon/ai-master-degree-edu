# Week 23 — 기대값의 측도론적 정의 (Ch.23)

> **파일명**: `chapters/ch23.html`  
> **Phase**: Phase 2-C · 측도 기반 확률론  
> **인터랙티브**: ✅ Fubini 정리 이중적분 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.23 · 측도 확률론 (Phase 2 — 9주차)` |
| 제목 | `기대값의 측도론적 정의` |
| 서브타이틀 | `Lebesgue 적분, Fubini 정리. 연속·이산 확률변수를 통합하는 기대값의 일반 이론.` |
| 이전 챕터 | `ch22.html` — Ch.22 확률변수의 수렴 |
| 다음 챕터 | `ch24.html` — Ch.24 고유값 분해·스펙트럼 이론 |

---

## EASY BOX 내용

**핵심 비유**: "Riemann 적분은 x축을 잘게 쪼개어 넓이를 구하지만, Lebesgue 적분은 y축(함수값)을 잘게 쪼개어 넓이를 구한다. 불연속이 많아도 Lebesgue가 잘 작동하는 이유."

**예시 리스트**:
- 이산 기대값: E[X] = Σxᵢ · P(X=xᵢ) — 주사위 기대값 3.5
- 연속 기대값: E[X] = ∫x · f(x)dx — 정규분포 기대값 μ
- AI에서: E[ℒ(θ,ω)] = ∫ℒ(θ,ω)dP(ω) — 기대 손실은 Lebesgue 적분으로 통합 정의

---

## 섹션 구성

### 섹션 1: Lebesgue 적분 — 기대값의 기반

**아이콘**: `∫`  
**제목**: `Lebesgue 적분 — 이산과 연속을 통합하는 적분`

**설명**: 기대값을 통합적으로 정의하려면 이산 합(Σ)과 연속 적분(∫)을 하나로 묶어야 합니다. 이것이 Lebesgue 적분의 역할입니다.

**수식 박스 1**:
- 레이블: `기대값의 통합 정의 (Lebesgue 적분)`
- 수식:
```
\mathbb{E}[X] = \int_\Omega X(\omega)\, dP(\omega)
```
- 주석:
  - `Ω`: 표본 공간
  - `dP(ω)`: 확률 측도 P에 대한 적분 (일반화된 "넓이 원소")
  - 이산 확률변수: `∫ X dP = Σᵢ xᵢ P(X=xᵢ)` — 합과 동치
  - 연속 확률변수: `∫ X dP = ∫ x f(x) dx` — Riemann 적분과 동치
  - 이 표기로 이산/연속 구분 없이 E[X]를 논할 수 있다

**표 (Riemann vs Lebesgue 적분 비교)**:

| 구분 | Riemann | Lebesgue |
|------|---------|---------|
| 분할 방향 | x축 (정의역) | y축 (치역) |
| 적용 범위 | 연속 함수 중심 | 측정 가능 함수 전반 |
| 불연속 처리 | 제한적 | 가산 불연속 허용 |
| 확률에서 | ∫f(x)dx (PDF) | ∫XdP (기대값 일반화) |

---

### 섹션 2: 기대값의 핵심 성질

**아이콘**: `E`  
**제목**: `기대값의 성질 — 측도론적 증명`

**수식 박스 2**:
- 레이블: `기대값의 주요 성질`
- 수식:
```
\begin{aligned}
&(1)\quad \mathbb{E}[aX + bY] = a\mathbb{E}[X] + b\mathbb{E}[Y] \quad\text{(선형성)} \\
&(2)\quad X \geq 0 \Rightarrow \mathbb{E}[X] \geq 0 \quad\text{(단조성)} \\
&(3)\quad \mathbb{E}[g(X)] = \int g(x) f_X(x)\, dx \quad\text{(LOTUS)} \\
&(4)\quad X, Y \text{ 독립} \Rightarrow \mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]
\end{aligned}
```
- 주석:
  - LOTUS: Law of the Unconscious Statistician — g(X)의 기대값을 X의 분포로만 계산
  - (4)의 역은 성립 안 함: E[XY]=E[X]E[Y] ≢ 독립

---

### 섹션 3: Fubini 정리 — 이중 기대값의 순서 교환

**아이콘**: `⇄`  
**제목**: `Fubini 정리 — E_X[E_Y[f(X,Y)]] = E_Y[E_X[f(X,Y)]]`

**설명**: 두 변수에 대한 기대값을 계산할 때, 어떤 변수를 먼저 적분해도 같은 결과를 줍니다. 이것이 Fubini 정리입니다. 복잡한 분포에서 기대값을 계산할 때 순서를 편의에 따라 바꿀 수 있습니다.

**수식 박스 3**:
- 레이블: `Fubini 정리 (이중 기대값 순서 교환)`
- 수식:
```
\mathbb{E}_{X,Y}[f(X,Y)]
= \mathbb{E}_X\!\left[\mathbb{E}_Y[f(X,Y) \mid X]\right]
= \mathbb{E}_Y\!\left[\mathbb{E}_X[f(X,Y) \mid Y]\right]
```
- 주석:
  - f ≥ 0이거나 E[|f|] < ∞일 때 성립
  - 반복 기대값 법칙(Tower property): E[X] = E[E[X|Y]]
  - AI 응용: E_x[E_y[ℒ(x,y)]] = E_(x,y)[ℒ(x,y)] — 배치 손실 계산

**수식 박스 4** (전체 기대값 법칙):
- 레이블: `전체 기대값 법칙 (Law of Total Expectation)`
- 수식:
```
\mathbb{E}[X] = \mathbb{E}_Y\!\left[\mathbb{E}[X \mid Y]\right]
= \sum_y P(Y=y)\, \mathbb{E}[X \mid Y=y]
```
- 주석:
  - "Y로 조건화하면 E[X]를 분해할 수 있다"
  - 동치: E[X] = E[E[X|Y]]
  - AI에서: 기대 손실 = Σ (클래스 확률) × (각 클래스에서의 손실)

---

### 섹션 4: 측도 변환 — 확률분포 변환 정리

**아이콘**: `🔄`  
**제목**: `Radon-Nikodym 도함수 — 확률분포 간 변환`

**설명**: 두 확률 측도 P와 Q가 같은 사건에 대해 정의될 때, Q를 P의 언어로 표현할 수 있습니다. 이것이 Radon-Nikodym 도함수이며, 중요도 샘플링(importance sampling)의 수학적 기반입니다.

**수식 박스 5**:
- 레이블: `Radon-Nikodym 도함수 (중요도 비율)`
- 수식:
```
\mathbb{E}_Q[f(X)] = \mathbb{E}_P\!\left[f(X) \cdot \frac{dQ}{dP}(X)\right]
```
- 주석:
  - `dQ/dP(x)`: Radon-Nikodym 도함수 (Q에서의 밀도 / P에서의 밀도)
  - 중요도 샘플링: `E_q[f] ≈ (1/n)Σᵢ f(xᵢ) · [q(xᵢ)/p(xᵢ)]`, xᵢ ~ p
  - KL divergence: `KL(Q‖P) = E_Q[log(dQ/dP)]`
  - RL의 PPO: 이전 정책 π_old 대비 새 정책 π의 Radon-Nikodym 비율 사용

---

## 인터랙티브: 이중 기대값 계산기

**제목**: `🔢 Fubini — E_X[E_Y[XY]] vs E_Y[E_X[XY]] 비교`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `fub-ex` | E[X] | 0 | 5 | 0.5 | 2.0 |
| `fub-ey` | E[Y] | 0 | 5 | 0.5 | 3.0 |

**JavaScript 로직**:
```javascript
function updateFubini() {
  const ex = parseFloat(document.getElementById('fub-ex').value);
  const ey = parseFloat(document.getElementById('fub-ey').value);
  document.getElementById('fub-ex-v').value = ex.toFixed(1);
  document.getElementById('fub-ey-v').value = ey.toFixed(1);

  // X,Y 독립이라면 E[XY] = E[X]E[Y]
  const exy = ex * ey;
  // E_X[E_Y[XY|X]] = E_X[X · E[Y]] = E[Y] · E[X]
  const way1 = `E_X[E_Y[XY|X]] = E_X[X · ${ey.toFixed(1)}] = ${ey.toFixed(1)} × E[X] = ${ey.toFixed(1)} × ${ex.toFixed(1)} = ${exy.toFixed(3)}`;
  // E_Y[E_X[XY|Y]] = E_Y[Y · E[X]] = E[X] · E[Y]
  const way2 = `E_Y[E_X[XY|Y]] = E_Y[${ex.toFixed(1)} · Y] = ${ex.toFixed(1)} × E[Y] = ${ex.toFixed(1)} × ${ey.toFixed(1)} = ${exy.toFixed(3)}`;

  document.getElementById('fub-res').innerHTML =
    `X, Y 독립, E[X]=${ex.toFixed(1)}, E[Y]=${ey.toFixed(1)}<br/>
     방법 1: ${way1}<br/>
     방법 2: ${way2}<br/>
     두 방법 모두 E[XY] = <strong>${exy.toFixed(3)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       Fubini: 순서를 바꿔도 같은 결과
     </span>`;
}
updateFubini();
```

---

## 퀴즈

**질문**: Fubini 정리가 E_X[E_Y[f(X,Y)]] = E_Y[E_X[f(X,Y)]]를 보장하는 핵심 조건은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) X와 Y가 독립일 때만 성립 | ❌ |
| (b) f ≥ 0이거나 E[|f(X,Y)|] < ∞일 때 성립 | ✅ |
| (c) f가 연속 함수일 때만 성립 | ❌ |
| (d) 항상 성립한다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 23)

```
[ ] Lebesgue 적분이 Riemann 적분과 다른 점을 설명할 수 있다
[ ] 기대값의 통합 정의 E[X] = ∫XdP를 설명할 수 있다
[ ] LOTUS를 사용해 E[g(X)]를 계산할 수 있다
[ ] Fubini 정리를 언제 쓰는지 설명할 수 있다
[ ] 전체 기대값 법칙 E[X] = E[E[X|Y]]를 증명 없이 설명할 수 있다
[ ] 중요도 샘플링이 Radon-Nikodym과 어떻게 연결되는지 안다
```
