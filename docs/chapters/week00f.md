# Week 00-F — ML 실습 입문 (Ch.00F)

> **파일명**: `chapters/ch00f.html`
> **위치**: Level 0 → Level 1 브릿지 챕터 3번 (최종 브릿지)
> **인터랙티브**: ✅ 모델 정확도 시뮬레이터 (훈련/테스트 분할)
> **우선순위**: P0 (브릿지 필수)
> **선행 조건**: `ch00e.html` (수학 브릿지) 완료

---

## 이 챕터가 필요한 이유 (갭 분석)

Level 0 커리큘럼은 `y = wx + b`를 **직접 w, b를 설정**해서 예측했다.
Phase 1 (Ch.07 MLE, Ch.08 MAP, Ch.09 Logistic Regression 등)은 **"학습"이 w를 자동으로 찾는다**고 전제한다.
그 사이에 "학습이란 무엇인가"를 한 번 실제로 경험해야 한다.

| Level 0에서 하는 것 | Phase 1에서 전제하는 것 | 이 챕터에서 경험하는 것 |
|------------------|----------------------|----------------------|
| w = 0.5로 직접 설정 | 학습으로 w를 자동 추정 | scikit-learn으로 학습 실행 |
| 정답/오답 직접 확인 | 정확도(accuracy) 지표 | train/test 분할 + 평가 |
| 분류 기준 직접 작성 | 결정 경계 자동 학습 | 분류기 학습 + 시각화 |

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.00F · 브릿지 (Level 0→1 · 실습 편)` |
| 제목 | `ML 실습 입문 — scikit-learn으로 첫 모델 학습` |
| 서브타이틀 | `"w를 직접 설정"에서 "컴퓨터가 w를 찾는" 단계로. Phase 1에서 배울 수식이 실제 코드로 어떻게 동작하는지 먼저 경험합니다.` |
| 이전 챕터 | `ch00e.html` — Ch.00E 수학 브릿지 |
| 다음 챕터 | `ch00a.html` — Ch.00A 로그와 지수함수 (Phase 1 시작) |

---

## EASY BOX 내용

**핵심 비유**: "악기를 연주하기 전에 악기가 어떤 소리를 내는지 한 번 들어보는 것"

**예시 리스트**:
- Level 0: "키 170이면 w×170+b=65로 예측" → w를 내가 고름
- Level 1: "데이터를 보고 컴퓨터가 가장 좋은 w를 찾는다" = 학습
- 이 챕터: scikit-learn으로 학습 버튼 한 번 눌러보기
- AI에서: 논문의 "We trained a model on 60K samples..."가 바로 이 과정이다

---

## 섹션 구성

### 섹션 1: 학습이란 무엇인가

**아이콘**: `🎯`
**제목**: `"학습" = 데이터를 보고 w를 자동으로 찾는 과정`

**설명**: Level 0에서 `w = 0.5, b = 10`으로 직접 설정했습니다. 하지만 실제 AI는 이 값을 데이터에서 자동으로 찾습니다. 이것이 "학습(training)"입니다.

**표 (Level 0 vs ML 학습)**:

| 단계 | Level 0 방식 | ML 학습 방식 |
|------|-------------|-------------|
| 파라미터 설정 | w, b를 직접 설정 | 데이터에서 자동으로 추정 |
| 평가 방법 | 눈으로 확인 | 정확도/오차로 수치 평가 |
| 새 데이터 처리 | 코드를 수정해야 함 | 학습된 모델로 바로 예측 |
| 개선 방법 | w를 직접 바꿔봄 | 더 많은 데이터, 더 좋은 알고리즘 |

**callout (info)**:
- 내용: Phase 1의 Ch.07 (MLE)는 "어떻게 w를 자동으로 찾는가"의 수학적 이유를 다룬다. 이 챕터에서는 그 결과를 먼저 경험하고, Phase 1에서 왜 그렇게 되는지 이해하는 순서다.

---

### 섹션 2: 훈련/테스트 분할 — 공정한 평가의 기초

**아이콘**: `✂️`
**제목**: `훈련 세트 vs 테스트 세트 — 왜 나눠야 하나`

**설명**: "내가 외운 문제만 나오면 100점 받는다". 학습한 데이터로 점수를 매기면 진짜 실력이 아닙니다. 새로운 데이터에서 얼마나 잘 예측하는지 보는 것이 진짜 평가입니다.

**표 (데이터 분할 용어)**:

| 용어 | 역할 | 비유 |
|------|------|------|
| 훈련 세트 (Training set) | 모델이 보고 학습하는 데이터 | 교과서로 공부 |
| 테스트 세트 (Test set) | 학습 후 평가하는 데이터 | 시험 문제 (처음 보는 것) |
| 검증 세트 (Validation set) | 학습 중 성능 확인 | 모의고사 |

**코드 예시**:
```python
from sklearn.model_selection import train_test_split

X = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  # 공부 시간
y = [50, 55, 60, 65, 70, 75, 80, 85, 90, 95]  # 점수

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)
# X_train: 7개, X_test: 3개 (30% 테스트로 사용)
```

**수식 박스**:
- 레이블: `모델 평가 — 정확도`
- 수식:
```
\text{Accuracy} = \frac{\text{맞게 예측한 수}}{\text{전체 예측 수}}
= \frac{1}{n}\sum_{i=1}^{n} \mathbf{1}[\hat{y}_i = y_i]
```
- 주석:
  - `ŷᵢ` (y-hat): 모델의 예측값
  - `yᵢ`: 실제 정답
  - `𝟙[·]`: 조건이 참이면 1, 거짓이면 0 (지시함수)
  - 정확도 = 맞은 개수 / 전체 개수

---

### 섹션 3: scikit-learn으로 첫 모델 실행

**아이콘**: `🤖`
**제목**: `scikit-learn — fit, predict, score 세 단어로 ML 완성`

**설명**: scikit-learn은 Python의 대표 ML 라이브러리입니다. 모든 모델이 같은 인터페이스를 씁니다: `fit()` (학습), `predict()` (예측), `score()` (평가).

**코드 예시 1 — 선형 회귀**:
```python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
import numpy as np

# 공부 시간 → 점수 데이터
hours = np.array([[1], [2], [3], [4], [5], [6], [7], [8]])
scores = np.array([55, 60, 65, 70, 75, 80, 85, 90])

X_train, X_test, y_train, y_test = train_test_split(
    hours, scores, test_size=0.25, random_state=0
)

model = LinearRegression()
model.fit(X_train, y_train)      # 학습: w, b를 자동으로 찾음

print(f"w = {model.coef_[0]:.2f}")      # 기울기 (가중치)
print(f"b = {model.intercept_:.2f}")    # 절편 (편향)

y_pred = model.predict(X_test)   # 예측
print(f"예측: {y_pred}")
print(f"정답: {y_test}")
```

**코드 예시 2 — 분류 (Level 0의 동물 분류기 ML 버전)**:
```python
from sklearn.neighbors import KNeighborsClassifier

# 크기, 무게 → 동물 종류
X = [[10, 5], [80, 40], [15, 8], [90, 50], [5, 2]]
y = ["고양이", "개", "고양이", "개", "고양이"]

model = KNeighborsClassifier(n_neighbors=1)
model.fit(X, y)

# 새 동물 예측 (크기=70, 무게=35)
result = model.predict([[70, 35]])
print(f"예측: {result[0]}")  # "개"
```

**callout (tip)**:
- 내용: `model.fit(X, y)` 한 줄이 Phase 1에서 배울 MLE, 경사하강법, 정규방정식 등의 수학을 모두 내부에서 실행한다. 이 챕터에서는 결과를 먼저 경험하고, Phase 1에서 "왜 이 결과가 나오는가"를 수식으로 이해한다.

---

### 섹션 4: 모델 평가 지표 맛보기

**아이콘**: `📏`
**제목**: `정확도만으로는 부족하다 — 다른 평가 지표들`

**설명**: 정확도가 높다고 항상 좋은 모델이 아닙니다. 예를 들어 암 진단 모델에서 "항상 음성"이라고 하면 정확도가 99%여도 쓸모없습니다.

**표 (자주 쓰는 평가 지표 — 맛보기)**:

| 지표 | 계산 | 언제 중요한가 |
|------|------|-------------|
| 정확도 (Accuracy) | 맞은 수 / 전체 수 | 클래스 비율이 균등할 때 |
| 정밀도 (Precision) | 양성 예측 중 진짜 양성 | 거짓 양성이 치명적일 때 (스팸 필터) |
| 재현율 (Recall) | 실제 양성 중 맞춘 수 | 놓치면 안 될 때 (암 진단) |
| MSE | 오차² 의 평균 | 회귀 모델 평가 |

**callout (info)**:
- 내용: Phase 1의 Ch.09B (모델 평가지표)에서 이 내용을 수식으로 상세히 다룬다. 이 챕터에서는 "지표가 여러 개 있고, 상황에 따라 다른 것을 쓴다"는 감각만 익힌다.

---

## 인터랙티브: 훈련/테스트 분할 시뮬레이터

**제목**: `🔢 테스트 비율에 따른 평가 — 몇 %를 테스트로 써야 할까?`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `test_ratio` | 테스트 비율 (%) | 10 | 50 | 5 | 20 |
| `total_data` | 전체 데이터 수 | 10 | 200 | 10 | 100 |

**JavaScript 로직**:
```javascript
function updateSplit() {
  const ratio = parseInt(document.getElementById('test_ratio').value);
  const total = parseInt(document.getElementById('total_data').value);
  document.getElementById('test_ratio-v').value = ratio;
  document.getElementById('total_data-v').value = total;

  const testN = Math.round(total * ratio / 100);
  const trainN = total - testN;

  const trainBar = '█'.repeat(Math.round(trainN / total * 20));
  const testBar = '░'.repeat(Math.round(testN / total * 20));

  let warning = '';
  if (testN < 5) warning = '⚠️ 테스트 데이터가 너무 적습니다 (평가 신뢰도 낮음)';
  else if (trainN < 10) warning = '⚠️ 훈련 데이터가 너무 적습니다 (모델이 잘 학습 못할 수 있음)';
  else warning = '✅ 적절한 분할 비율입니다';

  document.getElementById('split-res').innerHTML =
    `전체 ${total}개 데이터<br/>
     훈련 세트: ${trainBar} ${trainN}개 (${100-ratio}%)<br/>
     테스트 세트: ${testBar} ${testN}개 (${ratio}%)<br/><br/>
     ${warning}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       일반적으로 80:20 또는 70:30 비율을 사용. 데이터가 많을수록 테스트 비율 줄여도 됨.
     </span>`;
}
updateSplit();
```

---

## 퀴즈

**질문**: 학습한 데이터로 모델을 평가했더니 정확도 98%가 나왔다. 이 모델에 대한 올바른 판단은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 아주 좋은 모델이다 — 98%면 충분하다 | ❌ |
| (b) 의미 없다 — 학습 데이터로 평가하면 "외운 것"을 테스트하는 것이다 | ✅ |
| (c) 테스트 세트가 없으므로 판단 불가하지만 좋아 보인다 | ❌ |
| (d) 정확도가 너무 높으면 오히려 나쁜 모델이다 | ❌ |

**정답 위치**: (b)
**해설**: 학습 데이터로 평가하는 것은 "시험 문제를 미리 보고 시험 보는 것"과 같다. 반드시 한 번도 보지 않은 테스트 세트로 평가해야 진짜 성능을 알 수 있다. 이것이 Phase 1 Ch.08 (Bias-Variance)에서 다루는 "과적합(overfitting)"의 핵심이다.

---

## 주간 체크리스트 (Week 00-F)

```
[ ] "학습"의 의미: w, b를 데이터에서 자동으로 찾는 과정임을 이해한다
[ ] train_test_split으로 데이터를 80:20으로 나눌 수 있다
[ ] model.fit(X_train, y_train)을 실행할 수 있다
[ ] model.predict(X_test)로 예측값을 얻을 수 있다
[ ] 정확도(accuracy)를 계산하고 의미를 설명할 수 있다
[ ] "학습 데이터 정확도 ≠ 테스트 데이터 정확도"를 설명할 수 있다
[ ] 실습: 공부시간→점수 또는 동물 분류기를 scikit-learn으로 재구현
[ ] (선택) 훈련/테스트 정확도가 크게 다를 때 "과적합"이라 부름을 안다
```

---

## Level 0 → Level 1 전환 완료 체크리스트

이 챕터까지 완료하면 Phase 1 시작 준비 완료:

```
✅ Ch.00D: numpy, pandas, matplotlib 기본 사용
✅ Ch.00E: ∀, ∃, Σ, f: X→Y 수학 표기 읽기
✅ Ch.00F: 학습/예측/평가 흐름 경험
     ↓
✅ → Ch.00A 로그와 지수함수
✅ → Ch.00B 수열과 Σ 표기
✅ → Ch.00C 경우의 수·조합
     ↓
🚀 Phase 1 시작: Ch.01 집합과 함수
```
