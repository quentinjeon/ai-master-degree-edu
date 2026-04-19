# Week 19 — Lagrangian·KKT 조건 (Ch.19)

> **파일명**: `chapters/ch19.html`  
> **Phase**: Phase 2-B · 볼록 최적화  
> **인터랙티브**: ✅ 제약 최적화 시뮬레이터  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.19 · 볼록 최적화 (Phase 2 — 5주차)` |
| 제목 | `Lagrangian과 KKT 조건` |
| 서브타이틀 | `제약이 있는 최적화 문제. SVM, 정규화의 수학적 기반인 KKT 조건을 이해합니다.` |
| 이전 챕터 | `ch18.html` — Ch.18 볼록 집합·볼록 함수 |
| 다음 챕터 | `ch20.html` — Ch.20 수렴 속도 분석 |

---

## EASY BOX 내용

**핵심 비유**: "다이어트를 하면서 맛있는 음식을 최대한 먹고 싶다 — '하루 칼로리 2000kcal 이하'라는 제약이 있다. 칼로리 제약을 위반한 음식은 벌칙을 줘서 목적함수에 추가하는 것이 Lagrangian이다."

**예시 리스트**:
- 제약 없는 최적화: min f(x) — 경사하강법이면 충분
- 등호 제약: min f(x) s.t. g(x) = 0 → 제약 곡선 위에서 최솟값
- 부등호 제약: min f(x) s.t. h(x) ≤ 0 → 제약 영역 안에서 최솟값
- AI에서: SVM은 마진 최대화 문제로 KKT 조건으로 풀린다. L2 정규화 = Gaussian prior 가정의 KKT 형태

---

## 섹션 구성

### 섹션 1: 제약 최적화 문제의 형식

**아이콘**: `🔒`  
**제목**: `제약 최적화의 표준 형식`

**수식 박스 1**:
- 레이블: `표준 최적화 문제 (Standard Form)`
- 수식:
```
\min_{x \in \mathbb{R}^n}\; f(x)
\quad \text{s.t.} \quad
\begin{cases}
g_i(x) = 0, & i = 1, \ldots, m \quad \text{(등호 제약)} \\
h_j(x) \leq 0, & j = 1, \ldots, p \quad \text{(부등호 제약)}
\end{cases}
```
- 주석:
  - `f(x)`: 목적함수 (최소화 대상)
  - `gᵢ(x) = 0`: 반드시 만족해야 하는 등호 조건
  - `hⱼ(x) ≤ 0`: 반드시 만족해야 하는 부등호 조건
  - 가능 집합(feasible set): 모든 제약을 만족하는 x의 집합

**표 (AI 논문의 제약 최적화 예시)**:

| 문제 | 목적함수 | 제약 |
|------|---------|------|
| SVM | ½‖w‖² 최소화 | yᵢ(wᵀxᵢ + b) ≥ 1 |
| 확률 심플렉스 | Entropy 최대화 | Σpᵢ = 1, pᵢ ≥ 0 |
| Sparse coding | ‖Ax - b‖² 최소화 | ‖x‖₁ ≤ t |
| Policy gradient | Return 최대화 | KL(π‖π_old) ≤ δ |

---

### 섹션 2: Lagrangian 함수

**아이콘**: `ℒ`  
**제목**: `Lagrangian — 제약을 벌칙으로 목적함수에 포함`

**설명**: 제약 조건을 목적함수에 "벌칙항"으로 추가해 제약 없는 문제로 변환합니다. Lagrange 승수(λ, μ)는 제약 위반에 대한 "가격"입니다.

**수식 박스 2**:
- 레이블: `Lagrangian 함수`
- 수식:
```
\mathcal{L}(x, \lambda, \mu) = f(x) + \sum_{i=1}^{m} \lambda_i g_i(x) + \sum_{j=1}^{p} \mu_j h_j(x)
```
- 주석:
  - `λᵢ`: 등호 제약 gᵢ(x) = 0에 대한 Lagrange 승수 (부호 제한 없음)
  - `μⱼ ≥ 0`: 부등호 제약 hⱼ(x) ≤ 0에 대한 Lagrange 승수 (반드시 ≥ 0)
  - Lagrange 쌍대 함수: `d(λ,μ) = min_x ℒ(x,λ,μ)`
  - 쌍대 문제: `max_{λ,μ≥0} d(λ,μ)` → 항상 원래 문제보다 작거나 같음(약한 쌍대성)

---

### 섹션 3: KKT 조건 (Karush-Kuhn-Tucker)

**아이콘**: `✓`  
**제목**: `KKT 조건 — 최적해의 필요 조건`

**설명**: 볼록 최적화에서 KKT 조건은 최적해의 필요충분 조건입니다. 이 조건을 만족하는 점이 최적해입니다. SVM, 많은 ML 알고리즘의 해석적 풀이가 여기서 나옵니다.

**수식 박스 3**:
- 레이블: `KKT 최적 조건 (4가지)`
- 수식:
```
\text{KKT Conditions:}
\begin{cases}
(1)\ \nabla_x \mathcal{L}(x^*, \lambda^*, \mu^*) = 0 & \text{(정류 조건, Stationarity)} \\
(2)\ g_i(x^*) = 0,\ h_j(x^*) \leq 0 & \text{(가능성, Primal Feasibility)} \\
(3)\ \mu_j^* \geq 0 & \text{(쌍대 가능성, Dual Feasibility)} \\
(4)\ \mu_j^* h_j(x^*) = 0 & \text{(상보 여유, Complementary Slackness)}
\end{cases}
```
- 주석:
  - (1) 그라디언트가 0: 최적점에서 목적함수와 제약의 그라디언트가 균형
  - (2) 제약 만족: 원래 제약 조건이 성립
  - (3) μ ≥ 0: 부등호 제약의 Lagrange 승수는 양수
  - (4) 상보성: μⱼ > 0이면 hⱼ = 0 (제약이 활성), hⱼ < 0이면 μⱼ = 0 (비활성)

**callout (info)**:
- 내용: **상보 여유 조건**이 SVM의 서포트 벡터를 정의한다. μⱼ > 0인 데이터 포인트(마진에 정확히 닿은 점)만이 결정 경계에 영향을 준다 → 이것이 "Support Vector"다.

---

### 섹션 4: L2 정규화와 KKT

**아이콘**: `🔗`  
**제목**: `L2 정규화 = 제약 최적화의 KKT 형태`

**설명**: 딥러닝에서 L2 정규화(weight decay)는 실제로 "모델 크기 제약이 있는 최적화"의 KKT 형태와 동치입니다.

**수식 박스 4**:
- 레이블: `L2 정규화의 두 가지 표현`
- 수식:
```
\underbrace{\min_\theta \mathcal{L}(\theta) + \frac{\lambda}{2}\|\theta\|^2}_{\text{정규화 버전 (Lagrangian)}}
\quad \Longleftrightarrow \quad
\underbrace{\min_\theta \mathcal{L}(\theta) \quad \text{s.t.} \quad \|\theta\|^2 \leq t}_{\text{제약 버전}}
```
- 주석:
  - λ와 t는 하나가 결정되면 다른 하나가 결정된다 (KKT 상보 조건)
  - L2 정규화 = Gaussian prior 가정의 MAP = 놈 제약 최적화의 KKT 형태
  - 이 세 가지 해석이 같은 해를 준다는 것이 볼록 최적화의 강력함

---

## 인터랙티브: 2차원 제약 최적화 시뮬레이터

**제목**: `🔢 KKT 조건 체험 — 제약 위반 벌칙 조절`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `kkt-mu` | Lagrange 승수 μ | 0 | 5 | 0.1 | 1.0 |
| `kkt-x` | 현재 x값 | -3 | 3 | 0.1 | 1.5 |

**JavaScript 로직**:
```javascript
function updateKKT() {
  const mu = parseFloat(document.getElementById('kkt-mu').value);
  const x = parseFloat(document.getElementById('kkt-x').value);
  document.getElementById('kkt-mu-v').value = mu.toFixed(1);
  document.getElementById('kkt-x-v').value = x.toFixed(1);

  // min f(x) = x² s.t. h(x) = x-1 ≤ 0  (x ≤ 1 제약)
  const f = x * x;
  const h = x - 1; // 제약: h(x) ≤ 0
  const lagrangian = f + mu * h;
  const slack = h <= 0 ? '✅ 제약 만족' : '❌ 제약 위반';
  const compSlack = Math.abs(mu * h) < 0.01 ? '✅ 상보성 만족' : `⚠ μ·h = ${(mu*h).toFixed(3)} ≠ 0`;

  document.getElementById('kkt-res').innerHTML =
    `f(x) = x² = <strong>${f.toFixed(3)}</strong><br/>
     h(x) = x-1 = ${h.toFixed(3)} ${slack}<br/>
     Lagrangian = f + μh = ${f.toFixed(3)} + ${mu.toFixed(1)}×${h.toFixed(3)} = <strong>${lagrangian.toFixed(3)}</strong><br/>
     KKT 상보 여유: μ×h(x) = ${(mu*h).toFixed(3)} ${compSlack}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       최적해: x*=1 (제약 경계), μ*=2 (정류 조건: 2x* - μ* = 0)
     </span>`;
}
updateKKT();
```

---

## 퀴즈

**질문**: KKT 상보 여유 조건 μⱼ · hⱼ(x*) = 0이 의미하는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Lagrange 승수가 항상 0이다 | ❌ |
| (b) 부등호 제약이 항상 등호로 성립한다 | ❌ |
| (c) 활성 제약(hⱼ=0)만 Lagrange 승수(μⱼ>0)를 가진다 | ✅ |
| (d) 모든 제약이 위반된다 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 19)

```
[ ] 등호 제약과 부등호 제약의 차이를 설명할 수 있다
[ ] Lagrangian 함수를 주어진 문제에서 직접 쓸 수 있다
[ ] KKT 4가지 조건을 나열하고 각각 설명할 수 있다
[ ] 상보 여유 조건이 SVM의 서포트 벡터와 어떻게 연결되는지 안다
[ ] L2 정규화가 제약 최적화와 동치임을 설명할 수 있다
[ ] 논문에서 "KKT conditions"를 보면 어떤 맥락인지 안다
```
