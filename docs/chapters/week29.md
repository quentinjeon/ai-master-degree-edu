# Week 29 — VC 차원·Rademacher 복잡도 (Ch.29)

> **파일명**: `chapters/ch29.html`  
> **Phase**: Phase 3-A · 통계적 학습 이론  
> **인터랙티브**: ✅ VC 차원 시각화  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.29 · 통계적 학습 이론 (Phase 3 — 3주차)` |
| 제목 | `VC 차원과 Rademacher 복잡도` |
| 서브타이틀 | `무한 가설 클래스의 복잡도를 측정합니다. 신경망의 일반화 경계, "VC dimension d → n = O(d/ε²)"의 수학.` |
| 이전 챕터 | `ch28.html` — Ch.28 PAC 학습 이론 |
| 다음 챕터 | `ch30.html` — Ch.30 딥러닝 일반화 미스터리 |

---

## EASY BOX 내용

**핵심 비유**: "선형 분류기로 평면 위의 점들을 얼마나 다양하게 나눌 수 있는가? 3개 점은 어떻게 놓아도 가능하지만 4개 점은 불가능한 배치가 있다. 이 '최대 분류 가능 점 수'가 VC 차원이다."

**예시 리스트**:
- 1D 임계값 분류기: VC 차원 = 1 (점 1개는 분류 가능, 2개는 불가)
- 2D 선형 분류기: VC 차원 = 3 (3점은 가능, 4점은 XOR 불가)
- AI에서: 신경망의 VC 차원 = O(W log W) (W = 파라미터 수) → 일반화 경계가 이를 통해 유도되지만, 딥러닝의 실제 성능은 이론보다 훨씬 좋다 (Ch.30에서 다룸)

---

## 섹션 구성

### 섹션 1: Shattering과 VC 차원

**아이콘**: `V`  
**제목**: `VC 차원 — "분류 가능성"의 척도`

**설명**: 가설 클래스 ℋ가 점 집합 C를 "분쇄(shatter)"한다는 것은 C의 모든 가능한 이진 라벨 할당을 ℋ 안의 어떤 h가 완벽히 분류할 수 있다는 뜻입니다. VC 차원은 ℋ가 분쇄할 수 있는 최대 집합 크기입니다.

**수식 박스 1**:
- 레이블: `VC 차원의 정의`
- 수식:
```
\mathrm{VCdim}(\mathcal{H}) = \max\{|C| : \mathcal{H} \text{ shatters } C\}
```
- 주석:
  - `C`: 점들의 집합
  - "H가 C를 분쇄": ∀(y₁,...,y|C|) ∈ {0,1}|C|, ∃h ∈ H s.t. h(xᵢ) = yᵢ for all i
  - VCdim = d이면 크기 d인 집합은 분쇄 가능, 크기 d+1인 모든 집합은 분쇄 불가
  - 주의: 크기 d+1인 "어떤 집합"이 분쇄 가능해도 d+1보다 클 수 있다

**표 (주요 가설 클래스의 VC 차원)**:

| 가설 클래스 | 차원 | VC 차원 |
|------------|------|---------|
| 1D 임계값 h(x) = 1[x>θ] | 1 | 1 |
| 2D 선형 분류기 | 2 | 3 |
| d차원 선형 분류기 | d | d+1 |
| k차원 공에서의 선형 분류기 | — | k+1 |
| 신경망 (W 파라미터) | — | O(W log W) |
| 결정트리 (깊이 d) | — | O(2ᵈ) |

---

### 섹션 2: Fundamental Theorem of Statistical Learning

**아이콘**: `∗`  
**제목**: `통계적 학습의 기본 정리`

**설명**: VC 차원이 유한하면 PAC 학습 가능하고, 무한하면 PAC 학습 불가능합니다. 이것이 통계적 학습 이론의 핵심 정리입니다.

**수식 박스 2**:
- 레이블: `VC 차원 기반 일반화 경계`
- 수식:
```
P\!\left[L_D(h_S) - L_S(h_S) \leq \sqrt{\frac{2d\ln(en/d) + 2\ln(4/\delta)}{n}}\right] \geq 1-\delta
```
- 주석:
  - `d = VCdim(ℋ)`: VC 차원
  - 우변이 ε이 되도록 풀면 표본 복잡도: n = O(d/ε² + log(1/δ)/ε²)
  - 상수 e ≈ 2.718은 자연상수 (Sauer's Lemma에서 유래)
  - 핵심: n이 충분하면 훈련-검증 오차 차이가 ε 이하

**수식 박스 3** (표본 복잡도 요약):
- 레이블: `VC 기반 표본 복잡도`
- 수식:
```
m_\mathcal{H}(\varepsilon, \delta) = O\!\left(\frac{d + \log(1/\delta)}{\varepsilon^2}\right)
```
- 주석:
  - d = VC 차원, ε = 정확도, δ = 신뢰도
  - d차원 선형 분류기: mℋ = O(d/ε²) — 파라미터 수에 선형 비례
  - 신경망(W 파라미터): mℋ = O(W log W / ε²) — log 인자 추가

---

### 섹션 3: Rademacher 복잡도

**아이콘**: `R`  
**제목**: `Rademacher 복잡도 — 더 날카로운 일반화 경계`

**설명**: VC 차원은 이진 분류에만 적용되고 분포와 무관합니다. Rademacher 복잡도는 특정 분포와 표본에 의존하는 더 정밀한 복잡도 척도입니다.

**수식 박스 4**:
- 레이블: `경험적 Rademacher 복잡도`
- 수식:
```
\hat{\mathcal{R}}_S(\mathcal{H}) = \mathbb{E}_{\boldsymbol{\sigma}}\!\left[\sup_{h \in \mathcal{H}} \frac{1}{n}\sum_{i=1}^n \sigma_i h(x_i)\right]
```
- 주석:
  - `σᵢ ∈ {-1, +1}`: i.i.d. Rademacher 변수 (균등 분포)
  - "무작위 노이즈 라벨과 얼마나 상관관계를 가질 수 있는가"
  - 값이 클수록 ℋ가 더 복잡 (무작위 패턴도 맞출 수 있음)
  - Rademacher 복잡도 = 0 → 모든 h가 같은 예측 = 복잡도 없음

**수식 박스 5** (Rademacher 기반 일반화 경계):
- 레이블: `Rademacher 일반화 경계`
- 수식:
```
P\!\left[L_D(h) \leq L_S(h) + 2\mathcal{R}_n(\mathcal{H}) + \sqrt{\frac{\ln(2/\delta)}{2n}}\right] \geq 1-\delta
```
- 주석:
  - `ℛₙ(ℋ)`: 표본 크기 n에서의 Rademacher 복잡도
  - VC 경계보다 분포에 따라 더 타이트할 수 있음
  - 딥러닝에서: 실제로 Rademacher 복잡도가 낮아서 잘 일반화됨 (Ch.30)

---

### 섹션 4: Sauer's Lemma — 성장 함수

**아이콘**: `S`  
**제목**: `Sauer's Lemma — VC 차원에서 경계 유도의 핵심`

**설명**: 유한 ℋ 경계를 무한 ℋ로 확장할 때 필요한 수학적 도구입니다. 성장 함수(growth function)가 VC 차원으로 상한이 결정됩니다.

**수식 박스 6**:
- 레이블: `Sauer's Lemma`
- 수식:
```
\Pi_\mathcal{H}(n) \leq \sum_{i=0}^{d}\binom{n}{i} \leq \left(\frac{en}{d}\right)^d
```
- 주석:
  - `ΠH(n)`: 성장 함수 = n개 점에서 ℋ가 만들 수 있는 서로 다른 이진 패턴 수
  - d = VCdim(ℋ)
  - n ≤ d면 최대 2ⁿ, n > d면 다항식 (nᵈ)으로 증가
  - 이 다항식 증가가 일반화 경계를 유도할 때 핵심으로 쓰임

---

## 인터랙티브: VC 차원 시각화

**제목**: `🔢 2D 선형 분류기의 VC 차원 — VC = 3`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `vc-n` | 점 수 n | 2 | 5 | 1 | 3 |

**JavaScript 로직**:
```javascript
function updateVC() {
  const n = parseInt(document.getElementById('vc-n').value);
  document.getElementById('vc-n-v').value = n;

  // 성장 함수 (d=3, 2D 선형 분류기)
  const d = 3;
  const twoN = Math.pow(2, n);

  // Sauer's lemma 상한: Σ C(n,i) for i=0..d
  let sauer = 0;
  for (let i = 0; i <= Math.min(n, d); i++) {
    let comb = 1;
    for (let j = 0; j < i; j++) {
      comb = comb * (n - j) / (j + 1);
    }
    sauer += comb;
  }

  const canShatter = n <= d ? '✅ 분쇄 가능 (적절한 배치 존재)' : '❌ 분쇄 불가 (어떤 배치도 불가)';
  const growthDesc = n <= d ? `최대 ${twoN}개 패턴 (= 2ⁿ, 모두 가능)` : `최대 ${sauer}개 패턴 < ${twoN} (일부 불가)`;

  document.getElementById('vc-res').innerHTML =
    `2D 선형 분류기의 VC 차원 = ${d}<br/>
     n = ${n}개 점에 대해: ${canShatter}<br/>
     이분 수 (성장 함수): <strong>${sauer}</strong> (Sauer 상한)<br/>
     최대 가능 이분 수: 2ⁿ = <strong>${twoN}</strong><br/>
     ${growthDesc}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       n=4일 때 XOR 패턴이 불가: 선형 분류기 한계
     </span>`;
}
updateVC();
```

---

## 퀴즈

**질문**: d차원 선형 분류기(초평면)의 VC 차원은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) d | ❌ |
| (b) d+1 | ✅ |
| (c) 2d | ❌ |
| (d) d² | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 29)

```
[ ] VC 차원의 정의(분쇄)를 설명할 수 있다
[ ] 2D 선형 분류기의 VC 차원이 3임을 예시로 설명할 수 있다
[ ] VC 차원 기반 표본 복잡도 O(d/ε²)를 설명할 수 있다
[ ] 통계적 학습의 기본 정리(유한 VCdim ↔ PAC 학습 가능)를 설명할 수 있다
[ ] Rademacher 복잡도의 직관적 의미를 설명할 수 있다
[ ] Sauer's Lemma가 하는 역할을 설명할 수 있다
[ ] 신경망의 VC 차원이 크지만 실제로는 잘 일반화되는 것이 왜 역설인지 설명할 수 있다
```
