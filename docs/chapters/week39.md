# Week 39 — 방법론 설계 + SCI 논문 글쓰기 (Ch.39)

> **파일명**: `chapters/ch39.html`  
> **Phase**: Phase 3-C · 연구 방법론  
> **인터랙티브**: ✅ IMRaD 섹션별 글쓰기 점수기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.39 · 연구 방법론 (Phase 3 — 13주차)` |
| 제목 | `방법론 Novelty 설계 + SCI 논문 글쓰기` |
| 서브타이틀 | `"완전히 새롭지 않아도 논문이 된다." Novelty의 3가지 경로, IMRaD 구조, Abstract부터 Conclusion까지 학술 설득 글쓰기 완전 가이드.` |
| 이전 챕터 | `ch38.html` — Ch.38 데이터 수집·품질·윤리 |
| 다음 챕터 | `ch40.html` — Ch.40 리뷰 대응·재투고 전략 |

---

## EASY BOX 내용

**핵심 비유**: "논문의 novelty는 '발명'이 아니라 '발견'에 가깝다. 기존 재료를 새로운 방식으로 조합해서 새로운 무언가를 만들어내는 것 — 요리사가 기존 재료로 새 요리를 만드는 것처럼."

**예시 리스트**:
- 새로운 알고리즘 발명: 논문 상위 10%만 해당 (대부분은 이게 아니어도 됨)
- 새로운 문제 정의: "이 문제를 이렇게 정의하면 기존 방법으로 해결 못 한다"를 보이면 됨
- 새로운 검증: "기존 방법들을 새로운 현실적 설정에서 비교"만으로도 EMNLP/AAAI 논문이 됨
- AI에서: 비전공자에게 가장 현실적인 경로는 (1) 실무 문제를 학술 언어로 정의 + (2) 기존 방법을 그 문제에 체계적으로 비교 + (3) 한계를 분석하고 간단한 개선 제안

---

## 섹션 구성

### 섹션 1: Novelty의 3가지 경로

**아이콘**: `💡`  
**제목**: `Novelty — "완전히 새롭지 않아도 된다"의 수학`

**설명**: SCI 논문에서 요구하는 novelty는 완전히 새로운 발명이 아닙니다. 기존 연구 대비 "새로운 무언가"가 있으면 됩니다. 이 새로움은 세 가지 경로로 충분합니다.

**수식 박스 1**:
- 레이블: `Novelty의 3가지 경로`
- 수식:
```
\text{Novelty} \in
\begin{cases}
\text{경로 1: 새로운 문제} & \text{"이 문제를 이렇게 정의한 적 없다"} \\
\text{경로 2: 새로운 방법} & \text{"이 방법으로 이 문제를 푼 적 없다"} \\
\text{경로 3: 새로운 검증} & \text{"이 설정에서 체계적으로 비교한 적 없다"}
\end{cases}
```
- 주석:
  - 경로 1: 실무 경험이 있는 비전공자의 강점 — 현실적 문제 정의
  - 경로 2: 기존 방법 A와 B를 창의적으로 조합 → A+B가 처음이면 novel
  - 경로 3: "Survey + Benchmark" 논문 — 20개 방법을 동일 조건에서 비교
  - 하나만 있어도 논문이 됨, 둘 이상이면 상위 venue 가능

**표 (비전공자에게 현실적인 Novelty 예시)**:

| 경로 | 예시 | 적합한 분야 |
|------|------|-----------|
| 새 문제 | "HR 문서 기반 Q&A에서 개인정보 위험 측정 방법" | 도메인 전문 지식 있는 경우 |
| 새 방법 조합 | "RAG + 사실성 검증 레이어 조합" | AI 구현 경험 있는 경우 |
| 새 검증 | "LLM 5종을 의료 문서 요약에서 비교" | 도메인 접근권 있는 경우 |

---

### 섹션 2: IMRaD 구조 — 논문의 뼈대

**아이콘**: `🏗`  
**제목**: `IMRaD — SCI 논문의 표준 구조`

**설명**: 대부분의 SCI 논문은 IMRaD 구조를 따릅니다. Introduction-Method-Results-and-Discussion. 각 섹션의 역할을 이해하면 어디에 무엇을 쓸지 명확해집니다.

**표 (IMRaD 각 섹션의 역할과 핵심 질문)**:

| 섹션 | 핵심 역할 | 독자가 기대하는 답 | 흔한 실수 |
|------|---------|----------------|---------|
| Abstract | 전체 논문의 125단어 요약 | "이 논문을 읽어야 하는가?" | 결과 없이 방법만 설명 |
| Introduction | 문제→한계→제안→기여 4단계 | "왜 이게 중요하고 무엇을 했나?" | 배경 지식 나열로 끝남 |
| Related Work | 내 연구의 위치 설정 | "이 연구는 어디서 나온 것인가?" | 논문 나열 (위치 설정 없음) |
| Method | 재현 가능한 설명 | "이 방법을 내가 따라 할 수 있는가?" | 수식 없이 개념만 설명 |
| Experiments | 데이터·환경·지표 | "이 실험이 공정한가?" | Baseline 하이퍼파라미터 미공개 |
| Results | 수치 + 통계 | "결과가 의미 있는가?" | p-value, 분산 없는 단일 수치 |
| Discussion | 해석·한계·함의 | "결과가 뭘 의미하는가?" | Results 반복 (해석 없음) |
| Conclusion | 기여·한계·미래 연구 | "한 줄로 요약하면?" | Abstract 복사 붙여넣기 |

---

### 섹션 3: Abstract와 Introduction — 가장 중요한 두 섹션

**아이콘**: `✍`  
**제목**: `Abstract + Introduction — 90%의 독자가 더 이상 읽지 않는 곳`

**설명**: 리뷰어는 Abstract와 Introduction에서 논문 accept/reject 방향을 거의 결정합니다. 이 두 섹션이 설득력이 있어야 나머지도 읽습니다.

**수식 박스 2**:
- 레이블: `Abstract의 5문장 구조 (Hirsch 공식)`
- 수식:
```
\text{Abstract 5문장 구조:}
\begin{cases}
\text{문장 1:} & \text{문제의 중요성 (왜 중요한가)} \\
\text{문장 2:} & \text{기존 접근의 한계 (무엇이 부족한가)} \\
\text{문장 3:} & \text{우리의 제안 (무엇을 했는가)} \\
\text{문장 4:} & \text{핵심 결과 (숫자로)} \\
\text{문장 5:} & \text{함의 (왜 중요한가, 더 넓은 맥락에서)}
\end{cases}
```
- 주석:
  - 5문장이 125단어 안에 들어오면 좋은 Abstract
  - 문장 4에 반드시 숫자 포함: "improved by 6.2%", "reduced latency by 1.8s"
  - 문장 1이 "~는 중요하다"로 시작하면 약함 → 구체적 문제로 시작

**수식 박스 3** (Introduction의 4단락 구조):
- 레이블: `Introduction의 4단락 구조 (Booth 방법)`
- 수식:
```
\text{Introduction 4단락:}
\begin{cases}
\text{단락 1:} & \text{문제 배경 + 중요성 (도입)} \\
\text{단락 2:} & \text{기존 연구 요약 + 한계 (갭 제시)} \\
\text{단락 3:} & \text{우리의 접근 + 핵심 아이디어 (제안)} \\
\text{단락 4:} & \text{Contribution 3가지 + 논문 구조 (로드맵)}
\end{cases}
```
- 주석:
  - 단락 2 끝에 "However, ..." 또는 "Despite this, ..." 패턴이 핵심
  - Contribution은 "We propose...", "We demonstrate...", "We provide..." 로 시작
  - 논문 구조: "The rest of this paper is organized as follows..."

---

### 섹션 4: 학술 영어 문장 원칙

**아이콘**: `EN`  
**제목**: `학술 영어 — 화려함이 아니라 명확함`

**설명**: SCI 논문은 영어 원어민이 유리한 게임이 아닙니다. 명확한 구조와 정확한 표현이면 충분합니다. 오히려 과장되고 복잡한 문장이 신뢰를 낮춥니다.

**표 (학술 영어 나쁜 표현 vs 좋은 표현)**:

| 나쁜 표현 | 좋은 표현 | 이유 |
|---------|---------|------|
| "significantly improved" | "improved by 6.2% (p<0.01)" | 모호 → 정량적 |
| "Our method is better" | "Our method outperforms X by 3.1 F1 points" | 비교 기준 없음 → 명시 |
| "We think that..." | "Our analysis suggests that..." | 추측 → 근거 기반 |
| "Obviously, ..." | 제거 | 독자를 얕봄 |
| "utilize" | "use" | 불필요한 형식 |
| 수동태 남용 | 능동태 우선 | "was found to be" → "we found" |
| "In conclusion, to summarize" | "In conclusion" 또는 "To summarize" (하나만) | 중복 |

**callout (tip)**:
- 내용: **Claude/GPT로 글쓰기 지원 활용법**: AI를 사용할 때 "이 문장을 더 학술적으로 바꿔줘"가 아니라 "이 문장의 주장을 더 정확하게 표현해줘. 현재 문제는 비교 기준이 없다는 것이다"처럼 구체적 결함을 지목해야 합니다. AI는 의미를 만들지 않습니다 — 의미는 연구자가 만들어야 합니다.

---

## 인터랙티브: IMRaD 글쓰기 점수 진단기

**제목**: `🔢 논문 초고 완성도 체크리스트 — IMRaD 섹션별 진단`

**JavaScript 로직**:
```javascript
const imradItems = [
  { id: 'i1', text: 'Abstract: 5문장 구조 (배경·한계·제안·결과·함의) 포함', weight: 10 },
  { id: 'i2', text: 'Abstract: 핵심 결과가 숫자로 제시됨', weight: 8 },
  { id: 'i3', text: 'Introduction: 4단락 구조 (배경→갭→제안→기여)', weight: 10 },
  { id: 'i4', text: 'Introduction: Contribution 3개가 bullet point로 명시됨', weight: 8 },
  { id: 'i5', text: 'Related Work: 논문 나열이 아닌 위치 설정 형식', weight: 8 },
  { id: 'i6', text: 'Method: 재현 가능한 수준으로 작성됨 (알고리즘/수식 포함)', weight: 12 },
  { id: 'i7', text: 'Experiments: Baseline, 데이터, 지표가 모두 명시됨', weight: 10 },
  { id: 'i8', text: 'Results: 평균 + 표준편차 또는 신뢰구간 포함', weight: 10 },
  { id: 'i9', text: 'Discussion: 결과 해석 + 한계 + 예상과 다른 결과 설명', weight: 12 },
  { id: 'i10', text: 'Conclusion: Contribution 재확인 + 미래 연구 제안', weight: 7 },
  { id: 'i11', text: '모든 문장에서 "significantly", "obviously" 등 모호 표현 제거', weight: 5 },
];

function buildIMRaD() {
  const container = document.getElementById('imrad-container');
  imradItems.forEach(c => {
    const div = document.createElement('div');
    div.style.cssText = 'display:flex;align-items:center;gap:8px;margin:6px 0;';
    div.innerHTML = `<input type="checkbox" id="${c.id}" onchange="calcIMRaD()">
                     <label for="${c.id}">${c.text} <span style="color:var(--muted);font-size:.8rem">(${c.weight}점)</span></label>`;
    container.appendChild(div);
  });
}

function calcIMRaD() {
  let score = 0;
  imradItems.forEach(c => { if (document.getElementById(c.id)?.checked) score += c.weight; });
  const level = score < 40 ? '✏️ 초안 수준 (대폭 수정 필요)' :
                score < 65 ? '📝 1차 완성 (major revision 수준)' :
                score < 85 ? '📄 제출 가능 (workshop/venue 수준)' :
                             '🏆 SCI 제출 준비 완료';
  document.getElementById('imrad-result').innerHTML =
    `논문 초고 완성도: <strong>${score}점</strong> / 100점<br/>${level}`;
}

buildIMRaD();
calcIMRaD();
```

---

## 퀴즈

**질문**: SCI 논문 Introduction의 4단락 구조에서 가장 중요한 전환점은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) "In this paper, we propose..." | ❌ |
| (b) "However, existing approaches have limitations in..." — 기존 한계를 제시하며 갭을 만드는 전환 | ✅ |
| (c) "This paper is organized as follows..." | ❌ |
| (d) "In recent years, X has gained attention..." | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 39)

```
[ ] 내 연구 아이디어를 Novelty 3가지 경로 중 하나로 분류할 수 있다
[ ] IMRaD 각 섹션의 역할과 흔한 실수를 설명할 수 있다
[ ] Abstract를 5문장 구조로 작성할 수 있다
[ ] Introduction을 4단락 구조로 작성할 수 있다
[ ] Contribution을 "We propose/demonstrate/provide..." 형식으로 3개 쓸 수 있다
[ ] 학술 영어에서 나쁜 표현 vs 좋은 표현 10가지를 구분할 수 있다
[ ] 내 연구 주제의 논문 초고 Abstract를 실제로 작성할 수 있다
```
