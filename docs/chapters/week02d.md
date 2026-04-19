# Week 02-D — 경사하강법 numpy 구현 실습 (Ch.02D)

> **파일명**: `chapters/ch02d.html`
> **위치**: Phase 1 보강 챕터 — Ch.02 (미분과 최적화) 실습 확장
> **인터랙티브**: ✅ 경사하강법 loss 실시간 수렴 시뮬레이터
> **우선순위**: P0 (90일 플랜 Day 51~60 핵심 갭)
> **선행 조건**: `ch02.html` (미분과 최적화) + `ch03b.html` (선형회귀)

---

## 이 챕터가 필요한 이유 (갭 분석)

Ch.02는 경사하강법을 **슬라이더 시뮬레이터**로 가르친다.
90일 플랜 Day 51~60은 `θ = θ - η∇J(θ)`를 **numpy 코드로 직접 구현**해야 한다.

| Ch.02에서 하는 것 | 이 챕터에서 하는 것 |
|-----------------|------------------|
| 슬라이더로 학습률 변경 관찰 | `lr = 0.01` 직접 코드에 설정 |
| 시각화로 수렴 확인 | `loss` 값이 줄어드는 것을 print로 확인 |
| 수식 이해 | 수식 → 코드 1:1 대응 |
| 개념 이해 | 반복 학습 루프 직접 작성 |

**이 챕터 이후 할 수 있는 것**: 논문의 "Algorithm 1" 박스를 보고 Python 코드로 변환 가능

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.02D · 최적화 실습 (Phase 1 보강 · 90일 Day 51~60)` |
| 제목 | `경사하강법 직접 구현 — θ = θ - η∇J(θ)를 numpy로` |
| 서브타이틀 | `수식이 코드가 됩니다. 경사하강법을 scratch로 구현하면 모든 ML 논문의 Algorithm 섹션이 읽힙니다.` |
| 이전 챕터 | `ch03b.html` — Ch.03B 선형회귀·최소제곱법 |
| 다음 챕터 | `ch09.html` — Ch.09 ML연결1·Logistic |

---

## EASY BOX 내용

**핵심 비유**: "산에서 눈 감고 내려오기 — 발아래 경사가 가파르면 크게 이동, 평탄하면 조금 이동"

**예시 리스트**:
- 경사(gradient) = 지금 서 있는 곳의 기울기
- 학습률(η) = 한 걸음의 크기 (너무 크면 산을 건너뛰어 버림)
- 반복 = 한 걸음씩 계속 내려가기
- AI에서: 모든 딥러닝 학습의 핵심이 이것. "Adam", "SGD" 등 다 이 원리의 변형

---

## 섹션 구성

### 섹션 1: 수식 → 코드 1:1 번역

**아이콘**: `🔄`
**제목**: `θ = θ - η∇J(θ) 를 한 줄씩 코드로`

**설명**: 논문에서 "Algorithm 1"로 등장하는 경사하강법을 그대로 Python 코드로 옮깁니다. 수식의 각 기호가 코드의 어느 부분에 대응되는지 1:1로 연결합니다.

**수식 박스 1**:
- 레이블: `경사하강법 업데이트 규칙`
- 수식:
```
\theta^{(t+1)} = \theta^{(t)} - \eta \cdot \nabla_\theta J(\theta^{(t)})
```
- 주석:
  - `θ`: 학습 파라미터 (선형회귀에서는 w, b)
  - `η` (eta): 학습률 — 한 번에 얼마나 이동할지
  - `∇J(θ)`: 손실 J의 θ에 대한 gradient (현재 위치의 기울기)
  - `t → t+1`: 한 번의 업데이트 = 한 스텝

**코드 예시 (수식 → 코드 1:1)**:
```python
# θ = θ - η · ∇J(θ) 그대로 코드로
# 선형회귀: J(w,b) = (1/n)Σ(ŷᵢ - yᵢ)²

import numpy as np

# 합성 데이터 생성 (공부시간 → 점수)
np.random.seed(42)
X = np.random.rand(100, 1) * 10   # 공부시간 0~10시간
y = 5 * X.squeeze() + 50 + np.random.randn(100) * 3  # y = 5x + 50 + 노이즈

# 파라미터 초기화 (θ = [w, b])
w = 0.0   # 가중치 초기값
b = 0.0   # 편향 초기값
lr = 0.01  # η (학습률)
n = len(X)

# 반복 학습 루프
for epoch in range(100):
    # 1. 예측 (Forward)
    y_pred = w * X.squeeze() + b

    # 2. 손실 계산 (MSE)
    loss = np.mean((y_pred - y) ** 2)

    # 3. Gradient 계산 (∇J(θ))
    grad_w = (2/n) * np.dot(X.squeeze(), (y_pred - y))  # ∂J/∂w
    grad_b = (2/n) * np.sum(y_pred - y)                 # ∂J/∂b

    # 4. 파라미터 업데이트 (θ = θ - η·∇J)
    w = w - lr * grad_w   # ← 이것이 경사하강법
    b = b - lr * grad_b

    if epoch % 10 == 0:
        print(f"Epoch {epoch:3d} | Loss: {loss:.4f} | w={w:.3f} | b={b:.3f}")

print(f"\n최종: w={w:.3f} (정답: 5.0), b={b:.3f} (정답: 50.0)")
```

**callout (tip)**:
- 내용: **코드의 4단계가 모든 딥러닝의 뼈대다.** Forward → Loss → Gradient → Update. PyTorch에서는 `model(x)` → `loss = criterion(...)` → `loss.backward()` → `optimizer.step()`으로 표현한다. 같은 4단계다.

---

### 섹션 2: 학습률이 결과를 바꾼다

**아이콘**: `⚖️`
**제목**: `학습률 η — 너무 크면 발산, 너무 작으면 안 수렴`

**설명**: 학습률(η)은 경사하강법에서 가장 중요한 하이퍼파라미터입니다. 너무 크면 최솟값을 넘어서 발산하고, 너무 작으면 수렴이 너무 느립니다.

**표 (학습률에 따른 결과)**:

| 학습률 η | 결과 | 이유 |
|---------|------|------|
| 0.001 | 느리게 수렴 | 한 걸음이 너무 작다 |
| 0.01 | 적절히 수렴 | 균형 잡힌 이동 |
| 0.1 | 빠르게 수렴 또는 불안정 | 경우에 따라 다름 |
| 1.0 | 발산 (loss가 증가) | 너무 크게 건너뜀 |

**코드 체크**:
```python
# 학습률에 따른 차이 실험
for lr in [0.001, 0.01, 0.1, 1.0]:
    w, b = 0.0, 0.0
    for epoch in range(50):
        y_pred = w * X.squeeze() + b
        loss = np.mean((y_pred - y) ** 2)
        grad_w = (2/n) * np.dot(X.squeeze(), (y_pred - y))
        grad_b = (2/n) * np.sum(y_pred - y)
        w -= lr * grad_w
        b -= lr * grad_b
    final_loss = np.mean((w * X.squeeze() + b - y) ** 2)
    print(f"η={lr}: 최종 loss={final_loss:.2f}")
```

**수식 박스 2**:
- 레이블: `수렴 조건 (논문 연결)`
- 수식:
```
\text{수렴 조건 (볼록 함수)}: \eta < \frac{2}{L}
\quad\text{where } L \text{ is the Lipschitz constant of } \nabla J
```
- 주석:
  - `L`: 기울기 변화의 최대 속도 (Lipschitz 상수)
  - `η < 2/L`: 이 조건을 만족하면 수렴이 보장된다
  - 이것이 Phase 2 Ch.15~16에서 배울 수렴 이론의 기초
  - 논문의 "we set learning rate η = 0.01" 뒤에 이 수학이 있다

---

### 섹션 3: Mini-batch와 SGD

**아이콘**: `🎲`
**제목**: `SGD — 전체 데이터 대신 일부만 써서 업데이트`

**설명**: 지금까지 배운 것은 "전체 데이터의 gradient"로 업데이트하는 Batch GD입니다. 실제 딥러닝에서는 데이터 일부(mini-batch)만 써서 빠르게 업데이트하는 SGD(Stochastic Gradient Descent)를 씁니다.

**표 (GD 종류)**:

| 종류 | 한 번에 쓰는 데이터 | 장점 | 단점 |
|------|-----------------|------|------|
| Batch GD | 전체 (n개) | 정확한 gradient | 느림, 메모리 많이 필요 |
| SGD | 1개 | 빠름, 노이즈로 탈출 | 불안정 |
| Mini-batch GD | k개 (32, 64, 128...) | 균형 | — |

**코드 체크**:
```python
# Mini-batch GD 구현
batch_size = 32
w, b = 0.0, 0.0
lr = 0.01

for epoch in range(50):
    # 데이터 섞기
    idx = np.random.permutation(n)
    X_shuffle, y_shuffle = X[idx], y[idx]

    # mini-batch로 나눠서 업데이트
    for i in range(0, n, batch_size):
        X_batch = X_shuffle[i:i+batch_size].squeeze()
        y_batch = y_shuffle[i:i+batch_size]

        y_pred = w * X_batch + b
        grad_w = (2/len(X_batch)) * np.dot(X_batch, (y_pred - y_batch))
        grad_b = (2/len(X_batch)) * np.sum(y_pred - y_batch)
        w -= lr * grad_w
        b -= lr * grad_b

    loss = np.mean((w * X.squeeze() + b - y) ** 2)
    if epoch % 10 == 0:
        print(f"Epoch {epoch} | Loss: {loss:.4f}")
```

**callout (info)**:
- 내용: 논문에서 "We train with batch size 256, learning rate 0.001..."처럼 쓰인다. 이제 이 문장이 "256개씩 mini-batch로 경사하강법을 적용했다"는 뜻임을 안다. 이 챕터를 마치면 논문의 Training Details 섹션을 읽을 수 있다.

---

### 섹션 4: 수식 → 코드 체크리스트 (90일 플랜 Day 51~60)

**아이콘**: `✅`
**제목**: `코드 완성 체크리스트 — 이것을 혼자 쓸 수 있으면 완료`

**표 (구현 체크리스트)**:

| 체크 | 구현 항목 | 대응 수식 |
|------|----------|---------|
| ☐ | `y_pred = w * X + b` | ŷ = wᵀx + b |
| ☐ | `loss = np.mean((y_pred - y)**2)` | J(θ) = (1/n)Σ(ŷᵢ-yᵢ)² |
| ☐ | `grad_w = (2/n) * np.dot(X, y_pred-y)` | ∂J/∂w |
| ☐ | `w = w - lr * grad_w` | θ = θ - η∇J |
| ☐ | `for epoch in range(N): ...` | t = 0, 1, ..., T |
| ☐ | loss가 에폭마다 감소하는 것 확인 | 수렴 확인 |
| ☐ | η를 10배 키워서 발산하는 것 확인 | 수렴 조건 체험 |

---

## 인터랙티브: 경사하강법 수렴 시뮬레이터

**제목**: `🔢 학습률과 에폭 수에 따른 수렴 — loss가 어떻게 줄어드나`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `lr_val` | 학습률 η | 0.001 | 0.5 | 0.001 | 0.01 |
| `epochs_val` | 에폭 수 | 10 | 200 | 10 | 50 |

**JavaScript 로직**:
```javascript
function updateGD() {
  const lr = parseFloat(document.getElementById('lr_val').value);
  const epochs = parseInt(document.getElementById('epochs_val').value);
  document.getElementById('lr_val-v').value = lr.toFixed(3);
  document.getElementById('epochs_val-v').value = epochs;

  // 합성 데이터 (y = 5x + 50, 고정)
  const n = 30;
  const Xs = [], ys = [];
  for (let i = 0; i < n; i++) {
    const x = (i / n) * 10;
    Xs.push(x);
    ys.push(5 * x + 50 + (Math.sin(i * 3.7) * 3));
  }

  // 경사하강법
  let w = 0, b = 0;
  const lossHistory = [];

  for (let e = 0; e < epochs; e++) {
    const preds = Xs.map(x => w * x + b);
    const loss = preds.reduce((s, p, i) => s + (p - ys[i]) ** 2, 0) / n;
    lossHistory.push(loss.toFixed(2));

    if (loss > 1e8) break; // 발산 감지

    let gw = 0, gb = 0;
    for (let i = 0; i < n; i++) {
      gw += (2 / n) * Xs[i] * (preds[i] - ys[i]);
      gb += (2 / n) * (preds[i] - ys[i]);
    }
    w -= lr * gw;
    b -= lr * gb;
  }

  const finalLoss = parseFloat(lossHistory[lossHistory.length - 1]);
  const initLoss = parseFloat(lossHistory[0]);
  const converged = finalLoss < initLoss * 0.1;
  const diverged = finalLoss > initLoss * 10 || isNaN(finalLoss);

  const sample = lossHistory.filter((_, i) =>
    i % Math.max(1, Math.floor(epochs / 5)) === 0
  ).join(' → ');

  document.getElementById('gd-res').innerHTML =
    `η = ${lr}, epochs = ${epochs}<br/>
     초기 loss: ${initLoss} → 최종 loss: ${isNaN(finalLoss) ? '∞ (발산)' : finalLoss}<br/>
     loss 추이: ${sample}<br/>
     최종 파라미터: w = ${w.toFixed(3)} (정답: 5.0), b = ${b.toFixed(3)} (정답: 50.0)<br/>
     <strong>${diverged ? '⚠️ 발산! — 학습률이 너무 큽니다' : converged ? '✅ 수렴 완료' : '🔄 아직 수렴 중 — epochs를 늘리세요'}</strong>`;
}
updateGD();
```

---

## 퀴즈

**질문**: 경사하강법 코드에서 `w = w - lr * grad_w`를 수행하는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) w를 랜덤하게 바꾸기 위해 | ❌ |
| (b) gradient 방향으로 이동하면 loss가 증가하기 때문에 반대로 이동 | ✅ |
| (c) lr이 클수록 더 정확한 값에 수렴하기 때문 | ❌ |
| (d) 수식에서 덧셈 대신 뺄셈을 써야 코드가 작동하기 때문 | ❌ |

**정답 위치**: (b)
**해설**: gradient는 loss가 가장 빠르게 **증가**하는 방향이다. loss를 줄이려면 그 반대 방향으로 이동해야 한다. 그래서 `- lr * gradient`다. "경사하강"(Gradient **Descent**)의 핵심이다.

---

## 주간 체크리스트 (Week 02-D)

```
[ ] y_pred = w * X + b — numpy로 예측값 계산
[ ] loss = np.mean((y_pred - y)**2) — MSE 손실 계산
[ ] grad_w, grad_b 직접 계산 (편미분)
[ ] w = w - lr * grad_w 업데이트 1회 실행
[ ] for 루프로 100번 반복 → loss 감소 확인
[ ] lr = 0.001 vs 0.01 vs 0.1 결과 비교
[ ] lr = 1.0으로 설정 → 발산 직접 경험
[ ] mini-batch GD 구현 (batch_size = 32)
[ ] 최종: "Algorithm 1" 형식의 경사하강법 코드를 혼자 처음부터 작성
```
