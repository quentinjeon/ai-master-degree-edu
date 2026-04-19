# Week 21 — σ-대수·확률공간 (Ch.21)

> **파일명**: `chapters/ch21.html`  
> **Phase**: Phase 2-C · 측도 기반 확률론  
> **인터랙티브**: ✅ 가산 가산 집합족 탐색기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.21 · 측도 확률론 (Phase 2 — 7주차)` |
| 제목 | `σ-대수와 확률공간` |
| 서브타이틀 | `확률론의 수학적 기반. 논문의 "a.s. convergence", "measurable function"을 이해하는 출발점.` |
| 이전 챕터 | `ch20.html` — Ch.20 수렴 속도 분석 |
| 다음 챕터 | `ch22.html` — Ch.22 확률변수의 수렴 |

---

## EASY BOX 내용

**핵심 비유**: "확률을 엄밀하게 정의하려면 '어떤 사건에 대해 확률을 말할 수 있는가'를 먼저 정해야 한다. σ-대수는 '확률을 말할 수 있는 사건들의 목록'이다. 목록에 없는 사건의 확률은 정의가 안 된다."

**예시 리스트**:
- 동전 던지기: Ω = {앞, 뒤}, σ-대수 = {∅, {앞}, {뒤}, {앞,뒤}} — 이 4개에 대해서만 확률을 말할 수 있다
- 연속 확률변수: 실수에서 "구간 [a,b]의 확률"을 정의하려면 Borel σ-대수가 필요
- AI에서: 딥러닝 이론에서 "f is measurable"이 등장하는 이유 — 측정 가능한 함수여야 기대값이 정의됨

---

## 섹션 구성

### 섹션 1: 확률 공간의 3요소

**아이콘**: `(Ω,ℱ,P)`  
**제목**: `확률 공간 (Probability Space)`

**설명**: 엄밀한 확률론은 세 가지 요소로 이루어진 수학적 구조를 정의합니다. 이것이 측도론적 확률론의 기반이며, 모든 확률 증명의 출발점입니다.

**수식 박스 1**:
- 레이블: `확률 공간 (Ω, ℱ, P)`
- 수식:
```
(\Omega, \mathcal{F}, P) \text{ — 확률 공간의 3요소}
\begin{cases}
\Omega & \text{표본 공간 (Sample space): 가능한 모든 결과의 집합} \\
\mathcal{F} & \text{σ-대수 (sigma-algebra): 사건들의 집합족} \\
P & \text{확률 측도 (Probability measure): } \mathcal{F} \to [0,1]
\end{cases}
```
- 주석:
  - Ω: 주사위라면 {1,2,3,4,5,6}, 실수 확률변수라면 ℝ
  - ℱ: "확률을 논할 수 있는" 부분집합들의 모음 (σ-대수)
  - P: 각 사건 A ∈ ℱ에 확률값 P(A) ∈ [0,1]을 부여하는 함수

**표 (일상 예시로 보는 확률 공간)**:

| 실험 | Ω | ℱ의 원소 예시 | P |
|------|---|------------|---|
| 동전 | {H, T} | {H}, {T}, {H,T}, ∅ | P({H}) = 0.5 |
| 주사위 | {1,...,6} | {짝수} = {2,4,6} | P({짝수}) = 0.5 |
| 연속 | ℝ | [a, b], (-∞, c] | P([a,b]) = ∫ₐᵇ f(x)dx |

---

### 섹션 2: σ-대수의 정의

**아이콘**: `ℱ`  
**제목**: `σ-대수 — 사건들의 닫힌 집합족`

**설명**: σ-대수(sigma-algebra)는 표본 공간 Ω의 부분집합들 중에서 "확률을 말할 수 있는" 사건들을 체계적으로 모아놓은 구조입니다. 세 가지 성질을 만족해야 합니다.

**수식 박스 2**:
- 레이블: `σ-대수의 3가지 성질`
- 수식:
```
\mathcal{F} \subseteq 2^\Omega \text{ is a σ-algebra if:}
\begin{cases}
(1)\; \Omega \in \mathcal{F} & \text{(표본 공간 포함)} \\
(2)\; A \in \mathcal{F} \Rightarrow A^c \in \mathcal{F} & \text{(여사건에 닫힘)} \\
(3)\; A_1, A_2, \ldots \in \mathcal{F} \Rightarrow \bigcup_{n=1}^\infty A_n \in \mathcal{F} & \text{(가산 합집합에 닫힘)}
\end{cases}
```
- 주석:
  - (1) 전체 사건(반드시 일어남)이 포함
  - (2) A가 사건이면 A가 안 일어나는 것도 사건
  - (3) 가산 무한 개의 사건의 합집합도 사건
  - (2)+(3) → 가산 교집합에도 닫힘 (∩Aₙ = (∪Aₙᶜ)ᶜ)

**callout (tip)**:
- 내용: σ-대수가 "가산(countable)" 합집합에 닫혀야 하는 이유: 연속 확률변수에서 P(X = a) = 0이지만 P(X ∈ [a,b]) > 0처럼, 무한히 많은 점들의 합집합을 다뤄야 하기 때문이다.

---

### 섹션 3: Borel σ-대수

**아이콘**: `ℬ`  
**제목**: `Borel σ-대수 — 실수에서의 표준 σ-대수`

**설명**: 실수 ℝ에서 가장 자연스러운 σ-대수는 **Borel σ-대수** ℬ(ℝ)입니다. 모든 열린 구간을 포함하는 가장 작은 σ-대수로, 우리가 확률을 논하는 거의 모든 사건이 여기 포함됩니다.

**수식 박스 3**:
- 레이블: `Borel σ-대수`
- 수식:
```
\mathcal{B}(\mathbb{R}) = \sigma\!\left(\{(a, b) : a, b \in \mathbb{R}\}\right)
```
- 주석:
  - 모든 열린 구간 (a,b)를 포함하는 가장 작은 σ-대수
  - 여기서 생성(σ)이란 주어진 집합들을 포함하는 최소 σ-대수
  - 포함되는 집합: 열린/닫힌/반열린 구간, 가산 집합, 칸토르 집합...
  - 포함 안 되는 집합: 비가산 번에 걸쳐 선택이 필요한 집합 (존재하지만 측정 불가)

**개념 카드**:
- 제목: `🔑 측정 가능 함수 (Measurable Function) — AI 연결`
- 내용:
  - 확률변수 X: Ω → ℝ이 측정 가능하다는 것: X⁻¹(B) ∈ ℱ, ∀B ∈ ℬ(ℝ)
  - "X가 Borel 측정 가능" = 우리가 관심 있는 사건 {X ∈ B}에 확률을 말할 수 있음
  - AI에서: 손실함수 ℒ(θ, ω)가 ω에 대해 측정 가능해야 E[ℒ]이 정의됨

---

### 섹션 4: 확률 측도의 성질

**아이콘**: `P`  
**제목**: `확률 측도의 공리`

**수식 박스 4**:
- 레이블: `Kolmogorov 확률 공리`
- 수식:
```
P: \mathcal{F} \to [0,1] \text{ is a probability measure if:}
\begin{cases}
(1)\; P(\Omega) = 1 & \text{(전체 확률)} \\
(2)\; P(A) \geq 0,\ \forall A \in \mathcal{F} & \text{(비음성)} \\
(3)\; P\!\left(\bigsqcup_{n=1}^\infty A_n\right) = \sum_{n=1}^\infty P(A_n) & \text{(σ-가산 가법성)}
\end{cases}
```
- 주석:
  - ⊔: 서로소 합집합 (disjoint union)
  - (3)이 핵심: 서로소인 사건들의 확률은 더할 수 있다
  - P(Aᶜ) = 1 - P(A), P(∅) = 0, P(A∪B) = P(A)+P(B)-P(A∩B) — 모두 여기서 유도

---

## 인터랙티브: σ-대수 구성 탐색기

**제목**: `🔢 {a, b, c} 위의 σ-대수 — 가능한 사건 목록`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `sig-pick` | 추가할 부분집합 선택 | — | — | — | {a} |

**JavaScript 로직** (슬라이더 대신 버튼 선택 UI):
```javascript
// Ω = {a, b, c}의 모든 부분집합
const subsets = [
  '∅', '{a}', '{b}', '{c}',
  '{a,b}', '{a,c}', '{b,c}', '{a,b,c}'
];
const complements = {
  '∅': '{a,b,c}', '{a}': '{b,c}', '{b}': '{a,c}', '{c}': '{a,b}',
  '{a,b}': '{c}', '{a,c}': '{b}', '{b,c}': '{a}', '{a,b,c}': '∅'
};

function checkSigmaAlgebra(selected) {
  const set = new Set(selected);
  // 항상 ∅과 Ω 포함
  const msgs = [];
  if (!set.has('∅')) msgs.push('❌ ∅ 미포함');
  if (!set.has('{a,b,c}')) msgs.push('❌ Ω={a,b,c} 미포함');
  // 여사건 확인
  for (const s of set) {
    const comp = complements[s];
    if (comp && !set.has(comp)) msgs.push(`❌ ${s}의 여사건 ${comp} 미포함`);
  }
  return msgs.length === 0 ? '✅ 올바른 σ-대수!' : msgs.join('<br/>');
}

function updateSigma() {
  const examples = [
    ['∅', '{a,b,c}'],
    ['∅', '{a}', '{b,c}', '{a,b,c}'],
    ['∅', '{a}', '{b}', '{c}', '{a,b}', '{a,c}', '{b,c}', '{a,b,c}']
  ];
  const idx = parseInt(document.getElementById('sig-pick').value);
  const sel = examples[idx];
  const result = checkSigmaAlgebra(sel);
  document.getElementById('sig-res').innerHTML =
    `선택된 집합: ${sel.join(', ')}<br/>${result}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       최소 σ-대수: {∅, Ω} | 최대: Ω의 멱집합 2^Ω
     </span>`;
}
updateSigma();
```

---

## 퀴즈

**질문**: σ-대수 ℱ가 {∅, {a}, {b,c}, {a,b,c}}를 포함할 때, {b}가 포함되어 있지 않다면?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) ℱ는 여전히 올바른 σ-대수다 | ✅ |
| (b) {b}가 없으면 σ-대수가 아니다 | ❌ |
| (c) {b,c}가 있으면 {b}도 반드시 있어야 한다 | ❌ |
| (d) {b}의 여사건은 {a,c}이므로 {a,c}도 없어야 한다 | ❌ |

**정답 위치**: (a)

---

## 주간 체크리스트 (Week 21)

```
[ ] 확률 공간 (Ω, ℱ, P)의 각 요소를 설명할 수 있다
[ ] σ-대수의 3가지 성질을 나열할 수 있다
[ ] 유한 표본 공간에서 σ-대수 예시를 직접 만들 수 있다
[ ] Borel σ-대수 ℬ(ℝ)이 무엇인지 설명할 수 있다
[ ] 측정 가능 함수가 왜 필요한지 설명할 수 있다
[ ] Kolmogorov 확률 공리 3가지를 쓸 수 있다
[ ] 논문에서 "a.s."가 어떤 확률 공간 위의 개념인지 안다
```
