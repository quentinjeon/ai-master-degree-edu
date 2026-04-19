# Week 17 — 적분과 급수 (Ch.17)

> **파일명**: `chapters/ch17.html`  
> **Phase**: Phase 2-A · 해석학 기초  
> **인터랙티브**: ✅ 테일러 급수 근사 시각화  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.17 · 해석학 기초 (Phase 2 — 3주차)` |
| 제목 | `적분과 급수 — 오차 경계와 근사` |
| 서브타이틀 | `Riemann 적분, 테일러 급수, 오차 경계. 논문의 "approximation error" 분석을 이해합니다.` |
| 이전 챕터 | `ch16.html` — Ch.16 연속성과 미분 |
| 다음 챕터 | `ch18.html` — Ch.18 볼록 집합·볼록 함수 |

---

## EASY BOX 내용

**핵심 비유**: "복잡한 함수를 다항식으로 근사하는 것이 테일러 급수다. GPS가 실시간으로 복잡한 위성 계산 대신 간단한 다항식으로 빠르게 근사하는 것처럼, AI도 복잡한 함수를 테일러 근사로 분석한다."

**예시 리스트**:
- sin(x) ≈ x - x³/6 + x⁵/120 — x가 0 근처라면 그냥 sin(x) ≈ x로 충분히 정확
- 오차 경계: "이 근사의 오차는 최대 x⁵/120 이하" — 논문의 "error bound"가 이것
- AI에서: 손실함수를 θ 근처에서 2차 근사(Newton's method), 1차 근사(경사하강법) — 근사 차수가 알고리즘을 결정

---

## 섹션 구성

### 섹션 1: Riemann 적분 — 엄밀한 정의

**아이콘**: `∫`  
**제목**: `Riemann 적분`

**설명**: 적분을 "넓이"로 이해하는 것을 넘어, 수학적으로 엄밀하게 정의합니다. 구간을 n개로 쪼개어 직사각형 넓이의 합으로 근사하고, n → ∞ 극한을 취합니다.

**수식 박스 1**:
- 레이블: `Riemann 합과 적분`
- 수식:
```
\int_a^b f(x)\, dx = \lim_{n \to \infty} \sum_{i=1}^{n} f(x_i^*) \cdot \Delta x_i
\quad \text{where } \Delta x_i = \frac{b-a}{n}
```
- 주석:
  - `xᵢ*`: i번째 소구간 안의 임의의 점 (왼쪽 끝, 중간, 오른쪽 끝 모두 가능)
  - Riemann 적분이 존재 ⟺ 상합과 하합의 극한이 일치
  - 연속 함수는 항상 Riemann 적분 가능
  - 확률에서 `∫ f(x) dx = 1` (PDF 정규화) 이 개념으로 이해

**callout (info)**:
- 내용: Riemann 적분은 직관적이지만, 불연속이 많은 함수(딥러닝 이론의 확률 공간)를 다루기엔 한계가 있다. 이것이 다음 Chapter에서 배울 **Lebesgue 적분**의 동기다.

---

### 섹션 2: 테일러 급수

**아이콘**: `📈`  
**제목**: `테일러 급수 — 함수를 다항식으로 근사`

**설명**: 미분가능한 함수 f를 점 a 근처에서 다항식으로 무한히 정확하게 근사할 수 있습니다. 이것이 테일러 급수입니다. 항을 n개까지 자르면 테일러 다항식이 되고, 나머지가 오차입니다.

**수식 박스 2**:
- 레이블: `테일러 급수 (a=0 중심, Maclaurin 급수)`
- 수식:
```
f(x) = \sum_{k=0}^{\infty} \frac{f^{(k)}(0)}{k!} x^k
     = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f'''(0)}{3!}x^3 + \cdots
```
- 주석:
  - `f^(k)(0)`: f의 k번째 도함수를 x=0에서 평가한 값
  - `k!`: k 팩토리얼 = k × (k-1) × ... × 1
  - 수렴 반경(radius of convergence): 이 급수가 정확한 범위

**표 (주요 함수의 테일러 급수)**:

| 함수 | 테일러 급수 | AI 연결 |
|------|-----------|---------|
| `eˣ` | 1 + x + x²/2! + x³/3! + ... | softmax, 지수 감쇠 |
| `ln(1+x)` | x - x²/2 + x³/3 - ... | cross-entropy 근사 |
| `sin(x)` | x - x³/3! + x⁵/5! - ... | 위치 인코딩 |
| `(1+x)ⁿ` | 1 + nx + n(n-1)x²/2! + ... | 이항 근사 |
| `σ(x) = 1/(1+e⁻ˣ)` | 1/2 + x/4 - x³/48 + ... | sigmoid 선형화 |

---

### 섹션 3: 테일러 정리와 오차 경계

**아이콘**: `⚠`  
**제목**: `나머지 항 (Remainder) — 오차를 수치화한다`

**설명**: 테일러 급수를 n번째 항까지 자르면 오차가 발생합니다. 이 오차(나머지)를 수학적으로 bound하는 것이 테일러 정리입니다. 논문에서 "the approximation error is O(h²)"처럼 쓰는 게 이것입니다.

**수식 박스 3**:
- 레이블: `테일러 정리 — Lagrange 나머지`
- 수식:
```
f(x) = \sum_{k=0}^{n} \frac{f^{(k)}(a)}{k!}(x-a)^k + R_n(x)
\quad\text{where}\quad R_n(x) = \frac{f^{(n+1)}(\xi)}{(n+1)!}(x-a)^{n+1}
```
- 주석:
  - `ξ`: a와 x 사이의 어떤 점 (정확한 위치는 모르지만 존재)
  - `|Rₙ(x)| ≤ M |x-a|^{n+1} / (n+1)!` 여기서 M = max|f^{(n+1)}|
  - 오차는 `(x-a)^{n+1}` 에 비례 → x가 a에 가까울수록 작다
  - 논문의 "error bound O(h^p)"가 이 구조

**수식 박스 4** (2차 테일러 근사 — AI 핵심):
- 레이블: `2차 테일러 근사 (Newton's Method 기반)`
- 수식:
```
f(\theta + \Delta\theta) \approx f(\theta) + \nabla f(\theta)^\top \Delta\theta
+ \frac{1}{2} \Delta\theta^\top H(\theta) \Delta\theta
```
- 주석:
  - `∇f(θ)`: 그라디언트 (1차 도함수)
  - `H(θ)`: 헤시안 행렬 (2차 도함수)
  - 1차만 쓰면 경사하강법, 2차까지 쓰면 Newton's method
  - Adam 옵티마이저: 헤시안 없이 경험적으로 2차 정보를 추정

---

### 섹션 4: Big-O 표기법 — 논문 수렴 속도 읽기

**아이콘**: `O`  
**제목**: `Big-O 표기 — 수렴 속도를 한 눈에`

**수식 박스 5**:
- 레이블: `Big-O / Little-o 정의`
- 수식:
```
f(n) = O(g(n)) \iff \exists C, N_0\ \text{s.t.}\ n > N_0 \Rightarrow |f(n)| \leq C \cdot |g(n)|
```
- 주석:
  - `O(g)`: g보다 빠르게 커지지 않는다 (상한)
  - `o(g)`: g보다 훨씬 느리게 커진다 (f/g → 0)
  - `Ω(g)`: g보다 느리게 줄어들지 않는다 (하한)
  - 논문에서: "convergence rate O(1/T)" = T번 반복 후 오차가 최대 C/T

**표 (자주 등장하는 수렴 속도)**:

| 표기 | 의미 | 예시 |
|------|------|------|
| O(1/T) | 선형 감소 | 기본 SGD |
| O(1/√T) | 제곱근 감소 | 볼록 SGD |
| O(1/T²) | 2차 감소 | Nesterov 가속 |
| O(ρᵀ), ρ<1 | 지수 감소 | 강볼록 GD |
| O(log T / T) | 로그 요인 포함 | AdaGrad |

---

## 인터랙티브: 테일러 근사 오차 계산기

**제목**: `🔢 sin(x) 테일러 근사 — 항 수에 따른 오차`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `tay-x` | x 값 | 0 | 3.14 | 0.01 | 1.0 |
| `tay-n` | 사용 항 수 (1~5) | 1 | 5 | 1 | 2 |

**JavaScript 로직**:
```javascript
function factorial(k) { return k <= 1 ? 1 : k * factorial(k - 1); }

function updateTaylor() {
  const x = parseFloat(document.getElementById('tay-x').value);
  const n = parseInt(document.getElementById('tay-n').value);
  document.getElementById('tay-x-v').value = x.toFixed(2);
  document.getElementById('tay-n-v').value = n;

  // sin(x) Taylor: x - x³/3! + x⁵/5! - ...
  let approx = 0;
  let terms = [];
  for (let k = 0; k < n; k++) {
    const power = 2 * k + 1;
    const sign = (k % 2 === 0) ? 1 : -1;
    const term = sign * Math.pow(x, power) / factorial(power);
    approx += term;
    terms.push(`${sign > 0 ? '+' : '-'}x^${power}/${power}!`);
  }
  const exact = Math.sin(x);
  const error = Math.abs(exact - approx);

  document.getElementById('tay-res').innerHTML =
    `sin(${x.toFixed(2)}) ≈ ${terms.join(' ')} = <strong>${approx.toFixed(6)}</strong><br/>
     정확한 값: sin(${x.toFixed(2)}) = ${exact.toFixed(6)}<br/>
     오차 |Rₙ| = <strong>${error.toFixed(8)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       항 수를 늘릴수록 오차가 줄어든다 (수렴)
     </span>`;
}
updateTaylor();
```

---

## 퀴즈

**질문**: 논문에서 "경사하강법의 수렴 속도가 O(1/T)이다"의 의미는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) T번 반복 후 오차가 정확히 1/T이다 | ❌ |
| (b) T번 반복 후 오차가 최대 C/T 이하이다 (C는 상수) | ✅ |
| (c) T번 반복 후 수렴이 보장된다 | ❌ |
| (d) 학습률이 1/T이다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 17)

```
[ ] Riemann 적분의 직관(넓이)과 엄밀한 정의를 연결할 수 있다
[ ] 테일러 급수가 무엇인지 한 문장으로 설명할 수 있다
[ ] eˣ와 sin(x)의 테일러 급수를 쓸 수 있다
[ ] 오차 경계(나머지 항)가 무엇인지 설명할 수 있다
[ ] Big-O 표기를 보고 수렴 속도를 비교할 수 있다
[ ] O(1/T)와 O(1/√T)의 차이를 설명할 수 있다
[ ] 1차 vs 2차 테일러 근사가 GD vs Newton 방법과 어떻게 연결되는지 안다
```
