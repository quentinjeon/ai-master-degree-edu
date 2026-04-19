# Week 22 — 확률변수의 수렴 (Ch.22)

> **파일명**: `chapters/ch22.html`  
> **Phase**: Phase 2-C · 측도 기반 확률론  
> **인터랙티브**: ✅ 대수의 법칙 시뮬레이터  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.22 · 측도 확률론 (Phase 2 — 8주차)` |
| 제목 | `확률변수의 수렴 — a.s., 확률 수렴, 분포 수렴` |
| 서브타이틀 | `"거의 확실하게 수렴"이 뭔지 정확히 안다. LLN, CLT의 수학적 기반을 이해합니다.` |
| 이전 챕터 | `ch21.html` — Ch.21 σ-대수·확률공간 |
| 다음 챕터 | `ch23.html` — Ch.23 기대값의 측도론적 정의 |

---

## EASY BOX 내용

**핵심 비유**: "동전을 무한히 던지면 앞면 비율이 1/2에 가까워진다. 하지만 '항상 정확히' 가까워지는 건 아니다. 어쩌다 한 번 먼 날도 있을 수 있다. 이 차이를 정밀하게 구분하는 게 a.s. vs 확률 수렴이다."

**예시 리스트**:
- 거의 확실 수렴(a.s.): 무한 번 던지면 앞면 비율이 0.5가 안 될 확률 = 0 (대수의 법칙)
- 확률 수렴(in prob): 반복 횟수 늘수록 비율이 0.5와 ε 이상 다를 확률 → 0
- AI에서: SGD의 그라디언트 추정량이 진짜 그라디언트에 "확률 수렴"하는 것이 이론 분석의 기반

---

## 섹션 구성

### 섹션 1: 수렴의 4가지 종류

**아이콘**: `→`  
**제목**: `확률변수 수렴의 4가지 개념`

**표 (수렴 강도 비교)**:

| 수렴 종류 | 기호 | 강도 | 직관 |
|----------|------|------|------|
| 거의 확실 수렴 | Xₙ →ᵃˢ X | 가장 강함 | 확률 0인 예외 제외하고 점별 수렴 |
| 확률 수렴 | Xₙ →ᴾ X | 중간 | 차이가 클 확률 → 0 |
| Lᵖ 수렴 | Xₙ →^{Lp} X | 중간 | p번째 모멘트 수렴 |
| 분포 수렴 | Xₙ →ᴰ X | 가장 약함 | 분포함수만 수렴 |

**callout (info)**:
- 내용: 함의 관계: a.s. 수렴 ⟹ 확률 수렴 ⟹ 분포 수렴. 역방향은 일반적으로 성립 안 함. 논문에서 어떤 수렴 개념을 쓰는지 확인하는 것이 중요하다.

---

### 섹션 2: 거의 확실 수렴 (Almost Sure Convergence)

**아이콘**: `a.s.`  
**제목**: `거의 확실 수렴 — 가장 강한 수렴`

**수식 박스 1**:
- 레이블: `a.s. 수렴의 정의`
- 수식:
```
X_n \xrightarrow{a.s.} X
\iff
P\!\left(\left\{\omega \in \Omega : \lim_{n\to\infty} X_n(\omega) = X(\omega)\right\}\right) = 1
```
- 주석:
  - `ω`: 표본 공간의 각 원소 (각 실험 결과)
  - "확률 1인 사건 집합 위에서 점별 수렴"
  - 동치: P(|Xₙ - X| > ε 무한히 자주) = 0, ∀ε > 0
  - 대수의 법칙(LLN): X̄ₙ = (1/n)ΣXᵢ →ᵃˢ E[X] (강한 LLN)

---

### 섹션 3: 확률 수렴 (Convergence in Probability)

**아이콘**: `P→`  
**제목**: `확률 수렴 — 편한 개념, 많이 쓰임`

**수식 박스 2**:
- 레이블: `확률 수렴의 정의`
- 수식:
```
X_n \xrightarrow{P} X
\iff
\forall \varepsilon > 0,\quad \lim_{n \to \infty} P\!\left(|X_n - X| > \varepsilon\right) = 0
```
- 주석:
  - 차이가 ε보다 클 확률이 0에 수렴한다
  - a.s. 수렴보다 약하지만, 실용적으로 훨씬 많이 쓰임
  - 약한 LLN: X̄ₙ →ᴾ E[X] (약한 LLN — a.s.보다 약한 버전)
  - SGD 분석: ‖∇̃f - ∇f‖ →ᴾ 0 — 그라디언트 추정의 일관성

---

### 섹션 4: 분포 수렴과 CLT

**아이콘**: `📊`  
**제목**: `분포 수렴 — 중심극한정리(CLT)의 언어`

**수식 박스 3**:
- 레이블: `분포 수렴 (분포함수 기준)`
- 수식:
```
X_n \xrightarrow{D} X
\iff
\lim_{n\to\infty} F_{X_n}(x) = F_X(x) \quad \text{at all continuity points of } F_X
```
- 주석:
  - 분포함수 Fₓₙ(x) = P(Xₙ ≤ x)가 Fₓ(x)로 수렴
  - "약한 수렴(weak convergence)"이라고도 함
  - 확률변수 자체가 같아질 필요 없음 — 분포만 같아지면 됨

**수식 박스 4** (중심극한정리):
- 레이블: `중심극한정리 (CLT)`
- 수식:
```
\frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \xrightarrow{D} \mathcal{N}(0, 1)
\quad\text{as } n \to \infty
```
- 주석:
  - E[Xᵢ] = μ, Var[Xᵢ] = σ²인 i.i.d. 확률변수들의 표본평균
  - 표준화된 표본평균이 표준정규분포로 분포 수렴
  - 딥러닝: SGD 그라디언트 노이즈의 정규 근사, 초기화 이론의 기반
  - Bootstrap: CLT를 통한 신뢰구간 추정에 활용

**표 (LLN vs CLT 비교)**:

| 정리 | 다루는 것 | 수렴 종류 | 의미 |
|------|---------|---------|------|
| 강한 LLN | X̄ₙ | a.s. 수렴 | 표본평균은 거의 확실히 모평균으로 |
| 약한 LLN | X̄ₙ | 확률 수렴 | 표본평균은 확률적으로 모평균으로 |
| CLT | √n(X̄ₙ-μ)/σ | 분포 수렴 | 표준화된 표본평균 → N(0,1) |

---

## 인터랙티브: 대수의 법칙 시뮬레이터

**제목**: `🔢 동전 던지기 LLN — n이 커질수록 평균이 0.5로 수렴`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `lln-n` | 시뮬레이션 동전 수 n | 10 | 1000 | 10 | 100 |
| `lln-p` | 진짜 앞면 확률 p | 0.1 | 0.9 | 0.05 | 0.5 |

**JavaScript 로직**:
```javascript
function updateLLN() {
  const n = parseInt(document.getElementById('lln-n').value);
  const p = parseFloat(document.getElementById('lln-p').value);
  document.getElementById('lln-n-v').value = n;
  document.getElementById('lln-p-v').value = p.toFixed(2);

  // n번 베르누이 시뮬레이션
  let heads = 0;
  for (let i = 0; i < n; i++) {
    if (Math.random() < p) heads++;
  }
  const empirical = heads / n;
  const error = Math.abs(empirical - p);
  const se = Math.sqrt(p * (1 - p) / n); // 이론적 표준오차

  document.getElementById('lln-res').innerHTML =
    `n = ${n}번 중 앞면 ${heads}번<br/>
     경험 확률: <strong>${empirical.toFixed(4)}</strong> vs 진짜 p = ${p.toFixed(2)}<br/>
     오차 |p̂ - p| = <strong>${error.toFixed(4)}</strong><br/>
     이론적 표준오차 (SE) = σ/√n = ${se.toFixed(4)}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       n이 클수록 오차가 작아진다 — LLN 실증
     </span>`;
}
updateLLN();
```

---

## 퀴즈

**질문**: "Xₙ →ᵃˢ X (a.s. 수렴)"과 "Xₙ →ᴾ X (확률 수렴)"의 관계는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 완전히 같은 개념이다 | ❌ |
| (b) 확률 수렴이 a.s. 수렴보다 강하다 | ❌ |
| (c) a.s. 수렴이면 확률 수렴이 따라온다 (역은 일반적으로 성립 안 함) | ✅ |
| (d) 두 개념은 독립적이다 | ❌ |

**정답 위치**: (c)

---

## 주간 체크리스트 (Week 22)

```
[ ] 수렴 4종류(a.s., 확률, Lp, 분포)를 정의로 쓸 수 있다
[ ] a.s. 수렴과 확률 수렴의 차이를 예시로 설명할 수 있다
[ ] 강한 LLN과 약한 LLN의 차이를 설명할 수 있다
[ ] CLT를 수식으로 쓰고 직관적으로 설명할 수 있다
[ ] 논문에서 "a.s. convergence"를 보면 무슨 뜻인지 안다
[ ] SGD 수렴 분석에서 어떤 수렴 개념이 쓰이는지 안다
```
