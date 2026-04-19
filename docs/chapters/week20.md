# Week 20 — 수렴 속도 분석 (Ch.20)

> **파일명**: `chapters/ch20.html`  
> **Phase**: Phase 2-B · 볼록 최적화  
> **인터랙티브**: ✅ GD 수렴 속도 비교 시뮬레이터  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.20 · 볼록 최적화 (Phase 2 — 6주차)` |
| 제목 | `수렴 속도 분석` |
| 서브타이틀 | `GD가 얼마나 빠르게 최적해에 도달하는가. 논문의 Theorem "f(θₜ) - f* ≤ C/T"를 이해합니다.` |
| 이전 챕터 | `ch19.html` — Ch.19 Lagrangian·KKT |
| 다음 챕터 | `ch21.html` — Ch.21 σ-대수·확률공간 |

---

## EASY BOX 내용

**핵심 비유**: "산에서 내려올 때 '눈을 감고 발 아래 가장 가파른 방향으로 한 걸음'을 반복하는 게 경사하강법이다. 산 모양(볼록/강볼록)과 걸음 크기(학습률)에 따라 몇 걸음 만에 도착하는지가 달라진다."

**예시 리스트**:
- 볼록 GD: O(1/T) — 1000번 반복 후 오차가 초기의 1/1000
- 강볼록 GD: O(ρᵀ) — 지수 감소 — 훨씬 빠르다
- AI에서: "논문에서 Algorithm 1 converges at rate O(1/√T) for non-convex problems"가 이 분석이다

---

## 섹션 구성

### 섹션 1: 경사하강법의 수렴 조건

**아이콘**: `⛰`  
**제목**: `GD 수렴의 3가지 핵심 가정`

**표 (수렴 분석의 기본 가정)**:

| 가정 | 수식 | 의미 |
|------|------|------|
| L-smooth | ‖∇f(x)-∇f(y)‖ ≤ L‖x-y‖ | 그라디언트가 급변하지 않음 |
| 볼록 | f(λx+(1-λ)y) ≤ λf(x)+(1-λ)f(y) | 국소=전역 최솟값 |
| μ-강볼록 | f(y) ≥ f(x)+∇f(x)ᵀ(y-x)+μ/2‖y-x‖² | 더 강한 볼록성 |

**callout (info)**:
- 내용: 가정이 강할수록 수렴이 빠르다: 비볼록 < 볼록 < 강볼록. 딥러닝은 비볼록이지만 실제로는 잘 작동한다 — 이 격차를 설명하는 것이 최신 이론 연구의 주제다.

---

### 섹션 2: 볼록 함수의 GD 수렴 속도

**아이콘**: `📉`  
**제목**: `볼록 GD — O(1/T) 수렴`

**수식 박스 1**:
- 레이블: `볼록 GD 수렴 정리 (Theorem)`
- 수식:
```
\text{Theorem: 학습률 } \alpha = \frac{1}{L},\ f \text{ convex and L-smooth이면}

f\!\left(\frac{1}{T}\sum_{t=1}^T \theta_t\right) - f^* \leq \frac{L\,\|\theta_0 - \theta^*\|^2}{2T}
```
- 주석:
  - 좌변: T번 반복 후 평균 파라미터의 목적함수 값과 최적값 f*의 차이
  - 우변: C/T 형태 → 반복 횟수 T에 반비례하여 감소
  - 적절한 학습률 α = 1/L이 중요 — 너무 크면 발산, 너무 작으면 느림
  - 오차를 ε 이하로 줄이려면 T = O(1/ε) 번 반복 필요

---

### 섹션 3: 강볼록 함수의 GD 수렴 속도

**아이콘**: `🚀`  
**제목**: `강볼록 GD — O(ρᵀ) 지수 수렴`

**설명**: 함수가 강볼록이면 GD가 지수적으로 빠르게 수렴합니다. "선형 수렴(linear convergence)"이라고도 부릅니다.

**수식 박스 2**:
- 레이블: `강볼록 GD 수렴 정리`
- 수식:
```
\|\theta_T - \theta^*\|^2 \leq \left(1 - \frac{\mu}{L}\right)^T \|\theta_0 - \theta^*\|^2
```
- 주석:
  - `κ = L/μ`: 조건수(condition number) — 클수록 수렴이 느림
  - `ρ = 1 - μ/L = 1 - 1/κ < 1`: 매 반복마다 오차가 ρ배 감소
  - T번 후 오차: 초기 오차 × ρᵀ → 지수적 감소
  - 오차를 ε 이하로 줄이려면 T = O(κ log(1/ε)) 번 반복

**표 (볼록 vs 강볼록 수렴 비교)**:

| 구분 | 수렴 속도 | T번 후 오차 | ε 달성 반복 수 |
|------|---------|-----------|-------------|
| 볼록 GD | O(1/T) | C/T | O(1/ε) |
| 볼록 Nesterov | O(1/T²) | C/T² | O(1/√ε) |
| 강볼록 GD | O(ρᵀ) | ρᵀ × C₀ | O(κ log 1/ε) |
| 강볼록 Nesterov | O(ρ̃ᵀ) | ρ̃ᵀ × C₀ | O(√κ log 1/ε) |

---

### 섹션 4: SGD와 Mini-batch — 확률적 그라디언트

**아이콘**: `🎲`  
**제목**: `SGD — 빠르지만 노이즈 있는 수렴`

**설명**: 딥러닝에서는 전체 데이터를 쓰는 GD 대신 미니배치 SGD를 씁니다. 그라디언트 추정에 노이즈가 생기지만, 각 반복이 훨씬 빠릅니다.

**수식 박스 3**:
- 레이블: `SGD 업데이트와 그라디언트 노이즈`
- 수식:
```
\theta_{t+1} = \theta_t - \alpha_t \tilde{\nabla} f(\theta_t)
\quad\text{where}\quad \mathbb{E}[\tilde{\nabla} f(\theta_t)] = \nabla f(\theta_t)
```
- 주석:
  - `∇̃f(θ)`: 미니배치로 추정한 그라디언트 (노이즈 있음)
  - 불편 추정량: 기대값은 정확한 그라디언트와 같음
  - 학습률 αₜ가 일정하면 수렴 후에도 최솟값 주변을 진동
  - 감소 학습률 Σαₜ = ∞, Σαₜ² < ∞ 조건 → 수렴 보장 (Robbins-Monro)

**수식 박스 4** (SGD 수렴 속도):
- 레이블: `볼록 SGD 수렴`
- 수식:
```
\mathbb{E}\!\left[f\!\left(\bar{\theta}_T\right)\right] - f^* \leq \frac{G^2}{\mu\sqrt{T}}
\quad\text{(G = 그라디언트 노이즈 상한)}
```
- 주석:
  - O(1/√T) — GD O(1/T)보다 느리지만 각 반복이 O(n)배 빠름
  - G²: 그라디언트 분산의 상한 (노이즈가 클수록 수렴 느림)
  - 배치 크기 B를 늘리면 G² 감소 → 수렴 속도 개선 (선형 스케일링 법칙)

---

## 인터랙티브: GD vs SGD 수렴 비교

**제목**: `🔢 수렴 속도 비교 — T번 반복 후 오차`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `cv-T` | 반복 횟수 T | 1 | 500 | 1 | 100 |
| `cv-kappa` | 조건수 κ (강볼록) | 1 | 50 | 1 | 10 |

**JavaScript 로직**:
```javascript
function updateConv() {
  const T = parseInt(document.getElementById('cv-T').value);
  const kappa = parseInt(document.getElementById('cv-kappa').value);
  document.getElementById('cv-T-v').value = T;
  document.getElementById('cv-kappa-v').value = kappa;

  const C = 1.0; // 초기 거리² (정규화)
  const rho = 1 - 1 / kappa;

  const errConvex = (C / T).toExponential(3);        // 볼록 GD: O(1/T)
  const errNes    = (C / (T * T)).toExponential(3);   // Nesterov: O(1/T²)
  const errStrong = (Math.pow(rho, T) * C).toExponential(3); // 강볼록: O(ρᵀ)
  const errSGD    = (C / Math.sqrt(T)).toExponential(3);     // SGD: O(1/√T)

  document.getElementById('cv-res').innerHTML =
    `T = ${T}번 반복, κ = ${kappa} (ρ = ${rho.toFixed(3)})<br/>
     볼록 GD:       O(1/T)  = <strong>${errConvex}</strong><br/>
     Nesterov:      O(1/T²) = <strong>${errNes}</strong><br/>
     강볼록 GD:     O(ρᵀ)  = <strong>${errStrong}</strong><br/>
     볼록 SGD:      O(1/√T) = <strong>${errSGD}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       강볼록 GD는 κ가 작을수록 빠름 (ρ가 0에 가까울수록 빠른 지수 감소)
     </span>`;
}
updateConv();
```

---

## 퀴즈

**질문**: 강볼록(strongly convex) 함수에서 GD의 수렴 속도는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) O(1/T) — 선형 감소 | ❌ |
| (b) O(1/T²) — 2차 감소 | ❌ |
| (c) O(ρᵀ), ρ < 1 — 지수 감소 (선형 수렴) | ✅ |
| (d) O(√T) — 발산 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 20)

```
[ ] L-smooth 조건이 GD 수렴에 왜 필요한지 설명할 수 있다
[ ] 볼록 GD O(1/T) 수렴 정리를 논문 형태로 읽을 수 있다
[ ] 강볼록 GD가 지수 수렴함을 설명할 수 있다
[ ] 조건수 κ = L/μ이 크면 수렴이 느린 이유를 설명할 수 있다
[ ] SGD 수렴이 GD보다 느린데 왜 쓰는지 설명할 수 있다
[ ] 논문에서 "Algorithm 1 achieves ε-accuracy in O(1/ε) iterations"를 이해한다
[ ] Nesterov 가속이 O(1/T)를 O(1/T²)로 개선함을 안다
```
