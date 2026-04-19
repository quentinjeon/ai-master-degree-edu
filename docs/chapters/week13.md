# Week 13~14 — Calibration & 불확실성 (Ch.13)

> **파일명**: `chapters/ch13.html`  
> **인터랙티브**: ✅ Calibration 시뮬레이터  
> **우선순위**: P2

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.13 · 응용 (15주차)` |
| 제목 | `불확실성과 캘리브레이션` |
| 서브타이틀 | `AI를 언제 믿고 언제 멈출지 이해합니다. 자신만만하게 틀리는 AI가 제일 위험합니다.` |
| 이전 챕터 | `ch12.html` — Ch.12 딥러닝과 LLM |
| 다음 챕터 | `ch14.html` — Ch.14 신뢰구간과 가설검정 (확장) |

---

## EASY BOX 내용

**핵심 비유**: "친구가 '나 90% 확실해'라고 100번 말했는데 실제로 90번 맞으면 믿을 수 있다. 60번만 맞으면 과신이다. AI도 똑같다!"

**예시 리스트**:
- 날씨 예보가 "비 올 확률 70%"라고 했을 때, 실제로 70%의 경우 비가 오면 잘 calibrated
- AI 의료 진단이 "90% 확신"인데 실제로 60%만 맞으면 → 위험한 AI
- AI에서: 실서비스(의료, 금융, 자율주행)에서 calibration은 정확도만큼 중요

---

## 섹션 구성

### 섹션 1: 불확실성의 종류

**아이콘**: `🌀`  
**제목**: `불확실성의 두 가지 종류`

**표 (두 가지 불확실성)**:

| 종류 | 이름 | 원인 | 줄일 수 있는가? |
|------|------|------|---------------|
| Aleatoric | 우연적 불확실성 | 데이터 자체의 노이즈 | ❌ (본질적) |
| Epistemic | 인식론적 불확실성 | 모델의 지식 부족 | ✅ (더 많은 데이터로) |

**callout (info)**:
- 내용:
  - Aleatoric: 동전의 앞/뒤 — 100% 예측은 불가능
  - Epistemic: AI가 본 적 없는 언어로 된 질문 — 학습 데이터 확장으로 줄일 수 있음
  - 좋은 AI는 두 종류를 구분하고 적절히 표현해야 함

---

### 섹션 2: Calibration

**아이콘**: `🌡️`  
**제목**: `캘리브레이션 (Calibration)`

**설명**: 모델이 예측하는 신뢰도(confidence)가 실제 정확도(accuracy)와 일치하는 정도입니다.

**수식 박스 1**:
- 레이블: `완벽한 Calibration 조건`
- 수식:
```
P(\hat{Y} = Y \mid \hat{P} = p) = p, \quad \forall p \in [0, 1]
```
- 주석:
  - "신뢰도 p라고 말한 예측들 중에서, 실제로 p 비율이 정답이어야 함"
  - 90%라고 말하면 90%가 맞아야 함 → well-calibrated
  - 90%라고 말하는데 60%만 맞으면 → over-confident (흔한 문제)

**개념 카드**:
- 제목: `🔑 ECE (Expected Calibration Error)`
- 내용:
  - `ECE = Σ_b (n_b/n) |acc(b) - conf(b)|`
  - 신뢰도 구간(bin)별로 실제 정확도와의 차이를 가중 평균
  - ECE = 0이면 완벽한 calibration
  - 딥러닝 모델은 보통 over-confident (ECE > 0)

---

### 섹션 3: Threshold Decision & Human Handoff

**아이콘**: `🚦`  
**제목**: `임계값 결정 & 사람에게 넘기기`

**설명**: 실서비스에서 AI의 신뢰도에 따라 응답, 보류, 전문가 연결을 결정합니다.

**표 (의사결정 정책)**:

| 신뢰도 구간 | 결정 | 설명 |
|------------|------|------|
| p ≥ 0.9 | 자동 응답 | 충분히 확실함 |
| 0.5 ≤ p < 0.9 | 사람 확인 요청 | 불확실, 추가 검토 필요 |
| p < 0.5 | 전문가에게 연결 | AI가 모르는 영역 |

**callout (warn)**:
- 제목: `자신만만하게 틀리는 AI가 가장 위험`
- 내용: 정확도 90%의 모델이라도 over-confident하면 잘못된 결정 유발. 특히 의료, 법률, 금융 도메인에서는 "모르면 모른다고 말하는 AI"가 필수.

---

## 인터랙티브: Calibration 시뮬레이터

**제목**: `🔢 Calibration 확인 — 100번 예측 시뮬레이션`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `cal-conf` | 모델 신뢰도 | 0.1 | 1.0 | 0.05 | 0.9 |
| `cal-acc` | 실제 정확도 | 0.1 | 1.0 | 0.05 | 0.6 |

**JavaScript**:
```javascript
function updateCal() {
  const conf = parseFloat(document.getElementById('cal-conf').value);
  const acc = parseFloat(document.getElementById('cal-acc').value);
  document.getElementById('cal-conf-v').value = (conf*100).toFixed(0)+'%';
  document.getElementById('cal-acc-v').value = (acc*100).toFixed(0)+'%';

  const ece = Math.abs(conf - acc);
  const status = ece < 0.05 ? '✅ 잘 calibrated' 
    : conf > acc ? `⚠️ Over-confident (${(ece*100).toFixed(0)}% 과신)` 
    : `⚠️ Under-confident (${(ece*100).toFixed(0)}% 과소)`;

  document.getElementById('cal-res').innerHTML =
    `신뢰도: ${(conf*100).toFixed(0)}% | 실제 정확도: ${(acc*100).toFixed(0)}% | ECE ≈ <strong>${ece.toFixed(2)}</strong><br/>
    <span style="font-size:.78rem;color:var(--muted)">${status}</span>`;
}
updateCal();
```

---

## 퀴즈

**질문**: AI가 "95% 확신"이라고 100번 말했는데 실제로 60번만 맞았다. 이 모델의 문제는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Under-fitting | ❌ |
| (b) Over-confident (calibration 불량) | ✅ |
| (c) 표본 크기가 너무 작음 | ❌ |
| (d) Learning rate가 너무 큼 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 15)

```
[ ] Aleatoric vs Epistemic uncertainty를 구분할 수 있다
[ ] calibration을 정의할 수 있다
[ ] over-confident가 왜 위험한지 설명할 수 있다
[ ] ECE가 무엇인지 설명할 수 있다
[ ] threshold decision 정책을 설명할 수 있다
[ ] 실서비스에서 human handoff가 필요한 이유를 설명할 수 있다
```
