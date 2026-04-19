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
<!-- FILL: 제목을 "Ch.N — 챕터명 | 확률·수학·AI 교재" 형식으로 -->
<title>Ch.N — CHAPTER_TITLE | 확률·수학·AI 교재</title>

<script>
window.MathJax = {
  tex: { inlineMath:[['\\(','\\)']], displayMath:[['\\[','\\]']], tags:'ams' },
  options:{ skipHtmlTags:['script','noscript','style','textarea','pre'] }
};
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<style>
/* ══ CSS 변수 (절대 변경 금지) ══ */
:root {
  --bg:#0f1117; --surface:#1a1d27; --surface2:#22263a; --surface3:#2a2f47;
  --accent:#6c8aff; --accent2:#a78bfa; --accent3:#34d399; --accent4:#f59e0b; --accent5:#f87171;
  --text:#e2e8f0; --muted:#94a3b8; --border:#2d3148; --code:#141824;
  --hl:rgba(108,138,255,0.10);
}
*{box-sizing:border-box;margin:0;padding:0;}
body{
  font-family:'Segoe UI','Apple SD Gothic Neo',sans-serif;
  background:var(--bg);color:var(--text);line-height:1.75;
  padding:44px 56px;max-width:900px;margin:0 auto;
}
p{margin-bottom:13px;color:var(--text);font-size:.95rem;}

/* ══ HERO ══ */
.hero{background:linear-gradient(135deg,#1a1d2e,#22263a);border:1px solid var(--border);
  border-radius:16px;padding:44px 48px;margin-bottom:44px;position:relative;overflow:hidden;}
.hero::before{content:'';position:absolute;top:-60px;right:-60px;width:200px;height:200px;
  background:radial-gradient(circle,rgba(108,138,255,.14),transparent 70%);}
.hero .badge{display:inline-flex;align-items:center;gap:6px;
  background:rgba(108,138,255,.14);border:1px solid rgba(108,138,255,.3);
  color:var(--accent);padding:4px 12px;border-radius:99px;font-size:11px;font-weight:600;margin-bottom:14px;}
.hero h1{font-size:2rem;font-weight:800;line-height:1.2;margin-bottom:10px;
  background:linear-gradient(135deg,var(--text),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.hero p{color:var(--muted);font-size:.95rem;max-width:580px;}

/* ══ EASY BOX ══ */
.easy{background:linear-gradient(135deg,rgba(167,139,250,.08),rgba(52,211,153,.06));
  border:1px solid rgba(167,139,250,.25);border-radius:12px;padding:18px 22px;margin:14px 0;}
.easy .et{font-size:.78rem;font-weight:700;color:var(--accent2);letter-spacing:.1em;
  text-transform:uppercase;margin-bottom:8px;}
.easy p{font-size:.93rem;margin-bottom:6px;}
.easy ul{padding-left:18px;margin:6px 0;}
.easy li{font-size:.9rem;margin-bottom:4px;line-height:1.6;}

/* ══ SECTION TITLES ══ */
h2.st{font-size:1.4rem;font-weight:700;color:var(--text);margin:38px 0 14px;
  padding-bottom:10px;border-bottom:2px solid var(--border);display:flex;align-items:center;gap:10px;}
h2.st .ic{width:30px;height:30px;background:var(--hl);border-radius:8px;
  display:flex;align-items:center;justify-content:center;font-size:15px;}
h3.sub{font-size:1.05rem;font-weight:600;color:var(--accent2);margin:24px 0 9px;}

/* ══ FORMULA BOX ══ */
.fb{background:var(--code);border:1px solid var(--border);border-left:4px solid var(--accent);
  border-radius:8px;padding:18px 22px;margin:18px 0;overflow-x:auto;}
.fb .fl{font-size:10px;color:var(--accent);text-transform:uppercase;letter-spacing:.1em;margin-bottom:9px;font-weight:600;}
.fb .fn{font-size:.82rem;color:var(--muted);margin-top:9px;line-height:1.7;}

/* ══ CONCEPT CARD ══ */
.cc{background:var(--surface2);border:1px solid var(--border);border-radius:12px;padding:18px 22px;margin:18px 0;}
.cc .ct{font-size:.8rem;font-weight:700;color:var(--accent3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:7px;}

/* ══ CALLOUT ══ */
.callout{border-radius:10px;padding:14px 18px;margin:18px 0;display:flex;gap:11px;}
.callout.info{background:rgba(108,138,255,.08);border:1px solid rgba(108,138,255,.2);}
.callout.tip {background:rgba(52,211,153,.08); border:1px solid rgba(52,211,153,.2);}
.callout.warn{background:rgba(245,158,11,.08); border:1px solid rgba(245,158,11,.2);}
.callout .ci{font-size:17px;flex-shrink:0;}
.callout .cb p{margin-bottom:0;font-size:.88rem;}

/* ══ TABLE ══ */
table{width:100%;border-collapse:collapse;margin:14px 0 22px;font-size:.88rem;}
th{background:var(--surface2);color:var(--accent);padding:9px 13px;text-align:left;border-bottom:2px solid var(--border);}
td{padding:8px 13px;border-bottom:1px solid var(--border);}
tr:hover td{background:var(--hl);}

/* ══ STEPS ══ */
.step{display:flex;gap:14px;margin-bottom:18px;}
.sn{width:30px;height:30px;background:var(--accent);border-radius:50%;flex-shrink:0;
  display:flex;align-items:center;justify-content:center;font-weight:700;font-size:13px;color:#fff;}
.sb p{margin-bottom:0;}

/* ══ 2COL ══ */
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px;}
.col-box{background:var(--surface3);border-radius:8px;padding:13px 15px;}
.col-box h5{font-size:.75rem;color:var(--accent4);font-weight:700;text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;}
.col-box p,.col-box li{font-size:.85rem;margin-bottom:4px;line-height:1.55;}
.col-box ul{padding-left:16px;}

/* ══ QUIZ ══ */
.qb{background:var(--surface2);border:1px solid var(--border);border-radius:12px;padding:22px;margin:26px 0;}
.qb h4{color:var(--accent4);margin-bottom:13px;font-size:.97rem;}
.qo{display:flex;align-items:center;gap:9px;padding:9px 13px;border-radius:7px;
  border:1px solid var(--border);margin-bottom:7px;cursor:pointer;transition:all .18s;font-size:.88rem;}
.qo:hover{background:var(--hl);border-color:var(--accent);}
.qo.correct{background:rgba(52,211,153,.14);border-color:var(--accent3);color:var(--accent3);}
.qo.wrong{background:rgba(239,68,68,.10);border-color:#ef4444;color:#f87171;}
.qfb{margin-top:10px;font-size:.85rem;display:none;}
.qfb.show{display:block;}

/* ══ INTERACTIVE ══ */
.calc{background:var(--surface2);border:1px solid var(--border);border-radius:12px;padding:22px;margin:22px 0;}
.calc h4{color:var(--accent4);margin-bottom:14px;}
.cr{display:flex;align-items:center;gap:11px;margin-bottom:10px;flex-wrap:wrap;}
.cl{font-size:12px;color:var(--muted);min-width:115px;}
input[type=range]{flex:1;min-width:90px;accent-color:var(--accent);}
input[type=number]{width:72px;background:var(--code);border:1px solid var(--border);
  border-radius:6px;color:var(--text);padding:4px 7px;font-size:12px;}
.cres{margin-top:14px;padding:13px 17px;background:var(--code);
  border-radius:8px;font-size:.9rem;color:var(--accent3);font-weight:600;line-height:1.6;}

/* ══ NAV ══ */
.nb{display:flex;justify-content:space-between;margin-top:52px;padding-top:22px;border-top:1px solid var(--border);}
.btn{padding:9px 22px;border-radius:8px;border:none;cursor:pointer;font-size:13px;font-weight:600;transition:all .18s;}
.btn-p{background:var(--accent);color:#fff;}
.btn-p:hover{background:#5a78f0;transform:translateY(-1px);}
.btn-s{background:var(--surface2);color:var(--text);border:1px solid var(--border);}
.btn-s:hover{background:var(--hl);}
.btn:disabled{opacity:.3;cursor:default;transform:none!important;}

/* ══ TAG ══ */
.tag{display:inline-block;padding:2px 9px;border-radius:99px;font-size:10px;font-weight:600;margin-right:5px;}
.tb{background:rgba(108,138,255,.14);color:var(--accent);}
.tg{background:rgba(52,211,153,.14);color:var(--accent3);}
.tp{background:rgba(167,139,250,.14);color:var(--accent2);}
.ty{background:rgba(245,158,11,.14);color:var(--accent4);}

/* ══ SCROLLBAR ══ */
::-webkit-scrollbar{width:5px;height:5px;}
::-webkit-scrollbar-track{background:transparent;}
::-webkit-scrollbar-thumb{background:var(--border);border-radius:99px;}
</style>
</head>
<body>

<!-- ════════════════════════════════════════
  [1] HERO 블록 (필수)
  FILL: badge, h1, p
════════════════════════════════════════ -->
<div class="hero">
  <!-- FILL: "Ch.N · 카테고리 (N~N주차)" -->
  <div class="badge">Ch.N · CATEGORY (N주차)</div>
  <!-- FILL: 챕터 제목 (2줄 이하, 핵심 키워드 포함) -->
  <h1>CHAPTER_TITLE</h1>
  <!-- FILL: 한 문장 — "이 챕터를 배우면 무엇이 가능해지는지" -->
  <p>SUBTITLE_ONE_SENTENCE</p>
</div>


<!-- ════════════════════════════════════════
  [2] EASY BOX 블록 (필수)
  FILL: 초등학생 비유 + 예시 3개 + AI 연결
════════════════════════════════════════ -->
<div class="easy">
  <div class="et">🧒 초등학생 한 줄 설명</div>
  <!-- FILL: 핵심 비유를 <strong>으로 강조 -->
  <p>EASY_MAIN_SENTENCE — <strong>"핵심 비유를 여기에"</strong></p>
  <ul>
    <li>EASY_EXAMPLE_1 (일상 예시)</li>
    <li>EASY_EXAMPLE_2 (일상 예시)</li>
    <li>EASY_EXAMPLE_AI (AI/ML 연결 포인트)</li>
  </ul>
</div>


<!-- ════════════════════════════════════════
  [3] MAIN CONTENT 블록 (필수)
  섹션을 여러 개 넣을 수 있음
  각 섹션: h2.st > 설명 단락 > h3.sub > 세부 설명 > table/steps/두-col
════════════════════════════════════════ -->

<!-- == 섹션 1 == -->
<h2 class="st"><span class="ic">ICON_1</span>SECTION_TITLE_1</h2>
<p>SECTION_1_DESCRIPTION</p>

<!-- FILL: 개념 카드 (선택) -->
<div class="cc">
  <div class="ct">🔑 핵심 개념 — CONCEPT_NAME</div>
  <p>CONCEPT_DESCRIPTION</p>
  <!-- FILL: 수식이 있을 경우 -->
  \[ FORMULA_HERE \]
</div>

<!-- FILL: 표 (선택) -->
<h3 class="sub">TABLE_SUBTITLE</h3>
<table>
  <tr><th>열1</th><th>열2</th><th>열3</th></tr>
  <tr><td>행1열1</td><td>행1열2</td><td>행1열3</td></tr>
  <tr><td>행2열1</td><td>행2열2</td><td>행2열3</td></tr>
</table>

<!-- == 섹션 2 == -->
<h2 class="st"><span class="ic">ICON_2</span>SECTION_TITLE_2</h2>
<p>SECTION_2_DESCRIPTION</p>

<!-- FILL: 단계별 설명 (선택) -->
<div class="steps">
  <div class="step">
    <div class="sn">1</div>
    <div class="sb"><p><strong>STEP_1_TITLE</strong>: STEP_1_DESC</p></div>
  </div>
  <div class="step">
    <div class="sn">2</div>
    <div class="sb"><p><strong>STEP_2_TITLE</strong>: STEP_2_DESC</p></div>
  </div>
  <!-- FILL: 필요한 만큼 반복 -->
</div>


<!-- ════════════════════════════════════════
  [4] FORMULA BOX 블록 (필수, 수식 개수만큼 반복)
  FILL: fl(수식 이름), 수식, fn(각 기호 한국어 설명)
════════════════════════════════════════ -->
<div class="fb">
  <!-- FILL: 수식 이름 (영문 대문자) -->
  <div class="fl">FORMULA_NAME</div>
  <!-- FILL: MathJax 블록 수식 -->
  \[ FORMULA_HERE \]
  <!-- FILL: 각 기호 한국어 설명 (반드시 포함) -->
  <div class="fn">
    \(SYMBOL_1\): SYMBOL_1_DESC<br/>
    \(SYMBOL_2\): SYMBOL_2_DESC<br/>
    \(SYMBOL_3\): SYMBOL_3_DESC
  </div>
</div>

<!-- FILL: callout (선택, 챕터당 2~3개 이하) -->
<div class="callout tip">
  <div class="ci">✅</div>
  <div class="cb"><p><strong>KEY_POINT_TITLE:</strong> KEY_POINT_DESC</p></div>
</div>

<div class="callout warn">
  <div class="ci">⚠️</div>
  <div class="cb"><p><strong>흔한 오해:</strong> COMMON_MISTAKE_DESC</p></div>
</div>


<!-- ════════════════════════════════════════
  [5] INTERACTIVE 블록 (수치 개념 챕터에서 필수)
  해당 없으면 이 블록 전체 삭제
  FILL: 계산기 제목, 파라미터 목록, JavaScript 함수
════════════════════════════════════════ -->
<div class="calc">
  <!-- FILL: 계산기 제목 -->
  <h4>🔢 CALCULATOR_TITLE</h4>
  <p style="font-size:.82rem;color:var(--muted);margin-bottom:14px;">CALCULATOR_DESC</p>

  <!-- FILL: 파라미터 슬라이더 (필요한 만큼 반복) -->
  <div class="cr">
    <span class="cl">파라미터 \(\PARAM_SYMBOL\)</span>
    <input type="range" id="PARAM_ID" min="PARAM_MIN" max="PARAM_MAX" step="PARAM_STEP" value="PARAM_DEFAULT" oninput="updateCalc()"/>
    <input type="number" id="PARAM_ID-v" value="PARAM_DEFAULT" readonly/>
  </div>

  <!-- FILL: 결과 표시 -->
  <div class="cres" id="calc-res">결과가 여기에 표시됩니다.</div>
</div>


<!-- ════════════════════════════════════════
  [6] QUIZ 블록 (필수)
  FILL: 질문, 4개 선택지, 정답(true/false)
  정답 위치는 매 챕터마다 다르게!
════════════════════════════════════════ -->
<div class="qb">
  <h4>🧠 확인 문제</h4>
  <!-- FILL: 본문의 핵심 개념을 테스트하는 질문 -->
  <p style="margin-bottom:12px;">QUIZ_QUESTION</p>
  <!-- FILL: 정답(true)과 오답(false) 설정. 정답 위치 매 챕터마다 다르게 -->
  <div class="qo" onclick="ans(this, CORRECT_OR_FALSE)">(a) &nbsp; CHOICE_A</div>
  <div class="qo" onclick="ans(this, CORRECT_OR_FALSE)">(b) &nbsp; CHOICE_B</div>
  <div class="qo" onclick="ans(this, CORRECT_OR_FALSE)">(c) &nbsp; CHOICE_C</div>
  <div class="qo" onclick="ans(this, CORRECT_OR_FALSE)">(d) &nbsp; CHOICE_D</div>
  <div class="qfb"></div>
</div>


<!-- ════════════════════════════════════════
  [7] NAV BUTTONS 블록 (필수)
  FILL: 이전/다음 챕터 파일명
════════════════════════════════════════ -->
<div class="nb">
  <!-- FILL: 이전 챕터 이름과 파일 경로. 첫 챕터면 disabled -->
  <button class="btn btn-s" onclick="location.href='PREV_CHAPTER.html'">← 이전 챕터명</button>
  <!-- FILL: 다음 챕터 이름과 파일 경로 -->
  <button class="btn btn-p" onclick="location.href='NEXT_CHAPTER.html'">다음 챕터명 →</button>
</div>


<!-- ════════════════════════════════════════
  JAVASCRIPT (공통 함수 + 챕터별 계산기 함수)
════════════════════════════════════════ -->
<script>
// ── 퀴즈 (공통, 변경 금지) ──
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

// ── 체크박스 저장 (공통, 변경 금지) ──
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

// ── 챕터별 계산기 함수 (FILL: 인터랙티브 블록이 있을 경우만) ──
function updateCalc() {
  // FILL: 파라미터 읽기
  const PARAM = parseFloat(document.getElementById('PARAM_ID').value);
  document.getElementById('PARAM_ID-v').value = PARAM.toFixed(2);

  // FILL: 계산 로직
  const result = /* 계산식 */ PARAM;

  // FILL: 결과 출력
  document.getElementById('calc-res').innerHTML =
    `결과: <strong>${result.toFixed(4)}</strong>`;
}
// FILL: 페이지 로드 시 초기값으로 한 번 실행
// updateCalc();
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
