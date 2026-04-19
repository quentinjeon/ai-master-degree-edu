# Week 27 — 집중 부등식 (Ch.27)

> **파일명**: `chapters/ch27.html`  
> **Phase**: Phase 3-A · 통계적 학습 이론  
> **인터랙티브**: ✅ Hoeffding 경계 계산기  
> **우선순위**: P0 (Phase 3 첫 챕터)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.27 · 통계적 학습 이론 (Phase 3 — 1주차)` |
| 제목 | `집중 부등식 — 확률을 확정적으로 묶는다` |
| 서브타이틀 | `Markov, Chebyshev, Hoeffding, Bernstein. 논문의 "with high probability" 주장이 어디서 오는지 이해합니다.` |
| 이전 챕터 | `ch26.html` — Ch.26 행렬 미분 |
| 다음 챕터 | `ch28.html` — Ch.28 PAC 학습 이론 |

---

## EASY BOX 내용

**핵심 비유**: "동전을 100번 던질 때 앞면이 90번 나올 확률은 매우 낮다. 집중 부등식은 '얼마나 낮은지'를 수학적으로 보장하는 도구다 — 논문에서 'with probability at least 1-δ'가 이것이다."

**예시 리스트**:
- Markov: '평균이 μ인 양수 확률변수가 10μ를 넘을 확률은 최대 1/10' — 매우 느슨하지만 항상 성립
- Chebyshev: '평균에서 k표준편차 이상 떨어질 확률 ≤ 1/k²' — 분포 가정 없이 성립
- AI에서: 훈련 오차가 일반화 오차와 얼마나 가까운지 수학적으로 증명하는 것이 통계적 학습 이론의 핵심 — 이 증명이 집중 부등식으로 이루어진다

---

## 섹션 구성

### 섹션 1: Markov 부등식

**아이콘**: `M`  
**제목**: `Markov 부등식 — 가장 기본적인 확률 경계`

**설명**: 양수 확률변수에 대한 가장 단순한 경계. 분포에 대한 가정 없이 평균(기대값)만으로 꼬리 확률을 제한합니다.

**수식 박스 1**:
- 레이블: `Markov 부등식`
- 수식:
```
X \geq 0 \Rightarrow P(X \geq t) \leq \frac{\mathbb{E}[X]}{t}, \quad t > 0
```
- 주석:
  - `X ≥ 0`: 양수(비음수) 확률변수여야 함
  - `t`: 임계값 (경계를 구하고 싶은 값)
  - `E[X]/t`: 확률의 상한 — t가 클수록 경계가 타이트해짐
  - 증명: E[X] = ∫ x dP ≥ ∫_{X≥t} x dP ≥ t · P(X ≥ t)
  - 실전 활용: t = k·E[X]로 놓으면 P(X ≥ kμ) ≤ 1/k

**callout (warn)**:
- 내용: Markov 부등식은 매우 **느슨한** 경계를 줍니다. 예를 들어 P(X ≥ 2E[X]) ≤ 1/2인데, 실제로는 훨씬 작을 수 있습니다. 더 강한 가정(분산, 유계성)이 있으면 더 좋은 경계를 얻을 수 있습니다.

---

### 섹션 2: Chebyshev 부등식

**아이콘**: `σ`  
**제목**: `Chebyshev 부등식 — 분산으로 경계`

**설명**: 분산(2차 모멘트)을 활용하면 Markov보다 훨씬 좋은 경계를 얻을 수 있습니다. 이것이 Chebyshev 부등식입니다.

**수식 박스 2**:
- 레이블: `Chebyshev 부등식`
- 수식:
```
P(|X - \mu| \geq k\sigma) \leq \frac{1}{k^2}
\quad\text{동치:}\quad
P(|X - \mu| \geq t) \leq \frac{\mathrm{Var}[X]}{t^2}
```
- 주석:
  - `μ = E[X]`: 평균
  - `σ² = Var[X]`: 분산
  - `k`: 표준편차 단위로 측정한 이탈 거리
  - 증명: Markov 부등식을 `(X-μ)²`에 적용 → P(|X-μ| ≥ t) = P((X-μ)² ≥ t²) ≤ Var[X]/t²
  - 약한 LLN의 증명: X̄ₙ에 Chebyshev 적용 → Var[X̄ₙ] = σ²/n → 0

**callout (tip)**:
- 내용: Chebyshev는 **분포 가정 없이** 성립합니다. 정규분포에서 3σ를 넘을 확률이 실제로는 0.003이지만, Chebyshev는 ≤ 1/9 ≈ 0.11만 보장합니다. 더 강한 가정(예: 유계)이 있으면 훨씬 좋은 경계를 얻을 수 있습니다.

---

### 섹션 3: Hoeffding 부등식

**아이콘**: `H`  
**제목**: `Hoeffding 부등식 — 유계 확률변수의 지수 경계`

**설명**: 확률변수가 유계(bounded)이면 단순 분산이 아니라 구간 너비만으로 훨씬 tight한 지수 경계를 얻습니다. 이것이 통계적 학습 이론에서 가장 많이 쓰이는 집중 부등식입니다.

**수식 박스 3**:
- 레이블: `Hoeffding 부등식`
- 수식:
```
X_1, \ldots, X_n \text{ 독립}, \; a_i \leq X_i \leq b_i
\Rightarrow
P\!\left(\bar{X} - \mathbb{E}[\bar{X}] \geq t\right) \leq \exp\!\left(-\frac{2n^2t^2}{\sum_{i=1}^n (b_i - a_i)^2}\right)
```
- 주석:
  - `bᵢ - aᵢ`: i번째 변수의 구간 너비
  - `n`: 표본 크기
  - `t`: 이탈 크기 (평균과의 차이)
  - 동일 구간 [0,1] 가정 시: P(|X̄ - μ| ≥ t) ≤ 2exp(-2nt²)
  - 핵심: 지수 감소! n이 커질수록 경계가 매우 빠르게 0에 수렴

**수식 박스 4** (PAC 학습으로 연결):
- 레이블: `표본 복잡도 — ε,δ 프레임`
- 수식:
```
P\!\left(|\hat{\mu} - \mu| \geq \varepsilon\right) \leq \delta
\quad\Longleftrightarrow\quad
n \geq \frac{\ln(2/\delta)}{2\varepsilon^2}
```
- 주석:
  - `ε`: 허용 오차 (accuracy)
  - `δ`: 실패 확률 (confidence = 1-δ)
  - "ε-accurate with probability 1-δ"를 달성하기 위한 최소 표본 수
  - 논문에서: "For n ≥ log(1/δ)/ε² samples, with probability 1-δ..."

---

### 섹션 4: Bernstein 부등식과 McDiarmid 부등식

**아이콘**: `B`  
**제목**: `Bernstein & McDiarmid — 더 강력한 집중 경계`

**표 (집중 부등식 비교)**:

| 부등식 | 가정 | 경계 형태 | 강도 |
|--------|------|---------|------|
| Markov | X ≥ 0 | E[X]/t | 가장 약함 |
| Chebyshev | Var[X] 존재 | Var[X]/t² | 약함 |
| Hoeffding | [a,b]에 유계 | exp(-2nt²/(b-a)²) | 강함 |
| Bernstein | 유계 + 분산 경계 | exp(-t²/(2(σ²+bt/3))) | 더 강함 |
| McDiarmid | 함수가 한 변수에 민감도 c | exp(-2t²/Σcᵢ²) | 함수에 적용 |

**수식 박스 5** (McDiarmid 부등식):
- 레이블: `McDiarmid 부등식 (Bounded Differences)`
- 수식:
```
\sup_{x_i, x_i'} |f(x_1,\ldots,x_i,\ldots,x_n) - f(x_1,\ldots,x_i',\ldots,x_n)| \leq c_i
\Rightarrow
P\!\left(f(X) - \mathbb{E}[f(X)] \geq t\right) \leq \exp\!\left(-\frac{2t^2}{\sum_{i=1}^n c_i^2}\right)
```
- 주석:
  - `cᵢ`: i번째 변수를 바꿀 때 f가 최대로 변하는 양 (민감도)
  - f가 어떤 함수든 적용 가능 — 경험적 위험(empirical risk)에 자주 사용
  - AI에서: "훈련 손실이 개별 샘플 하나를 바꿀 때 최대 2/n만큼 변한다" → McDiarmid 적용

---

## 인터랙티브: Hoeffding 경계 계산기

**제목**: `🔢 Hoeffding 부등식 — 표본 수 vs 신뢰도 트레이드오프`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `hf-n` | 표본 수 n | 10 | 1000 | 10 | 100 |
| `hf-eps` | 허용 오차 ε | 0.01 | 0.3 | 0.01 | 0.1 |

**JavaScript 로직**:
```javascript
function updateHoeffding() {
  const n = parseInt(document.getElementById('hf-n').value);
  const eps = parseFloat(document.getElementById('hf-eps').value);
  document.getElementById('hf-n-v').value = n;
  document.getElementById('hf-eps-v').value = eps.toFixed(2);

  // Hoeffding: P(|X̄ - μ| ≥ ε) ≤ 2 exp(-2nε²)  ([0,1] 유계 가정)
  const bound = 2 * Math.exp(-2 * n * eps * eps);
  const confidence = (1 - bound) * 100;

  // 역방향: ε을 달성하기 위한 최소 n (δ = 0.05)
  const delta = 0.05;
  const nRequired = Math.ceil(Math.log(2 / delta) / (2 * eps * eps));

  document.getElementById('hf-res').innerHTML =
    `표본 n=${n}, 허용 오차 ε=${eps.toFixed(2)} (X가 [0,1]에 유계)<br/>
     P(|X̄ - μ| ≥ ${eps.toFixed(2)}) ≤ <strong>${bound.toFixed(6)}</strong><br/>
     신뢰도 = 1 - δ ≥ <strong>${confidence.toFixed(2)}%</strong><br/>
     ---<br/>
     δ=0.05 (95% 신뢰도)로 ε=${eps.toFixed(2)} 달성에 필요한 n:<br/>
     n ≥ log(2/0.05) / (2ε²) = <strong>${nRequired}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       n을 늘릴수록 경계가 지수적으로 감소 (Hoeffding의 강점)
     </span>`;
}
updateHoeffding();
```

---

## 퀴즈

**질문**: Hoeffding 부등식이 Chebyshev보다 강한 경계를 주는 핵심 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 분산이 작을 때만 적용된다 | ❌ |
| (b) 분포의 유계성(bounded)을 추가로 가정하여 지수 감소 경계를 준다 | ✅ |
| (c) 표본이 많을 때는 항상 적용된다 | ❌ |
| (d) 기대값이 알려져 있어야 한다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 27)

```
[ ] Markov 부등식을 유도 수준으로 설명할 수 있다
[ ] Chebyshev 부등식을 Markov에서 유도할 수 있다
[ ] Hoeffding 부등식의 형태와 조건을 쓸 수 있다
[ ] "with probability at least 1-δ"를 수식으로 표현할 수 있다
[ ] ε-δ 프레임에서 표본 복잡도 n ≥ log(1/δ)/ε²를 설명할 수 있다
[ ] McDiarmid 부등식을 경험적 위험에 적용할 수 있다
[ ] 논문에서 집중 부등식을 보면 어떤 가정이 쓰였는지 파악할 수 있다
```
