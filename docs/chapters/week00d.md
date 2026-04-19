# Week 00-D — Python 데이터 과학 도구 (Ch.00D)

> **파일명**: `chapters/ch00d.html`
> **위치**: Level 0 → Level 1 브릿지 챕터 1번
> **인터랙티브**: ✅ 평균/분산 계산기 (numpy 비교)
> **우선순위**: P0 (브릿지 필수)
> **선행 조건**: Level 0 커리큘럼 완료 (Python 기초 6개월)

---

## 이 챕터가 필요한 이유 (갭 분석)

Level 0 커리큘럼은 순수 Python (리스트, for, def)만 사용한다.
그러나 Phase 1 (Ch.01~14)의 모든 실습 코드는 **numpy, pandas, matplotlib**을 전제한다.

| Level 0에서 하는 것 | Phase 1에서 필요한 것 | 이 챕터에서 배우는 것 |
|------------------|-------------------|--------------------|
| `sum(리스트) / len(리스트)` | `np.mean(array)` | numpy 배열 + 기본 함수 |
| 딕셔너리로 데이터 저장 | `pd.DataFrame` | pandas 기본 조작 |
| print로 결과 출력 | `plt.plot(x, y)` | matplotlib 기본 시각화 |

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.00D · 브릿지 (Level 0→1 · 도구 편)` |
| 제목 | `Python 데이터 과학 도구 — numpy, pandas, matplotlib` |
| 서브타이틀 | `지금까지 직접 짠 코드를 강력한 도구로 바꿉니다. 이 세 가지를 알면 AI 논문의 실험 코드가 읽힙니다.` |
| 이전 챕터 | Level 0 완료 (성적표 자동 계산기, 예측 프로그램) |
| 다음 챕터 | `ch00e.html` — Ch.00E 수학 브릿지 |

---

## EASY BOX 내용

**핵심 비유**: "지금까지 직접 삽으로 팠다면, 이제 포클레인을 배우는 것"

**예시 리스트**:
- 리스트 평균 직접 짜면 5줄 코드 → `np.mean(data)` 한 줄
- 100개 학생 성적 표 만들기 → 딕셔너리 100개 vs `pd.DataFrame` 한 방
- 점수 분포 보기 → print로 숫자 나열 vs `plt.hist(scores)` 한 줄

**AI 연결**: 논문에서 `import numpy as np` `import torch` 첫 두 줄이 보이는 이유가 있다. 모든 AI 연구 코드의 뼈대가 이 도구들이다.

---

## 섹션 구성

### 섹션 1: numpy — 숫자 배열의 언어

**아이콘**: `🔢`
**제목**: `numpy — 수학을 코드로 옮기는 가장 빠른 방법`

**설명**: numpy(Numerical Python)는 숫자 배열을 다루는 라이브러리입니다. Python 리스트보다 훨씬 빠르고, 수학 연산을 한 줄로 처리합니다. AI 논문의 모든 행렬 연산 코드는 numpy(또는 이를 기반으로 한 PyTorch/TensorFlow)로 구현됩니다.

**표 (Python 리스트 vs numpy)**:

| 작업 | 순수 Python | numpy |
|------|------------|-------|
| 배열 만들기 | `[1, 2, 3, 4, 5]` | `np.array([1, 2, 3, 4, 5])` |
| 평균 | `sum(x)/len(x)` | `np.mean(x)` |
| 표준편차 | 10줄 이상 코드 | `np.std(x)` |
| 행렬 곱 | 이중 for문 | `np.dot(A, B)` |
| 모든 원소 +1 | `[v+1 for v in x]` | `x + 1` |

**코드 예시**:
```python
import numpy as np

scores = np.array([90, 85, 92, 78, 95])
print(np.mean(scores))   # 88.0
print(np.std(scores))    # 5.97...
print(np.max(scores))    # 95
print(scores + 5)        # [95 90 97 83 100] — 모든 원소에 +5
```

**수식 박스**:
- 레이블: `numpy와 수학 표기의 대응`
- 수식:
```
\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i
\quad \Leftrightarrow \quad
\texttt{np.mean(x)}
```
- 주석:
  - `x̄`: 평균 (x bar) — `np.mean(x)`
  - `Σ`: 합산 — numpy가 내부에서 처리
  - `n`: 원소 개수 — `len(x)` 또는 `x.shape[0]`

**callout (tip)**:
- 내용: Phase 1의 모든 수식 박스에는 수식과 numpy 코드가 함께 등장합니다. 예: 평균 `E[X] = Σxᵢ/n` → `np.mean(x)`. numpy를 알면 수식이 코드로 바로 보입니다.

---

### 섹션 2: pandas — 데이터 표 다루기

**아이콘**: `📊`
**제목**: `pandas — 엑셀을 Python으로`

**설명**: pandas는 "DataFrame"이라는 표 구조로 데이터를 다룹니다. CSV 파일을 읽거나, 조건으로 필터링하거나, 그룹별로 계산하는 것이 모두 pandas의 역할입니다. 실제 AI 프로젝트의 데이터 전처리는 거의 pandas로 합니다.

**표 (pandas 기본 작업)**:

| 작업 | 코드 |
|------|------|
| DataFrame 만들기 | `pd.DataFrame({"이름": [...], "점수": [...]})` |
| CSV 읽기 | `pd.read_csv("data.csv")` |
| 특정 열 선택 | `df["점수"]` |
| 조건 필터링 | `df[df["점수"] >= 90]` |
| 평균 계산 | `df["점수"].mean()` |
| 그룹별 평균 | `df.groupby("반")["점수"].mean()` |

**코드 예시**:
```python
import pandas as pd

df = pd.DataFrame({
    "이름": ["김철수", "이영희", "박민준"],
    "점수": [90, 85, 92],
    "반": ["A", "B", "A"]
})
print(df[df["점수"] >= 90])          # 90점 이상만
print(df.groupby("반")["점수"].mean()) # 반별 평균
```

**callout (info)**:
- 내용: 논문의 "Experiments" 섹션에서 "We use the MNIST dataset with 60,000 training samples..."처럼 쓰인다. 이 데이터를 불러오고 전처리하는 코드가 pandas + numpy 조합이다. 데이터 로딩 코드를 읽을 수 있으면 논문 재현이 훨씬 쉬워진다.

---

### 섹션 3: matplotlib — 숫자를 그림으로

**아이콘**: `📈`
**제목**: `matplotlib — 데이터를 눈으로 보는 법`

**설명**: matplotlib은 데이터를 그래프로 그리는 라이브러리입니다. 논문의 Figure가 대부분 matplotlib(또는 seaborn)으로 만들어집니다. 선 그래프, 막대 그래프, 히스토그램, 산점도를 한 줄 코드로 그릴 수 있습니다.

**표 (자주 쓰는 그래프 유형)**:

| 그래프 | 코드 | 언제 쓰나 |
|--------|------|----------|
| 선 그래프 | `plt.plot(x, y)` | 학습 곡선, 수렴 추이 |
| 막대 그래프 | `plt.bar(labels, values)` | 모델 비교 |
| 히스토그램 | `plt.hist(data)` | 데이터 분포 확인 |
| 산점도 | `plt.scatter(x, y)` | 두 변수 관계 |

**코드 예시**:
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 11)               # 1~10
y = 1 / x                          # 수열 aₙ = 1/n

plt.plot(x, y, marker='o')
plt.xlabel("n")
plt.ylabel("1/n")
plt.title("수열 aₙ = 1/n의 수렴")
plt.axhline(y=0, color='red', linestyle='--', label='극한값 L=0')
plt.legend()
plt.savefig("convergence.png")
```

**callout (tip)**:
- 내용: 논문에서 "Figure 2(a) shows the training loss curve..."를 보면, 그 그림이 바로 `plt.plot(epoch, loss)` 한 줄로 만들어진 것입니다. 코드와 논문 Figure가 어떻게 연결되는지 이해하면 논문 재현 능력이 생깁니다.

---

## 인터랙티브: numpy vs 순수 Python 속도 비교

**제목**: `🔢 numpy의 힘 — 평균과 표준편차 직접 계산해보기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `ndata` | 데이터 개수 | 5 | 100 | 5 | 20 |

**JavaScript 로직**:
```javascript
function updateNumpyDemo() {
  const n = parseInt(document.getElementById('ndata').value);
  document.getElementById('ndata-v').value = n;

  // 무작위 점수 생성 (시드 고정 흉내)
  const data = [];
  for (let i = 0; i < n; i++) {
    data.push(Math.round(50 + 40 * Math.sin(i * 1.7 + 0.5)));
  }

  const mean = data.reduce((a, b) => a + b, 0) / n;
  const variance = data.reduce((a, b) => a + (b - mean) ** 2, 0) / n;
  const std = Math.sqrt(variance);
  const minV = Math.min(...data);
  const maxV = Math.max(...data);

  document.getElementById('numpy-res').innerHTML =
    `데이터 ${n}개 생성: [${data.slice(0, 5).join(', ')}${n > 5 ? ', ...' : ''}]<br/>
     <strong>평균 (np.mean)</strong>: ${mean.toFixed(2)}<br/>
     <strong>표준편차 (np.std)</strong>: ${std.toFixed(2)}<br/>
     <strong>최소 (np.min)</strong>: ${minV} &nbsp;|&nbsp; <strong>최대 (np.max)</strong>: ${maxV}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       순수 Python: 이 계산에 ${n * 3}줄 코드 필요 → numpy: 5줄로 완료
     </span>`;
}
updateNumpyDemo();
```

---

## 퀴즈

**질문**: numpy 배열 `x = np.array([10, 20, 30, 40, 50])`에서 `x + 5`의 결과는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 15 (첫 번째 원소만 +5) | ❌ |
| (b) `[10, 20, 30, 40, 55]` (마지막만 +5) | ❌ |
| (c) `[15, 25, 35, 45, 55]` (모든 원소에 +5) | ✅ |
| (d) 오류 발생 (리스트와 숫자는 더할 수 없음) | ❌ |

**정답 위치**: (c)
**해설**: numpy의 "브로드캐스팅" — 스칼라(단일 값)와 배열을 연산하면 모든 원소에 적용된다. 이것이 `y = wx + b`를 numpy로 쓸 때 `y = w * x + b` 한 줄이 되는 이유다.

---

## 주간 체크리스트 (Week 00-D)

```
[ ] numpy 배열 만들기: np.array([1, 2, 3])
[ ] numpy 기본 함수: np.mean, np.std, np.max, np.min
[ ] numpy 브로드캐스팅: x + 5, x * 2 등
[ ] pandas DataFrame 만들기
[ ] pandas 조건 필터링: df[df["컬럼"] > 값]
[ ] matplotlib 선 그래프: plt.plot(x, y)
[ ] matplotlib 히스토그램: plt.hist(data)
[ ] 실습: Level 0 성적 분석 프로그램을 numpy/pandas로 재작성
```
