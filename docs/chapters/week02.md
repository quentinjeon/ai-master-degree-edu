# Week 02 — 미분과 최적화 (Ch.2)

> **파일명**: `chapters/ch02.html`  
> **대응 섹션**: `c2`  
> **인터랙티브**: ✅ 경사하강법 시뮬레이터  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.2 · 기초 수학 (1~2주차)` |
| 제목 | `미분과 최적화` |
| 서브타이틀 | `AI 학습의 엔진인 경사하강법(Gradient Descent)을 이해하기 위한 핵심 개념입니다.` |
| 이전 챕터 | `ch01.html` — Ch.1 집합과 함수 |
| 다음 챕터 | `ch03.html` — Ch.3 선형대수 기초 |

---

## EASY BOX 내용

**핵심 비유**: "미분은 '조금 변할 때 얼마나 달라지는지'이고, 경사하강법은 '산에서 제일 빠른 길로 내려오는 것'처럼 AI가 실수를 줄여나가는 방법이다."

**예시 리스트**:
- 미분: 자동차 속도계 — "지금 이 순간 얼마나 빨리 달리는지" = 순간 변화율
- 경사하강법: 눈을 감고 발로 경사를 느끼며 내려오는 것 — 가장 가파른 방향으로 한 걸음씩
- AI에서: 딥러닝의 학습 = 손실함수를 최소화하는 파라미터를 경사하강법으로 찾는 것

---

## 섹션 구성

### 섹션 1: 미분 (Derivative)

**아이콘**: `∂`  
**제목**: `미분 (Derivative)`

**설명**: 미분은 함수의 **순간 변화율**입니다. "입력이 조금 변할 때 출력이 얼마나 변하는가?"

**수식 박스 1**:
- 레이블: `미분의 정의 (극한)`
- 수식: `f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}`
- 주석: `h→0`: 변화량을 무한히 작게 가져갈 때의 변화율 비율

**표 (기본 미분 공식)**:

| 함수 f(x) | 미분 f'(x) | AI 활용 |
|-----------|-----------|---------|
| `xⁿ` | `nxⁿ⁻¹` | 다항식 층 |
| `eˣ` | `eˣ` | Softmax 분모 |
| `ln x` | `1/x` | Log-likelihood, CE loss |
| `σ(x) = 1/(1+e⁻ˣ)` | `σ(x)(1-σ(x))` | 시그모이드 역전파 |
| `ReLU(x) = max(0,x)` | `𝟙[x>0]` | 딥러닝 활성화 함수 |

---

### 섹션 2: 그레디언트 (Gradient)

**아이콘**: `∇`  
**제목**: `편미분과 그레디언트`

**설명**: 여러 파라미터가 있을 때 각각에 대한 편미분을 벡터로 모은 것이 그레디언트입니다.

**수식 박스 2**:
- 레이블: `그레디언트 벡터`
- 수식: `\nabla_\theta \mathcal{L} = \left( \frac{\partial \mathcal{L}}{\partial \theta_1}, \frac{\partial \mathcal{L}}{\partial \theta_2}, \ldots, \frac{\partial \mathcal{L}}{\partial \theta_n} \right)^\top`
- 주석: `∇_θℒ`는 손실함수가 가장 빠르게 **증가**하는 방향. 음수 방향 = 가장 빠르게 감소하는 방향

---

### 섹션 3: 경사하강법 (Gradient Descent)

**아이콘**: `⬇`  
**제목**: `경사하강법 (Gradient Descent)`

**설명**: AI 학습의 핵심: 손실함수를 최소화하도록 파라미터를 반복적으로 업데이트합니다.

**수식 박스 3**:
- 레이블: `파라미터 업데이트 규칙`
- 수식: `\theta_{t+1} = \theta_t - \eta \cdot \nabla_\theta \mathcal{L}(\theta_t)`
- 주석: `η` (에타): 학습률(learning rate). 너무 크면 발산, 너무 작으면 느리게 수렴. `t`: 업데이트 스텝

**callout (warn)**:
- 제목: `연쇄 법칙 (Chain Rule)`
- 내용: 역전파(Backpropagation)의 핵심. `∂ℒ/∂x = (∂ℒ/∂z) · (∂z/∂x)` — 합성 함수를 거꾸로 미분하며 그레디언트 전달

---

## 인터랙티브: 경사하강법 시뮬레이터

**제목**: `🔢 경사하강법 시뮬레이터 — f(x) = x²`

**설명**: 간단한 함수 f(x) = x²에서 경사하강법을 체험. f'(x) = 2x이므로 `x_{t+1} = x_t - η·2x_t`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `gd-x0` | 시작점 x₀ | -5 | 5 | 0.1 | 4 |
| `gd-lr` | 학습률 η | 0.01 | 0.99 | 0.01 | 0.3 |
| `gd-it` | 반복 횟수 | 1 | 20 | 1 | 5 |

**JavaScript 로직**:
```javascript
function updateGD() {
  const x0 = parseFloat(document.getElementById('gd-x0').value);
  const lr  = parseFloat(document.getElementById('gd-lr').value);
  const it  = parseInt(document.getElementById('gd-it').value);
  // 값 표시 업데이트
  document.getElementById('gd-x0-v').value = x0.toFixed(1);
  document.getElementById('gd-lr-v').value = lr.toFixed(2);
  document.getElementById('gd-it-v').value = it;
  // 시뮬레이션
  let x = x0, steps = [x0.toFixed(3)];
  for (let i = 0; i < it; i++) {
    x -= lr * 2 * x;   // f'(x) = 2x
    steps.push(x.toFixed(3));
  }
  document.getElementById('gd-res').innerHTML =
    `시작 x₀=${x0} → ${it}번 후 x=<strong>${x.toFixed(5)}</strong>, f(x)=<strong>${(x*x).toFixed(7)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">경로: ${steps.slice(0,6).join(' → ')}${steps.length>6?' → …':''}</span>`;
}
// 초기 실행
updateGD();
```

---

## 퀴즈

**질문**: `f(x) = x³ + 2x`의 도함수 f'(x)는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) `3x + 2` | ❌ |
| (b) `3x² + 2` | ✅ |
| (c) `x² + 2x` | ❌ |
| (d) `3x³ + 2` | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 2)

```
[ ] 미분이 순간 변화율이라는 걸 설명할 수 있다
[ ] f'(x) = 2x처럼 기본 미분 공식 3개를 안다
[ ] 그레디언트 벡터가 무엇인지 설명할 수 있다
[ ] 경사하강법 업데이트 규칙을 쓸 수 있다
[ ] 학습률(η)이 너무 크거나 작을 때 문제를 설명할 수 있다
[ ] 연쇄 법칙의 의미를 말로 설명할 수 있다
```
