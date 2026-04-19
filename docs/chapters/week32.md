# Week 32 — 정보 기하학·Fisher Information (Ch.32)

> **파일명**: `chapters/ch32.html`  
> **Phase**: Phase 3-B · 딥러닝 이론 심화  
> **인터랙티브**: ✅ Fisher 정보 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.32 · 딥러닝 이론 심화 (Phase 3 — 6주차)` |
| 제목 | `정보 기하학과 Fisher Information` |
| 서브타이틀 | `확률분포 공간의 기하학. Cramér-Rao bound, 자연 그라디언트, KFAC의 수학적 기반.` |
| 이전 챕터 | `ch31.html` — Ch.31 Transformer Attention |
| 다음 챕터 | `ch33.html` — Ch.33 마팅게일·확률 과정 |

---

## EASY BOX 내용

**핵심 비유**: "지도에서 두 도시의 거리를 잴 때 유클리드 거리가 맞다. 하지만 확률분포 공간에서의 '거리'는 다르게 측정해야 한다 — Fisher Information은 이 공간의 곡률을 측정하고, 자연 그라디언트는 이 곡률을 반영한 경사하강법이다."

**예시 리스트**:
- Fisher 정보: "이 데이터에서 파라미터를 얼마나 정확하게 추정할 수 있는가"의 척도
- Cramér-Rao: "어떤 추정기도 Fisher 정보의 역수보다 분산이 작을 수 없다" — 추정 이론의 하한
- AI에서: 자연 그라디언트(Natural Gradient)는 파라미터 공간의 기하학을 반영한 SGD 변형 → KFAC, Shampoo 옵티마이저의 이론적 기반

---

## 섹션 구성

### 섹션 1: Score 함수와 Fisher Information

**아이콘**: `ℐ`  
**제목**: `Fisher Information — 데이터가 파라미터를 얼마나 알려주는가`

**설명**: Fisher Information은 로그 가능도의 그라디언트(Score 함수)의 분산입니다. 이 값이 클수록 데이터가 파라미터에 대한 정보를 더 많이 담고 있습니다.

**수식 박스 1**:
- 레이블: `Score 함수와 Fisher Information`
- 수식:
```
s(\theta; x) = \nabla_\theta \log p(x;\theta) \quad\text{(Score 함수)}
\quad\Rightarrow\quad
\mathcal{I}(\theta) = \mathbb{E}_{p(x;\theta)}\!\left[s(\theta;x)s(\theta;x)^\top\right]
= -\mathbb{E}_{p(x;\theta)}\!\left[\nabla_\theta^2 \log p(x;\theta)\right]
```
- 주석:
  - Score: 로그 가능도의 파라미터에 대한 그라디언트
  - E[Score] = 0 (로그 가능도의 기댓값 그라디언트)
  - Fisher = Score의 공분산 = 로그 가능도의 음의 헤시안의 기댓값
  - 큰 Fisher 정보 → 파라미터를 정밀하게 추정 가능

**표 (분포별 Fisher Information)**:

| 분포 | 파라미터 | Fisher I(θ) |
|------|---------|------------|
| Bernoulli(p) | p | 1/(p(1-p)) |
| Poisson(λ) | λ | 1/λ |
| N(μ, σ²) (μ) | μ | 1/σ² |
| N(μ, σ²) (σ²) | σ² | 1/(2σ⁴) |

---

### 섹션 2: Cramér-Rao 하한

**아이콘**: `≥`  
**제목**: `Cramér-Rao 하한 — 추정 분산의 절대적 하한`

**설명**: 어떤 불편 추정기(unbiased estimator)도 Fisher Information의 역수보다 작은 분산을 가질 수 없습니다. 이것이 추정 이론의 가장 중요한 결과입니다.

**수식 박스 2**:
- 레이블: `Cramér-Rao 하한`
- 수식:
```
\mathrm{Var}[\hat{\theta}] \geq \frac{1}{\mathcal{I}(\theta)} = \frac{1}{n\mathcal{I}_1(\theta)}
\quad\text{(n개 i.i.d. 샘플)}
```
- 주석:
  - `Var[θ̂]`: 추정기의 분산 (작을수록 좋음)
  - `1/I(θ)`: Cramér-Rao 하한 — 달성 가능한 최소 분산
  - n이 클수록 하한이 낮아짐 (더 정밀한 추정 가능)
  - MLE는 점근적으로 이 하한을 달성함 (효율적 추정기)

**수식 박스 3** (MLE의 효율성):
- 레이블: `MLE의 점근적 효율성`
- 수식:
```
\sqrt{n}(\hat{\theta}_{MLE} - \theta^*) \xrightarrow{D} \mathcal{N}(0, \mathcal{I}(\theta^*)^{-1})
```
- 주석:
  - n이 크면 MLE는 Cramér-Rao 하한을 달성하는 최적 추정기
  - 분산 = 1/(n·I(θ)) → Fisher 정보가 클수록 MLE의 분산이 작음
  - 이것이 MLE가 통계학의 표준 방법인 이유

---

### 섹션 3: 자연 그라디언트 (Natural Gradient)

**아이콘**: `∇̃`  
**제목**: `자연 그라디언트 — Fisher Information이 반영된 경사하강법`

**설명**: 일반 그라디언트 하강법은 파라미터 공간이 유클리드라고 가정합니다. 하지만 확률분포 공간에서의 기하학(Riemannian metric)은 Fisher Information으로 측정됩니다. 자연 그라디언트는 이 기하학을 반영합니다.

**수식 박스 4**:
- 레이블: `자연 그라디언트 업데이트`
- 수식:
```
\tilde{\nabla}_\theta L(\theta) = \mathcal{I}(\theta)^{-1} \nabla_\theta L(\theta)
\quad\Rightarrow\quad
\theta_{t+1} = \theta_t - \eta\, \mathcal{I}(\theta_t)^{-1} \nabla_\theta L(\theta_t)
```
- 주석:
  - I(θ)⁻¹: Fisher Information 역행렬 (Riemannian metric의 역)
  - 일반 GD가 직선으로 내려가면, 자연 GD는 분포 공간의 측지선을 따라 내려감
  - Fisher Information이 클수록 해당 방향의 실제 업데이트 크기가 작아짐
  - 실제: I(θ)가 매우 크므로 KFAC 등으로 근사

**callout (info)**:
- 내용: **왜 자연 그라디언트가 좋은가?** 파라미터를 두 배로 늘려도(리파라미터화) 자연 그라디언트는 불변(invariant)이지만, 일반 그라디언트는 달라집니다. 즉, 자연 그라디언트는 "모델 표현에 무관한" 최적화 방향입니다.

---

### 섹션 4: Fisher Information과 KL Divergence

**아이콘**: `KL`  
**제목**: `KL Divergence의 2차 근사 = Fisher Information`

**설명**: KL divergence를 파라미터 공간에서 2차 테일러 근사하면 Fisher Information이 나옵니다. 이것이 정보 기하학의 핵심 연결고리입니다.

**수식 박스 5**:
- 레이블: `KL Divergence의 Fisher Information 근사`
- 수식:
```
\mathrm{KL}(p_\theta \| p_{\theta+\Delta\theta})
\approx \frac{1}{2} \Delta\theta^\top \mathcal{I}(\theta) \Delta\theta + O(\|\Delta\theta\|^3)
```
- 주석:
  - 파라미터가 Δθ만큼 변할 때 분포 간 KL divergence
  - Fisher Information이 KL의 곡률(헤시안)
  - 자연 그라디언트 = "KL 거리를 일정하게 유지하면서 loss를 최대한 줄이는 방향"
  - 이것이 PPO, TRPO 등 RL 알고리즘의 KL 제약과 연결

---

## 인터랙티브: Fisher Information 계산기

**제목**: `🔢 Bernoulli 분포의 Fisher Information — p에 따른 변화`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `fi-p` | 확률 p | 0.01 | 0.99 | 0.01 | 0.5 |
| `fi-n` | 표본 수 n | 1 | 1000 | 1 | 100 |

**JavaScript 로직**:
```javascript
function updateFisher() {
  const p = parseFloat(document.getElementById('fi-p').value);
  const n = parseInt(document.getElementById('fi-n').value);
  document.getElementById('fi-p-v').value = p.toFixed(2);
  document.getElementById('fi-n-v').value = n;

  // Bernoulli: I(p) = 1/(p(1-p))
  const fisher1 = 1 / (p * (1 - p));
  const fisherN = n * fisher1;
  const cramerRao = 1 / fisherN;
  const mleVariance = p * (1 - p) / n; // Var[X̄]

  document.getElementById('fi-res').innerHTML =
    `Bernoulli(p=${p.toFixed(2)}), n=${n}개 표본<br/>
     Fisher I₁(p) = 1/(p(1-p)) = <strong>${fisher1.toFixed(3)}</strong><br/>
     n개 샘플 Fisher I_n(p) = nI₁ = <strong>${fisherN.toFixed(3)}</strong><br/>
     Cramér-Rao 하한 Var[θ̂] ≥ 1/I_n = <strong>${cramerRao.toFixed(6)}</strong><br/>
     MLE 분산 Var[X̄] = p(1-p)/n = <strong>${mleVariance.toFixed(6)}</strong><br/>
     MLE 효율성 확인: ${Math.abs(cramerRao - mleVariance) < 1e-10 ? '✅ MLE가 CR 하한 달성' : `CR 하한 / MLE분산 = ${(cramerRao/mleVariance).toFixed(4)}`}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       p=0.5에서 Fisher 정보가 최소 (가장 불확실) — p→0 or 1에서 최대
     </span>`;
}
updateFisher();
```

---

## 퀴즈

**질문**: Cramér-Rao 하한이 의미하는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 모든 추정기의 분산은 Fisher Information보다 작아야 한다 | ❌ |
| (b) 불편 추정기의 분산은 1/I(θ) 이상이다 — MLE는 이를 점근적으로 달성 | ✅ |
| (c) Fisher 정보가 클수록 추정이 어렵다 | ❌ |
| (d) 추정기는 항상 분산이 1/I(θ)이다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 32)

```
[ ] Fisher Information의 정의(Score의 분산)를 설명할 수 있다
[ ] Cramér-Rao 하한을 수식으로 쓸 수 있다
[ ] MLE가 점근적으로 효율적임을 설명할 수 있다
[ ] 자연 그라디언트 업데이트 공식을 쓸 수 있다
[ ] KL divergence의 2차 근사가 Fisher Information임을 설명할 수 있다
[ ] 논문에서 "natural gradient", "KFAC"를 보면 이 개념과의 연결을 안다
[ ] 자연 그라디언트가 리파라미터화 불변인 이유를 설명할 수 있다
```
