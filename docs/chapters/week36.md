# Week 36 — 연구 설계·실험 방법론 (Ch.36)

> **파일명**: `chapters/ch36.html`  
> **Phase**: Phase 3-C · 연구 방법론  
> **인터랙티브**: ✅ 연구 가설 설계 도구  
> **우선순위**: P0 (Phase 3 최종 챕터)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.36 · 연구 방법론 (Phase 3 — 10주차)` |
| 제목 | `연구 설계·실험 방법론` |
| 서브타이틀 | `"논문을 읽는 사람"에서 "논문을 쓰는 사람"으로. 연구 가설 생성, Ablation 설계, 통계적 결론, 재현 가능한 실험의 완전 가이드.` |
| 이전 챕터 | `ch35.html` — Ch.35 논문 비판 독해 방법론 |
| 다음 챕터 | — (Phase 3 졸업) |

---

## EASY BOX 내용

**핵심 비유**: "좋은 과학 실험은 '하나씩 바꾸기' 원칙을 따른다 — 요리 레시피를 바꿀 때 소금도 바꾸고 온도도 바꾸면 무엇이 맛을 바꿨는지 모른다. Ablation study는 요인을 하나씩 제거해서 각각의 기여도를 측정하는 것이다."

**예시 리스트**:
- 잘못된 실험: "새 모델 A가 기존 B보다 좋다" — A가 더 많은 데이터를 썼을 가능성
- 좋은 실험: "동일 데이터, 동일 FLOPs, 동일 시드에서 A와 B를 비교 → A가 5개 시드 평균 2.3±0.4% 우수"
- AI에서: NeurIPS/ICML 논문 리뷰어가 "ablation 부족"이라고 reject하는 것이 가장 흔한 이유

---

## 섹션 구성

### 섹션 1: 연구 가설 생성과 검증 설계

**아이콘**: `H`  
**제목**: `연구 가설 — 검증 가능한 주장 만들기`

**설명**: 좋은 연구 가설은 구체적이고 검증 가능해야 합니다. 너무 일반적인 주장("더 좋다")이 아니라 정량적이고 반증 가능한 형태여야 합니다.

**수식 박스 1**:
- 레이블: `가설의 구조 — PICO 프레임`
- 수식:
```
\text{좋은 가설 구조:}
\begin{cases}
\text{Population (P)} & \text{어떤 설정/도메인에서?} \\
\text{Intervention (I)} & \text{제안하는 방법은?} \\
\text{Comparison (C)} & \text{무엇과 비교하는가?} \\
\text{Outcome (O)} & \text{어떤 지표로 측정하는가?}
\end{cases}
```
- 주석:
  - 예시: "ImageNet 분류(P)에서 Sparse Attention(I)을 Dense Attention(C)과 비교하여 Top-1 정확도(O)를 측정한다"
  - 모호한 가설: "우리 방법이 더 좋다" → 검증 불가
  - 좋은 가설: "동일 FLOPs 예산에서 A가 B보다 Top-1 정확도 1% 이상 높다"
  - 귀무가설(H₀): "A와 B의 차이 없다" — 이것을 기각하는 것이 목표

**표 (가설 품질 체크리스트)**:

| 품질 기준 | 나쁜 예 | 좋은 예 |
|---------|--------|--------|
| 구체성 | "더 빠르다" | "GPU 시간 기준 2배 빠르다" |
| 측정 가능성 | "좋은 성능" | "BLEU Score 기준 3점 이상 향상" |
| 반증 가능성 | "항상 좋다" | "CIFAR-10에서 ResNet-50 기준 +1% 이상" |
| 공정성 | "우리 방법 vs 약한 baseline" | "동일 파라미터 수에서 비교" |

---

### 섹션 2: Ablation Study 설계

**아이콘**: `✂`  
**제목**: `Ablation Study — 각 요소의 기여도를 분리`

**설명**: Ablation study는 제안하는 방법의 각 구성 요소를 하나씩 제거하여 각각이 얼마나 기여하는지 측정합니다. NeurIPS/ICML 논문 리뷰에서 가장 많이 요청되는 실험입니다.

**수식 박스 2**:
- 레이블: `Ablation 설계 원칙 — 요소별 독립 기여도`
- 수식:
```
\text{전체 방법 M} = \text{Component}_1 + \text{Component}_2 + \cdots + \text{Component}_k
\Rightarrow \text{Ablation}_i = M \setminus \text{Component}_i
```
- 주석:
  - 각 variant에서 한 가지 요소만 제거 (나머지는 동일)
  - 표 형식: M (전체) > M-A > M-B > M-A-B > baseline
  - 상호작용 효과: A와 B가 함께 있을 때만 효과가 나는 경우 → 개별 ablation만으로 부족

**표 (Ablation 설계 예시 — Transformer 모델)**:

| 변형 | Multi-Head | FFN | LayerNorm | Positional Enc | 정확도 |
|------|:--------:|:---:|:--------:|:------------:|:------:|
| Full | ✅ | ✅ | ✅ | ✅ | 92.1% |
| -MH | ❌ | ✅ | ✅ | ✅ | 88.4% (-3.7%) |
| -FFN | ✅ | ❌ | ✅ | ✅ | 89.2% (-2.9%) |
| -LN | ✅ | ✅ | ❌ | ✅ | 85.1% (-7.0%) |
| -PE | ✅ | ✅ | ✅ | ❌ | 90.3% (-1.8%) |
| Base | ❌ | ❌ | ❌ | ❌ | 71.2% |

**callout (tip)**:
- 내용: **좋은 Ablation의 요소**: (1) 제거 순서를 무작위화하지 않고 독립 제거, (2) 각 변형에서 별도 하이퍼파라미터 최적화 (하나의 최적값을 공유하면 불공정), (3) 여러 시드에서 평균과 표준편차 보고.

---

### 섹션 3: 통계적으로 올바른 결론 도출

**아이콘**: `📊`  
**제목**: `통계적 실험 설계 — 결과를 믿을 수 있게`

**설명**: ML 실험에서 여러 랜덤 시드로 반복해서 평균과 분산을 보고하는 것이 표준입니다. 단일 실험 결과는 통계적으로 신뢰할 수 없습니다.

**수식 박스 3**:
- 레이블: `신뢰구간과 유의성 검정`
- 수식:
```
\bar{x} \pm t_{n-1, \alpha/2} \cdot \frac{s}{\sqrt{n}}
\quad\text{(t-신뢰구간, 95% CI)}
```
- 주석:
  - `x̄`: n번의 실험 평균
  - `s`: 표준편차
  - `t_{n-1, 0.025}`: t-분포 임계값 (n=5이면 ≈ 2.776)
  - 겹치지 않는 신뢰구간 → 두 방법에 유의한 차이 있음
  - n=5 이상 시드, 특히 초기화와 데이터 분할 모두 무작위화

**수식 박스 4** (효과 크기):
- 레이블: `Cohen's d — 효과 크기 측정`
- 수식:
```
d = \frac{\bar{x}_A - \bar{x}_B}{s_{pooled}}
\quad\text{where}\quad
s_{pooled} = \sqrt{\frac{(n_A-1)s_A^2 + (n_B-1)s_B^2}{n_A+n_B-2}}
```
- 주석:
  - d < 0.2: 작은 효과 (통계적 유의해도 실용적 가치 낮음)
  - d ≈ 0.5: 중간 효과
  - d > 0.8: 큰 효과
  - p-value만으로는 부족 — 효과 크기를 함께 보고해야 함

---

### 섹션 4: 재현 가능한 실험 프로토콜

**아이콘**: `🔁`  
**제목**: `재현 가능성 — 다음 연구자가 그대로 할 수 있어야`

**설명**: ML 연구의 재현성 위기(reproducibility crisis)는 심각합니다. 2022년 조사에서 NeurIPS 논문의 절반 이상이 원 결과를 재현하지 못했습니다. 재현 가능한 실험의 기준을 배웁니다.

**수식 박스 5**:
- 레이블: `재현 가능한 실험 체크리스트`
- 수식:
```
\text{재현 가능성 스코어} = \sum_{i=1}^{N} w_i \cdot \mathbf{1}[\text{기준}_i \text{ 충족}]
```
- 주석:
  - 코드 공개 (w=20): GitHub URL 명시
  - 데이터 공개 (w=15): 전처리 포함
  - 하이퍼파라미터 완전 공개 (w=15): 모든 파라미터 값
  - 랜덤 시드 고정 (w=10): numpy, torch.manual_seed 포함
  - 계산 자원 명시 (w=10): GPU 종류, 시간, 비용
  - 실패 실험 보고 (w=10): 작동 안 한 것도 포함

**표 (재현성 vs 새로움 트레이드오프)**:

| 관행 | 재현성 | 새로움 | 권장 |
|------|:------:|:------:|:----:|
| 단일 시드 결과 | ❌ | 빠름 | ❌ |
| 3~5 시드 평균/표준편차 | ✅ | 보통 | ✅ |
| 공개 코드 + 모델 | ✅✅ | 느림 | ✅✅ |
| 사전 등록 (pre-registration) | ✅✅✅ | 매우 느림 | 대규모 연구 권장 |

---

## 인터랙티브: 연구 가설 설계 도구

**제목**: `🔢 실험 설계 품질 점수 — Stanford 연구 방법론 체크리스트`

**JavaScript 로직**:
```javascript
const researchCriteria = [
  { id: 'r1', text: '가설이 PICO 형식으로 구체적으로 정의됨', weight: 10 },
  { id: 'r2', text: '귀무가설(H₀)이 명확히 정의됨', weight: 10 },
  { id: 'r3', text: 'Ablation study가 각 요소를 독립 제거', weight: 15 },
  { id: 'r4', text: '5개 이상 랜덤 시드로 반복 실험', weight: 10 },
  { id: 'r5', text: '평균과 표준편차(또는 CI) 보고', weight: 10 },
  { id: 'r6', text: 'Baseline이 동일 FLOPs/파라미터 수에서 비교', weight: 15 },
  { id: 'r7', text: '코드와 데이터 완전 공개', weight: 10 },
  { id: 'r8', text: '하이퍼파라미터 탐색 과정 공개', weight: 10 },
  { id: 'r9', text: '실패 실험과 음성 결과(negative results) 포함', weight: 5 },
  { id: 'r10', text: '계산 자원(GPU 종류, 시간, 비용) 명시', weight: 5 },
];

function buildResearchChecklist() {
  const container = document.getElementById('research-checklist');
  researchCriteria.forEach(c => {
    const div = document.createElement('div');
    div.style.cssText = 'display:flex;align-items:center;gap:8px;margin:6px 0;';
    div.innerHTML = `<input type="checkbox" id="${c.id}" onchange="calcResScore()">
                     <label for="${c.id}">${c.text} <span style="color:var(--muted);font-size:.8rem">(${c.weight}점)</span></label>`;
    container.appendChild(div);
  });
}

function calcResScore() {
  let score = 0;
  researchCriteria.forEach(c => {
    if (document.getElementById(c.id)?.checked) score += c.weight;
  });
  const level = score < 30 ? '⚠️ 부족 (major revision 수준)' :
                score < 60 ? '📄 기본 (workshop 논문 수준)' :
                score < 80 ? '📊 양호 (venue 논문 수준)' :
                             '🏆 우수 (top venue + 재현성 모범)';
  document.getElementById('res-score').innerHTML =
    `연구 방법론 점수: <strong>${score}점</strong> / 100점<br/>${level}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       80점 이상: NeurIPS/ICML/ICLR 제출 가능한 연구 방법론
     </span>`;
}

buildResearchChecklist();
calcResScore();
```

---

## 퀴즈

**질문**: ML 실험에서 단일 랜덤 시드의 결과만 보고할 때의 주요 문제점은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 모델 학습이 느려진다 | ❌ |
| (b) 초기화에 따른 분산이 숨겨져 결과의 재현 가능성과 신뢰성을 알 수 없다 | ✅ |
| (c) 검증 데이터가 부족해진다 | ❌ |
| (d) 하이퍼파라미터 탐색이 어려워진다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 36)

```
[ ] PICO 형식으로 검증 가능한 연구 가설을 작성할 수 있다
[ ] Ablation 설계 원칙(독립 제거)을 적용할 수 있다
[ ] 실험 결과에 95% 신뢰구간과 표준편차를 계산할 수 있다
[ ] Cohen's d로 효과 크기를 계산하고 해석할 수 있다
[ ] 재현 가능한 실험 체크리스트 10개를 적용할 수 있다
[ ] 논문 리뷰에서 "재현성 문제"를 찾아낼 수 있다
[ ] 자신의 아이디어로 연구 가설 → Ablation → 실험 설계 전체 흐름을 작성할 수 있다
```

---

## Phase 3 졸업 확인

```
Phase 3 전체 완료 조건 (Stanford AI 석사급):

[ ] Phase 3-A (통계적 학습 이론):
    [ ] Ch.27 집중 부등식: Hoeffding으로 표본 복잡도 유도 가능
    [ ] Ch.28 PAC 학습: 유한 ℋ 일반화 경계 유도 가능
    [ ] Ch.29 VC 차원: 선형 분류기 VC=d+1, Rademacher 정의 이해
    [ ] Ch.30 Double Descent: 암묵적 정규화, NTK 직관 설명 가능

[ ] Phase 3-B (딥러닝 이론):
    [ ] Ch.31 Transformer: Attention 수식 독립 유도, Multi-Head 이해
    [ ] Ch.32 Fisher 정보: CR 하한, 자연 그라디언트 연결
    [ ] Ch.33 마팅게일: a.s. 수렴과 SGD 노이즈 연결
    [ ] Ch.34 최적화: Adam 4단계 수식, AdamW 차이 설명

[ ] Phase 3-C (연구 방법론):
    [ ] Ch.35 논문 비판: Contribution 분해, 수식 검증, 반박 생성
    [ ] Ch.36 연구 설계: PICO 가설, Ablation, 재현 가능한 실험

최종 졸업 기준:
"NeurIPS/ICML/ICLR 2024-2025 논문 중 하나를 골라
 Ch.35 프로토콜로 완전 분석 + Ch.36 기준으로 확장 실험을 설계할 수 있다."
```
