# Week 09~10 — ML 연결 1: Logistic Regression (Ch.9)

> **파일명**: `chapters/ch09.html`  
> **인터랙티브**: ✅ sigmoid 계산기 + BCE loss 시뮬레이터  
> **우선순위**: P1

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.9 · ML 연결 (11주차)` |
| 제목 | `AI가 확률로 배우는 법` |
| 서브타이틀 | `P(y|x)를 모델링하는 방법. Logistic Regression으로 확률론이 AI 학습에 연결되는 원리를 배웁니다.` |
| 이전 챕터 | `ch08.html` — Ch.8 MAP & Bias-Variance |
| 다음 챕터 | `ch10.html` — Ch.10 Softmax & 다중분류 |

---

## EASY BOX 내용

**핵심 비유**: "AI는 '이 답이 맞을 가능성이 얼마나 큰가?'를 배우는 기계다. P(y=1|x)처럼 확률로 답을 낸다."

**예시 리스트**:
- 이메일이 스팸일 확률: P(스팸|이메일 내용)
- 사진이 고양이일 확률: P(고양이|이미지)
- AI에서: Logistic Regression = P(y=1|x) = σ(wᵀx + b)

---

## 섹션 구성

### 섹션 1: 지도학습 구조

**아이콘**: `📚`  
**제목**: `지도학습 (Supervised Learning)의 수학적 구조`

**설명**: 지도학습은 입력 x에서 출력 y를 예측하는 함수 P(y|x)를 데이터로부터 학습하는 과정입니다.

**표 (Regression vs Classification)**:

| 구분 | 출력 타입 | 분포 | 손실함수 |
|------|-----------|------|---------|
| 회귀 (Regression) | 연속값 ŷ ∈ ℝ | Gaussian | MSE (L2 loss) |
| 이진 분류 (Binary) | 이진값 y ∈ {0,1} | Bernoulli | Binary Cross-Entropy |
| 다중 분류 (Multi) | 범주형 y ∈ {0,...,K} | Categorical | Cross-Entropy |

---

### 섹션 2: Sigmoid 함수

**아이콘**: `σ`  
**제목**: `시그모이드 함수 (Sigmoid)`

**설명**: 임의의 실수를 (0, 1) 사이의 확률로 변환하는 함수입니다.

**수식 박스 1**:
- 레이블: `시그모이드 함수`
- 수식:
```
\sigma(z) = \frac{1}{1 + e^{-z}} \in (0, 1)
\qquad
\sigma'(z) = \sigma(z)(1 - \sigma(z))
```
- 주석:
  - `z`: 선형 변환값 (= wᵀx + b)
  - 출력: 0~1 사이의 확률값
  - 미분: 자기 자신으로 표현됨 → 역전파 계산 편리

---

### 섹션 3: Logistic Regression

**아이콘**: `📈`  
**제목**: `로지스틱 회귀 (Logistic Regression)`

**수식 박스 2**:
- 레이블: `Logistic Regression — P(y=1|x) 모델링`
- 수식:
```
P(y=1 \mid \mathbf{x}; \mathbf{w}, b) = \sigma(\mathbf{w}^\top \mathbf{x} + b)
= \frac{1}{1 + e^{-(\mathbf{w}^\top \mathbf{x} + b)}}
```
- 주석:
  - `w`: 가중치 벡터 (학습 대상)
  - `b`: 편향 (bias)
  - `σ(·)`: sigmoid — 확률로 변환

**수식 박스 3 (BCE Loss)**:
- 레이블: `Binary Cross-Entropy Loss (BCE) = 음의 로그우도`
- 수식:
```
\mathcal{L}_{BCE} = -\frac{1}{n}\sum_{i=1}^{n}
\left[ y_i \log \hat{p}_i + (1-y_i)\log(1-\hat{p}_i) \right]
```
- 주석:
  - `y_i ∈ {0,1}`: 정답 레이블
  - `p̂_i = σ(wᵀxᵢ + b)`: 모델의 예측 확률
  - `y=1`일 때: `-log(p̂)` — 확신할수록(p̂→1) 손실 작음
  - `y=0`일 때: `-log(1-p̂)` — 0에 확신할수록 손실 작음

**개념 카드**:
- 제목: `🔑 BCE 최소화 = MLE`
- 내용: BCE를 최소화하는 것은 Bernoulli 분포에 대한 MLE와 정확히 동치. **딥러닝 분류 학습의 수학적 근거**

---

## 인터랙티브 1: sigmoid 계산기

**제목**: `🔢 Sigmoid 계산기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `sig-z` | 입력값 z | -10 | 10 | 0.1 | 0 |

**JavaScript**:
```javascript
function updateSig() {
  const z = parseFloat(document.getElementById('sig-z').value);
  document.getElementById('sig-z-v').value = z.toFixed(1);
  const p = 1 / (1 + Math.exp(-z));
  const dp = p * (1 - p);
  document.getElementById('sig-res').innerHTML =
    `σ(${z.toFixed(1)}) = <strong>${p.toFixed(4)}</strong> (확률) &nbsp;|&nbsp; σ'(z) = <strong>${dp.toFixed(4)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">
      z>0: 양성 클래스 가능성 높음 | z=0: 50% | z<0: 음성 클래스 가능성 높음
    </span>`;
}
updateSig();
```

## 인터랙티브 2: BCE Loss 시뮬레이터

**제목**: `🔢 BCE Loss — AI의 벌점 확인`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `bce-p` | 모델 예측 확률 p̂ | 0.01 | 0.99 | 0.01 | 0.8 |
| `bce-y` | 정답 레이블 y | 0 | 1 | 1 | 1 |

**JavaScript**:
```javascript
function updateBCE() {
  const p = parseFloat(document.getElementById('bce-p').value);
  const y = parseInt(document.getElementById('bce-y').value);
  document.getElementById('bce-p-v').value = p.toFixed(2);
  document.getElementById('bce-y-v').value = y;

  const loss = -(y * Math.log(p) + (1 - y) * Math.log(1 - p));
  document.getElementById('bce-res').innerHTML =
    `BCE Loss = <strong>${loss.toFixed(4)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">
      정답 y=${y}, 예측 p̂=${p.toFixed(2)} → 
      ${y===1 ? `-log(${p.toFixed(2)}) = ${(-Math.log(p)).toFixed(4)}` 
               : `-log(${(1-p).toFixed(2)}) = ${(-Math.log(1-p)).toFixed(4)}`}
    </span>`;
}
updateBCE();
```

---

## 퀴즈

**질문**: Logistic Regression에서 sigmoid 함수를 쓰는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 계산이 빠르기 때문에 | ❌ |
| (b) 임의의 실수를 (0,1) 사이의 확률로 변환하기 위해 | ✅ |
| (c) 미분이 불가능하기 때문에 | ❌ |
| (d) 다중 클래스를 위해 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 11)

```
[ ] P(y|x)가 지도학습의 목표임을 설명할 수 있다
[ ] regression과 classification의 차이를 안다
[ ] sigmoid 함수의 입력/출력/미분을 설명할 수 있다
[ ] Logistic Regression 수식을 쓸 수 있다
[ ] BCE loss를 설명할 수 있다
[ ] BCE 최소화 = MLE임을 설명할 수 있다
```
