# Week 24 — 고유값 분해·스펙트럼 이론 (Ch.24)

> **파일명**: `chapters/ch24.html`  
> **Phase**: Phase 2-D · 선형대수 심화  
> **인터랙티브**: ✅ Rayleigh quotient 슬라이더  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.24 · 선형대수 심화 (Phase 2 — 10주차)` |
| 제목 | `고유값 분해와 스펙트럼 이론` |
| 서브타이틀 | `행렬의 "방향과 크기"를 분해합니다. PCA, 공분산 분석, Attention의 수학적 기반.` |
| 이전 챕터 | `ch23.html` — Ch.23 기대값의 측도론적 정의 |
| 다음 챕터 | `ch25.html` — Ch.25 SVD 완전 유도 |

---

## EASY BOX 내용

**핵심 비유**: "행렬에 벡터를 곱하면 벡터가 회전·늘어난다. 어떤 특별한 방향(고유벡터)은 회전 없이 그냥 늘어나기만 한다 — 늘어나는 배율이 고유값이다."

**예시 리스트**:
- 공분산 행렬의 고유벡터 = 데이터가 가장 많이 퍼진 방향 → PCA의 주성분
- 큰 고유값 = 그 방향으로 데이터 분산이 크다 = 중요한 정보 방향
- AI에서: Attention 행렬의 스펙트럼이 학습 동역학을 결정 / LoRA가 낮은 랭크 고유값 분해를 활용

---

## 섹션 구성

### 섹션 1: 고유값과 고유벡터 정의

**아이콘**: `λ`  
**제목**: `고유값 분해 (Eigendecomposition)`

**수식 박스 1**:
- 레이블: `고유값·고유벡터 정의`
- 수식:
```
A \mathbf{v} = \lambda \mathbf{v},\quad \mathbf{v} \neq \mathbf{0}
```
- 주석:
  - `λ`: 고유값 (eigenvalue) — 스칼라
  - `v`: 고유벡터 (eigenvector) — 행렬 A에 의해 방향이 변하지 않는 벡터
  - 방향은 그대로, 크기만 λ배 변한다 (λ < 0이면 방향 반전)
  - 찾는 법: det(A - λI) = 0 풀기 → 특성 다항식

**수식 박스 2** (대각화):
- 레이블: `고유값 분해 (Eigendecomposition)`
- 수식:
```
A = V \Lambda V^{-1}
\quad\text{where}\quad
\Lambda = \mathrm{diag}(\lambda_1, \ldots, \lambda_n),\quad V = [\mathbf{v}_1 \mid \cdots \mid \mathbf{v}_n]
```
- 주석:
  - V의 열이 고유벡터, Λ의 대각 원소가 고유값
  - 대각화 가능 조건: n개의 선형 독립 고유벡터 존재
  - Aᵏ = V Λᵏ V⁻¹ — 행렬 거듭제곱을 고유값으로 쉽게 계산

---

### 섹션 2: 대칭 행렬과 스펙트럼 정리

**아이콘**: `S`  
**제목**: `대칭 행렬의 스펙트럼 정리 — PCA의 수학적 기반`

**설명**: 공분산 행렬처럼 대칭인 경우(A = Aᵀ) 훨씬 강력한 성질이 있습니다. 직교 분해가 보장됩니다.

**수식 박스 3**:
- 레이블: `스펙트럼 정리 (Spectral Theorem)`
- 수식:
```
A = A^\top \Rightarrow A = Q \Lambda Q^\top
\quad (Q^\top Q = I,\ \Lambda = \mathrm{diag}(\lambda_1, \ldots, \lambda_n))
```
- 주석:
  - Q의 열들이 서로 직교하는 정규화된 고유벡터 (정규직교 기저)
  - 모든 고유값이 실수 (λᵢ ∈ ℝ)
  - Q⁻¹ = Qᵀ → 역행렬이 필요 없음 (계산 편의)
  - 공분산 행렬 Σ는 항상 대칭 → 스펙트럼 정리 적용

---

### 섹션 3: 양정치 행렬 (PSD)

**아이콘**: `≽`  
**제목**: `양반정치(PSD) 행렬 — 볼록 함수, 공분산, Hessian`

**수식 박스 4**:
- 레이블: `양반정치 행렬 (Positive Semi-Definite)`
- 수식:
```
A \succeq 0 \iff \mathbf{x}^\top A \mathbf{x} \geq 0,\quad \forall \mathbf{x} \in \mathbb{R}^n
\iff \text{모든 고유값 } \lambda_i \geq 0
```
- 주석:
  - A ≻ 0 (양정치): 모든 고유값 > 0 → 역행렬 존재
  - A ≽ 0 (양반정치): 모든 고유값 ≥ 0 → 공분산 행렬의 조건
  - Hessian ≽ 0 ⟺ 볼록 함수 (Ch.18의 2계 조건)
  - 논문에서: "Σ ≽ 0" = Σ는 양반정치 = 공분산 행렬

**표 (행렬 종류와 고유값)**:

| 종류 | 표기 | 고유값 조건 | 예시 |
|------|------|-----------|------|
| 양정치 | A ≻ 0 | 모든 λ > 0 | 역행렬 존재 공분산 |
| 양반정치 | A ≽ 0 | 모든 λ ≥ 0 | 공분산, Gram 행렬 |
| 부정치 | — | 양·음 혼합 | 비볼록 loss의 Hessian |
| 음정치 | A ≺ 0 | 모든 λ < 0 | 오목 함수 Hessian |

---

### 섹션 4: Rayleigh Quotient — PCA의 핵심

**아이콘**: `R`  
**제목**: `Rayleigh Quotient — 분산 최대화가 PCA`

**설명**: PCA에서 "가장 분산이 큰 방향"을 찾는 것은 수학적으로 Rayleigh quotient 최대화와 같습니다.

**수식 박스 5**:
- 레이블: `Rayleigh Quotient`
- 수식:
```
R(A, \mathbf{x}) = \frac{\mathbf{x}^\top A \mathbf{x}}{\mathbf{x}^\top \mathbf{x}}
\quad\Rightarrow\quad
\max_{\mathbf{x} \neq 0} R(A, \mathbf{x}) = \lambda_{\max}(A)
```
- 주석:
  - Rayleigh quotient = 단위벡터 방향으로의 이차형식 (분산)
  - 최대값 = 최대 고유값 λ_max, 그 방향 = 첫 번째 주성분
  - 최솟값 = 최소 고유값 λ_min
  - PCA 계산: 공분산 행렬의 고유값 분해 → 고유값 내림차순으로 주성분 정렬

**개념 카드**:
- 제목: `🔑 PCA = 공분산 행렬의 고유값 분해`
- 내용:
  - 공분산 행렬 Σ = (1/n)XᵀX (중심화 후)
  - Σ의 고유벡터 = 주성분 방향
  - Σ의 고유값 = 각 주성분이 설명하는 분산
  - 상위 k개 주성분만 사용 = 차원 축소 (k << d)
  - 설명 분산 비율 = (Σᵢ₌₁ᵏ λᵢ) / (Σᵢ₌₁ⁿ λᵢ)

---

## 인터랙티브: Rayleigh Quotient 시각화

**제목**: `🔢 2×2 행렬의 Rayleigh Quotient — 방향에 따른 변화`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `rq-a11` | A[1,1] | -3 | 3 | 0.5 | 2.0 |
| `rq-a22` | A[2,2] | -3 | 3 | 0.5 | 0.5 |
| `rq-theta` | 벡터 각도 θ (도) | 0 | 180 | 5 | 45 |

**JavaScript 로직**:
```javascript
function updateRQ() {
  const a11 = parseFloat(document.getElementById('rq-a11').value);
  const a22 = parseFloat(document.getElementById('rq-a22').value);
  const theta = parseFloat(document.getElementById('rq-theta').value) * Math.PI / 180;
  document.getElementById('rq-a11-v').value = a11.toFixed(1);
  document.getElementById('rq-a22-v').value = a22.toFixed(1);
  document.getElementById('rq-theta-v').value = (theta * 180 / Math.PI).toFixed(0);

  // x = [cos θ, sin θ] (단위벡터), A = diag(a11, a22)
  const x1 = Math.cos(theta), x2 = Math.sin(theta);
  const rq = a11 * x1 * x1 + a22 * x2 * x2; // xᵀAx = a11*cos²θ + a22*sin²θ

  const lambdaMax = Math.max(a11, a22);
  const lambdaMin = Math.min(a11, a22);

  document.getElementById('rq-res').innerHTML =
    `A = diag(${a11.toFixed(1)}, ${a22.toFixed(1)})<br/>
     x = (cos${(theta*180/Math.PI).toFixed(0)}°, sin${(theta*180/Math.PI).toFixed(0)}°) = (${x1.toFixed(3)}, ${x2.toFixed(3)})<br/>
     Rayleigh quotient R(A,x) = xᵀAx = <strong>${rq.toFixed(4)}</strong><br/>
     최대 고유값 λ_max = ${lambdaMax.toFixed(1)} | 최소 = ${lambdaMin.toFixed(1)}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       θ를 바꿔서 R이 λ_min과 λ_max 사이에서 변함을 확인하세요
     </span>`;
}
updateRQ();
```

---

## 퀴즈

**질문**: 공분산 행렬 Σ의 첫 번째 주성분(PCA)은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 최소 고유값에 대응하는 고유벡터 | ❌ |
| (b) 최대 고유값에 대응하는 고유벡터 (가장 분산이 큰 방향) | ✅ |
| (c) 모든 고유값의 합 | ❌ |
| (d) 고유값의 평균 방향 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 24)

```
[ ] 고유값·고유벡터의 정의를 Av = λv로 쓸 수 있다
[ ] 대각화 A = VΛV⁻¹의 각 요소를 설명할 수 있다
[ ] 대칭 행렬의 스펙트럼 정리 A = QΛQᵀ를 설명할 수 있다
[ ] 양정치(PD)와 양반정치(PSD)의 차이를 설명할 수 있다
[ ] Rayleigh quotient가 PCA와 어떻게 연결되는지 설명할 수 있다
[ ] 논문에서 "Σ ≽ 0"을 보면 무슨 뜻인지 안다
[ ] PCA 설명 분산 비율 계산 방법을 안다
```
