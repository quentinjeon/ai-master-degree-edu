# Week 10~11 — ML 연결 2: Softmax & 다중분류 (Ch.10)

> **파일명**: `chapters/ch10.html`  
> **인터랙티브**: ✅ softmax 계산기  
> **우선순위**: P1

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.10 · ML 연결 (12주차)` |
| 제목 | `Softmax와 다중분류` |
| 서브타이틀 | `여러 클래스 중 하나를 확률로 고르는 방법. Softmax가 Categorical 분포와 어떻게 연결되는지 배웁니다.` |
| 이전 챕터 | `ch09.html` — Ch.9 AI가 확률로 배우는 법 |
| 다음 챕터 | `ch11.html` — Ch.11 정보이론 |

---

## EASY BOX 내용

**핵심 비유**: "고양이, 강아지, 토끼 중 하나를 골라야 할 때, AI는 각각의 가능성을 계산해서 가장 큰 것을 고른다. 그 가능성이 확률이 되도록 만드는 것이 Softmax다."

**예시 리스트**:
- 점수: [3.0, 1.0, 0.5] → softmax → [0.84, 0.11, 0.05]
- 세 값의 합은 항상 1 (확률 조건 만족)
- AI에서: 이미지 분류의 마지막 레이어는 반드시 softmax (또는 sigmoid)

---

## 섹션 구성

### 섹션 1: Softmax 함수

**아이콘**: `🔢`  
**제목**: `Softmax 함수`

**설명**: Softmax는 K개의 임의의 실수(logit)를 K개의 확률(합=1)로 변환하는 함수입니다.

**수식 박스 1**:
- 레이블: `Softmax 함수`
- 수식:
```
\text{softmax}(\mathbf{z})_k = \frac{e^{z_k}}{\sum_{j=1}^{K} e^{z_j}}, \quad k = 1, \ldots, K
```
- 주석:
  - `z_k`: k번째 클래스의 점수 (logit)
  - 지수 함수로 양수화 후, 합으로 나눠서 확률로 변환
  - 출력값: 모두 (0,1) 사이이고 합이 1 → 확률분포

**표 (sigmoid vs softmax)**:

| | Sigmoid | Softmax |
|--|---------|---------|
| 출력 개수 | 1개 | K개 |
| 출력 범위 | (0,1) | (0,1), 합=1 |
| 사용 | 이진분류 | 다중분류 |
| 분포 | Bernoulli | Categorical |
| 손실 | BCE | Cross-Entropy |

---

### 섹션 2: 다중분류 Cross-Entropy

**아이콘**: `🏹`  
**제목**: `다중분류 Cross-Entropy Loss`

**수식 박스 2**:
- 레이블: `Categorical Cross-Entropy Loss`
- 수식:
```
\mathcal{L}_{CE} = -\frac{1}{n}\sum_{i=1}^{n}\sum_{k=1}^{K} y_{ik} \log \hat{p}_{ik}
= -\frac{1}{n}\sum_{i=1}^{n} \log \hat{p}_{i, y_i}
```
- 주석:
  - `y_ik`: one-hot 인코딩 (실제 정답 클래스만 1, 나머지 0)
  - `p̂_ik`: softmax 출력 (k번째 클래스의 예측 확률)
  - 단순화: 정답 클래스의 예측 확률의 음의 로그값만 남음

---

### 섹션 3: Naive Bayes와 정규화

**아이콘**: `🧮`  
**제목**: `Naive Bayes 분류기`

**설명**: 베이즈 정리를 직접 이용하는 분류기입니다. 모든 특성이 조건부 독립이라는 강한 가정을 사용합니다.

**수식 박스 3**:
- 레이블: `Naive Bayes 분류 규칙`
- 수식:
```
\hat{y} = \argmax_k P(Y=k) \prod_{j=1}^{d} P(X_j \mid Y=k)
```
- 주석:
  - `P(Y=k)`: 클래스 k의 prior (전체 데이터에서 k 비율)
  - `P(Xⱼ|Y=k)`: j번째 특성의 조건부 확률
  - "Naive": 각 특성이 서로 독립이라고 가정 (실제로는 아닐 수 있음)

---

## 인터랙티브: Softmax 계산기

**제목**: `🔢 Softmax 계산기 (3-class 예시)`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `sm-z1` | 클래스 1 점수 z₁ | -5 | 5 | 0.1 | 3.0 |
| `sm-z2` | 클래스 2 점수 z₂ | -5 | 5 | 0.1 | 1.0 |
| `sm-z3` | 클래스 3 점수 z₃ | -5 | 5 | 0.1 | 0.5 |

**JavaScript**:
```javascript
function updateSM() {
  const z1 = parseFloat(document.getElementById('sm-z1').value);
  const z2 = parseFloat(document.getElementById('sm-z2').value);
  const z3 = parseFloat(document.getElementById('sm-z3').value);
  document.getElementById('sm-z1-v').value = z1.toFixed(1);
  document.getElementById('sm-z2-v').value = z2.toFixed(1);
  document.getElementById('sm-z3-v').value = z3.toFixed(1);

  const exps = [Math.exp(z1), Math.exp(z2), Math.exp(z3)];
  const sumExp = exps.reduce((a, b) => a + b, 0);
  const probs = exps.map(e => e / sumExp);
  const maxIdx = probs.indexOf(Math.max(...probs));
  const labels = ['클래스 1', '클래스 2', '클래스 3'];

  document.getElementById('sm-res').innerHTML =
    `P(클래스1) = <strong>${probs[0].toFixed(4)}</strong> | 
     P(클래스2) = <strong>${probs[1].toFixed(4)}</strong> | 
     P(클래스3) = <strong>${probs[2].toFixed(4)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       합계 = ${(probs[0]+probs[1]+probs[2]).toFixed(4)} | 
       예측: <strong style="color:var(--accent3)">${labels[maxIdx]}</strong>
     </span>`;
}
updateSM();
```

---

## 퀴즈

**질문**: softmax의 출력값이 항상 만족해야 하는 조건 두 가지는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 각 값이 음수이고 합이 0 | ❌ |
| (b) 각 값이 (0,1) 사이이고 합이 1 | ✅ |
| (c) 각 값이 정수이고 합이 K | ❌ |
| (d) 각 값이 동일하고 합이 K | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 12)

```
[ ] softmax 함수를 수식으로 설명할 수 있다
[ ] sigmoid와 softmax의 차이를 설명할 수 있다
[ ] Categorical Cross-Entropy loss를 설명할 수 있다
[ ] one-hot 인코딩이 무엇인지 안다
[ ] Naive Bayes의 "naive" 가정이 무엇인지 안다
[ ] 정규화(L1, L2)가 MAP prior와 연결됨을 안다
```
