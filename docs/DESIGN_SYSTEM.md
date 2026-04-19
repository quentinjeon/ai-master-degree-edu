# DESIGN SYSTEM: 컴포넌트 스니펫 카탈로그

> 에이전트는 이 파일의 스니펫을 그대로 복사해서 사용한다.  
> 절대로 CSS 변수 값을 직접 색상 코드로 대체하지 않는다.

---

## 0. HTML 파일 헤드 (공통)

모든 챕터 HTML의 `<head>` 상단에 반드시 포함:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Ch.N — 챕터 제목 | 확률·수학·AI 교재</title>
<script>
window.MathJax = {
  tex: { inlineMath:[['\\(','\\)']], displayMath:[['\\[','\\]']], tags:'ams' },
  options:{ skipHtmlTags:['script','noscript','style','textarea','pre'] }
};
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<link rel="stylesheet" href="../styles/main.css"/>
<!-- 또는 인라인 CSS를 사용할 경우: <style>...</style> -->
</head>
```

---

## 1. CSS 변수 전체 (공통 `:root`)

```css
:root {
  --bg: #0f1117;
  --surface: #1a1d27;
  --surface2: #22263a;
  --surface3: #2a2f47;
  --accent: #6c8aff;
  --accent2: #a78bfa;
  --accent3: #34d399;
  --accent4: #f59e0b;
  --accent5: #f87171;
  --text: #e2e8f0;
  --muted: #94a3b8;
  --border: #2d3148;
  --code: #141824;
  --hl: rgba(108,138,255,0.10);
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Segoe UI', 'Apple SD Gothic Neo', sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.75;
  padding: 44px 56px;
  max-width: 900px;
  margin: 0 auto;
}
p { margin-bottom: 13px; color: var(--text); font-size: .95rem; }
```

---

## 2. HERO 컴포넌트

```html
<!-- 사용 예시 -->
<div class="hero">
  <div class="badge">Ch.4 · 확률 &amp; 통계 (3~4주차)</div>
  <h1>확률론 기초</h1>
  <p>불확실성을 수학적으로 표현하는 언어. AI 모델의 예측은 본질적으로 확률입니다.</p>
</div>
```

```css
.hero {
  background: linear-gradient(135deg, #1a1d2e, #22263a);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 44px 48px;
  margin-bottom: 44px;
  position: relative;
  overflow: hidden;
}
.hero::before {
  content: '';
  position: absolute;
  top: -60px; right: -60px;
  width: 200px; height: 200px;
  background: radial-gradient(circle, rgba(108,138,255,.14), transparent 70%);
}
.hero .badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: rgba(108,138,255,.14);
  border: 1px solid rgba(108,138,255,.3);
  color: var(--accent);
  padding: 4px 12px;
  border-radius: 99px;
  font-size: 11px;
  font-weight: 600;
  margin-bottom: 14px;
}
.hero h1 {
  font-size: 2rem;
  font-weight: 800;
  line-height: 1.2;
  margin-bottom: 10px;
  background: linear-gradient(135deg, var(--text), var(--accent2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.hero p { color: var(--muted); font-size: .95rem; max-width: 580px; }
```

---

## 3. EASY BOX (초등학생 설명)

```html
<!-- 사용 예시 -->
<div class="easy">
  <div class="et">🧒 초등학생 한 줄 설명</div>
  <p>확률은 <strong>"어떤 일이 일어날 가능성을 0~1 사이 숫자로 나타낸 것"</strong>이다.</p>
  <ul>
    <li>동전 앞면이 나올 확률: 0.5 (반반)</li>
    <li>내일 비 올 확률: 0.3 (조금 있음)</li>
    <li>AI의 "고양이" 예측 확률: 0.9 (꽤 확신)</li>
  </ul>
</div>
```

```css
.easy {
  background: linear-gradient(135deg, rgba(167,139,250,.08), rgba(52,211,153,.06));
  border: 1px solid rgba(167,139,250,.25);
  border-radius: 12px;
  padding: 18px 22px;
  margin: 14px 0;
}
.easy .et {
  font-size: .78rem;
  font-weight: 700;
  color: var(--accent2);
  letter-spacing: .1em;
  text-transform: uppercase;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.easy p { font-size: .93rem; color: var(--text); margin-bottom: 6px; }
.easy ul { padding-left: 18px; margin: 6px 0; }
.easy li { font-size: .9rem; color: var(--text); margin-bottom: 4px; line-height: 1.6; }
```

---

## 4. 섹션 제목 (h2, h3)

```html
<!-- h2 섹션 제목 -->
<h2 class="st"><span class="ic">🎲</span>확률의 기본 개념</h2>

<!-- h3 소제목 -->
<h3 class="sub">조건부확률의 직관</h3>
```

```css
h2.st {
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--text);
  margin: 38px 0 14px;
  padding-bottom: 10px;
  border-bottom: 2px solid var(--border);
  display: flex;
  align-items: center;
  gap: 10px;
}
h2.st .ic {
  width: 30px; height: 30px;
  background: var(--hl);
  border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  font-size: 15px;
}
h3.sub {
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--accent2);
  margin: 24px 0 9px;
}
```

---

## 5. FORMULA BOX (수식)

```html
<!-- 사용 예시 -->
<div class="fb">
  <div class="fl">베이즈 정리 (Bayes' Theorem)</div>
  \[ P(\theta \mid \mathcal{D}) = \frac{P(\mathcal{D} \mid \theta) \cdot P(\theta)}{P(\mathcal{D})} \]
  <div class="fn">
    \(P(\theta \mid \mathcal{D})\): 사후확률 (posterior) — 데이터를 본 후 믿음<br/>
    \(P(\mathcal{D} \mid \theta)\): 우도 (likelihood) — θ일 때 데이터 관측 확률<br/>
    \(P(\theta)\): 사전확률 (prior) — 데이터 보기 전 믿음
  </div>
</div>
```

```css
.fb {
  background: var(--code);
  border: 1px solid var(--border);
  border-left: 4px solid var(--accent);
  border-radius: 8px;
  padding: 18px 22px;
  margin: 18px 0;
  overflow-x: auto;
}
.fb .fl {
  font-size: 10px;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: .1em;
  margin-bottom: 9px;
  font-weight: 600;
}
.fb .fn {
  font-size: .82rem;
  color: var(--muted);
  margin-top: 9px;
  line-height: 1.7;
}
```

---

## 6. 핵심 개념 카드 (Concept Card)

```html
<!-- 사용 예시 -->
<div class="cc">
  <div class="ct">🔑 핵심 개념 — 머신러닝 모델은 함수다</div>
  <p>모든 AI 모델은 수학적으로 하나의 함수로 표현된다:</p>
  \[ f_\theta : \mathcal{X} \to \mathcal{Y} \]
  <p style="margin-top:8px;">파라미터 \(\theta\)를 가진 모델이 입력을 출력으로 변환한다.</p>
</div>
```

```css
.cc {
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 18px 22px;
  margin: 18px 0;
}
.cc .ct {
  font-size: .8rem;
  font-weight: 700;
  color: var(--accent3);
  text-transform: uppercase;
  letter-spacing: .08em;
  margin-bottom: 7px;
}
```

---

## 7. CALLOUT (알림 박스)

```html
<!-- info (파랑) -->
<div class="callout info">
  <div class="ci">💡</div>
  <div class="cb"><p>정보성 내용을 여기에 작성</p></div>
</div>

<!-- tip (초록) -->
<div class="callout tip">
  <div class="ci">✅</div>
  <div class="cb"><p><strong>논문에서 자주 보이는 표현:</strong> ...</p></div>
</div>

<!-- warn (노랑) -->
<div class="callout warn">
  <div class="ci">⚠️</div>
  <div class="cb"><p><strong>흔한 오해:</strong> ...</p></div>
</div>
```

```css
.callout {
  border-radius: 10px;
  padding: 14px 18px;
  margin: 18px 0;
  display: flex;
  gap: 11px;
}
.callout.info { background: rgba(108,138,255,.08); border: 1px solid rgba(108,138,255,.2); }
.callout.tip  { background: rgba(52,211,153,.08);  border: 1px solid rgba(52,211,153,.2); }
.callout.warn { background: rgba(245,158,11,.08);  border: 1px solid rgba(245,158,11,.2); }
.callout .ci  { font-size: 17px; flex-shrink: 0; }
.callout .cb p { margin-bottom: 0; font-size: .88rem; }
```

---

## 8. 표 (Table)

```html
<!-- 사용 예시 -->
<table>
  <tr><th>기호</th><th>읽는 법</th><th>의미</th><th>예시</th></tr>
  <tr><td>\(x \in A\)</td><td>x는 A에 속한다</td><td>원소 관계</td><td>\(3 \in \{1,2,3\}\)</td></tr>
  <tr><td>\(\forall x\)</td><td>모든 x에 대해</td><td>전칭 한정자</td><td>\(\forall x \ge 0\)</td></tr>
</table>
```

```css
table { width: 100%; border-collapse: collapse; margin: 14px 0 22px; font-size: .88rem; }
th { background: var(--surface2); color: var(--accent); padding: 9px 13px; text-align: left; border-bottom: 2px solid var(--border); }
td { padding: 8px 13px; border-bottom: 1px solid var(--border); }
tr:hover td { background: var(--hl); }
```

---

## 9. QUIZ (퀴즈)

```html
<!-- 사용 예시 -->
<div class="qb">
  <h4>🧠 확인 문제</h4>
  <p style="margin-bottom:12px;">P(A|B)를 올바르게 해석한 것은?</p>
  <div class="qo" onclick="ans(this, false)">(a) &nbsp; A와 B가 동시에 일어날 확률</div>
  <div class="qo" onclick="ans(this, false)">(b) &nbsp; A 또는 B가 일어날 확률</div>
  <div class="qo" onclick="ans(this, true)">(c) &nbsp; B가 일어났을 때 A가 일어날 확률</div>
  <div class="qo" onclick="ans(this, false)">(d) &nbsp; A가 일어났을 때 B가 일어날 확률</div>
  <div class="qfb"></div>
</div>
```

```css
.qb { background: var(--surface2); border: 1px solid var(--border); border-radius: 12px; padding: 22px; margin: 26px 0; }
.qb h4 { color: var(--accent4); margin-bottom: 13px; font-size: .97rem; }
.qo {
  display: flex; align-items: center; gap: 9px;
  padding: 9px 13px; border-radius: 7px;
  border: 1px solid var(--border); margin-bottom: 7px;
  cursor: pointer; transition: all .18s; font-size: .88rem;
}
.qo:hover { background: var(--hl); border-color: var(--accent); }
.qo.correct { background: rgba(52,211,153,.14); border-color: var(--accent3); color: var(--accent3); }
.qo.wrong   { background: rgba(239,68,68,.10);  border-color: #ef4444; color: #f87171; }
.qfb { margin-top: 10px; font-size: .85rem; display: none; }
.qfb.show { display: block; }
```

---

## 10. INTERACTIVE 계산기

```html
<!-- 슬라이더 행 패턴 -->
<div class="calc">
  <h4>🔢 정규분포 계산기</h4>
  <p style="font-size:.82rem;color:var(--muted);margin-bottom:14px;">μ와 σ를 바꾸며 분포 모양의 변화를 확인하세요.</p>

  <div class="cr">
    <span class="cl">평균 \(\mu\)</span>
    <input type="range" id="mu" min="-5" max="5" step=".1" value="0" oninput="updateCalc()"/>
    <input type="number" id="mu-v" value="0" readonly/>
  </div>
  <div class="cr">
    <span class="cl">표준편차 \(\sigma\)</span>
    <input type="range" id="sg" min=".1" max="3" step=".1" value="1" oninput="updateCalc()"/>
    <input type="number" id="sg-v" value="1" readonly/>
  </div>

  <div class="cres" id="calc-res">결과가 여기에 표시됩니다.</div>
</div>
```

```css
.calc { background: var(--surface2); border: 1px solid var(--border); border-radius: 12px; padding: 22px; margin: 22px 0; }
.calc h4 { color: var(--accent4); margin-bottom: 14px; }
.cr { display: flex; align-items: center; gap: 11px; margin-bottom: 10px; flex-wrap: wrap; }
.cl { font-size: 12px; color: var(--muted); min-width: 115px; }
input[type=range] { flex: 1; min-width: 90px; accent-color: var(--accent); }
input[type=number] {
  width: 72px; background: var(--code); border: 1px solid var(--border);
  border-radius: 6px; color: var(--text); padding: 4px 7px; font-size: 12px;
}
.cres {
  margin-top: 14px; padding: 13px 17px; background: var(--code);
  border-radius: 8px; font-size: .9rem; color: var(--accent3); font-weight: 600; line-height: 1.6;
}
```

---

## 11. 2컬럼 그리드

```html
<!-- 사용 예시: "왜 배우나 + 어디에 쓰나" -->
<div class="two-col">
  <div class="col-box">
    <h5>🤖 AI에서 왜 중요해?</h5>
    <p>설명...</p>
    <ul>
      <li>예시 1</li>
      <li>예시 2</li>
    </ul>
  </div>
  <div class="col-box">
    <h5>📐 수식으로는</h5>
    <p>\( P(A \mid B) = \dfrac{P(A \cap B)}{P(B)} \)</p>
  </div>
</div>
```

```css
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 14px; }
.col-box { background: var(--surface3); border-radius: 8px; padding: 13px 15px; }
.col-box h5 { font-size: .75rem; color: var(--accent4); font-weight: 700; text-transform: uppercase; letter-spacing: .08em; margin-bottom: 8px; }
.col-box p, .col-box li { font-size: .85rem; color: var(--text); margin-bottom: 4px; line-height: 1.55; }
.col-box ul { padding-left: 16px; }
```

---

## 12. 단계별 리스트 (Steps)

```html
<!-- 사용 예시: 가설 검정 절차 -->
<div class="steps">
  <div class="step">
    <div class="sn">1</div>
    <div class="sb"><p><strong>귀무가설 H₀</strong> 설정: "두 방법 사이에 차이가 없다"</p></div>
  </div>
  <div class="step">
    <div class="sn">2</div>
    <div class="sb"><p><strong>대립가설 H₁</strong> 설정: "새로운 방법이 더 좋다"</p></div>
  </div>
</div>
```

```css
.step { display: flex; gap: 14px; margin-bottom: 18px; }
.sn {
  width: 30px; height: 30px; background: var(--accent);
  border-radius: 50%; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  font-weight: 700; font-size: 13px; color: #fff;
}
.sb p { margin-bottom: 0; }
```

---

## 13. NAV BUTTONS

```html
<!-- 중간 챕터 -->
<div class="nb">
  <button class="btn btn-s" onclick="history.back()">← 이전 챕터</button>
  <button class="btn btn-p" onclick="location.href='ch05.html'">Ch.5 확률분포 →</button>
</div>

<!-- 첫 챕터 (이전 버튼 비활성) -->
<div class="nb">
  <button class="btn btn-s" disabled>← 이전</button>
  <button class="btn btn-p" onclick="location.href='ch02.html'">Ch.2 미분 →</button>
</div>
```

```css
.nb { display: flex; justify-content: space-between; margin-top: 52px; padding-top: 22px; border-top: 1px solid var(--border); }
.btn { padding: 9px 22px; border-radius: 8px; border: none; cursor: pointer; font-size: 13px; font-weight: 600; transition: all .18s; }
.btn-p { background: var(--accent); color: #fff; }
.btn-p:hover { background: #5a78f0; transform: translateY(-1px); }
.btn-s { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
.btn-s:hover { background: var(--hl); }
.btn:disabled { opacity: .3; cursor: default; transform: none !important; }
```

---

## 14. 태그 (Tag)

```html
<span class="tag tb">파란 태그</span>
<span class="tag tg">초록 태그</span>
<span class="tag tp">보라 태그</span>
<span class="tag ty">노란 태그</span>
```

```css
.tag { display: inline-block; padding: 2px 9px; border-radius: 99px; font-size: 10px; font-weight: 600; margin-right: 5px; }
.tb { background: rgba(108,138,255,.14); color: var(--accent); }
.tg { background: rgba(52,211,153,.14);  color: var(--accent3); }
.tp { background: rgba(167,139,250,.14); color: var(--accent2); }
.ty { background: rgba(245,158,11,.14);  color: var(--accent4); }
```

---

## 15. 공통 JavaScript 함수 (모든 챕터 필수)

```html
<script>
// ── 퀴즈 정답 처리 ──
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

// ── 체크박스 로컬스토리지 저장 ──
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
```

---

## 16. 체크리스트 아이템 (주간 계획용)

```html
<div class="check-item">
  <input type="checkbox" id="w1-c1" onchange="saveCheck(this)">
  <label for="w1-c1">함수가 무엇인지 설명할 수 있다</label>
</div>
```

```css
.check-item { display: flex; align-items: flex-start; gap: 8px; margin-bottom: 7px; }
.check-item input[type=checkbox] {
  width: 16px; height: 16px; accent-color: var(--accent3);
  cursor: pointer; flex-shrink: 0; margin-top: 2px;
}
.check-item label { font-size: .83rem; color: var(--text); cursor: pointer; line-height: 1.5; }
.check-item input:checked + label { color: var(--muted); text-decoration: line-through; }
```

---

## 17. 스크롤바 (공통)

```css
::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 99px; }
```
