# Week 05 — 확률변수와 확률분포 (Ch.5 Part 1)

> **파일명**: `chapters/ch05.html`  
> **대응 섹션**: `c5`  
> **인터랙티브**: ✅ 정규분포 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.5 · 확률 & 통계 (5주차)` |
| 제목 | `확률변수와 확률분포` |
| 서브타이틀 | `랜덤한 일을 숫자로 표현하고, 그 숫자들이 어떻게 분포하는지 배웁니다.` |
| 이전 챕터 | `ch04.html` — Ch.4 확률론 기초 |
| 다음 챕터 | `ch06.html` — Ch.6 기대값과 분산 |

---

## EASY BOX 내용

**핵심 비유**: "확률변수는 랜덤하게 일어나는 일을 숫자로 바꾼 것이다. 분포는 '어떤 값이 얼마나 자주 나오는지 보여주는 지도'다."

**예시 리스트**:
- 동전 앞면=1, 뒷면=0 → 이제 계산이 가능해짐!
- 주사위 결과 1~6 → 평균, 분산 등을 수학적으로 다룰 수 있음
- AI에서: 단어 → 숫자(token ID), 이미지 → 픽셀값 배열. 모든 것이 확률변수로 모델링됨

---

## 섹션 구성

### 섹션 1: 확률변수 (Random Variable)

**아이콘**: `X`  
**제목**: `확률변수 (Random Variable)`

**설명**: 확률변수는 표본공간 Ω의 각 결과에 실수를 할당하는 함수입니다. 확률변수를 통해 "사건"을 "숫자"로 변환하여 수학적으로 계산할 수 있습니다.

**표 (이산형 vs 연속형)**:

| 구분 | 설명 | 예시 | AI 활용 |
|------|------|------|---------|
| 이산형 | 셀 수 있는 값 | 주사위, 클래스 레이블 | 분류 출력 y ∈ {0,1,...,K} |
| 연속형 | 연속적인 값 (무한히 많음) | 온도, 가격, 픽셀 강도 | 회귀 출력, 잠재 벡터 |

**표 (PMF vs PDF vs CDF)**:

| 함수 | 이름 | 적용 | 의미 |
|------|------|------|------|
| PMF | 확률질량함수 | 이산형 | P(X=k) — k가 나올 확률 |
| PDF | 확률밀도함수 | 연속형 | f(x) — x 근방의 확률 밀도 |
| CDF | 누적분포함수 | 둘 다 | F(x) = P(X ≤ x) |

---

### 섹션 2: 주요 분포

**아이콘**: `📊`  
**제목**: `AI에서 자주 등장하는 확률분포`

**표 (주요 분포 비교)**:

| 분포 | 파라미터 | 평균 | AI 활용 |
|------|---------|------|---------|
| Bernoulli Bern(p) | p ∈ [0,1] | p | 이진 분류 (sigmoid 출력) |
| Binomial Bin(n,p) | n, p | np | n번 중 성공 횟수 |
| Categorical Cat(p) | p ∈ Δᴷ | — | 다중 분류 (softmax 출력) |
| Gaussian N(μ,σ²) | μ, σ² | μ | 회귀, 가중치 초기화, VAE |
| Exponential Exp(λ) | λ > 0 | 1/λ | 이벤트 간격 |

---

### 섹션 3: 정규분포 (Gaussian)

**아이콘**: `🔔`  
**제목**: `정규분포 (Normal/Gaussian Distribution)`

**설명**: 자연계와 데이터에서 가장 흔하게 나타나는 분포. 중심극한정리에 의해 충분히 많은 샘플의 합은 정규분포에 수렴합니다.

**수식 박스 1**:
- 레이블: `정규분포 확률밀도함수 (PDF)`
- 수식: `\mathcal{N}(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\!\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)`
- 주석:
  - `μ`: 평균 — 분포의 중심
  - `σ²`: 분산 — 분포의 퍼짐 정도
  - `σ`: 표준편차 = √σ²

**callout (tip)**:
- 제목: `정규분포가 자주 쓰이는 이유`
- 내용: **중심극한정리(CLT)**: 독립적인 확률변수들의 합은 n이 커질수록 정규분포에 수렴. 딥러닝 가중치 초기화, 노이즈 모델링, VAE 잠재 공간 등에 사용.

**수식 박스 2 (다변량 정규분포)**:
- 레이블: `다변량 정규분포 (논문에서 자주 등장)`
- 수식: `\mathcal{N}(\mathbf{x} \mid \boldsymbol{\mu}, \boldsymbol{\Sigma}) = \frac{1}{(2\pi)^{d/2}|\boldsymbol{\Sigma}|^{1/2}} \exp\!\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^\top\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)`
- 주석:
  - `μ ∈ ℝᵈ`: 평균 벡터
  - `Σ ∈ ℝ^{d×d}`: 공분산 행렬 (대칭, 양의 정부호)
  - `d`: 차원 수

---

## 인터랙티브: 정규분포 계산기

**제목**: `🔢 정규분포 계산기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `nd-mu` | 평균 μ | -5 | 5 | 0.1 | 0 |
| `nd-sg` | 표준편차 σ | 0.1 | 3 | 0.1 | 1 |
| `nd-x` | 관측값 x | -5 | 5 | 0.1 | 1 |

**JavaScript 로직**:
```javascript
function ndPDF(x, mu, sg) {
  return (1 / (sg * Math.sqrt(2 * Math.PI))) * Math.exp(-0.5 * ((x - mu) / sg) ** 2);
}
function updateND() {
  const mu = parseFloat(document.getElementById('nd-mu').value);
  const sg = parseFloat(document.getElementById('nd-sg').value);
  const x  = parseFloat(document.getElementById('nd-x').value);
  document.getElementById('nd-mu-v').value = mu.toFixed(1);
  document.getElementById('nd-sg-v').value = sg.toFixed(1);
  document.getElementById('nd-x-v').value  = x.toFixed(1);

  const pdf = ndPDF(x, mu, sg);
  const z = (x - mu) / sg;
  document.getElementById('nd-res').innerHTML =
    `f(${x.toFixed(1)}) = <strong>${pdf.toFixed(5)}</strong> &nbsp;|&nbsp; z-score = <strong>${z.toFixed(3)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">μ=${mu.toFixed(1)}, σ=${sg.toFixed(1)}, σ²=${(sg*sg).toFixed(2)}</span>`;
}
updateND();
```

---

## 퀴즈

**질문**: `N(5, 4)`의 평균과 표준편차는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 평균 5, 표준편차 4 | ❌ |
| (b) 평균 5, 표준편차 2 (σ²=4이므로 σ=√4=2) | ✅ |
| (c) 평균 4, 표준편차 5 | ❌ |
| (d) 평균 5, 표준편차 16 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 5)

```
[ ] 사건과 확률변수의 차이를 설명할 수 있다
[ ] 이산형과 연속형 확률변수를 구분할 수 있다
[ ] PMF, PDF, CDF를 각각 설명할 수 있다
[ ] Bernoulli, Gaussian 분포를 설명할 수 있다
[ ] Categorical 분포가 softmax와 연결됨을 안다
[ ] N(μ, σ²)에서 μ와 σ²의 의미를 설명할 수 있다
```
