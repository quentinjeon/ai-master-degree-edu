# Week 33 — 마팅게일·확률 과정 (Ch.33)

> **파일명**: `chapters/ch33.html`  
> **Phase**: Phase 3-B · 딥러닝 이론 심화  
> **인터랙티브**: ✅ Random Walk 시뮬레이터  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.33 · 딥러닝 이론 심화 (Phase 3 — 7주차)` |
| 제목 | `마팅게일과 확률 과정` |
| 서브타이틀 | `"공정한 게임"의 수학. Doob's inequality, Optional Stopping Theorem. SGD 수렴 증명의 확률론적 기반.` |
| 이전 챕터 | `ch32.html` — Ch.32 정보 기하학 |
| 다음 챕터 | `ch34.html` — Ch.34 고급 최적화 이론 |

---

## EASY BOX 내용

**핵심 비유**: "카지노에서 아무리 전략을 써도 평균적으로 돈을 딸 수 없는 '공정한 게임'이 마팅게일이다. 미래 기댓값이 현재 값과 같다는 조건 — 이것이 SGD의 그라디언트 노이즈를 분석하는 수학적 언어다."

**예시 리스트**:
- 마팅게일: 코인 게임에서 누적 수익 — E[Sₙ|S₁,...,Sₙ₋₁] = Sₙ₋₁ (미래 기댓값 = 현재)
- Doob's inequality: 마팅게일이 크게 뛸 확률의 상한 — 집중 부등식의 일반화
- AI에서: SGD의 파라미터 업데이트 수열이 마팅게일류(martingale-like)이고, 이 성질로 수렴을 증명

---

## 섹션 구성

### 섹션 1: 확률 과정 기초

**아이콘**: `🌊`  
**제목**: `확률 과정 — 시간에 따라 변하는 확률변수의 수열`

**설명**: 확률 과정은 시간 인덱스를 가진 확률변수의 모임입니다. 머신러닝에서 학습 과정, 강화학습에서 에이전트-환경 상호작용이 모두 확률 과정입니다.

**수식 박스 1**:
- 레이블: `확률 과정의 정의`
- 수식:
```
\{X_t\}_{t=0}^\infty,\quad X_t: \Omega \to \mathbb{R}^d
```
- 주석:
  - 각 시각 t에서 X_t는 확률변수
  - t = 0, 1, 2, ... (이산 시간) 또는 t ∈ ℝ₊ (연속 시간)
  - 샘플 경로(sample path): 고정된 ω에서 t → X_t(ω)의 궤적
  - ML에서: θ₀, θ₁, θ₂, ... (SGD 반복)이 확률 과정

**표 (중요한 확률 과정 유형)**:

| 과정 | 정의 | ML 연결 |
|------|------|---------|
| 마코프 체인 | P(Xₜ|X₀,...,Xₜ₋₁) = P(Xₜ|Xₜ₋₁) | RL의 MDP |
| 랜덤 워크 | Sₙ = X₁+...+Xₙ | SGD 경로 |
| 마팅게일 | E[Xₙ|X₁,...,Xₙ₋₁] = Xₙ₋₁ | 공정한 게임, SGD |
| 브라운 운동 | 연속 랜덤 워크 | 확률미분방정식 |

---

### 섹션 2: 마팅게일 정의와 성질

**아이콘**: `E`  
**제목**: `마팅게일 — 공정한 게임의 수학적 정의`

**수식 박스 2**:
- 레이블: `마팅게일 정의`
- 수식:
```
\{M_t\} \text{ is a martingale w.r.t. } \{\mathcal{F}_t\} \iff
\mathbb{E}[M_{t+1} \mid \mathcal{F}_t] = M_t \quad \forall t
```
- 주석:
  - `ℱₜ`: t 시점까지의 정보를 담은 σ-대수 (filtration)
  - "현재까지 알고 있는 모든 정보를 조건으로 할 때 미래 기댓값 = 현재"
  - 슈퍼마팅게일: E[Mₜ₊₁|ℱₜ] ≤ Mₜ (점점 감소하는 경향)
  - SGD 적용: fₜ = f(θₜ)의 기댓값 수열이 슈퍼마팅게일 → 수렴 증명 가능

**수식 박스 3** (마팅게일 차분 수열):
- 레이블: `마팅게일 차분 수열 — SGD 노이즈 분석`
- 수식:
```
d_t = M_t - M_{t-1} \Rightarrow \mathbb{E}[d_t | \mathcal{F}_{t-1}] = 0
\quad\text{(마팅게일 차분)}
```
- 주석:
  - 각 단계의 변화량의 조건부 기댓값 = 0
  - SGD: 미니배치 그라디언트 ∇̃f(θ)의 노이즈 εₜ = ∇̃f(θₜ) - ∇f(θₜ)가 마팅게일 차분
  - 마팅게일 차분의 합: 집중 부등식을 적용할 수 있어 SGD 수렴 증명에 활용

---

### 섹션 3: Doob의 부등식

**아이콘**: `D`  
**제목**: `Doob's Maximal Inequality — 마팅게일의 집중 경계`

**설명**: 마팅게일이 어느 시점에서나 임계값을 넘을 확률을 최대화하는 경계. 확률 알고리즘의 최악 경우 분석에 사용됩니다.

**수식 박스 4**:
- 레이블: `Doob의 최대 부등식`
- 수식:
```
P\!\left(\max_{1 \leq k \leq n} M_k \geq \lambda\right) \leq \frac{\mathbb{E}[M_n^+]}{\lambda}
\quad\text{(비음 슈퍼마팅게일)}
```
- 주석:
  - `M_n⁺ = max(Mₙ, 0)`: 양수 부분
  - 어느 시점에서든 크게 뛸 확률의 상한 — 마코프 부등식의 마팅게일 버전
  - L² 버전: P(max|Mₖ| ≥ λ) ≤ E[Mₙ²]/λ²

**수식 박스 5** (Azuma-Hoeffding):
- 레이블: `Azuma-Hoeffding 부등식 — 마팅게일의 지수 경계`
- 수식:
```
|d_t| \leq c_t \text{ a.s.} \Rightarrow
P(M_n - M_0 \geq t) \leq \exp\!\left(-\frac{t^2}{2\sum_{k=1}^n c_k^2}\right)
```
- 주석:
  - 각 마팅게일 차분이 유계이면 지수 집중 경계
  - McDiarmid 부등식(Ch.27)이 Azuma-Hoeffding의 특수 케이스
  - ML 적용: SGD에서 각 배치 그라디언트 변화량이 유계이면 이 경계 사용

---

### 섹션 4: Optional Stopping Theorem

**아이콘**: `τ`  
**제목**: `선택적 정지 정리 — 전략적으로 멈춰도 기댓값은 같다`

**설명**: 공정한 게임(마팅게일)에서 언제 멈추는 전략을 써도 (합리적 조건 하에) 기대 수익은 처음과 같습니다. 도박사의 오류를 수학적으로 반박합니다.

**수식 박스 6**:
- 레이블: `선택적 정지 정리 (Optional Stopping Theorem)`
- 수식:
```
\tau \text{ 유계 stopping time}, \{M_t\} \text{ 마팅게일}
\Rightarrow \mathbb{E}[M_\tau] = \mathbb{E}[M_0]
```
- 주석:
  - `τ`: stopping time — 현재까지의 정보만으로 정지 여부 결정
  - 조건: τ가 유계이거나, uniformly integrable
  - "더블링 전략"이 실패하는 이유: 유한 자본에서 τ가 유계가 아님
  - ML 응용: 조기 종료(early stopping)가 평균적으로 bias를 도입하지 않는 조건

---

## 인터랙티브: Random Walk 시뮬레이터

**제목**: `🔢 마팅게일 Random Walk — E[Sₙ] = S₀ 확인`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `rw-n` | 걸음 수 n | 10 | 500 | 10 | 100 |
| `rw-trials` | 시뮬레이션 횟수 | 10 | 200 | 10 | 50 |

**JavaScript 로직**:
```javascript
function updateRW() {
  const n = parseInt(document.getElementById('rw-n').value);
  const trials = parseInt(document.getElementById('rw-trials').value);
  document.getElementById('rw-n-v').value = n;
  document.getElementById('rw-trials-v').value = trials;

  // Simple symmetric random walk: +1 or -1 with prob 0.5
  let finalVals = [];
  let maxVals = [];
  for (let t = 0; t < trials; t++) {
    let s = 0;
    let maxS = 0;
    for (let i = 0; i < n; i++) {
      s += Math.random() < 0.5 ? 1 : -1;
      if (s > maxS) maxS = s;
    }
    finalVals.push(s);
    maxVals.push(maxS);
  }

  const meanFinal = finalVals.reduce((a, b) => a + b, 0) / trials;
  const meanMax = maxVals.reduce((a, b) => a + b, 0) / trials;
  const theoreticalSD = Math.sqrt(n);

  document.getElementById('rw-res').innerHTML =
    `대칭 랜덤 워크 (p=0.5), n=${n}걸음, ${trials}번 시뮬레이션<br/>
     E[Sₙ] 이론값 = <strong>0</strong> (마팅게일 성질)<br/>
     시뮬레이션 E[Sₙ] ≈ <strong>${meanFinal.toFixed(3)}</strong> (≈0 확인)<br/>
     이론적 표준편차 = √n = <strong>${theoreticalSD.toFixed(2)}</strong><br/>
     평균 최대값 E[max Sₖ] ≈ <strong>${meanMax.toFixed(2)}</strong> (≈√(2n/π) = ${(Math.sqrt(2*n/Math.PI)).toFixed(2)})<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       E[Sₙ]=0이지만 경로는 √n 정도 퍼진다 — 마팅게일의 핵심
     </span>`;
}
updateRW();
```

---

## 퀴즈

**질문**: 마팅게일 {Mₜ}에서 E[Mₜ₊₁|ℱₜ] = Mₜ 가 의미하는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 미래 값이 현재 값과 항상 같다 | ❌ |
| (b) 현재까지 알고 있는 모든 정보를 조건으로 할 때 다음 값의 기댓값이 현재 값이다 | ✅ |
| (c) Mₜ는 단조증가 수열이다 | ❌ |
| (d) Mₜ는 항상 양수이다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 33)

```
[ ] 마팅게일의 정의를 수식으로 쓸 수 있다
[ ] 마팅게일 vs 슈퍼마팅게일의 차이를 설명할 수 있다
[ ] 마팅게일 차분 수열이 SGD 노이즈와 어떻게 연결되는지 설명할 수 있다
[ ] Doob의 최대 부등식을 설명할 수 있다
[ ] Azuma-Hoeffding 부등식을 Hoeffding과 비교할 수 있다
[ ] Optional Stopping Theorem의 의미를 설명할 수 있다
[ ] 논문에서 "martingale difference sequence"가 나오면 어떤 맥락인지 안다
```
