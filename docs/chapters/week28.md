# Week 28 — PAC 학습 이론 (Ch.28)

> **파일명**: `chapters/ch28.html`  
> **Phase**: Phase 3-A · 통계적 학습 이론  
> **인터랙티브**: ✅ 표본 복잡도 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.28 · 통계적 학습 이론 (Phase 3 — 2주차)` |
| 제목 | `PAC 학습 이론 — 얼마나 많은 데이터가 필요한가` |
| 서브타이틀 | `Probably Approximately Correct. 표본 복잡도, 일관성, 일반화 오차. 논문의 "sample complexity O(d/ε²)" 분석을 이해합니다.` |
| 이전 챕터 | `ch27.html` — Ch.27 집중 부등식 |
| 다음 챕터 | `ch29.html` — Ch.29 VC 차원·Rademacher 복잡도 |

---

## EASY BOX 내용

**핵심 비유**: "스팸 필터를 학습시키려면 얼마나 많은 이메일을 봐야 하나? PAC 이론은 '원하는 정확도(ε)와 신뢰도(1-δ)를 달성하기 위해 최소 n개의 데이터가 필요하다'는 수학적 보장을 준다."

**예시 리스트**:
- ε-정확: 학습된 분류기의 진짜 오차가 최적 오차보다 ε 이하
- 1-δ 신뢰도: 이 보장이 1-δ의 확률로 성립 (δ는 실패 확률)
- AI에서: 논문에서 "Algorithm X achieves ε-accurate classifier with n = O(d log(1/δ)/ε²) samples"라고 쓰는 것이 PAC 복잡도 결과

---

## 섹션 구성

### 섹션 1: PAC 학습 프레임워크

**아이콘**: `🎓`  
**제목**: `PAC Learning — 수학적 프레임워크`

**설명**: PAC(Probably Approximately Correct) 학습 이론은 학습 문제를 엄밀하게 정의합니다. 데이터 분포 D에서 i.i.d.로 뽑은 n개 샘플로 학습기를 훈련시켜, 진짜 오차(true error)가 작을 확률이 높아야 합니다.

**수식 박스 1**:
- 레이블: `PAC 학습 가능성의 정의`
- 수식:
```
\text{가설 클래스 } \mathcal{H} \text{ 가 PAC 학습 가능}
\iff
\exists \text{ 알고리즘 } A,\ m_\mathcal{H}(\varepsilon, \delta) \text{ s.t.}
\forall D,\ n \geq m_\mathcal{H}(\varepsilon,\delta):
P_{S \sim D^n}\!\left[L_D(A(S)) \leq \min_{h \in \mathcal{H}} L_D(h) + \varepsilon\right] \geq 1-\delta
```
- 주석:
  - `ε`: 정확도 파라미터 (작을수록 좋음)
  - `δ`: 신뢰도 파라미터 (작을수록 실패 확률 낮음)
  - `L_D(h)`: 분포 D에 대한 h의 진짜(true) 오차
  - `mℋ(ε,δ)`: 표본 복잡도 — ε,δ 달성에 필요한 최소 샘플 수

**표 (PAC 프레임워크의 주요 개념)**:

| 개념 | 기호 | 의미 |
|------|------|------|
| 가설 클래스 | ℋ | 가능한 모든 학습 모델의 집합 |
| 진짜 오차 | L_D(h) | 분포 D에서 h의 오류율 |
| 경험적 오차 | L_S(h) | 훈련 샘플 S에서 h의 오류율 |
| 표본 복잡도 | mℋ(ε,δ) | ε-accurate with prob 1-δ를 보장하는 최소 n |
| 일관 학습기 | ERM | 경험적 오차를 최소화하는 알고리즘 |

---

### 섹션 2: 경험적 위험 최소화 (ERM)

**아이콘**: `↓`  
**제목**: `ERM — 훈련 오차를 최소화하면 일반화되는가?`

**설명**: 가장 직관적인 학습 알고리즘은 훈련 데이터에서 오차(경험적 위험)를 최소화하는 것(ERM)입니다. 이것이 언제 일반화를 보장하는지를 PAC 이론이 분석합니다.

**수식 박스 2**:
- 레이블: `경험적 위험 최소화 (ERM)`
- 수식:
```
h_S = \mathrm{ERM}_{\mathcal{H}}(S) = \underset{h \in \mathcal{H}}{\arg\min}\ L_S(h)
\quad\text{where}\quad L_S(h) = \frac{1}{n}\sum_{i=1}^n \mathbf{1}[h(x_i) \neq y_i]
```
- 주석:
  - `h_S`: 훈련 데이터 S에서 ERM으로 선택된 가설
  - `L_S(h)`: 경험적 위험 = 훈련 오류율
  - ERM이 PAC 학습 가능하려면 ℋ가 "유한하거나" VC 차원이 유한해야 함
  - 무한 ℋ에서 ERM의 일반화를 다루는 것이 VC 이론의 핵심 (다음 챕터)

---

### 섹션 3: 유한 가설 클래스의 일반화 경계

**아이콘**: `|ℋ|`  
**제목**: `유한 ℋ의 일반화 경계 — |ℋ|이 클수록 더 많은 데이터`

**수식 박스 3**:
- 레이블: `유한 가설 클래스의 일반화 경계`
- 수식:
```
P\!\left[\exists h \in \mathcal{H}: L_D(h) - L_S(h) > \varepsilon\right]
\leq |\mathcal{H}| \cdot e^{-2n\varepsilon^2}
```
- 주석:
  - `|ℋ|`: 가설 클래스의 크기 (가능한 모델 수)
  - Union bound 적용: 각 가설에 대해 Hoeffding을 적용한 뒤 더함
  - δ = |ℋ|exp(-2nε²)로 놓으면 표본 복잡도: n ≥ log(|ℋ|/δ) / (2ε²)
  - 결론: "더 복잡한 모델 클래스(|ℋ| 큼)는 더 많은 데이터 필요"

**수식 박스 4** (표본 복잡도):
- 레이블: `유한 ℋ의 표본 복잡도`
- 수식:
```
m_\mathcal{H}(\varepsilon, \delta) \leq \frac{\log|\mathcal{H}| + \log(1/\delta)}{2\varepsilon^2}
```
- 주석:
  - log|ℋ|: 가설 클래스의 "비트 수" — 모델 복잡도
  - log(1/δ): 신뢰도 요구에 따른 추가 샘플
  - 2ε²: 정확도 요구 (작을수록 더 많은 샘플)

---

### 섹션 4: Bias-Variance에서 PAC으로

**아이콘**: `⚖`  
**제목**: `이론과 실전 연결 — PAC이 Bias-Variance를 설명하는가`

**설명**: PAC 이론은 통계의 Bias-Variance tradeoff를 더 엄밀하게 설명합니다. 모델이 복잡할수록(|ℋ| 큼) 훈련 오차는 낮지만 일반화 경계가 나빠집니다.

**수식 박스 5**:
- 레이블: `Estimation Error vs Approximation Error`
- 수식:
```
L_D(h_S) \leq \underbrace{L_D(h^*)}_{\text{approximation error}} + \underbrace{L_D(h_S) - L_D(h^*)}_{\text{estimation error}}
```
- 주석:
  - `h*`: 진짜 최적 가설 (ℋ에서)
  - Approximation error: ℋ가 얼마나 좋은 모델을 포함하는가 (bias)
  - Estimation error: 유한한 데이터로 h*에 얼마나 가까이 갈 수 있는가 (variance)
  - 모델 복잡도 ↑ → approximation error ↓, estimation error ↑

**callout (info)**:
- 내용: PAC 이론은 "완벽하게 분리 가능한 데이터"(Realizable) 설정에서 시작합니다. 실제 노이즈 있는 데이터에 대한 이론을 "Agnostic PAC"이라 합니다. 딥러닝은 agnostic 설정에서도 놀랍도록 잘 작동하는데, 이것이 "딥러닝 일반화 미스터리"(Ch.30)입니다.

---

## 인터랙티브: 표본 복잡도 계산기

**제목**: `🔢 PAC 표본 복잡도 — ε과 δ에 따른 필요 샘플 수`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `pac-eps` | 정확도 ε | 0.01 | 0.3 | 0.01 | 0.05 |
| `pac-delta` | 실패 확률 δ | 0.01 | 0.3 | 0.01 | 0.05 |
| `pac-logH` | log|ℋ| (비트) | 1 | 50 | 1 | 10 |

**JavaScript 로직**:
```javascript
function updatePAC() {
  const eps = parseFloat(document.getElementById('pac-eps').value);
  const delta = parseFloat(document.getElementById('pac-delta').value);
  const logH = parseInt(document.getElementById('pac-logH').value);
  document.getElementById('pac-eps-v').value = eps.toFixed(2);
  document.getElementById('pac-delta-v').value = delta.toFixed(2);
  document.getElementById('pac-logH-v').value = logH;

  // n ≥ (log|H| + log(1/δ)) / (2ε²)
  const nRequired = Math.ceil((logH * Math.log(2) + Math.log(1 / delta)) / (2 * eps * eps));

  // Hoeffding bound (no |H| factor, single hypothesis)
  const nSingle = Math.ceil(Math.log(2 / delta) / (2 * eps * eps));

  document.getElementById('pac-res').innerHTML =
    `목표: ε=${eps.toFixed(2)}, δ=${delta.toFixed(2)}, log|ℋ|=${logH} bits<br/>
     유한 ℋ 표본 복잡도: n ≥ <strong>${nRequired.toLocaleString()}</strong><br/>
     단일 가설(|ℋ|=1): n ≥ ${nSingle.toLocaleString()}<br/>
     ℋ 복잡도 증가에 의한 추가 샘플: <strong>${(nRequired - nSingle).toLocaleString()}</strong>개<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       ε을 절반으로 줄이면 n이 4배, δ를 줄이면 log 비율로 증가
     </span>`;
}
updatePAC();
```

---

## 퀴즈

**질문**: PAC 학습에서 표본 복잡도 mℋ(ε, δ)가 log(|ℋ|)에 비례하는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 더 많은 모델을 보려면 각 모델에 대한 오차를 계산해야 하기 때문 | ❌ |
| (b) Union bound로 모든 h ∈ ℋ에 동시에 일반화를 보장해야 해서 |ℋ| 배 증가 → 로그로 흡수 | ✅ |
| (c) |ℋ|이 클수록 각 샘플의 정보량이 늘어서 | ❌ |
| (d) VC 차원이 log|ℋ|와 같기 때문 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 28)

```
[ ] PAC 학습의 정의(ε-정확, 1-δ 신뢰도)를 수식으로 쓸 수 있다
[ ] ERM이 무엇인지 설명할 수 있다
[ ] 유한 가설 클래스의 일반화 경계 유도를 따라갈 수 있다
[ ] 표본 복잡도 공식 n ≥ (log|ℋ|+log(1/δ)) / (2ε²)를 설명할 수 있다
[ ] Approximation error와 estimation error의 차이를 설명할 수 있다
[ ] 논문에서 "sample complexity O(d/ε²)"를 보면 이 맥락임을 안다
[ ] Realizable vs Agnostic 설정의 차이를 설명할 수 있다
```
