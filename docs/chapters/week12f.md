# Week 12F — LLM 평가 방법론 (Ch.12F)

> **파일명**: `chapters/ch12f.html`  
> **Phase**: Unit 3 · AI & ML 연결 (고도화)  
> **인터랙티브**: ✅ BLEU vs LLM-Judge 메트릭 비교기  
> **우선순위**: P1  
> **기반 강의**: [Stanford CME295 L8 — LLM Evaluation](https://www.youtube.com/watch?v=8fNP4N46RRo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=8) (1:49:25)  
> **슬라이드**: [fall25-cme295-lecture8.pdf](https://cme295.stanford.edu/slides/fall25-cme295-lecture8.pdf)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12F · LLM 평가 (Unit 3 고도화)` |
| 제목 | `LLM 평가 방법론` |
| 서브타이틀 | `"이 AI가 얼마나 좋은가?"를 측정하는 법 — BLEU의 한계부터 LLM-as-a-Judge까지, 논문 결과 섹션을 읽기 위한 필수 지식.` |
| 이전 챕터 | `ch12e.html` — Ch.12E RAG: 검색 증강 생성 |
| 다음 챕터 | `ch13.html` — Ch.13 Calibration & 불확실성 |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L8**: [LLM Evaluation (1:49:25)](https://www.youtube.com/watch?v=8fNP4N46RRo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=8)
- **슬라이드**: [강의자료 PDF](https://cme295.stanford.edu/slides/fall25-cme295-lecture8.pdf)
- **핵심 논문**: Zheng et al., 2023 — "Judging LLM-as-a-Judge"

---

## EASY BOX 내용

**핵심 비유**: "학생 글짓기를 채점하는 방법이다. ①단순히 표준 답안과 단어가 얼마나 겹치는지 세는 법(BLEU/ROUGE), ②전문가 교사가 직접 읽고 점수를 주는 법(인간 평가), ③AI 교사가 대신 읽고 채점하는 법(LLM-as-a-Judge) — 각각 장단점이 다르다."

**예시 리스트**:
- BLEU가 높아도 내용이 이상한 경우: "The cat sat on the mat" → "The mat sat on the cat" (단어는 같지만 의미 반전)
- 인간 평가의 한계: 비용 높음, 주관적, 느림
- LLM-as-a-Judge: GPT-4가 채점자 역할 → 인간 평가와 상관관계 높음
- AI에서: Chatbot Arena, LMSYS, AlpacaEval이 LLM-as-a-Judge 기반

---

## 섹션 구성

### 섹션 1: 전통적 자동 평가 메트릭과 한계

**아이콘**: `📏`  
**제목**: `BLEU·ROUGE — 단어 겹침 기반 평가의 한계`

**설명**: BLEU(Bilingual Evaluation Understudy)는 기계 번역 평가를 위해 고안되었지만, 개방형 텍스트 생성 평가에는 한계가 있습니다. 동의어, 문체 다양성, 의미적 정확성을 측정하지 못합니다.

**수식 박스 1**:
- 레이블: `BLEU 점수 (Papineni et al., 2002)`
- 수식:
```
\text{BLEU} = BP \cdot \exp\!\left(\sum_{n=1}^{N} w_n \log p_n\right)
\quad\text{where}\quad BP = \begin{cases} 1 & c \geq r \\ e^{1-r/c} & c < r \end{cases}
```
- 주석:
  - `p_n`: n-gram 정밀도 (생성 텍스트 중 참조 텍스트에 나온 n-gram 비율)
  - `w_n = 1/N`: n-gram 가중치 (보통 N=4로 동일 가중치)
  - `BP`: 짧은 생성에 불이익 주는 Brevity Penalty
  - `c`: 생성 텍스트 길이, `r`: 참조 텍스트 길이

**수식 박스 2**:
- 레이블: `ROUGE-L (Lin, 2004)`
- 수식:
```
\text{ROUGE-L} = \frac{(1+\beta^2) \cdot R_{lcs} \cdot P_{lcs}}{R_{lcs} + \beta^2 P_{lcs}}
\quad\text{where}\quad R_{lcs} = \frac{LCS(X,Y)}{m},\; P_{lcs} = \frac{LCS(X,Y)}{n}
```
- 주석:
  - `LCS(X,Y)`: 참조(X)와 생성(Y) 텍스트의 최장 공통 부분 수열 길이
  - `m`: 참조 텍스트 길이, `n`: 생성 텍스트 길이
  - ROUGE-L: 요약 평가에 자주 사용 (순서까지 고려)
  - β=1.2 (재현율 강조): 요약 분야 표준

**표 (전통 평가 메트릭 비교)**:

| 메트릭 | 장점 | 단점 |
|--------|------|------|
| BLEU | 빠름, 재현 가능 | 의미 무시, 동의어 패널티 |
| ROUGE | 요약에 적합 | BLEU와 유사한 한계 |
| BERTScore | 의미 유사도 반영 | 계산 비용 높음 |
| METEOR | 동의어, 어간 고려 | 언어별 리소스 필요 |

---

### 섹션 2: LLM-as-a-Judge

**아이콘**: `⚖`  
**제목**: `LLM-as-a-Judge — AI가 AI를 평가한다`

**설명**: GPT-4 같은 강력한 LLM을 평가자로 사용하는 방법입니다. 정해진 기준(Rubric)을 프롬프트로 제공하고, LLM이 점수 + 설명을 생성합니다. 인간 평가와의 상관관계가 BLEU보다 훨씬 높습니다.

**단계별 리스트**:
```
Step 1: 평가 기준 정의 (Rubric)
  → 정확성, 유창성, 근거성, 유해성 등 기준 설정
  → 각 기준의 점수 척도 정의 (1-5점 또는 1-10점)

Step 2: 평가 프롬프트 설계
  → "다음 응답을 아래 기준으로 평가하고 점수와 이유를 설명하라"

Step 3: LLM 실행 (GPT-4, Claude 등)
  → 응답 + 루브릭 → 점수 + 설명 출력

Step 4: 집계 & 분석
  → 여러 평가자 실행 → 평균, 분산 계산
```

**개념 카드**:
- 제목: `🔑 LLM-as-a-Judge 편향 유형 (Zheng et al., 2023)`
- 내용:
  - **위치 편향**: 먼저 제시된 응답을 선호 → 응답 순서를 랜덤화해야 함
  - **자기 편향**: 평가자 LLM과 동일 계열 모델에 유리한 점수
  - **길이 편향**: 더 긴 응답을 더 좋다고 판단하는 경향
  - **포맷 편향**: 마크다운, 리스트 형식의 응답에 유리
  - **해결책**: 위치 스왑(A vs B → B vs A), 다수 평가자, Chain-of-Thought 요구

---

### 섹션 3: 비교 평가 — Arena 스타일

**아이콘**: `🏟`  
**제목**: `Chatbot Arena — 절대 점수 대신 상대적 순위`

**설명**: LMSYS Chatbot Arena는 두 모델을 무작위로 A/B 테스트하여 사용자가 더 좋은 응답을 선택하게 합니다. 이 비교 결과로 Elo 점수를 계산하여 모델 순위를 매깁니다.

**수식 박스 3**:
- 레이블: `Elo 레이팅 업데이트`
- 수식:
```
R_A' = R_A + K \cdot (S_A - E_A)
\quad\text{where}\quad E_A = \frac{1}{1 + 10^{(R_B - R_A)/400}}
```
- 주석:
  - `R_A, R_B`: 모델 A, B의 현재 Elo 점수
  - `E_A`: 모델 A가 이길 기대 확률 (로지스틱 함수)
  - `S_A`: 실제 결과 (이기면 1, 비기면 0.5, 지면 0)
  - `K`: 업데이트 속도 (보통 K=4~32)

**callout (tip)**:
- 내용: **현재 LLM 순위 확인**: [LMSYS Chatbot Arena](https://lmarena.ai/) — 수백만 건의 인간 투표로 만들어진 가장 신뢰할 수 있는 LLM 벤치마크. 논문에서 "Arena ELO" 수치가 인용될 때 이 데이터를 의미합니다.

---

### 섹션 4: 평가 지표 선택 가이드

**아이콘**: `🎯`  
**제목**: `연구 목적별 평가 지표 선택`

**표 (태스크별 권장 평가 방법)**:

| 태스크 | 1차 추천 | 2차 추천 | 주의사항 |
|--------|---------|---------|---------|
| 기계 번역 | BLEU, chrF | 인간 평가 | BLEU로만 판단 금지 |
| 요약 | ROUGE-L + BERTScore | LLM-as-a-Judge | 추출 요약과 생성 요약 분리 |
| 질의응답 | Exact Match, F1 | Faithfulness | 오픈/클로즈드 도메인 구분 |
| 일반 대화 | LLM-as-a-Judge | Arena ELO | 평가자 모델 편향 주의 |
| 코드 생성 | 단위 테스트 통과율 | HumanEval | 단위 테스트 누수 주의 |
| RAG | RAGAS (Faithfulness + Relevancy) | 인간 평가 | 검색·생성 따로 평가 |

---

## 인터랙티브: 평가 메트릭 비교기

**제목**: `📊 BLEU vs LLM-Judge — 언제 어떤 메트릭을?`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `eval-bleu` | BLEU 점수 | 0 | 1 | 0.01 | 0.35 |
| `eval-judge` | LLM-Judge 점수 (1-10) | 1 | 10 | 0.5 | 7.5 |
| `eval-human` | 인간 평가 점수 (1-10) | 1 | 10 | 0.5 | 8.0 |

**JavaScript 로직**:
```javascript
function updateEval() {
  const bleu = parseFloat(document.getElementById('eval-bleu').value);
  const judge = parseFloat(document.getElementById('eval-judge').value);
  const human = parseFloat(document.getElementById('eval-human').value);
  document.getElementById('eval-bleu-v').value = bleu.toFixed(2);
  document.getElementById('eval-judge-v').value = judge.toFixed(1);
  document.getElementById('eval-human-v').value = human.toFixed(1);

  const judgeNorm = judge / 10;
  const humanNorm = human / 10;

  const bleuHumanCorr = Math.max(0, 1 - Math.abs(bleu - humanNorm) * 2);
  const judgeHumanCorr = Math.max(0, 1 - Math.abs(judgeNorm - humanNorm) * 1.5);

  const bleuCategory = bleu > 0.5 ? '높음 (기계번역 수준)' : bleu > 0.3 ? '보통' : '낮음 (창의적 생성)';

  document.getElementById('eval-res').innerHTML =
    `BLEU: <strong>${bleu.toFixed(2)}</strong> → ${bleuCategory}<br/>
     LLM-Judge와 인간 평가 차이: <strong>${Math.abs(judgeNorm - humanNorm).toFixed(2)}</strong><br/>
     BLEU-인간 일치도 추정: ${(bleuHumanCorr * 100).toFixed(0)}%<br/>
     LLM-Judge-인간 일치도 추정: ${(judgeHumanCorr * 100).toFixed(0)}%<br/>
     추천 메트릭: ${bleu < 0.3 ? 'LLM-Judge (BLEU가 낮아 불신뢰)' : '두 메트릭 병행 사용'}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       일반적으로 LLM-Judge가 인간 평가와 더 높은 상관관계를 가집니다. (Zheng et al., 2023)
     </span>`;
}
updateEval();
```

---

## 퀴즈

**질문**: LLM-as-a-Judge에서 "위치 편향"이란 무엇인가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 긴 응답에 더 높은 점수를 주는 경향 | ❌ |
| (b) 두 응답 비교 시 먼저 제시된 응답을 선호하는 경향 | ✅ |
| (c) 같은 회사의 모델에 높은 점수를 주는 경향 | ❌ |
| (d) 마크다운 형식의 응답을 선호하는 경향 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 12F)

```
[ ] BLEU 수식의 각 항 (p_n, BP)을 설명할 수 있다
[ ] BLEU가 개방형 생성 평가에 부적합한 이유를 2가지 설명할 수 있다
[ ] LLM-as-a-Judge의 4가지 편향 유형을 설명할 수 있다
[ ] Chatbot Arena의 Elo 점수 계산 방식을 설명할 수 있다
[ ] 주어진 태스크에 적합한 평가 메트릭을 선택할 수 있다
[ ] RAG 평가에서 RAGAS가 필요한 이유를 설명할 수 있다
[ ] [CME295 L8 강의](https://www.youtube.com/watch?v=8fNP4N46RRo&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=8) 시청 완료
```
