# Week 16 — 연속성과 미분의 엄밀한 정의 (Ch.16)

> **파일명**: `chapters/ch16.html`  
> **Phase**: Phase 2-A · 해석학 기초  
> **인터랙티브**: ✅ Lipschitz 상수 시각화  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.16 · 해석학 기초 (Phase 2 — 2주차)` |
| 제목 | `연속성과 미분의 엄밀한 정의` |
| 서브타이틀 | `Lipschitz 연속과 미분가능성 조건. 논문에서 "Gradient Lipschitz"가 왜 등장하는지 이해합니다.` |
| 이전 챕터 | `ch15.html` — Ch.15 수열과 극한 |
| 다음 챕터 | `ch17.html` — Ch.17 적분과 급수 |

---

## EASY BOX 내용

**핵심 비유**: "미끄럼틀은 연속 — 발을 떼지 않고 쭉 미끄러진다. 계단은 불연속 — 중간에 확 튀어 오른다. AI 학습에서 loss 함수가 연속이어야 경사하강법이 작동한다."

**예시 리스트**:
- 연속: x² 함수는 x를 조금만 바꿔도 f(x)가 조금만 바뀐다 → 손으로 그릴 때 펜을 떼지 않아도 된다
- Lipschitz 연속: f를 조금 바꿨을 때 출력이 최대 L배 이상 안 바뀐다 → L이 작을수록 "순한" 함수
- AI에서: 논문에서 "Assumption: f is L-smooth (L-Lipschitz gradient)"는 경사하강법 수렴 증명의 핵심 조건이다

---

## 섹션 구성

### 섹션 1: 연속의 ε-δ 정의

**아이콘**: `〰`  
**제목**: `연속함수의 엄밀한 정의 — ε-δ 논증`

**설명**: 함수 f가 점 a에서 연속이라는 것은 직관적으로 "a 근처에서 f도 f(a) 근처에 있다"는 뜻입니다. ε-δ 언어로 정확히 표현하면 다음과 같습니다.

**수식 박스 1**:
- 레이블: `연속의 ε-δ 정의`
- 수식:
```
f \text{ is continuous at } a
\iff
\forall \varepsilon > 0,\ \exists \delta > 0\ \text{s.t.}\ |x - a| < \delta \Rightarrow |f(x) - f(a)| < \varepsilon
```
- 주석:
  - `δ`: x를 a 근처 δ 안으로 제한
  - `ε`: 그러면 f(x)가 f(a)와 ε 이내로 가까워진다
  - ε-δ 정의 = 수열 극한 정의의 함수 버전
  - 불연속점에서는 δ를 아무리 작게 해도 f가 크게 튀는 경우가 생긴다

**callout (tip)**:
- 내용: 연속 함수는 컴팩트(closed and bounded) 집합 위에서 **최솟값과 최댓값이 반드시 존재**한다 (Extreme Value Theorem). Loss function이 연속이고 파라미터 공간이 유계이면 최솟값 존재를 보장할 수 있다.

---

### 섹션 2: Lipschitz 연속

**아이콘**: `📐`  
**제목**: `Lipschitz 연속 — AI 논문 수렴 증명의 핵심 조건`

**설명**: Lipschitz 연속은 ε-δ보다 훨씬 강한 조건입니다. 함수의 "변화 속도"에 상한을 둡니다. 입력이 아무리 빠르게 변해도, 출력이 그보다 L배 이상 빠르게 변하지 않습니다.

**수식 박스 2**:
- 레이블: `L-Lipschitz 연속 정의`
- 수식:
```
|f(x) - f(y)| \leq L \cdot \|x - y\|,\quad \forall x, y \in \mathcal{X}
```
- 주석:
  - `L`: Lipschitz 상수 (작을수록 함수가 "순함")
  - `‖x - y‖`: 입력 사이의 거리 (유클리드 노름)
  - L = 0이면 상수 함수, L → ∞이면 매우 급변하는 함수
  - 미분가능하면 `L = sup|f'(x)|`가 Lipschitz 상수

**표 (함수별 Lipschitz 상수)**:

| 함수 | Lipschitz 상수 L | 비고 |
|------|-----------------|------|
| `f(x) = x` | 1 | 항등함수 |
| `f(x) = sin(x)` | 1 | |f'| ≤ 1 |
| `f(x) = x²` (ℝ 전체) | ∞ (비-Lipschitz) | 기울기 무한 |
| `f(x) = x²` ([−B, B] 제한) | 2B | 유계 구간에서 |
| ReLU `f(x) = max(0,x)` | 1 | |f'| ≤ 1 |
| sigmoid `σ(x)` | 0.25 | σ'(x) ≤ 1/4 |

---

### 섹션 3: L-smooth 함수 (그라디언트 Lipschitz)

**아이콘**: `∇`  
**제목**: `L-smooth — 경사하강법 수렴 증명의 핵심`

**설명**: AI 논문에서 "f is L-smooth"라는 표현이 자주 등장합니다. 이는 함수 자체가 아니라 **그라디언트**가 Lipschitz 연속임을 의미합니다. 이 조건이 있어야 경사하강법의 수렴을 보장할 수 있습니다.

**수식 박스 3**:
- 레이블: `L-smooth (그라디언트 Lipschitz)`
- 수식:
```
\|\nabla f(x) - \nabla f(y)\| \leq L \cdot \|x - y\|,\quad \forall x, y
```
- 주석:
  - 동치 조건: `f(y) ≤ f(x) + ∇f(x)ᵀ(y-x) + (L/2)‖y-x‖²`
  - 의미: 함수를 2차 함수로 위에서 근사할 수 있다 (상한 포물선 존재)
  - 논문에서: "Under Assumption A1 (f is L-smooth), GD with step size α = 1/L converges at rate O(1/T)"

**callout (warn)**:
- 내용: **자주 혼동되는 것**: Lipschitz 연속 ≠ L-smooth. Lipschitz는 함수 자체, L-smooth는 그라디언트에 적용. 논문에서 두 조건이 함께 나오면 각각 무엇에 대한 조건인지 확인하자.

---

### 섹션 4: 미분가능성과 연속성의 관계

**아이콘**: `f'`  
**제목**: `미분가능 → 연속, 연속 ↛ 미분가능`

**수식 박스 4**:
- 레이블: `도함수의 엄밀한 정의`
- 수식:
```
f'(a) = \lim_{h \to 0} \frac{f(a+h) - f(a)}{h}
```
- 주석:
  - 이 극한이 존재할 때 f는 a에서 미분가능
  - 미분가능 ⟹ 연속 (역은 성립 안 함)
  - ReLU `max(0,x)`: 0에서 연속이지만 미분 불가 (좌미분 = 0, 우미분 = 1)
  - AI에서: ReLU가 0에서 미분 불가 → 실전에서는 0으로 처리하거나 subgradient 사용

**표 (연속 vs 미분가능)**:

| 성질 | 의미 | 예시 |
|------|------|------|
| 연속 but 미분불가 | 꺾인 점 존재 | ReLU at 0, |x| at 0 |
| 미분가능 | 모든 점에서 접선 존재 | x², sin(x) |
| C¹ (연속 미분가능) | 도함수도 연속 | 대부분 ML 손실함수 |
| C∞ (무한 미분가능) | 모든 차수 미분 존재 | Gaussian, softmax |

---

## 인터랙티브: Lipschitz 상수 시각화

**제목**: `🔢 L-Lipschitz 조건 확인 — 기울기 상한`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `lip-L` | Lipschitz 상수 L | 0.1 | 5.0 | 0.1 | 1.0 |
| `lip-dx` | 입력 차이 |x-y| | 0.01 | 2.0 | 0.5 | 0.5 |

**JavaScript 로직**:
```javascript
function updateLip() {
  const L = parseFloat(document.getElementById('lip-L').value);
  const dx = parseFloat(document.getElementById('lip-dx').value);
  document.getElementById('lip-L-v').value = L.toFixed(1);
  document.getElementById('lip-dx-v').value = dx.toFixed(2);

  const maxDf = L * dx;

  // 예시: f(x) = sin(x), L=1 → |f(x)-f(y)| ≤ |x-y|
  const x = 0.5, y = x + dx;
  const dfSin = Math.abs(Math.sin(y) - Math.sin(x));
  const dfX2  = Math.abs(y*y - x*x); // x² 는 비-Lipschitz

  document.getElementById('lip-res').innerHTML =
    `Lipschitz 조건: |f(x) - f(y)| ≤ <strong>L × |x-y| = ${L.toFixed(1)} × ${dx.toFixed(2)} = ${maxDf.toFixed(3)}</strong><br/>
     sin(x) 예시: |sin(${(x+dx).toFixed(2)}) - sin(${x})| = ${dfSin.toFixed(3)} ${dfSin <= maxDf ? '≤ L·|dx| ✅' : '> L·|dx| ❌'}<br/>
     x² 예시 (x=${x}): |y²-x²| = ${dfX2.toFixed(3)} ${dfX2 <= maxDf ? '≤ L·|dx| ✅' : '> L·|dx| ❌'}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       x²는 전체 ℝ에서 Lipschitz가 아니다 (L을 크게 해야 만족)
     </span>`;
}
updateLip();
```

---

## 퀴즈

**질문**: 논문에서 "f is L-smooth (L-Lipschitz gradient)"가 의미하는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) f 자체가 Lipschitz 연속이다 | ❌ |
| (b) ‖∇f(x) - ∇f(y)‖ ≤ L‖x-y‖가 성립한다 | ✅ |
| (c) f의 값이 L보다 작다 | ❌ |
| (d) f는 L번 미분 가능하다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 16)

```
[ ] ε-δ 정의로 연속을 설명할 수 있다
[ ] Lipschitz 연속과 ε-δ 연속의 차이를 설명할 수 있다
[ ] Lipschitz 상수가 크면 어떤 의미인지 설명할 수 있다
[ ] L-smooth(그라디언트 Lipschitz)의 정의를 쓸 수 있다
[ ] 논문에서 "Assumption A1: f is L-smooth"를 보고 무슨 뜻인지 안다
[ ] 미분가능성과 연속성의 관계를 예시와 함께 설명할 수 있다
[ ] ReLU가 0에서 왜 subgradient를 쓰는지 설명할 수 있다
```
