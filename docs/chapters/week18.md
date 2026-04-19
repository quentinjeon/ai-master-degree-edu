# Week 18 — 볼록 집합·볼록 함수 (Ch.18)

> **파일명**: `chapters/ch18.html`  
> **Phase**: Phase 2-B · 볼록 최적화  
> **인터랙티브**: ✅ Jensen 부등식 시각화  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.18 · 볼록 최적화 (Phase 2 — 4주차)` |
| 제목 | `볼록 집합과 볼록 함수` |
| 서브타이틀 | `"오목한 그릇"에 공을 굴리면 반드시 바닥에 도달한다. 볼록 함수의 최솟값은 항상 유일하고 보장됩니다.` |
| 이전 챕터 | `ch17.html` — Ch.17 적분과 급수 |
| 다음 챕터 | `ch19.html` — Ch.19 Lagrangian·KKT 조건 |

---

## EASY BOX 내용

**핵심 비유**: "'오목한 그릇'에 구슬을 굴리면 어느 방향에서 출발하든 항상 가운데 바닥으로 굴러온다. 이 그릇 모양의 함수가 볼록 함수다 — 경사하강법이 반드시 최솟값에 도달하는 이유."

**예시 리스트**:
- 볼록: x² (아래로 오목한 포물선) — 어느 두 점을 이어도 그 선분이 함수 위에 있다
- 비볼록: sin(x) — 여러 골짜기가 있어서 경사하강법이 어떤 골짜기에 갇히는지 알 수 없다
- AI에서: Logistic regression의 cross-entropy loss는 볼록 → 경사하강법이 최적해 보장. 딥러닝은 비볼록 → 보장 없음 (하지만 실제로는 잘 된다)

---

## 섹션 구성

### 섹션 1: 볼록 집합 (Convex Set)

**아이콘**: `⬡`  
**제목**: `볼록 집합 — 두 점을 이은 선분이 항상 집합 안에`

**설명**: 집합 C가 볼록이라는 것은 집합 내 임의의 두 점을 이은 선분이 항상 집합 안에 있다는 뜻입니다. 최적화 문제에서 파라미터 공간(제약 집합)이 볼록이면 분석이 훨씬 쉬워집니다.

**수식 박스 1**:
- 레이블: `볼록 집합의 정의`
- 수식:
```
C \subseteq \mathbb{R}^n \text{ is convex}
\iff
\forall x, y \in C,\ \forall \lambda \in [0,1]:\ \lambda x + (1-\lambda)y \in C
```
- 주석:
  - `λx + (1-λ)y`: x와 y의 볼록 결합 (convex combination) — 두 점을 잇는 선분의 점
  - λ=0이면 y, λ=1이면 x, λ=0.5이면 중간점
  - 볼록 집합 예: ℝⁿ 전체, 공, 정육면체, 반공간 {x: aᵀx ≤ b}
  - 비볼록 집합 예: 도넛 모양, 두 개의 분리된 구

**표 (볼록 vs 비볼록 집합)**:

| 집합 | 볼록 여부 | 이유 |
|------|----------|------|
| ℝⁿ 전체 | ✅ | 항상 선분이 내부에 |
| 공(ball) | ✅ | 원 안의 두 점을 이은 선분은 원 안에 |
| 정육면체 | ✅ | 직사각형·다면체 |
| 도넛(torus) | ❌ | 중심 구멍을 가로지르는 선분이 밖으로 나감 |
| 정수 격자점 | ❌ | 두 정수를 이은 선분은 실수를 포함 |

---

### 섹션 2: 볼록 함수 (Convex Function)

**아이콘**: `∪`  
**제목**: `볼록 함수 — 어느 두 점을 이어도 함수 위에 있다`

**설명**: 함수 f가 볼록이라는 것은 그래프 위 임의의 두 점을 이은 선분이 항상 함수 그래프 위에(또는 위에) 있다는 뜻입니다. 볼록 함수는 **국소 최솟값 = 전역 최솟값**이라는 황금 성질을 갖습니다.

**수식 박스 2**:
- 레이블: `볼록 함수의 정의`
- 수식:
```
f \text{ is convex}
\iff
f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda) f(y),\quad \forall x,y,\ \lambda \in [0,1]
```
- 주석:
  - 좌변: 중간점의 함수값
  - 우변: 두 함수값의 볼록 결합 (선분의 높이)
  - "함수가 선분 아래에 있다" = 볼록
  - 엄격 볼록 (strictly convex): 등호 제외 → 최솟값이 유일

**개념 카드**:
- 제목: `🔑 볼록 함수의 황금 성질`
- 내용:
  - **국소 최솟값 = 전역 최솟값**: 어디서 내려가도 같은 바닥에 도달
  - **경사하강법 수렴 보장**: 볼록 함수에서 GD는 반드시 최솟값으로 수렴
  - **2계 조건**: f가 미분가능하면 Hessian H ≽ 0 (양반정치) ⟺ 볼록

**수식 박스 3** (2계 조건):
- 레이블: `볼록 함수의 2계 조건 (미분가능한 경우)`
- 수식:
```
f \text{ is convex} \iff \nabla^2 f(x) \succeq 0\quad \forall x
\quad\text{(Hessian이 양반정치)}
```
- 주석:
  - `∇²f(x) ⪰ 0`: Hessian의 모든 고유값 ≥ 0
  - f(x) = x²이면 f''(x) = 2 > 0 → 볼록
  - f(x) = -x²이면 f''(x) = -2 < 0 → 오목(concave)
  - 딥러닝 loss landscape는 대부분 비볼록 (음의 고유값 존재)

---

### 섹션 3: Jensen's Inequality

**아이콘**: `J`  
**제목**: `Jensen 부등식 — 정보이론과 VAE의 수학적 기반`

**설명**: Jensen 부등식은 볼록 함수의 핵심 성질입니다. 볼록 함수에 기대값을 적용할 때 부등식 방향이 결정됩니다. 정보이론의 KL divergence ≥ 0 증명, VAE의 ELBO 유도 등에 핵심으로 등장합니다.

**수식 박스 4**:
- 레이블: `Jensen 부등식`
- 수식:
```
f \text{ convex} \Rightarrow f\!\left(\mathbb{E}[X]\right) \leq \mathbb{E}[f(X)]
```
- 주석:
  - 볼록 함수에서: 기대값의 함수 ≤ 함수의 기대값
  - 오목 함수에서: 부등호 반전 (log는 오목이므로 E[log X] ≤ log E[X])
  - 직관: 평균점의 함수값이 함수값들의 평균보다 작다 (볼록이면)

**수식 박스 5** (KL divergence ≥ 0 증명):
- 레이블: `KL Divergence ≥ 0 증명 (Jensen 활용)`
- 수식:
```
\mathrm{KL}(p \| q) = \mathbb{E}_p\!\left[\log \frac{p(x)}{q(x)}\right]
= -\mathbb{E}_p\!\left[\log \frac{q(x)}{p(x)}\right]
\geq -\log \mathbb{E}_p\!\left[\frac{q(x)}{p(x)}\right] = -\log 1 = 0
```
- 주석:
  - log는 오목 → Jensen 부등식 적용하면 E[log X] ≤ log E[X]
  - 따라서 -E[log(q/p)] ≥ -log E[q/p] = -log 1 = 0
  - 결론: KL(p‖q) ≥ 0, 등호는 p = q일 때만 성립
  - 이 증명이 VAE, 정보이론, EM 알고리즘의 기반

---

## 인터랙티브: Jensen 부등식 시각화

**제목**: `🔢 Jensen 부등식 — f(E[X]) vs E[f(X)]`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `jen-x1` | 첫 번째 점 x₁ | -3 | 3 | 0.1 | -1.0 |
| `jen-x2` | 두 번째 점 x₂ | -3 | 3 | 0.1 | 2.0 |
| `jen-lam` | 가중치 λ | 0 | 1 | 0.05 | 0.5 |

**JavaScript 로직**:
```javascript
function updateJensen() {
  const x1 = parseFloat(document.getElementById('jen-x1').value);
  const x2 = parseFloat(document.getElementById('jen-x2').value);
  const lam = parseFloat(document.getElementById('jen-lam').value);
  document.getElementById('jen-x1-v').value = x1.toFixed(1);
  document.getElementById('jen-x2-v').value = x2.toFixed(1);
  document.getElementById('jen-lam-v').value = lam.toFixed(2);

  // f(x) = x² (볼록)
  const f = x => x * x;
  const xMean = lam * x1 + (1 - lam) * x2;
  const lhs = f(xMean);          // f(E[X])
  const rhs = lam * f(x1) + (1 - lam) * f(x2); // E[f(X)]

  document.getElementById('jen-res').innerHTML =
    `f(x) = x² (볼록 함수)<br/>
     λx₁ + (1-λ)x₂ = ${lam.toFixed(2)}×${x1.toFixed(1)} + ${(1-lam).toFixed(2)}×${x2.toFixed(1)} = <strong>${xMean.toFixed(3)}</strong><br/>
     f(E[X]) = f(${xMean.toFixed(3)}) = <strong>${lhs.toFixed(4)}</strong><br/>
     E[f(X)] = λf(x₁) + (1-λ)f(x₂) = <strong>${rhs.toFixed(4)}</strong><br/>
     Jensen: f(E[X]) ${lhs <= rhs + 1e-9 ? '≤' : '>'} E[f(X)] ${lhs <= rhs + 1e-9 ? '✅' : '❌ (오차)'}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       볼록 함수에서 항상 f(평균) ≤ (함수값들의 평균)
     </span>`;
}
updateJensen();
```

---

## 퀴즈

**질문**: 딥러닝 loss function이 비볼록(non-convex)일 때 경사하강법은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 전역 최솟값에 반드시 수렴한다 | ❌ |
| (b) 발산한다 | ❌ |
| (c) 국소 최솟값이나 안장점에 수렴할 수 있다 (전역 보장 없음) | ✅ |
| (d) 수렴하지 않는다 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 18)

```
[ ] 볼록 집합의 정의를 보고 그림으로 설명할 수 있다
[ ] 볼록 함수의 정의를 수식으로 쓸 수 있다
[ ] Hessian이 양반정치이면 볼록임을 설명할 수 있다
[ ] 볼록 함수에서 국소 최솟값 = 전역 최솟값임을 설명할 수 있다
[ ] Jensen 부등식을 볼록/오목에 대해 각각 쓸 수 있다
[ ] KL divergence ≥ 0의 증명 흐름을 따라갈 수 있다
[ ] Logistic regression이 왜 전역 최적해를 보장하는지 설명할 수 있다
```
