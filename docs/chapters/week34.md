# Week 34 — 고급 최적화 이론 (Ch.34)

> **파일명**: `chapters/ch34.html`  
> **Phase**: Phase 3-B · 딥러닝 이론 심화  
> **인터랙티브**: ✅ Adam vs SGD 수렴 비교기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.34 · 딥러닝 이론 심화 (Phase 3 — 8주차)` |
| 제목 | `고급 최적화 이론 — Adam·Momentum·Natural Gradient` |
| 서브타이틀 | `Adam이 왜 잘 작동하는가? Momentum의 이론, AdaGrad에서 Adam까지의 유도. 논문의 optimizer 분석을 이해합니다.` |
| 이전 챕터 | `ch33.html` — Ch.33 마팅게일·확률 과정 |
| 다음 챕터 | `ch35.html` — Ch.35 논문 비판 독해 방법론 |

---

## EASY BOX 내용

**핵심 비유**: "SGD는 경사가 가파른 방향으로만 내려간다. Adam은 '최근에 많이 움직인 방향은 조심스럽게, 적게 움직인 방향은 과감하게'라는 전략을 쓴다 — 각 파라미터에 맞는 맞춤형 학습률을 자동으로 조절한다."

**예시 리스트**:
- 좁고 긴 골짜기: SGD는 진동하며 느리게 내려가고, Adam은 빠르게 수렴
- Momentum: 이전 업데이트 방향을 기억해 지그재그를 줄임
- AI에서: 대부분의 딥러닝 논문에서 "Adam with lr=3e-4"처럼 쓴다 — 이 lr이 왜 그 값인지, 언제 SGD가 더 좋은지 이해하는 것이 목표

---

## 섹션 구성

### 섹션 1: Momentum — 진동을 줄이는 관성

**아이콘**: `→`  
**제목**: `Momentum SGD — 이전 그라디언트를 기억한다`

**설명**: Momentum은 이전 그라디언트 방향의 지수이동평균을 현재 업데이트에 더합니다. 좁고 긴 손실 함수에서 진동을 줄이고 수렴을 가속합니다.

**수식 박스 1**:
- 레이블: `Heavy-Ball Momentum (Polyak 1964)`
- 수식:
```
v_{t+1} = \beta v_t - \eta \nabla f(\theta_t)
\quad\Rightarrow\quad
\theta_{t+1} = \theta_t + v_{t+1}
```
- 주석:
  - `β ∈ [0, 1)`: momentum 계수 (일반적으로 0.9)
  - `vₜ`: 속도 벡터 (이전 그라디언트들의 지수이동평균)
  - β = 0이면 일반 SGD
  - 유효 학습률: 볼록 함수에서 η/(1-β) — β=0.9이면 10배

**수식 박스 2** (Nesterov 가속):
- 레이블: `Nesterov Accelerated Gradient (NAG)`
- 수식:
```
v_{t+1} = \beta v_t - \eta \nabla f(\theta_t + \beta v_t)
\quad\Rightarrow\quad
\theta_{t+1} = \theta_t + v_{t+1}
```
- 주석:
  - "미래 위치에서 그라디언트를 먼저 평가" — lookahead
  - 볼록 함수: O(1/T) → O(1/T²) 수렴 가속 (Ch.20에서 본 Nesterov 가속)
  - PyTorch: `torch.optim.SGD(momentum=0.9, nesterov=True)`

---

### 섹션 2: AdaGrad — 적응적 학습률의 시작

**아이콘**: `G`  
**제목**: `AdaGrad — 파라미터별 맞춤 학습률`

**설명**: AdaGrad는 각 파라미터에 대해 지금까지 나온 그라디언트의 제곱합을 누적하고, 이를 학습률에 나눕니다. 자주 업데이트되는 파라미터는 학습률이 자연히 감소합니다.

**수식 박스 3**:
- 레이블: `AdaGrad 업데이트`
- 수식:
```
G_{t,ii} = \sum_{s=1}^t g_{s,i}^2
\quad\Rightarrow\quad
\theta_{t+1,i} = \theta_{t,i} - \frac{\eta}{\sqrt{G_{t,ii} + \varepsilon}} g_{t,i}
```
- 주석:
  - `G_{t,ii}`: i번째 파라미터의 그라디언트 제곱 누적합
  - 자주 나타나는 특징: G가 커서 학습률이 작아짐 (희소 데이터에 유리)
  - 한계: G가 계속 증가 → 학습률이 결국 0에 수렴 (학습 멈춤)
  - NLP에서 좋은 성능, 이미지에서 덜 효과적

---

### 섹션 3: Adam — 현대 딥러닝의 표준 옵티마이저

**아이콘**: `A`  
**제목**: `Adam — 모멘텀 + 적응적 학습률의 결합`

**설명**: Adam(Adaptive Moment Estimation)은 1차 모멘트(그라디언트의 지수이동평균)와 2차 모멘트(그라디언트 제곱의 지수이동평균)를 모두 추적합니다.

**수식 박스 4**:
- 레이블: `Adam 업데이트 (Kingma & Ba, 2015)`
- 수식:
```
\begin{aligned}
m_t &= \beta_1 m_{t-1} + (1-\beta_1) g_t \quad\text{(1차 모멘트)} \\
v_t &= \beta_2 v_{t-1} + (1-\beta_2) g_t^2 \quad\text{(2차 모멘트)} \\
\hat{m}_t &= \frac{m_t}{1-\beta_1^t},\quad \hat{v}_t = \frac{v_t}{1-\beta_2^t} \quad\text{(바이어스 보정)} \\
\theta_{t+1} &= \theta_t - \frac{\eta}{\sqrt{\hat{v}_t} + \varepsilon} \hat{m}_t
\end{aligned}
```
- 주석:
  - `β₁ ≈ 0.9`: 1차 모멘트 감쇠 (그라디언트 방향의 지수이동평균)
  - `β₂ ≈ 0.999`: 2차 모멘트 감쇠 (그라디언트 크기의 지수이동평균)
  - `ε ≈ 1e-8`: 수치 안정성
  - 바이어스 보정: 초기에 mₜ, vₜ가 0으로 초기화되어 편향되는 것을 수정
  - 학습률 3e-4: "Karpathy constant" — 대부분의 경우 좋은 시작점

**표 (옵티마이저 비교)**:

| 옵티마이저 | 업데이트 | 장점 | 단점 |
|----------|---------|------|------|
| SGD | -η∇f | 단순, 이해 쉬움 | 학습률 선택 민감 |
| Momentum | -η(v + ∇f) | 진동 감소 | β 선택 필요 |
| AdaGrad | -η/√G · ∇f | 희소 문제에 좋음 | 학습률 소멸 |
| RMSprop | -η/√v · ∇f | AdaGrad 개선 | 없음 |
| Adam | -η/√v̂ · m̂ | 빠른 수렴, 적응적 | 일반화 gap |
| AdamW | Adam + weight decay | 정규화 내장 | — |

---

### 섹션 4: Adam의 이론적 한계와 AdamW

**아이콘**: `⚠`  
**제목**: `Adam의 일반화 Gap과 AdamW의 해결`

**설명**: Adam이 빠르게 수렴하지만 SGD보다 일반화 성능이 낮은 경우가 있습니다. 이것이 "Adam의 일반화 gap" 문제입니다. AdamW는 L2 정규화를 올바르게 구현합니다.

**수식 박스 5**:
- 레이블: `Adam vs AdamW — L2 정규화의 차이`
- 수식:
```
\text{Adam + L2 (틀림):}\quad g_t \leftarrow \nabla f(\theta_t) + \lambda\theta_t
\quad\Rightarrow\quad \theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{\hat{v}_t}+\varepsilon}\hat{m}_t
```
```
\text{AdamW (맞음):}\quad \theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{\hat{v}_t}+\varepsilon}\hat{m}_t - \eta\lambda\theta_t
```
- 주석:
  - Adam에 L2를 그라디언트에 더하면 적응적 학습률의 영향을 받아 weight decay가 균일하지 않음
  - AdamW: weight decay를 업데이트에서 분리하여 직접 파라미터에 적용
  - 현대 LLM 훈련: AdamW가 표준 (GPT-3, LLaMA, Mistral 모두 AdamW 사용)

**callout (warn)**:
- 내용: **Adam의 수렴이 보장 안 되는 경우**: 비볼록 문제에서 Adam은 수렴이 보장되지 않습니다 (Reddi et al., 2018). AMSGrad가 이를 수정했지만 실전 성능은 Adam이 더 좋은 경우가 많습니다. "이론과 실전의 gap"의 대표적 사례.

---

## 인터랙티브: Adam vs SGD 수렴 비교기

**제목**: `🔢 Adam vs SGD — 좁은 골짜기에서의 수렴 비교`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `opt-lr` | 학습률 η | 0.001 | 0.5 | 0.001 | 0.01 |
| `opt-steps` | 반복 횟수 | 10 | 500 | 10 | 100 |

**JavaScript 로직**:
```javascript
function updateOpt() {
  const lr = parseFloat(document.getElementById('opt-lr').value);
  const steps = parseInt(document.getElementById('opt-steps').value);
  document.getElementById('opt-lr-v').value = lr.toFixed(3);
  document.getElementById('opt-steps-v').value = steps;

  // f(x,y) = x² + 100y² (좁은 골짜기, 조건수 κ=100)
  // SGD
  let [xSGD, ySGD] = [3.0, 0.5];
  let sgdLoss = [];
  for (let t = 0; t < steps; t++) {
    const gx = 2 * xSGD, gy = 200 * ySGD;
    xSGD -= lr * gx; ySGD -= lr * gy;
    sgdLoss.push(xSGD**2 + 100*ySGD**2);
  }

  // Adam (simplified, β₁=0.9, β₂=0.999)
  let [xA, yA] = [3.0, 0.5];
  let [mx, my, vx, vy] = [0, 0, 0, 0];
  const b1 = 0.9, b2 = 0.999, eps = 1e-8;
  let adamLoss = [];
  for (let t = 1; t <= steps; t++) {
    const gx = 2*xA, gy = 200*yA;
    mx = b1*mx + (1-b1)*gx; my = b1*my + (1-b1)*gy;
    vx = b2*vx + (1-b2)*gx*gx; vy = b2*vy + (1-b2)*gy*gy;
    const mhx = mx/(1-b1**t), mhy = my/(1-b1**t);
    const vhx = vx/(1-b2**t), vhy = vy/(1-b2**t);
    xA -= lr * mhx / (Math.sqrt(vhx)+eps);
    yA -= lr * mhy / (Math.sqrt(vhy)+eps);
    adamLoss.push(xA**2 + 100*yA**2);
  }

  const sgdFinal = sgdLoss[sgdLoss.length-1].toExponential(3);
  const adamFinal = adamLoss[adamLoss.length-1].toExponential(3);

  document.getElementById('opt-res').innerHTML =
    `f(x,y) = x² + 100y² (조건수 κ=100), η=${lr.toFixed(3)}, ${steps}번 반복<br/>
     SGD 최종 손실: <strong>${sgdFinal}</strong><br/>
     Adam 최종 손실: <strong>${adamFinal}</strong><br/>
     Adam/SGD 비율: ${(parseFloat(adamFinal)/parseFloat(sgdFinal)).toFixed(3)}배<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       조건수가 큰 함수에서 Adam이 SGD보다 훨씬 빠르게 수렴함을 확인
     </span>`;
}
updateOpt();
```

---

## 퀴즈

**질문**: AdamW가 Adam + L2 정규화와 다른 핵심 차이는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) AdamW는 L2를 사용하지 않는다 | ❌ |
| (b) AdamW는 weight decay를 그라디언트가 아닌 파라미터에 직접 적용하여 적응적 학습률의 영향을 받지 않게 한다 | ✅ |
| (c) AdamW는 2차 모멘트를 쓰지 않는다 | ❌ |
| (d) AdamW는 학습률 스케줄러가 없어도 된다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 34)

```
[ ] Momentum의 수식과 β의 역할을 설명할 수 있다
[ ] Nesterov 가속이 일반 Momentum과 다른 점을 설명할 수 있다
[ ] AdaGrad의 업데이트 공식과 한계를 설명할 수 있다
[ ] Adam의 4단계(m, v, bias correction, update)를 수식으로 쓸 수 있다
[ ] Adam의 일반화 gap이 무엇인지 설명할 수 있다
[ ] AdamW가 Adam + L2와 다른 이유를 설명할 수 있다
[ ] 논문에서 "trained with AdamW, lr=3e-4, β₁=0.9, β₂=0.999"를 보면 의미를 안다
```
