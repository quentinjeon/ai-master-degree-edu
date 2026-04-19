# Week 04 — 확률론 기초 & 조건부확률 (Ch.4)

> **파일명**: `chapters/ch04.html`  
> **대응 섹션**: `c4`  
> **인터랙티브**: ✅ 베이즈 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.4 · 확률 & 통계 (3~4주차)` |
| 제목 | `확률론 기초` |
| 서브타이틀 | `불확실성을 수학적으로 표현하는 언어. AI 모델의 예측은 본질적으로 확률입니다.` |
| 이전 챕터 | `ch03.html` — Ch.3 선형대수 |
| 다음 챕터 | `ch05.html` — Ch.5 확률분포 |

---

## EASY BOX 내용

**핵심 비유**: "확률은 '어떤 일이 일어날 가능성을 0~1 사이 숫자로 나타낸 것'이다. 1이면 무조건, 0이면 절대 안 일어난다."

**예시 리스트**:
- 동전 앞면: 50% = 0.5
- 내일 비: 30% = 0.3
- AI에서: 고양이 예측 0.9, 강아지 예측 0.1 — AI는 확률로 생각한다

---

## 섹션 구성

### 섹션 1: 확률의 기본 공리

**아이콘**: `🎲`  
**제목**: `확률의 3가지 기본 공리 (Kolmogorov)`

**설명**: 모든 확률 이론의 기초가 되는 3가지 공리입니다.

**수식 박스 1**:
- 레이블: `Kolmogorov 공리`
- 수식: `\text{1. } P(A) \ge 0 \qquad \text{2. } P(\Omega) = 1 \qquad \text{3. } P(A \cup B) = P(A) + P(B) \text{ (A, B 배반 시)}`
- 주석: `Ω`: 표본공간 (모든 결과의 집합), 공리 3: 배반(disjoint) 사건의 합집합 확률은 더한 값

**표 (핵심 용어)**:

| 용어 | 설명 | 예시 |
|------|------|------|
| 표본공간 Ω | 발생 가능한 모든 결과 | 동전: {앞, 뒤} |
| 사건 A | 표본공간의 부분집합 | 앞면만: {앞} |
| P(A) | 사건 A의 확률 | P({앞}) = 0.5 |
| 여사건 Aᶜ | A가 아닌 사건 | 뒷면: P(Aᶜ) = 1 - P(A) |

---

### 섹션 2: 조건부확률

**아이콘**: `|`  
**제목**: `조건부확률 (Conditional Probability)`

**EASY BOX (섹션 내 추가)**:
- 비유: 비가 올 확률이 30%였는데, 먹구름을 봤다. 그러면 비 올 확률이 80%로 올라간다. **새 정보가 생기면 확률이 바뀐다!**

**수식 박스 2**:
- 레이블: `조건부확률 공식`
- 수식: `P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad P(B) > 0`
- 주석: "B가 일어났음을 알게 됐을 때, A가 일어날 확률". 표본공간이 Ω에서 B로 줄어든다.

**callout (info)**:
- 내용: AI에서 P(y|x)는 "입력 x가 주어졌을 때 y가 나올 확률" — 모든 분류기/회귀기의 목표

---

### 섹션 3: 베이즈 정리

**아이콘**: `🔄`  
**제목**: `베이즈 정리 (Bayes' Theorem)`

**EASY BOX (섹션 내 추가)**:
- 비유: "처음 생각(prior)" + "새 정보(likelihood)" = "업데이트된 생각(posterior)"
- 예시: 병이 드묾(prior 낮음) → 검사 양성(새 정보) → 하지만 검사기도 틀릴 수 있음! → posterior 계산 필요

**수식 박스 3**:
- 레이블: `베이즈 정리`
- 수식:
```
\underbrace{P(\theta \mid \mathcal{D})}_{\text{posterior}} = 
\frac{\overbrace{P(\mathcal{D} \mid \theta)}^{\text{likelihood}} \cdot \overbrace{P(\theta)}^{\text{prior}}}
{\underbrace{P(\mathcal{D})}_{\text{evidence}}}
```
- 주석:
  - posterior: 데이터를 본 후 믿음
  - likelihood: θ일 때 데이터 관측 확률
  - prior: 데이터 보기 전 믿음
  - evidence: 정규화 상수, 보통 ∝를 써서 생략

**표 (4가지 요소)**:

| 용어 | 의미 | 비유 | AI 예시 |
|------|------|------|---------|
| Prior P(θ) | 데이터 보기 전 믿음 | 병원 가기 전 내 생각 | 파라미터 초기 분포 |
| Likelihood P(D|θ) | θ일 때 D가 나올 확률 | 이 동전으로 9번 앞면 가능성 | 데이터 생성 확률 |
| Posterior P(θ|D) | D를 보고 업데이트된 믿음 | 검사 결과 본 후 내 생각 | 학습 후 파라미터 분포 |
| Evidence P(D) | 정규화 상수 | 기준점 | 보통 ∝ 사용 |

**단계별 설명**:
1. **Prior 설정**: 데이터 없이 파라미터에 대한 초기 믿음 설정
2. **Likelihood 계산**: 현재 파라미터로 이 데이터가 나올 확률 계산
3. **Posterior 업데이트**: 베이즈 정리로 믿음을 업데이트
4. **반복**: 새 데이터가 올 때마다 posterior가 새로운 prior가 됨

---

## 인터랙티브: 베이즈 계산기 (병 검사 예제)

**제목**: `🔢 베이즈 정리 계산기 — 병 검사 예제`

**설명**: P(병|양성) = P(양성|병) × P(병) / P(양성)을 직접 계산해보세요.

**파라미터**:

| ID | 이름 | min | max | step | default | 설명 |
|----|------|-----|-----|------|---------|------|
| `bay-prev` | 병 유병률 P(병) | 0.001 | 0.5 | 0.001 | 0.01 | prior |
| `bay-sens` | 검사 민감도 P(양성|병) | 0.5 | 1.0 | 0.01 | 0.95 | sensitivity |
| `bay-spec` | 검사 특이도 P(음성|건강) | 0.5 | 1.0 | 0.01 | 0.90 | specificity |

**JavaScript 로직**:
```javascript
function updateBayes() {
  const prev = parseFloat(document.getElementById('bay-prev').value);
  const sens = parseFloat(document.getElementById('bay-sens').value);
  const spec = parseFloat(document.getElementById('bay-spec').value);
  document.getElementById('bay-prev-v').value = (prev*100).toFixed(1) + '%';
  document.getElementById('bay-sens-v').value = (sens*100).toFixed(0) + '%';
  document.getElementById('bay-spec-v').value = (spec*100).toFixed(0) + '%';

  const falsePositiveRate = 1 - spec;
  const pPositive = sens * prev + falsePositiveRate * (1 - prev);
  const posterior = (sens * prev) / pPositive;

  document.getElementById('bay-res').innerHTML =
    `P(병 | 양성) = <strong>${(posterior*100).toFixed(1)}%</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">
      P(양성) = ${(pPositive*100).toFixed(2)}% | 
      유병률 ${(prev*100).toFixed(1)}%, 
      양성인데 실제 병: ${(posterior*100).toFixed(1)}%
    </span>`;
}
updateBayes();
```

---

## 퀴즈

**질문**: 베이즈 정리에서 "데이터를 보기 전의 믿음"을 나타내는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) posterior | ❌ |
| (b) prior | ✅ |
| (c) likelihood | ❌ |
| (d) evidence | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 4)

```
[ ] 표본공간·사건·확률의 관계를 설명할 수 있다
[ ] P(A|B)를 말로 설명할 수 있다
[ ] 독립성 조건 P(A∩B) = P(A)P(B)를 안다
[ ] prior/likelihood/posterior를 각각 설명할 수 있다
[ ] 베이즈 정리를 실제 예시(병 검사)로 설명할 수 있다
[ ] base rate fallacy가 무엇인지 안다
```
