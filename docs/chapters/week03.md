# Week 03 — 선형대수 기초 (Ch.3)

> **파일명**: `chapters/ch03.html`  
> **대응 섹션**: `c3`  
> **인터랙티브**: ✅ 내적 계산기 (선택)  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.3 · 기초 수학 (3주차)` |
| 제목 | `선형대수 기초` |
| 서브타이틀 | `딥러닝에서 데이터와 모델 파라미터는 벡터와 행렬로 표현됩니다. 그 언어를 익힙니다.` |
| 이전 챕터 | `ch02.html` — Ch.2 미분과 최적화 |
| 다음 챕터 | `ch04.html` — Ch.4 확률론 기초 |

---

## EASY BOX 내용

**핵심 비유**: "벡터는 '여러 숫자를 순서대로 묶은 것'이고, 행렬은 '숫자를 격자 모양으로 배열한 것'이다. 딥러닝의 모든 계산은 이것으로 이루어진다."

**예시 리스트**:
- 벡터: 키, 몸무게, 나이를 [170, 65, 25]처럼 묶으면 한 사람을 숫자로 표현 가능
- 행렬: 반 학생 30명의 정보를 30×3 표로 만들면 → 데이터 행렬
- AI에서: 입력 이미지 (224×224×3) → 행렬/텐서. 가중치 W ∈ ℝ^{d_out × d_in} → 행렬

---

## 섹션 구성

### 섹션 1: 벡터 (Vector)

**아이콘**: `→`  
**제목**: `벡터 (Vector)`

**설명**: 벡터는 크기와 방향을 가진 수의 배열입니다. 하나의 데이터 샘플은 보통 벡터로 표현됩니다.

**수식 박스 1**:
- 레이블: `열벡터 표기`
- 수식: `\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_d \end{bmatrix} \in \mathbb{R}^d`
- 주석: `d`: 특성(feature) 차원, `x_i`: i번째 특성값, `ℝᵈ`: d차원 실수 공간

**수식 박스 2 (내적)**:
- 레이블: `내적 (Dot Product) — Attention의 핵심`
- 수식: `\mathbf{a} \cdot \mathbf{b} = \mathbf{a}^\top \mathbf{b} = \sum_{i=1}^{d} a_i b_i = \|\mathbf{a}\|\|\mathbf{b}\|\cos\theta`
- 주석: `cos θ ≈ 1`이면 두 벡터가 비슷한 방향 (유사). Transformer Attention에서 Q·Kᵀ 연산의 기초

---

### 섹션 2: 행렬 (Matrix)

**아이콘**: `⊞`  
**제목**: `행렬 (Matrix)`

**설명**: 행렬은 2차원 배열입니다. 뉴럴 네트워크의 가중치 레이어는 행렬로 표현됩니다.

**수식 박스 3**:
- 레이블: `선형 레이어 = 행렬 곱 + 활성화`
- 수식: `\mathbf{h} = \sigma(\mathbf{W}\mathbf{x} + \mathbf{b})`
- 주석: `W ∈ ℝ^{d_out × d_in}`: 가중치 행렬, `b`: 편향 벡터, `σ`: 활성화 함수 (ReLU, sigmoid 등)

**표 (행렬 연산 요약)**:

| 연산 | 표기 | AI 역할 |
|------|------|---------|
| 전치 | `Aᵀ` | 차원 변환, Attention QKᵀ |
| 행렬 곱 | `AB` | 레이어 순전파 (forward pass) |
| 역행렬 | `A⁻¹` | 최소제곱법 해석적 풀이 |
| L2 노름 | `‖x‖₂ = √(Σxᵢ²)` | 정규화, 거리 계산 |
| 고유값 분해 | `A = QΛQᵀ` | PCA, 주성분 분석 |
| SVD | `A = UΣVᵀ` | LoRA, 차원 축소 |

**개념 카드**:
- 제목: `🔑 논문에서 자주 보이는 표현`
- 내용 리스트:
  - `X ∈ ℝ^{n×d}` — n개 샘플, d개 특성의 데이터 행렬
  - `W ∈ ℝ^{d_out × d_in}` — 선형 레이어의 가중치 행렬
  - `Attention(Q,K,V) = softmax(QKᵀ/√d_k) V` — Transformer의 핵심 연산

---

## 인터랙티브: 행렬 곱 shape 계산기 (선택)

**제목**: `🔢 행렬 곱 Shape 계산기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `mat-m` | A의 행 (m) | 1 | 10 | 1 | 3 |
| `mat-k` | A의 열 = B의 행 (k) | 1 | 10 | 1 | 4 |
| `mat-n` | B의 열 (n) | 1 | 10 | 1 | 2 |

**JavaScript 로직**:
```javascript
function updateMat() {
  const m = parseInt(document.getElementById('mat-m').value);
  const k = parseInt(document.getElementById('mat-k').value);
  const n = parseInt(document.getElementById('mat-n').value);
  document.getElementById('mat-m-v').value = m;
  document.getElementById('mat-k-v').value = k;
  document.getElementById('mat-n-v').value = n;
  document.getElementById('mat-res').innerHTML =
    `A (${m}×${k}) × B (${k}×${n}) = AB (<strong>${m}×${n}</strong>)<br/>
    <span style="font-size:.78rem;color:var(--muted)">연산 횟수: ${m}×${k}×${n} = ${m*k*n}번의 곱셈</span>`;
}
updateMat();
```

---

## 퀴즈

**질문**: `A ∈ ℝ^{3×4}`, `B ∈ ℝ^{4×2}`일 때, AB의 크기는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) `4 × 4` | ❌ |
| (b) `3 × 4` | ❌ |
| (c) `3 × 2` | ✅ |
| (d) `4 × 2` | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 3)

```
[ ] 벡터와 스칼라의 차이를 설명할 수 있다
[ ] x ∈ ℝᵈ 표기를 해석할 수 있다
[ ] 내적의 의미를 "유사도"로 설명할 수 있다
[ ] 행렬 곱의 shape 규칙을 안다 (m×k × k×n = m×n)
[ ] 선형 레이어 h = σ(Wx + b)를 설명할 수 있다
[ ] Transformer의 Attention 연산이 내적임을 안다
```
