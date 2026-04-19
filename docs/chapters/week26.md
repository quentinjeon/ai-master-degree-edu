# Week 26 — 행렬 미분 (Ch.26)

> **파일명**: `chapters/ch26.html`  
> **Phase**: Phase 2-D · 선형대수 심화  
> **인터랙티브**: ✅ 이차형식 그라디언트 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.26 · 선형대수 심화 (Phase 2 — 12주차)` |
| 제목 | `행렬 미분 — 역전파의 수학` |
| 서브타이틀 | `∂(Ax)/∂x, ∂(xᵀAx)/∂x를 계산합니다. 딥러닝 역전파를 행렬로 표현하는 법.` |
| 이전 챕터 | `ch25.html` — Ch.25 SVD 완전 유도 |
| 다음 챕터 | `chapters/ch27.html` — Phase 3 시작 (추후 개발) |

---

## EASY BOX 내용

**핵심 비유**: "스칼라를 벡터로 미분하면 벡터가 나온다(그라디언트). 벡터를 벡터로 미분하면 행렬이 나온다(야코비안). 행렬 미분은 이 개념을 체계화한 것 — 딥러닝의 역전파가 바로 이것이다."

**예시 리스트**:
- 손실 L이 스칼라, 파라미터 θ가 벡터 → ∂L/∂θ가 그라디언트 벡터
- 선형 레이어 y = Wx → ∂L/∂W가 행렬 = 각 가중치에 대한 그라디언트
- AI에서: 역전파(backpropagation)는 체인룰을 행렬 미분으로 표현한 것 — 이걸 이해하면 직접 구현 가능

---

## 섹션 구성

### 섹션 1: 미분의 표기 규칙

**아이콘**: `∂`  
**제목**: `행렬 미분 표기 — 분자 레이아웃 vs 분모 레이아웃`

**설명**: 행렬 미분에는 두 가지 표기 규칙이 있습니다. 논문마다 다를 수 있어서 헷갈리기 쉽습니다.

**표 (행렬 미분 표기 정리)**:

| 미분 대상 | 분모 | 결과 | 크기 | 이름 |
|---------|------|------|------|------|
| 스칼라 f | 벡터 x (n×1) | ∂f/∂x | n×1 | 그라디언트 |
| 벡터 f (m×1) | 벡터 x (n×1) | ∂f/∂x | m×n | 야코비안 |
| 스칼라 f | 행렬 A (m×n) | ∂f/∂A | m×n | 행렬 그라디언트 |

**callout (warn)**:
- 내용: 딥러닝 프레임워크(PyTorch)는 "분모 레이아웃" 사용. 논문은 "분자 레이아웃"을 쓰기도 함. 항상 논문의 Notation 섹션에서 어떤 규칙을 쓰는지 확인하자.

---

### 섹션 2: 핵심 공식 — 선형, 이차형식

**아이콘**: `=`  
**제목**: `자주 쓰이는 행렬 미분 공식`

**수식 박스 1**:
- 레이블: `선형 함수의 그라디언트`
- 수식:
```
\frac{\partial}{\partial \mathbf{x}} (\mathbf{a}^\top \mathbf{x}) = \mathbf{a}
\qquad
\frac{\partial}{\partial \mathbf{x}} (A\mathbf{x}) = A^\top
\qquad
\frac{\partial}{\partial \mathbf{x}} (\mathbf{x}^\top A) = A
```
- 주석:
  - aᵀx = Σᵢ aᵢxᵢ를 xⱼ로 미분하면 aⱼ → 그라디언트는 a
  - Ax의 그라디언트: (Ax)ᵢ에 대해 미분 → Aᵀ (분모 레이아웃 기준)
  - 선형 레이어 포워드: y = Wx → ∂L/∂x = Wᵀ(∂L/∂y)

**수식 박스 2**:
- 레이블: `이차형식의 그라디언트`
- 수식:
```
\frac{\partial}{\partial \mathbf{x}} (\mathbf{x}^\top A \mathbf{x}) = (A + A^\top)\mathbf{x}
\quad\xrightarrow{A = A^\top}\quad 2A\mathbf{x}
```
- 주석:
  - A가 대칭이면 그라디언트 = 2Ax
  - 예: ‖x‖² = xᵀIx → 그라디언트 = 2Ix = 2x
  - OLS 손실 ‖Ax-b‖² = (Ax-b)ᵀ(Ax-b) → 그라디언트 = 2Aᵀ(Ax-b)

**표 (자주 쓰이는 행렬 미분 공식 모음)**:

| 함수 | 그라디언트 |
|------|-----------|
| aᵀx | a |
| xᵀa | a |
| Ax | Aᵀ |
| xᵀAx (A 대칭) | 2Ax |
| ‖x‖² | 2x |
| ‖Ax - b‖² | 2Aᵀ(Ax - b) |
| log det(A) | A⁻ᵀ |
| trace(AᵀB) | B |

---

### 섹션 3: 체인룰 — 역전파의 수학

**아이콘**: `∘`  
**제목**: `행렬 체인룰 — 딥러닝 역전파`

**설명**: 여러 함수가 합성되면 체인룰을 사용합니다. 딥러닝의 역전파는 이 체인룰을 레이어별로 반복 적용한 것입니다.

**수식 박스 3**:
- 레이블: `야코비안 체인룰`
- 수식:
```
\text{If } \mathbf{z} = f(\mathbf{y}),\; \mathbf{y} = g(\mathbf{x}),\text{ then:}
\quad
\frac{\partial \mathbf{z}}{\partial \mathbf{x}} = \frac{\partial \mathbf{z}}{\partial \mathbf{y}} \cdot \frac{\partial \mathbf{y}}{\partial \mathbf{x}}
\quad\text{(야코비안 행렬 곱)}
```
- 주석:
  - ∂z/∂y: m×k 야코비안
  - ∂y/∂x: k×n 야코비안
  - 결과: m×n 야코비안 (행렬 곱)
  - 역전파: 출력에서 입력으로 야코비안들을 역순으로 곱함

**수식 박스 4** (선형 레이어 역전파):
- 레이블: `선형 레이어 역전파 — 완전 유도`
- 수식:
```
\text{포워드: } \mathbf{y} = W\mathbf{x} + \mathbf{b}
\quad\Rightarrow\quad
\frac{\partial L}{\partial W} = \frac{\partial L}{\partial \mathbf{y}} \cdot \mathbf{x}^\top,
\quad
\frac{\partial L}{\partial \mathbf{x}} = W^\top \cdot \frac{\partial L}{\partial \mathbf{y}}
```
- 주석:
  - `∂L/∂y`: 다음 레이어에서 전파된 그라디언트 (upstream gradient)
  - `∂L/∂W = δyxᵀ`: 가중치 그라디언트는 upstream × 입력의 외적
  - `∂L/∂x = Wᵀδy`: 입력 그라디언트는 Wᵀ × upstream
  - 이것이 PyTorch nn.Linear의 역전파 계산 공식

---

### 섹션 4: 행렬 미분 응용 — OLS와 Softmax

**아이콘**: `✏`  
**제목**: `행렬 미분 실전 — OLS 정규방정식 유도`

**수식 박스 5**:
- 레이블: `OLS 손실의 행렬 미분`
- 수식:
```
\mathcal{L}(\mathbf{w}) = \|X\mathbf{w} - \mathbf{y}\|^2
\quad\Rightarrow\quad
\frac{\partial \mathcal{L}}{\partial \mathbf{w}} = 2X^\top(X\mathbf{w} - \mathbf{y}) = \mathbf{0}
\quad\Rightarrow\quad
\hat{\mathbf{w}} = (X^\top X)^{-1} X^\top \mathbf{y}
```
- 주석:
  - 그라디언트 = 0 조건 → 정규방정식
  - 2Xᵀ(Xw - y) = 0 → XᵀXw = Xᵀy → w = (XᵀX)⁻¹Xᵀy
  - 이것이 Ch.3B에서 배운 정규방정식을 행렬 미분으로 유도한 것

**수식 박스 6** (Softmax 그라디언트):
- 레이블: `Softmax 야코비안`
- 수식:
```
\mathbf{p} = \mathrm{softmax}(\mathbf{z}),\quad p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}
\quad\Rightarrow\quad
\frac{\partial p_i}{\partial z_j} = p_i(\delta_{ij} - p_j)
```
- 주석:
  - `δᵢⱼ`: Kronecker delta (i=j이면 1, 아니면 0)
  - Softmax 야코비안은 대각 행렬이 아님 — 모든 출력이 모든 입력에 의존
  - Cross-entropy + softmax의 합산 그라디언트: ∂(CE)/∂z = p - y (원-핫 레이블)

---

## 인터랙티브: 이차형식 그라디언트 계산기

**제목**: `🔢 xᵀAx의 그라디언트 — 벡터 미분 실습`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `md-x1` | x₁ | -3 | 3 | 0.5 | 1.0 |
| `md-x2` | x₂ | -3 | 3 | 0.5 | 2.0 |
| `md-a11` | A[1,1] | -2 | 2 | 0.5 | 1.0 |
| `md-a22` | A[2,2] | -2 | 2 | 0.5 | 1.0 |

**JavaScript 로직**:
```javascript
function updateMatDiff() {
  const x1 = parseFloat(document.getElementById('md-x1').value);
  const x2 = parseFloat(document.getElementById('md-x2').value);
  const a11 = parseFloat(document.getElementById('md-a11').value);
  const a22 = parseFloat(document.getElementById('md-a22').value);
  document.getElementById('md-x1-v').value = x1.toFixed(1);
  document.getElementById('md-x2-v').value = x2.toFixed(1);
  document.getElementById('md-a11-v').value = a11.toFixed(1);
  document.getElementById('md-a22-v').value = a22.toFixed(1);

  // A = diag(a11, a22) (대칭), f = xᵀAx = a11*x1² + a22*x2²
  const f = a11 * x1 * x1 + a22 * x2 * x2;
  // ∂f/∂x = 2Ax = [2*a11*x1, 2*a22*x2]
  const g1 = 2 * a11 * x1;
  const g2 = 2 * a22 * x2;

  document.getElementById('md-res').innerHTML =
    `A = diag(${a11.toFixed(1)}, ${a22.toFixed(1)}), x = (${x1.toFixed(1)}, ${x2.toFixed(1)})<br/>
     f = xᵀAx = ${a11.toFixed(1)}×${x1.toFixed(1)}² + ${a22.toFixed(1)}×${x2.toFixed(1)}² = <strong>${f.toFixed(3)}</strong><br/>
     그라디언트 ∂f/∂x = 2Ax = <strong>(${g1.toFixed(3)}, ${g2.toFixed(3)})</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       A가 대칭이면 ∂(xᵀAx)/∂x = 2Ax — 핵심 공식
     </span>`;
}
updateMatDiff();
```

---

## 퀴즈

**질문**: 선형 레이어 y = Wx에서 손실 L에 대한 x의 그라디언트 ∂L/∂x는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) W · (∂L/∂y) | ❌ |
| (b) Wᵀ · (∂L/∂y) | ✅ |
| (c) (∂L/∂y) · Wᵀ | ❌ |
| (d) ∂L/∂y 그 자체 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 26)

```
[ ] 행렬 미분 표기(∂f/∂x)의 크기를 설명할 수 있다
[ ] ∂(aᵀx)/∂x = a를 유도할 수 있다
[ ] ∂(xᵀAx)/∂x = 2Ax (A 대칭)를 설명할 수 있다
[ ] ‖Ax-b‖²의 그라디언트를 계산할 수 있다
[ ] 선형 레이어의 역전파 공식(∂L/∂W, ∂L/∂x)을 유도할 수 있다
[ ] OLS 정규방정식을 행렬 미분으로 유도할 수 있다
[ ] Phase 2 전체를 마쳤다: 해석학·볼록 최적화·측도 확률론·선형대수 심화
```

---

## Phase 2 졸업 확인

```
Phase 2 전체 완료 조건:
[ ] Ch.15-17 (해석학): ε-N 정의, Lipschitz, 테일러 급수, Big-O
[ ] Ch.18-20 (볼록 최적화): 볼록 함수, KKT, 수렴 속도
[ ] Ch.21-23 (측도 확률론): σ-대수, a.s. 수렴, Lebesgue 적분
[ ] Ch.24-26 (선형대수): 고유값, SVD, 행렬 미분

졸업 기준: NeurIPS 논문의 Theorem 섹션을 혼자서 검증할 수 있다.
```
