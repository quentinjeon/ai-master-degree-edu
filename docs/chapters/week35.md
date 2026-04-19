# Week 35 — 논문 비판 독해 방법론 (Ch.35)

> **파일명**: `chapters/ch35.html`  
> **Phase**: Phase 3-C · 연구 방법론  
> **인터랙티브**: ✅ 논문 분석 체크리스트 인터랙티브  
> **우선순위**: P0 (Stanford AI 핵심)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.35 · 연구 방법론 (Phase 3 — 9주차)` |
| 제목 | `논문 비판 독해 방법론` |
| 서브타이틀 | `"읽는 사람"에서 "검증하는 사람"으로. Contribution 분해, 수식 검증, 실험 비판, 연구 질문 생성까지 — Stanford AI 석사의 논문 독해 프로토콜.` |
| 이전 챕터 | `ch34.html` — Ch.34 고급 최적화 이론 |
| 다음 챕터 | `ch36.html` — Ch.36 연구 설계·실험 방법론 |

---

## EASY BOX 내용

**핵심 비유**: "명탐정은 사건을 들을 때 '이야기를 믿는 사람'이 아니라 '증거를 검증하는 사람'이다. 논문도 마찬가지 — Stanford 수준은 저자의 주장을 그대로 받아들이는 것이 아니라, 수식을 직접 유도하고, 실험을 재설계하고, '이 주장이 틀릴 수 있는 조건'을 찾아내는 것이다."

**예시 리스트**:
- 논문 읽기 (초급): Abstract를 이해하고 결론을 기억한다
- 논문 읽기 (중급): 핵심 수식을 이해하고 실험 결과를 확인한다
- AI에서 (Stanford 수준): Contribution을 기존 문헌에 위치시키고, 수식을 독립적으로 유도하고, "이 결과가 과장됐을 가능성"을 정량적으로 분석한다

---

## 섹션 구성

### 섹션 1: 논문의 구조와 핵심 추출

**아이콘**: `📄`  
**제목**: `논문 구조 분석 — 5분 안에 핵심 파악하기`

**설명**: 효율적인 논문 독해는 구조를 이해하는 것에서 시작합니다. Abstract → Introduction → Conclusion을 먼저 읽어 "무엇을, 왜, 어떻게"를 파악한 다음 수식과 실험으로 들어갑니다.

**표 (논문 섹션별 핵심 추출 방법)**:

| 섹션 | 찾아야 할 것 | Stanford 수준 질문 |
|------|-----------|-----------------|
| Abstract | Contribution 1~3개 | "이것이 정말 새로운가?" |
| Introduction | 문제 정의, 기존 연구의 한계 | "기존 방법의 한계가 공정하게 서술됐는가?" |
| Related Work | 비교 대상 논문들 | "인용이 누락된 중요 논문이 있는가?" |
| Method | 핵심 수식, 알고리즘 | "이 수식이 실제로 작동하는가? 직접 유도 가능한가?" |
| Experiments | Baseline, 데이터셋, 평가지표 | "Baseline이 충분히 강한가? 데이터셋 선택에 편향이 있는가?" |
| Results | 성능 숫자 | "차이가 통계적으로 유의한가? 분산은?" |
| Ablation | 각 요소의 기여도 | "충분한 Ablation인가? 빠진 실험이 있는가?" |

**개념 카드**:
- 제목: `🔑 Contribution 분해 프레임 (Stanford 3-Layer)`
- 내용:
  - **Layer 1 — 기술적 Contribution**: 새로운 알고리즘/아키텍처/이론이 무엇인가
  - **Layer 2 — 경험적 Contribution**: 어떤 벤치마크에서 얼마나 개선됐는가
  - **Layer 3 — 개념적 Contribution**: 기존 이해를 어떻게 바꾸는가 (insights)
  - 대부분의 논문은 Layer 1+2. Layer 3까지 있는 논문이 최상위 venue에 실림

---

### 섹션 2: 수식 검증 프로토콜

**아이콘**: `∇`  
**제목**: `수식 완전 검증 — "믿지 말고 유도하라"`

**설명**: Stanford 수준의 논문 독해는 핵심 수식을 독립적으로 유도할 수 있어야 합니다. 논문에서 "it can be shown that..."으로 생략된 부분이 바로 검증해야 할 곳입니다.

**수식 박스 1**:
- 레이블: `수식 검증 4단계 프로토콜`
- 수식:
```
\text{Step 1: 기호 목록} \quad \theta \in \mathbb{R}^d,\ f:\mathbb{R}^d\to\mathbb{R},\ \ldots
\text{Step 2: 가정 목록} \quad \text{Assumption A1 (L-smooth), A2 (convex), ...}
\text{Step 3: 독립 유도} \quad \text{논문 없이 수식 전개}
\text{Step 4: 반례 탐색} \quad \text{가정이 위반되면 수식이 틀리는가?}
```
- 주석:
  - Step 1: 논문의 Notation 섹션을 모두 정리
  - Step 2: 각 Theorem의 가정을 명시적으로 나열 (논문에서 흩어진 경우 많음)
  - Step 3: 직접 유도 → 논문과 다른 결과가 나오면 오류 발견 가능
  - Step 4: 반례 = 논문 주장의 한계를 이해

**callout (warn)**:
- 내용: **논문의 오류 발견 빈도**: 저명한 논문에도 수식 오류가 있습니다. NeurIPS 2024에서도 accepted paper에서 Theorem 증명 오류가 다수 발견됐습니다. 직접 검증하는 습관이 연구 역량의 핵심입니다.

---

### 섹션 3: 실험 설계 비판 체크리스트

**아이콘**: `🔬`  
**제목**: `실험 비판 — "이 결과가 신뢰할 만한가?"`

**설명**: 논문의 실험 설계를 체계적으로 비판합니다. Baseline 선택, 데이터셋 편향, 통계적 유의성, 공정한 비교가 핵심입니다.

**표 (실험 비판 체크리스트)**:

| 비판 포인트 | Stanford 수준 질문 | 빨간 깃발 |
|-----------|----------------|---------|
| Baseline 강도 | "최신 SOTA와 비교했는가? 튜닝은 공평한가?" | Baseline이 2년 이상 지난 것 |
| 하이퍼파라미터 | "제안 방법만 튜닝하지 않았는가?" | Baseline의 lr, 배치크기가 최적화 안 됨 |
| 데이터셋 선택 | "왜 이 벤치마크인가? 의도적 선택인가?" | 자기 방법이 잘 되는 데이터셋만 선택 |
| 분산/오차 | "표준편차, 신뢰구간을 보고했는가?" | 표준편차 없는 단일 수치 |
| 스케일 | "다른 크기의 모델/데이터에서도 성립하는가?" | 단 하나의 setting에서만 실험 |
| 재현성 | "코드, 데이터, 랜덤 시드가 공개됐는가?" | "코드 공개 예정" 문구 |

**수식 박스 2**:
- 레이블: `통계적 유의성 검정 — 논문 결과가 "진짜"인가`
- 수식:
```
p\text{-value} = P(\text{관측된 결과 이상 극단적인 결과} | H_0)
\quad\Rightarrow\quad
\text{유의} \iff p < \alpha \text{ (일반적으로 } \alpha = 0.05\text{)}
```
- 주석:
  - H₀: "두 방법의 성능에 차이 없다" (귀무가설)
  - p < 0.05: 5% 이하 확률로만 이런 결과가 나옴 → 차이가 실제일 가능성
  - ML 논문에서 p-value 없는 성능 비교는 통계적으로 신뢰하기 어려움
  - 주의: p-value가 작아도 effect size가 작으면 실용적 의의 없음

---

### 섹션 4: 연구 질문 생성 — 논문을 넘어서

**아이콘**: `?`  
**제목**: `연구 질문 생성 — Stanford 수준의 핵심 역량`

**설명**: 논문을 읽는 최종 목표는 새로운 연구 질문을 생성하는 것입니다. 이것이 "논문을 이해하는 사람"과 "논문을 확장하는 사람"의 차이입니다.

**표 (연구 질문 생성 프레임워크)**:

| 유형 | 질문 형태 | 예시 |
|------|---------|------|
| 일반화 | "다른 도메인/데이터에서도 성립하는가?" | "영어 NLP 결과가 다국어에서도 유효한가?" |
| 한계 탐색 | "이 방법이 실패하는 조건은?" | "분포 변화(distribution shift)에서 성능은?" |
| 단순화 | "복잡도를 줄여도 비슷한 성능인가?" | "full attention 없이 sparse attention으로 같은 결과?" |
| 이론-실전 Gap | "이론 증명의 가정이 실전에서 성립하는가?" | "L-smooth 가정이 실제 loss landscape에서 맞는가?" |
| 교차 영역 | "다른 분야 방법을 이곳에 적용하면?" | "컴퓨터 비전의 augmentation이 NLP에서?" |
| 확장 | "더 큰/작은 스케일에서 어떻게 되는가?" | "100억 파라미터에서도 같은 scaling law?" |

**수식 박스 3**:
- 레이블: `논리적 반박 생성 — 주장을 부정하는 조건 찾기`
- 수식:
```
\text{주장: 방법 A가 B보다 성능 우수 (f_A > f_B)}
\text{반박 조건 탐색:}
\begin{cases}
\text{분포 변화} & P_{test} \neq P_{train} \\
\text{데이터 크기} & n \to \infty \text{ or } n \to 0 \\
\text{도메인 이동} & \text{다른 태스크/언어/모달리티} \\
\text{계산 예산} & \text{동일 FLOPs 대비 비교}
\end{cases}
```
- 주석:
  - 논문이 주장하는 조건(설정)에서 벗어났을 때 결론이 뒤집히면 주장이 약함
  - "fair comparison"의 핵심: 동일 계산 예산(FLOPs)에서 비교
  - "동일 파라미터 수"가 아닌 "동일 추론 비용"이 더 공정한 경우가 많음

---

## 인터랙티브: 논문 분석 체크리스트

**제목**: `🔢 논문 비판 점수 계산기 — Stanford 프로토콜`

**파라미터** (체크박스 UI):

**JavaScript 로직**:
```javascript
const criteria = [
  { id: 'c1', text: '수식 3개 이상 독립 유도 완료', weight: 15 },
  { id: 'c2', text: 'Contribution을 1~3개로 압축', weight: 10 },
  { id: 'c3', text: 'Baseline의 공정성 평가 완료', weight: 15 },
  { id: 'c4', text: '통계적 유의성 확인 (분산/오차 확인)', weight: 10 },
  { id: 'c5', text: '데이터셋 선택 편향 분석', weight: 10 },
  { id: 'c6', text: '한계 조건 3개 이상 도출', weight: 15 },
  { id: 'c7', text: '새로운 연구 질문 2개 이상 생성', weight: 15 },
  { id: 'c8', text: '확장 실험 설계 1개 이상', weight: 10 },
];

function buildChecklist() {
  const container = document.getElementById('checklist-container');
  criteria.forEach(c => {
    const div = document.createElement('div');
    div.style.cssText = 'display:flex;align-items:center;gap:8px;margin:6px 0;';
    div.innerHTML = `<input type="checkbox" id="${c.id}" onchange="calcScore()">
                     <label for="${c.id}">${c.text} <span style="color:var(--muted);font-size:.8rem">(${c.weight}점)</span></label>`;
    container.appendChild(div);
  });
}

function calcScore() {
  let score = 0;
  criteria.forEach(c => {
    if (document.getElementById(c.id)?.checked) score += c.weight;
  });
  const level = score < 30 ? '📖 입문 (논문 읽기 수준)' :
                score < 60 ? '📊 중급 (이해·분석 수준)' :
                score < 80 ? '🔬 고급 (비판적 독해 수준)' :
                             '🎓 Stanford 수준 (설계·확장 가능)';
  document.getElementById('score-res').innerHTML =
    `총점: <strong>${score}점</strong> / 100점<br/>${level}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       80점 이상: NeurIPS/ICML 논문 리뷰어 수준의 비판 독해 가능
     </span>`;
}

buildChecklist();
calcScore();
```

---

## 퀴즈

**질문**: Stanford 수준 논문 독해에서 Baseline 비교의 "빨간 깃발(red flag)"은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Baseline이 논문의 방법보다 느리다 | ❌ |
| (b) Baseline만 하이퍼파라미터 최적화를 하지 않아 불공정한 비교가 이루어졌다 | ✅ |
| (c) Baseline이 최신 논문에서 나왔다 | ❌ |
| (d) Baseline보다 성능이 0.1% 높다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 35)

```
[ ] 논문의 Contribution을 기술적/경험적/개념적 3계층으로 분해할 수 있다
[ ] 핵심 수식을 4단계 프로토콜(기호→가정→유도→반례)로 검증할 수 있다
[ ] 실험 비판 체크리스트 6개 포인트를 모두 확인할 수 있다
[ ] 통계적 유의성의 의미를 설명하고 논문에서 확인할 수 있다
[ ] 연구 질문 생성 6가지 유형에서 각 1개씩 질문을 만들 수 있다
[ ] 논리적 반박을 분포변화·데이터 크기·도메인 이동 관점에서 생성할 수 있다
[ ] 특정 논문을 골라 이 방법론으로 완전 분석 리포트를 작성할 수 있다
```

---

## 부록: 실전 논문 비판 리포트 템플릿

```markdown
## 논문 비판 분석 리포트

### 1. 기본 정보
- 제목, 저자, Venue, 연도
- 인용 수 (Google Scholar 기준)

### 2. Contribution 분해
- 기술적: [핵심 알고리즘/이론]
- 경험적: [주요 벤치마크 결과]
- 개념적: [새로운 인사이트]

### 3. 수식 검증 결과
- 검증한 수식: [수식 번호]
- 독립 유도 결과: ✅ 일치 / ❌ 불일치 (차이점: ...)
- 가정 위반 조건: ...

### 4. 실험 비판
- Baseline 공정성: [평가]
- 통계적 유의성: [확인 여부]
- 데이터셋 편향: [있음/없음, 근거]
- 빠진 실험: [제안]

### 5. 생성된 연구 질문
- Q1: [일반화 질문]
- Q2: [한계 탐색 질문]
- Q3: [확장 실험 제안]

### 6. 종합 평가
- 강점: ...
- 한계: ...
- 연구 가치: [높음/중간/낮음]
```
