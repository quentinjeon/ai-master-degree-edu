# Week 11~12 — 정보이론 (Ch.11)

> **파일명**: `chapters/ch11.html`  
> **인터랙티브**: ✅ Entropy & KL divergence 계산기  
> **우선순위**: P2

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.11 · 정보이론 (13주차)` |
| 제목 | `정보이론: Entropy, Cross-Entropy, KL Divergence` |
| 서브타이틀 | `딥러닝 손실함수의 수학적 근거. 불확실성을 정량화하고 두 분포의 차이를 측정합니다.` |
| 이전 챕터 | `ch10.html` — Ch.10 Softmax & 다중분류 |
| 다음 챕터 | `ch12.html` — Ch.12 딥러닝과 LLM |

---

## EASY BOX 내용

**핵심 비유**: "Entropy는 '얼마나 헷갈리는지'. Cross-Entropy는 '정답과 AI 예측이 얼마나 다른지 재는 벌점'. KL Divergence는 '두 생각이 얼마나 다른지'."

**예시 리스트**:
- Entropy: 동전이 앞/뒤 반반 → 제일 헷갈림 → H 최대. 항상 앞면 → 전혀 헷갈리지 않음 → H = 0
- Cross-Entropy: AI가 "고양이 99%"라고 했는데 정답이 고양이 → 벌점 작음. "고양이 5%"인데 정답이 고양이 → 벌점 큼
- AI에서: 딥러닝 분류 학습 = Cross-Entropy 손실 최소화 = KL(P||Q) 최소화

---

## 섹션 구성

### 섹션 1: Shannon Entropy

**아이콘**: `🌀`  
**제목**: `엔트로피 (Entropy) — 불확실성의 척도`

**설명**: Shannon Entropy는 확률분포의 불확실성(정보량)을 측정합니다.

**수식 박스 1**:
- 레이블: `Shannon Entropy`
- 수식: `H(P) = -\sum_{x} P(x) \log_2 P(x) \ge 0`
- 주석:
  - log₂ 사용 시: 단위는 "bits" (밑이 e이면 "nats")
  - P(x) = 1인 항은 0 (0 log 0 = 0으로 정의)
  - 균등분포일 때 최대: H(uniform on K) = log₂K bits
  - 확실할수록 (P=1인 값 있음) H = 0

**표 (entropy 예시)**:

| 분포 | H 값 | 의미 |
|------|------|------|
| [1.0, 0.0] | 0 bits | 완전히 확실 |
| [0.5, 0.5] | 1 bit | 최대 불확실 (동전) |
| [0.9, 0.1] | 0.469 bits | 어느 정도 확실 |
| 균등 (K클래스) | log₂K bits | K가 클수록 불확실 |

---

### 섹션 2: Cross-Entropy

**아이콘**: `🏹`  
**제목**: `교차 엔트로피 (Cross-Entropy)`

**설명**: 정답 분포 P를 모델 분포 Q로 인코딩할 때 필요한 평균 비트 수입니다.

**수식 박스 2**:
- 레이블: `Cross-Entropy`
- 수식:
```
H(P, Q) = -\sum_{x} P(x) \log Q(x) = H(P) + D_{KL}(P \| Q)
```
- 주석:
  - P: 정답 분포 (one-hot label)
  - Q: 모델의 예측 분포 (softmax 출력)
  - H(P): 정답 자체의 불확실성 (학습으로 줄일 수 없음)
  - D_KL(P||Q): P와 Q의 차이 (학습으로 줄이는 부분)

---

### 섹션 3: KL Divergence

**아이콘**: `↔️`  
**제목**: `KL 발산 (Kullback-Leibler Divergence)`

**수식 박스 3**:
- 레이블: `KL Divergence (비대칭 거리)`
- 수식:
```
D_{KL}(P \| Q) = \sum_x P(x) \log \frac{P(x)}{Q(x)} = H(P,Q) - H(P) \ge 0
```
- 주석:
  - 항상 ≥ 0 (Gibbs 부등식)
  - P = Q일 때만 0
  - **비대칭**: D_KL(P||Q) ≠ D_KL(Q||P)
  - "P를 Q로 근사할 때의 정보 손실"

**표 (세 개념 비교)**:

| 개념 | 수식 | 의미 | AI 역할 |
|------|------|------|---------|
| Entropy H(P) | `-∑P log P` | P 자체의 불확실성 | 정보량 측정, 결정트리 |
| Cross-Entropy H(P,Q) | `-∑P log Q` | Q로 P를 표현할 때 비용 | 분류 손실함수 |
| KL Divergence D(P‖Q) | `∑P log(P/Q)` | P와 Q의 차이 | VAE, RLHF, 지식증류 |

**callout (info)**:
- 내용: `H(P,Q) = H(P) + D_KL(P||Q)`. 정답 레이블이 one-hot이면 H(P)=0 이므로 CE = KL. 따라서 **CE 최소화 = KL 최소화 = 두 분포를 최대한 비슷하게 만들기**

---

## 인터랙티브: Entropy & KL 계산기

**제목**: `🔢 Entropy와 KL Divergence 계산기 (2클래스)`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `ent-p` | 정답분포 P(x=1) | 0.01 | 0.99 | 0.01 | 0.5 |
| `ent-q` | 모델분포 Q(x=1) | 0.01 | 0.99 | 0.01 | 0.8 |

**JavaScript**:
```javascript
function updateEnt() {
  const p1 = parseFloat(document.getElementById('ent-p').value);
  const p0 = 1 - p1;
  const q1 = parseFloat(document.getElementById('ent-q').value);
  const q0 = 1 - q1;
  document.getElementById('ent-p-v').value = p1.toFixed(2);
  document.getElementById('ent-q-v').value = q1.toFixed(2);

  const H_P = -(p1*Math.log2(p1) + p0*Math.log2(p0));
  const H_PQ = -(p1*Math.log2(q1) + p0*Math.log2(q0));
  const KL = H_PQ - H_P;

  document.getElementById('ent-res').innerHTML =
    `H(P) = <strong>${H_P.toFixed(4)}</strong> bits &nbsp;|&nbsp; 
     H(P,Q) = <strong>${H_PQ.toFixed(4)}</strong> bits &nbsp;|&nbsp; 
     D_KL(P‖Q) = <strong>${KL.toFixed(4)}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       P=[${p1.toFixed(2)},${p0.toFixed(2)}], Q=[${q1.toFixed(2)},${q0.toFixed(2)}] | 
       H(P,Q) = H(P) + KL 확인: ${(H_P + KL).toFixed(4)}
     </span>`;
}
updateEnt();
```

---

## 퀴즈

**질문**: Cross-Entropy H(P,Q)를 최소화하는 것은 어떤 것과 동치인가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) H(P)를 최소화하는 것 | ❌ |
| (b) KL(P‖Q)를 최소화하는 것 | ✅ (H(P)는 상수이므로) |
| (c) Q의 Entropy를 최소화하는 것 | ❌ |
| (d) P와 Q의 합을 최소화하는 것 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 13)

```
[ ] Entropy H(P)를 수식과 함께 설명할 수 있다
[ ] 균등분포에서 entropy가 최대임을 알 수 있다
[ ] Cross-Entropy H(P,Q)를 설명할 수 있다
[ ] KL Divergence를 설명할 수 있다
[ ] H(P,Q) = H(P) + KL(P‖Q) 관계를 설명할 수 있다
[ ] CE 최소화 = KL 최소화임을 설명할 수 있다
```
