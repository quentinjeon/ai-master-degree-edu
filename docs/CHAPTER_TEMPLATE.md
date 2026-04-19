# CHAPTER TEMPLATE: 챕터 HTML 빈 템플릿

> **에이전트 사용 지침**
> 1. 이 파일을 복사한다
> 2. `<!-- FILL: ... -->` 주석이 있는 부분을 명세 파일(`docs/chapters/weekNN.md`)의 내용으로 채운다
> 3. `PLACEHOLDER` 텍스트를 실제 내용으로 교체한다
> 4. `docs/PRD.md`의 품질 기준 체크리스트를 모두 통과한다

---

## 챕터 HTML 전체 템플릿

아래 코드를 `chapters/chNN.html`로 저장하고 내용을 채운다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Ch.N — CHAPTER_TITLE | 확률·수학·AI 교재</title>
<script>
window.MathJax = {
  tex: { inlineMath:[['\\(','\\)']], displayMath:[['\\[','\\]']], tags:'ams' },
  options:{ skipHtmlTags:['script','noscript','style','textarea','pre'] }
};
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<link rel="stylesheet" href="../style.css"/>
</head>
<body>

<!-- ════════════════════ [1] HERO ════════════════════ -->
<div class="hero">
  <div class="badge">Ch.N · CATEGORY (N주차)</div>
  <h1>CHAPTER_TITLE</h1>
  <p>SUBTITLE_ONE_SENTENCE</p>
</div>


<!-- ════════════════════ [2] EASY BOX ════════════════════ -->
<div class="easy">
  <div class="et">🧒 초등학생 한 줄 설명</div>
  <p><strong>"핵심 비유를 여기에"</strong> EASY_MAIN_SENTENCE</p>
  <ul>
    <li>EASY_EXAMPLE_1 (일상 예시)</li>
    <li>EASY_EXAMPLE_2 (일상 예시)</li>
    <li>AI에서: EASY_EXAMPLE_AI (AI/ML 연결 포인트 — 반드시 포함)</li>
  </ul>
</div>


<!-- ════════════════════ [3] MAIN CONTENT ════════════════════ -->

<!-- ── 섹션 1 ── -->
<h2 class="st"><span class="ic">ICON_1</span>SECTION_TITLE_1</h2>
<p>SECTION_1_DESCRIPTION</p>

<!-- 개념 카드 (선택) -->
<div class="cc">
  <div class="ct">🔑 CONCEPT_NAME</div>
  <p>CONCEPT_DESCRIPTION</p>
</div>

<!-- 표 (선택) -->
<h3 class="sub">TABLE_SUBTITLE</h3>
<table>
  <tr><th>열1</th><th>열2</th><th>열3</th></tr>
  <tr><td>행1열1</td><td>행1열2</td><td>행1열3</td></tr>
  <tr><td>행2열1</td><td>행2열2</td><td>행2열3</td></tr>
</table>

<!-- ── 섹션 2 ── -->
<h2 class="st"><span class="ic">ICON_2</span>SECTION_TITLE_2</h2>
<p>SECTION_2_DESCRIPTION</p>

<!-- 단계별 설명 (선택) -->
<div class="steps">
  <div class="step">
    <div class="sn">1</div>
    <div class="sb"><p><strong>STEP_1_TITLE</strong>: STEP_1_DESC</p></div>
  </div>
  <div class="step">
    <div class="sn">2</div>
    <div class="sb"><p><strong>STEP_2_TITLE</strong>: STEP_2_DESC</p></div>
  </div>
</div>

<!-- 2컬럼 비교 (선택) -->
<div class="two-col">
  <div class="col-box">
    <h5>LEFT_TITLE</h5>
    <p>LEFT_CONTENT</p>
  </div>
  <div class="col-box">
    <h5>RIGHT_TITLE</h5>
    <p>RIGHT_CONTENT</p>
  </div>
</div>


<!-- ════════════════════ [4] FORMULA BOX ════════════════════ -->
<!-- 수식 개수만큼 반복. .fn 주석 반드시 포함 -->
<div class="fb">
  <div class="fl">FORMULA_NAME (영문 대문자)</div>
  \[ FORMULA_HERE \]
  <div class="fn">
    \(SYMBOL_1\): SYMBOL_1_DESC<br/>
    \(SYMBOL_2\): SYMBOL_2_DESC<br/>
    \(SYMBOL_3\): SYMBOL_3_DESC
  </div>
</div>

<!-- callout (챕터당 2~3개 이하) -->
<div class="callout tip">
  <div class="ci">✅</div>
  <div class="cb"><p><strong>KEY_POINT_TITLE:</strong> KEY_POINT_DESC</p></div>
</div>

<div class="callout warn">
  <div class="ci">⚠️</div>
  <div class="cb"><p><strong>흔한 오해:</strong> COMMON_MISTAKE_DESC</p></div>
</div>

<div class="callout info">
  <div class="ci">💡</div>
  <div class="cb"><p><strong>INFO_TITLE:</strong> INFO_DESC</p></div>
</div>


<!-- ════════════════════ [5] INTERACTIVE ════════════════════ -->
<!-- 수치 개념 챕터에서 필수. 해당 없으면 이 블록 전체 삭제 -->
<div class="calc">
  <h4>🔢 CALCULATOR_TITLE</h4>
  <p style="font-size:.82rem;color:var(--muted);margin-bottom:14px;">CALCULATOR_DESC</p>

  <div class="cr">
    <span class="cl">파라미터 \(PARAM_SYMBOL\)</span>
    <input type="range" id="PARAM_ID" min="PARAM_MIN" max="PARAM_MAX" step="PARAM_STEP" value="PARAM_DEFAULT" oninput="updateCalc()"/>
    <input type="number" id="PARAM_ID-v" value="PARAM_DEFAULT" readonly/>
  </div>

  <div class="cres" id="calc-res">결과가 여기에 표시됩니다.</div>
</div>


<!-- ════════════════════ [6] QUIZ ════════════════════ -->
<!-- 정답 위치는 매 챕터마다 다르게 (항상 같은 위치 금지) -->
<div class="qb">
  <h4>🧠 확인 문제</h4>
  <p style="margin-bottom:12px;">QUIZ_QUESTION</p>
  <div class="qo" onclick="ans(this, false)">(a) &nbsp; CHOICE_A</div>
  <div class="qo" onclick="ans(this, false)">(b) &nbsp; CHOICE_B</div>
  <div class="qo" onclick="ans(this, true)">(c) &nbsp; CHOICE_C (정답 위치 예시 — 매 챕터마다 바꿀 것)</div>
  <div class="qo" onclick="ans(this, false)">(d) &nbsp; CHOICE_D</div>
  <div class="qfb"></div>
</div>


<!-- ════════════════════ [7] NAV BUTTONS ════════════════════ -->
<!-- 첫 챕터 이전 버튼: disabled 속성 추가. 마지막 챕터 이후 버튼: 홈(../index.html)으로 -->
<div class="nb">
  <button class="btn btn-s" onclick="location.href='PREV_CHAPTER.html'">← Ch.N-1 이전 챕터명</button>
  <button class="btn btn-p" onclick="location.href='NEXT_CHAPTER.html'">Ch.N+1 다음 챕터명 →</button>
</div>


<!-- ════════════════════ JAVASCRIPT ════════════════════ -->
<script>
// ── 챕터별 계산기 (인터랙티브 블록이 있을 때만 구현) ──
function updateCalc() {
  const PARAM = parseFloat(document.getElementById('PARAM_ID').value);
  document.getElementById('PARAM_ID-v').value = PARAM.toFixed(2);

  const result = PARAM; // FILL: 실제 계산 로직으로 교체

  document.getElementById('calc-res').innerHTML =
    `결과: <strong>${result.toFixed(4)}</strong>`;
}
updateCalc(); // 페이지 로드 시 초기값으로 한 번 실행

// ── 퀴즈 ──
function ans(el, correct) {
  const siblings = el.parentElement.querySelectorAll('.qo');
  siblings.forEach(s => { s.classList.remove('correct','wrong'); s.onclick = null; });
  el.classList.add(correct ? 'correct' : 'wrong');
  const fb = el.parentElement.querySelector('.qfb');
  fb.classList.add('show');
  fb.style.color = correct ? 'var(--accent3)' : 'var(--accent5)';
  fb.textContent = correct
    ? '✅ 정답! CORRECT_FEEDBACK_MSG'        // FILL: 정답 시 핵심 설명 한 줄
    : '❌ 틀렸습니다. WRONG_FEEDBACK_MSG';   // FILL: 오답 시 힌트 한 줄
}

// ── 체크박스 저장 ──
function saveCheck(el) {
  localStorage.setItem('chk_' + el.id, el.checked ? '1' : '0');
}
function loadChecks() {
  document.querySelectorAll('input[type=checkbox]').forEach(el => {
    const v = localStorage.getItem('chk_' + el.id);
    if (v !== null) el.checked = v === '1';
  });
}
loadChecks();
</script>
</body>
</html>
```

---

## 체크리스트 (개발 완료 전 필수 확인)

챕터 HTML 파일을 완성한 후 아래를 모두 확인한다:

- [ ] `[1]` HERO 블록: badge, h1, p 모두 채워져 있는가
- [ ] `[2]` EASY BOX: 일상 비유가 있고 마지막 예시가 AI와 연결되는가
- [ ] `[3]` MAIN CONTENT: h2.st 섹션이 2개 이상 있는가
- [ ] `[4]` FORMULA BOX: 모든 수식에 `.fn` 주석이 있는가
- [ ] `[5]` INTERACTIVE: 해당 챕터라면 슬라이더가 실시간 업데이트되는가
- [ ] `[6]` QUIZ: 정답이 본문 내용과 일치하는가, 4지선다인가
- [ ] `[7]` NAV: 이전/다음 링크가 실제 파일 경로와 일치하는가
- [ ] CSS 변수를 직접 색상 코드로 하드코딩하지 않았는가
- [ ] `PLACEHOLDER` / `FILL` 텍스트가 남아 있지 않은가
- [ ] MathJax 수식이 `\( \)` 또는 `\[ \]`로 감싸져 있는가
