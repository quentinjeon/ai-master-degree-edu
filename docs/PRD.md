# PRD: 논문을 위한 확률·수학·AI 인터랙티브 교재

> **버전**: 1.0.0  
> **작성일**: 2026-04-19  
> **프로젝트 경로**: `/Users/junyongsub/percent_ml/`

---

## 1. 프로젝트 개요

### 1.1 목적

AI 논문을 읽고 쓰기 위해 필요한 **기초 수학·확률·통계·정보이론** 개념을 단계별로 익히는 인터랙티브 HTML 교재를 만든다.

### 1.2 핵심 철학

> "초등학생도 이해할 수 있는 비유 + 논문 수준의 수식"을 **같은 페이지에서** 제공한다.

모든 개념은 다음 3가지 층위로 설명되어야 한다:

| 층위 | 목표 독자 | 설명 방식 |
|------|-----------|-----------|
| **Layer 1** | 초등학생 | 일상 비유, 예시 리스트, 그림 언어 |
| **Layer 2** | 학부생 | 직관적 해설, 개념 카드, 표 |
| **Layer 3** | 대학원생/연구자 | 수학 공식 (MathJax), 논문 기호, AI 연결 |

### 1.3 대상 사용자

- 논문을 처음 쓰려는 대학원 준비생
- AI/ML을 공부하지만 수학 배경이 약한 개발자
- 확률·통계를 실무 AI와 연결하고 싶은 모든 학습자

---

## 2. 전체 구조

### 2.1 파일 구조

```
percent_ml/
├── index.html                    ← 메인 진입점 (사이드바 + 모든 섹션 통합)
├── chapters/                     ← 챕터별 독립 HTML (선택적 분리)
│   ├── ch01.html
│   ├── ch02.html
│   └── ... (ch16.html까지)
└── docs/                         ← 개발 문서 (에이전트용)
    ├── PRD.md                    ← 이 파일
    ├── DESIGN_SYSTEM.md          ← CSS/UI 컴포넌트 규칙
    ├── CHAPTER_TEMPLATE.md       ← 챕터 HTML 빈 템플릿
    └── chapters/                 ← 챕터별 콘텐츠 명세
        ├── week01.md
        ├── week02.md
        └── ... (week16.md까지)
```

### 2.2 섹션 목록 (사이드바 기준)

| 섹션 ID | 섹션명 | 대응 주차 |
|---------|--------|-----------|
| `home` | 홈 & 전체 로드맵 | — |
| `dict` | 개념 사전 (초등학생 버전) | — |
| `plan` | 16주 공부 계획 | — |
| `c1` | Ch.1 집합과 함수 | 1~2주 |
| `c2` | Ch.2 미분과 최적화 | 1~2주 |
| `c3` | Ch.3 선형대수 기초 | 3주 |
| `c4` | Ch.4 확률론 기초 | 3~4주 |
| `c5` | Ch.5 확률분포 | 5~7주 |
| `c6` | Ch.6 통계적 추론 | 8~10주 |
| `c7` | Ch.7 AI·ML 연결고리 | 11~16주 |

> 향후 확장: 각 주차를 독립 챕터로 분리하여 총 16개 챕터 HTML 생성

---

## 3. 챕터 개발 규칙 (필수)

### 3.1 챕터 필수 블록 (순서 고정)

모든 챕터는 아래 7개 블록을 **반드시 이 순서대로** 포함해야 한다.

```
[1] HERO          — 뱃지 + 제목 + 서브텍스트
[2] EASY BOX      — 🧒 초등학생 버전 설명
[3] MAIN CONTENT  — 핵심 개념 설명 (섹션 제목 + 단락)
[4] FORMULA BOX   — 수식 (MathJax) + 직관적 주석
[5] INTERACTIVE   — 계산기/시뮬레이터 (수치 개념일 때 필수)
[6] QUIZ          — 4지선다 확인 문제
[7] NAV BUTTONS   — 이전/다음 챕터 이동
```

### 3.2 각 블록 세부 요구사항

#### [1] HERO 블록

```html
<div class="hero">
  <div class="badge">Ch.N · 카테고리명 (N주차)</div>
  <h1>챕터 제목</h1>
  <p>한 문장 설명 — 왜 이 챕터를 배우는지</p>
</div>
```

- 뱃지: 챕터 번호, 카테고리, 해당 주차를 표시
- 제목: 2줄 이하, 핵심 키워드 포함
- 설명: 학습자가 "이걸 배우면 뭐가 좋아지는지"를 한 줄로

#### [2] EASY BOX 블록

```html
<div class="easy">
  <div class="et">🧒 초등학생 한 줄 설명</div>
  <p><strong>핵심 비유를 굵게</strong> 표시한 1~2문장</p>
  <ul>
    <li>구체적인 예시 1</li>
    <li>구체적인 예시 2</li>
    <li>AI와 연결되는 포인트</li>
  </ul>
</div>
```

- 반드시 일상 비유를 사용할 것 (축구, 날씨, 요리, 신호등 등)
- 추상적 표현 금지. "이것은 ~이다" 대신 "~처럼 생각해봐"
- 마지막 예시는 반드시 AI/머신러닝과 연결

#### [3] MAIN CONTENT 블록

```html
<h2 class="st"><span class="ic">아이콘</span>섹션 제목</h2>
<p>설명 단락. Layer 2 수준 (직관 위주, 수식 최소화)</p>

<h3 class="sub">소제목</h3>
<p>세부 설명</p>
<table>...</table>
```

- `h2.st`: 주요 섹션 제목. 아이콘(`.ic`) 필수
- `h3.sub`: 소제목 (보라색 accent2)
- 표(`table`): 개념 비교, 기호 정리에 사용
- 단락은 3~5문장 이하로 간결하게

#### [4] FORMULA BOX 블록

```html
<div class="fb">
  <div class="fl">수식 레이블 (대문자 영문)</div>
  \[ 수식 \]
  <div class="fn">직관적 주석 — 각 기호의 의미를 한국어로</div>
</div>
```

- 수식 레이블(`.fl`): 영문 대문자, 수식의 이름/출처
- MathJax 인라인: `\( \)`, 블록: `\[ \]`
- 주석(`.fn`): 반드시 포함. 수식의 각 기호를 한국어로 설명
- 복잡한 수식은 분해해서 여러 `.fb` 블록으로 나눌 것

#### [5] INTERACTIVE 블록 (해당 시 필수)

```html
<div class="calc">
  <h4>🔢 제목</h4>
  <p><!-- 짧은 설명 --></p>
  <div class="cr">
    <span class="cl">파라미터 이름</span>
    <input type="range" id="고유ID" min="..." max="..." step="..." value="..." oninput="함수명()"/>
    <input type="number" id="고유ID-v" readonly/>
  </div>
  <!-- 추가 파라미터 반복 -->
  <div class="cres" id="결과ID">초기 텍스트</div>
</div>
```

- 슬라이더는 반드시 숫자 출력 필드와 쌍으로
- 결과는 실시간으로 업데이트 (oninput 이벤트)
- JavaScript 함수는 `<script>` 태그 하단에 위치

**인터랙티브 요소가 필요한 챕터:**

| 챕터 | 인터랙티브 요소 |
|------|----------------|
| Ch.2 미분 | 경사하강법 시뮬레이터 |
| Ch.5 분포 | 정규분포 계산기 |
| Ch.6 통계 | 신뢰구간 계산기 |
| Ch.4 확률 | 베이즈 업데이터 |
| Ch.7 ML | 손실함수 시각화 |

#### [6] QUIZ 블록

```html
<div class="qb">
  <h4>🧠 확인 문제</h4>
  <p style="margin-bottom:12px;">질문 문장</p>
  <div class="qo" onclick="ans(this, false)">(a) &nbsp; 오답 선택지</div>
  <div class="qo" onclick="ans(this, true)">(b) &nbsp; 정답 선택지</div>
  <div class="qo" onclick="ans(this, false)">(c) &nbsp; 오답 선택지</div>
  <div class="qo" onclick="ans(this, false)">(d) &nbsp; 오답 선택지</div>
  <div class="qfb"></div>
</div>
```

- 반드시 4지선다
- 정답은 `ans(this, true)`, 오답은 `ans(this, false)`
- 정답 위치는 매 챕터마다 다르게 (항상 (b)가 정답이면 안 됨)
- 문제는 본문에서 배운 핵심 개념을 직접 테스트

#### [7] NAV BUTTONS 블록

```html
<div class="nb">
  <button class="btn btn-s" onclick="goTo('이전챕터ID')">← 이전 챕터명</button>
  <button class="btn btn-p" onclick="goTo('다음챕터ID')">다음 챕터명 →</button>
</div>
```

- 첫 챕터: 이전 버튼 `disabled`
- 마지막 챕터: 다음 버튼이 홈으로 이동

---

## 4. 디자인 시스템 요약

### 4.1 색상 토큰

```css
--bg: #0f1117          /* 전체 배경 */
--surface: #1a1d27     /* 사이드바 배경 */
--surface2: #22263a    /* 카드 배경 */
--surface3: #2a2f47    /* 중첩 카드 배경 */
--accent: #6c8aff      /* 주요 강조색 (파란 보라) */
--accent2: #a78bfa     /* 소제목색 (연보라) */
--accent3: #34d399     /* 성공/팁 (초록) */
--accent4: #f59e0b     /* 경고/퀴즈 (노랑) */
--accent5: #f87171     /* 오류/위험 (빨강) */
--text: #e2e8f0        /* 본문 텍스트 */
--muted: #94a3b8       /* 보조 텍스트 */
--border: #2d3148      /* 테두리 */
--code: #141824        /* 코드/수식 배경 */
```

### 4.2 컴포넌트 클래스 목록

| 클래스 | 용도 |
|--------|------|
| `.hero` | 챕터 상단 히어로 |
| `.easy` | 초등학생 설명 박스 |
| `.fb` | 수식 박스 (파란 왼쪽 보더) |
| `.fl` | 수식 박스 레이블 |
| `.fn` | 수식 박스 주석 |
| `.cc` | 핵심 개념 카드 |
| `.ct` | 개념 카드 제목 |
| `.callout.info` | 정보 알림 (파랑) |
| `.callout.tip` | 팁 알림 (초록) |
| `.callout.warn` | 주의 알림 (노랑) |
| `.qb` | 퀴즈 박스 |
| `.qo` | 퀴즈 선택지 |
| `.qfb` | 퀴즈 피드백 |
| `.calc` | 인터랙티브 계산기 |
| `.cr` | 계산기 입력 행 |
| `.cl` | 계산기 라벨 |
| `.cres` | 계산기 결과 |
| `.two-col` | 2컬럼 그리드 |
| `.col-box` | 2컬럼 내부 박스 |
| `.nb` | 이전/다음 네비게이션 |
| `.btn-p` | 주요 버튼 (파란 배경) |
| `.btn-s` | 보조 버튼 (회색 배경) |
| `h2.st` | 섹션 제목 |
| `h3.sub` | 소제목 |
| `.tag.tb/.tg/.tp/.ty` | 태그 (파/초/보/노) |
| `.steps + .step + .sn` | 단계별 리스트 |

---

## 5. 16주 챕터 명세

### 5.1 챕터별 개발 우선순위

| 우선순위 | 챕터 | 파일 | 인터랙티브 |
|----------|------|------|-----------|
| P0 | Ch.1 집합과 함수 | `ch01.html` | ✗ |
| P0 | Ch.2 미분과 최적화 | `ch02.html` | ✅ GD 시뮬레이터 |
| P0 | Ch.3 선형대수 | `ch03.html` | ✗ |
| P0 | Ch.4 확률론·조건부확률·베이즈 | `ch04.html` | ✅ 베이즈 계산기 |
| P0 | Ch.5 확률변수·분포 | `ch05.html` | ✅ 분포 계산기 |
| P1 | Ch.6 기대값·분산 | `ch06.html` | ✅ 기대값 계산기 |
| P1 | Ch.7 표본·통계량·MLE | `ch07.html` | ✅ MLE 시뮬레이터 |
| P1 | Ch.8 MAP·Bias-Variance | `ch08.html` | ✅ B-V 시각화 |
| P1 | Ch.9 ML 연결 1 (Logistic) | `ch09.html` | ✅ sigmoid 계산기 |
| P1 | Ch.10 ML 연결 2 (Softmax) | `ch10.html` | ✅ softmax 계산기 |
| P2 | Ch.11 정보이론 | `ch11.html` | ✅ entropy 계산기 |
| P2 | Ch.12 딥러닝·LLM | `ch12.html` | ✅ temperature 슬라이더 |
| P2 | Ch.13 Calibration | `ch13.html` | ✅ ECE 시각화 |
| P2 | Ch.14 신뢰구간·가설검정 | `ch14.html` | ✅ CI 계산기 |
| P2 | Ch.15 불확실성·의사결정 | `ch15.html` | ✗ |
| P2 | Ch.16 논문 읽기·쓰기 | `ch16.html` | ✗ |

---

## 6. JavaScript 공통 함수

모든 챕터 HTML에 반드시 포함해야 하는 공통 함수:

```javascript
// 퀴즈 정답 처리
function ans(el, correct) {
  const siblings = el.parentElement.querySelectorAll('.qo');
  siblings.forEach(s => { s.classList.remove('correct','wrong'); s.onclick = null; });
  el.classList.add(correct ? 'correct' : 'wrong');
  const fb = el.parentElement.querySelector('.qfb');
  fb.classList.add('show');
  fb.style.color = correct ? 'var(--accent3)' : 'var(--accent5)';
  fb.textContent = correct
    ? '✅ 정답! 잘 이해하고 있습니다.'
    : '❌ 틀렸습니다. 본문을 다시 확인해보세요.';
}

// 체크박스 로컬스토리지 저장
function saveCheck(el) {
  localStorage.setItem('chk_' + el.id, el.checked ? '1' : '0');
}
function loadChecks() {
  document.querySelectorAll('input[type=checkbox]').forEach(el => {
    const v = localStorage.getItem('chk_' + el.id);
    if (v !== null) el.checked = v === '1';
  });
}

// MathJax 재렌더링
function rerenderMath() {
  if (window.MathJax) MathJax.typesetPromise();
}
```

---

## 7. 품질 기준 (에이전트 체크리스트)

챕터 개발 완료 전 반드시 확인:

- [ ] 7개 필수 블록이 순서대로 모두 있는가
- [ ] EASY BOX에 일상 비유가 있는가
- [ ] 모든 수식에 `.fn` 주석이 있는가
- [ ] 퀴즈 정답이 본문 내용과 일치하는가
- [ ] 인터랙티브 요소가 실시간으로 업데이트되는가
- [ ] 이전/다음 챕터 링크가 정확한가
- [ ] 모든 CSS 클래스가 DESIGN_SYSTEM.md에 정의된 것만 사용하는가
- [ ] MathJax 수식이 `\( \)` 또는 `\[ \]`로 감싸져 있는가
- [ ] 초등학생 설명과 수식 설명이 같은 개념을 다루는가

---

## 8. 에이전트 개발 지침

### 8.1 챕터 개발 순서

1. `docs/chapters/weekNN.md` 명세 파일을 읽는다
2. `docs/CHAPTER_TEMPLATE.md`의 빈 템플릿을 복사한다
3. 명세 파일의 내용으로 템플릿을 채운다
4. `docs/DESIGN_SYSTEM.md`의 컴포넌트 스니펫을 활용한다
5. 품질 기준 체크리스트를 모두 통과하면 완료

### 8.2 절대 금지 사항

- ❌ CSS 변수 값을 직접 색상 코드로 하드코딩 (`color: #6c8aff` 대신 `color: var(--accent)`)
- ❌ `style` 속성으로 레이아웃 구조 변경 (컴포넌트 클래스만 사용)
- ❌ `<script>` 밖에서 JavaScript 실행
- ❌ MathJax 없이 수식을 일반 텍스트로 작성
- ❌ EASY BOX를 생략하거나 수식만으로 대체
- ❌ 퀴즈를 주관식으로 변경

### 8.3 권장 사항

- ✅ 개념 하나당 `.fb` 블록 하나 원칙 (너무 많은 수식을 한 박스에 넣지 않기)
- ✅ 표(`table`)는 5줄 이하로 (그 이상은 섹션을 나눌 것)
- ✅ `callout`은 챕터당 2~3개 이하 (경고 남발 금지)
- ✅ 인터랙티브 계산기 결과는 `innerHTML`로 HTML 포함 출력 가능
