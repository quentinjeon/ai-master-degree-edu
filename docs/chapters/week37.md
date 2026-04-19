# Week 37 — 선행연구 구조화·문헌 지도 (Ch.37)

> **파일명**: `chapters/ch37.html`  
> **Phase**: Phase 3-C · 연구 방법론  
> **인터랙티브**: ✅ 선행연구 구조화 표 생성기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.37 · 연구 방법론 (Phase 3 — 11주차)` |
| 제목 | `선행연구 구조화와 문헌 지도 그리기` |
| 서브타이틀 | `"공부"가 아닌 "빈칸 찾기"로 논문을 읽는다. 20편에서 나의 연구 위치를 정하고, Related Work를 학술 구조로 쓰는 완전 가이드.` |
| 이전 챕터 | `ch36.html` — Ch.36 연구 설계·실험 방법론 |
| 다음 챕터 | `ch38.html` — Ch.38 데이터 수집·품질·윤리 |

---

## EASY BOX 내용

**핵심 비유**: "새 카페를 열기 전에 동네 카페를 다 가봐야 한다 — 어디가 붐비고, 어디가 한가하고, 뭐가 없는지. 선행연구 구조화는 '기존 카페 지도'를 그려서 내 카페가 어디 있어야 하는지 찾는 것이다."

**예시 리스트**:
- 논문을 문장 단위로 읽으면 한 편에 2시간이 걸리고 기억에 안 남는다
- 6가지 질문 틀로 읽으면 15분 안에 핵심을 뽑을 수 있고, 20편을 비교할 수 있다
- AI에서: Related Work 섹션은 "내 연구가 기존 연구의 어느 빈칸을 채우는가"를 보여주는 학술 지도 — 이걸 잘 쓰는 사람이 논문 accept 확률이 높다

---

## 섹션 구성

### 섹션 1: 논문을 빠르게 읽는 6가지 질문 틀

**아이콘**: `📋`  
**제목**: `6Q 독해 프레임 — 15분 안에 논문의 핵심 추출`

**설명**: 논문을 처음부터 끝까지 문장으로 읽으면 비효율적입니다. 6가지 핵심 질문에 답하는 방식으로 읽으면 어떤 논문도 15분 안에 구조화할 수 있습니다.

**수식 박스 1**:
- 레이블: `6Q 독해 프레임 (순서 고정)`
- 수식:
```
\text{6Q 독해 순서:}
\begin{cases}
Q1. & \text{이 논문이 해결하려는 문제는 무엇인가?} \\
Q2. & \text{왜 그 문제가 중요한가? (motivation)} \\
Q3. & \text{기존 방법의 한계는 무엇인가?} \\
Q4. & \text{저자의 핵심 제안(contribution)은 무엇인가?} \\
Q5. & \text{어떤 데이터·실험으로 검증했는가?} \\
Q6. & \text{결과와 한계는 무엇인가?}
\end{cases}
```
- 주석:
  - Q1~Q3: Abstract + Introduction에서 대부분 답 가능 (5분)
  - Q4: Method 첫 문단 + 그림 설명 (5분)
  - Q5: Experiments 섹션 헤더 + 표 (3분)
  - Q6: Conclusion + Limitation (2분)
  - 이 15분 리딩 후 확장 독해 여부 결정

**표 (6Q 적용 예시 — GPT-3 논문)**:

| Q | 내용 |
|---|------|
| Q1 | 수백억 개 파라미터의 언어모델이 few-shot으로 다양한 태스크를 수행할 수 있는가? |
| Q2 | 태스크별 파인튜닝 비용이 크고, 새 태스크마다 레이블 데이터 필요 |
| Q3 | 기존 모델은 각 태스크에 파인튜닝 필요 → 일반화 능력 부족 |
| Q4 | 175B 파라미터 모델의 In-context learning → few-shot만으로 SOTA 수준 |
| Q5 | 28개 NLP 벤치마크, few-shot vs zero-shot vs fine-tuned 비교 |
| Q6 | 강력하지만 bias 문제, 해석 어려움, compute 비용이 큼 |

---

### 섹션 2: 선행연구 구조화 표 — 20편의 지도 만들기

**아이콘**: `🗺`  
**제목**: `문헌 지도 — 내 연구의 위치를 정하는 표`

**설명**: 20편 정도의 논문을 읽으면 패턴이 보입니다. 어떤 평가지표가 표준이고, 어떤 한계가 반복되고, 아직 아무도 답하지 않은 질문이 보입니다. 이 패턴이 연구 기회입니다.

**표 (선행연구 구조화 표 템플릿)**:

| 논문 | 문제 정의 | 방법론 | 데이터셋 | 평가지표 | 핵심 결과 | 한계 | 내 연구와 연결 |
|------|---------|--------|---------|---------|---------|------|--------------|
| 논문A | ... | ... | ... | ... | ... | ... | ... |
| 논문B | ... | ... | ... | ... | ... | ... | ... |

**callout (tip)**:
- 내용: **패턴 발견 체크리스트**: 표를 채운 후 열별로 훑으면서 확인할 것. (1) "한계" 열에 같은 단어가 반복 → 그것이 연구 기회. (2) "평가지표" 열에 같은 것이 많음 → 그것이 커뮤니티 표준. (3) "데이터셋" 열에 특정 데이터셋이 압도적 → 그것으로 실험해야 비교 가능. (4) "방법론" 열에 아무도 안 한 조합 → 그것이 novelty 후보.

---

### 섹션 3: Related Work 쓰기 — 나열이 아닌 위치 설정

**아이콘**: `📍`  
**제목**: `Related Work — "내 연구가 여기 있다"를 보여주는 섹션`

**설명**: 비전공자가 Related Work를 쓸 때 가장 많이 하는 실수는 논문을 나열하는 것입니다. Related Work의 목적은 기존 연구를 소개하는 게 아니라, **내 연구가 기존 연구의 어느 빈칸에 있는지**를 독자에게 설득하는 것입니다.

**수식 박스 2**:
- 레이블: `Related Work의 3가지 구조 패턴`
- 수식:
```
\text{패턴 1 (흐름형):}
\quad \text{초기 연구} \to \text{중간 발전} \to \text{최근 연구} \to \text{남은 한계}
```
```
\text{패턴 2 (범주형):}
\quad \text{그룹 A (우리와 관련 없음)} + \text{그룹 B (일부 관련)} + \text{그룹 C (직접 관련)} \to \text{차별점}
```
```
\text{패턴 3 (비교형):}
\quad \text{방법 X: 한계 A} + \text{방법 Y: 한계 B} + \text{우리: A와 B 모두 해결}
```
- 주석:
  - 초보자: 패턴 1이 가장 쓰기 쉬움
  - 중급: 패턴 2로 분류 구조 보여주기
  - 고급: 패턴 3으로 직접 비교 → 자신감 있는 위치 설정
  - 논문마다 하나를 선택하거나 조합

**표 (Related Work 좋은 문장 vs 나쁜 문장)**:

| 나쁜 표현 | 좋은 표현 |
|---------|---------|
| "X et al. proposed a method for..." | "Unlike X et al., who assume static distributions, we address dynamic..." |
| "Y et al. studied the problem of..." | "While Y et al. achieve strong results on English, performance on low-resource languages remains limited..." |
| "Recent works have explored..." | "Prior works on Z focus on accuracy, but overlook latency constraints in deployment settings." |

---

### 섹션 4: 연구 위치 다이어그램 — 두 축으로 내 연구 표시하기

**아이콘**: `⊕`  
**제목**: `2×2 문헌 지도 — 기존 연구 vs 내 연구 시각화`

**설명**: 문헌을 두 가지 핵심 차원(예: 정확도 vs 비용, 일반성 vs 특수성)으로 배치하면 내 연구의 위치가 시각적으로 명확해집니다. 많은 논문이 Introduction에서 이 방식으로 Related Work를 요약합니다.

**수식 박스 3**:
- 레이블: `2×2 문헌 지도 설계`
- 수식:
```
\text{두 축 선택 기준:}
\begin{cases}
\text{축 1}: & \text{논문들이 가장 많이 trade-off하는 차원} \\
\text{축 2}: & \text{내 연구가 기존과 다른 핵심 차원}
\end{cases}
\Rightarrow \text{내 연구 = 아무도 없는 사분면에 위치}
```
- 주석:
  - 예: 축1 = 속도(fast↔slow), 축2 = 일반성(general↔domain-specific)
  - 기존 연구 A: 빠르지만 도메인 특수 / 기존 연구 B: 느리지만 일반
  - 내 연구: 빠르고 일반적인 사분면 → novelty 시각화

---

## 인터랙티브: 선행연구 구조화 표 생성기

**제목**: `🔢 6Q 독해 점수 계산기 — 논문 한 편 분석 완성도`

**JavaScript 로직**:
```javascript
const q6items = [
  { id: 'q1', text: 'Q1: 해결하는 문제를 한 문장으로 쓸 수 있다', weight: 15 },
  { id: 'q2', text: 'Q2: 문제의 중요성(motivation)을 설명할 수 있다', weight: 10 },
  { id: 'q3', text: 'Q3: 기존 방법의 구체적 한계를 1가지 이상 쓸 수 있다', weight: 15 },
  { id: 'q4', text: 'Q4: 저자의 핵심 기여를 1~3개로 압축할 수 있다', weight: 20 },
  { id: 'q5', text: 'Q5: 실험 데이터셋과 평가지표를 쓸 수 있다', weight: 15 },
  { id: 'q6', text: 'Q6: 논문의 한계를 1가지 이상 쓸 수 있다', weight: 15 },
  { id: 'q7', text: '내 연구와의 연결점 또는 차별점을 1가지 쓸 수 있다', weight: 10 },
];

function buildQ6() {
  const container = document.getElementById('q6-container');
  q6items.forEach(c => {
    const div = document.createElement('div');
    div.style.cssText = 'display:flex;align-items:center;gap:8px;margin:6px 0;';
    div.innerHTML = `<input type="checkbox" id="${c.id}" onchange="calcQ6()">
                     <label for="${c.id}">${c.text} <span style="color:var(--muted);font-size:.8rem">(${c.weight}점)</span></label>`;
    container.appendChild(div);
  });
}

function calcQ6() {
  let score = 0;
  q6items.forEach(c => { if (document.getElementById(c.id)?.checked) score += c.weight; });
  const level = score < 40 ? '📖 1회독 수준 (이해만 됨)' :
                score < 70 ? '📝 정리 수준 (구조화 가능)' :
                score < 90 ? '🔬 분석 수준 (비판 가능)' :
                             '✅ 완전 독해 (내 연구와 연결 완료)';
  document.getElementById('q6-result').innerHTML =
    `독해 완성도: <strong>${score}점</strong> / 100점<br/>${level}`;
}

buildQ6();
calcQ6();
```

---

## 퀴즈

**질문**: Related Work 섹션의 진짜 목적은 무엇인가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 기존 논문을 최대한 많이 인용하는 것 | ❌ |
| (b) 내 연구가 기존 연구의 어느 빈칸을 채우는지 독자에게 설득하는 것 | ✅ |
| (c) 기존 방법의 단점을 상세히 비판하는 것 | ❌ |
| (d) 논문의 길이를 채우는 것 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 37)

```
[ ] 6Q 프레임으로 논문 1편을 15분 안에 구조화할 수 있다
[ ] 20편의 선행연구를 구조화 표로 정리할 수 있다
[ ] "한계" 열에서 반복 패턴을 발견하고 연구 기회를 찾을 수 있다
[ ] Related Work의 3가지 구조 패턴(흐름/범주/비교) 중 하나를 적용할 수 있다
[ ] 기존 연구를 나열하지 않고 "위치 설정"하는 문장을 쓸 수 있다
[ ] 내 연구를 2×2 문헌 지도에서 표시할 수 있다
[ ] 관심 주제의 논문 20편을 선정하고 구조화 표 초안을 완성할 수 있다
```
