# Week 38 — 데이터 수집·품질·윤리 관리 (Ch.38)

> **파일명**: `chapters/ch38.html`  
> **Phase**: Phase 3-C · 연구 방법론  
> **인터랙티브**: ✅ 데이터 품질 점수 진단기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.38 · 연구 방법론 (Phase 3 — 12주차)` |
| 제목 | `데이터 수집·품질·윤리 관리` |
| 서브타이틀 | `"데이터는 재료가 아니라 연구의 절반이다." 수집 기준, 편향 분석, 라벨 품질, 윤리적 고려, 논문 내 데이터 서술 방법까지.` |
| 이전 챕터 | `ch37.html` — Ch.37 선행연구 구조화 |
| 다음 챕터 | `ch39.html` — Ch.39 방법론 설계 + 논문 글쓰기 |

---

## EASY BOX 내용

**핵심 비유**: "요리사는 재료를 모으는 것만큼 재료의 신선도와 출처를 확인한다. 상한 재료로 만든 음식은 아무리 요리 실력이 좋아도 맛이 없다 — 편향된 데이터로 학습한 모델도 마찬가지다."

**예시 리스트**:
- 좋아 보이는 데이터셋의 함정: 영어 텍스트 90%에 한국어 10% → "다국어" 주장은 편향됨
- 라벨 품질 문제: Amazon 리뷰 데이터에서 스팸 리뷰가 15% 포함 → 감성 분석 결과 오염
- AI에서: SCI 논문에서 "Data Collection" 섹션이 약하면 리뷰어는 실험 전체를 의심한다 — 데이터 서술이 논문 신뢰의 절반

---

## 섹션 구성

### 섹션 1: 데이터 수집 기준 설계

**아이콘**: `🗂`  
**제목**: `데이터 수집 — "어디서, 얼마나, 어떻게" 정하기`

**설명**: 논문 데이터는 "주어지는 것"이 아니라 "설계하는 것"입니다. 수집 기준이 명확해야 실험의 재현성과 공정성이 보장됩니다.

**수식 박스 1**:
- 레이블: `데이터 수집 설계의 5W1H`
- 수식:
```
\text{데이터 수집 설계:}
\begin{cases}
\text{Who}   & \text{누가 생성한 데이터인가? (생산자 특성)} \\
\text{What}  & \text{어떤 형태인가? (텍스트/이미지/표/수치)} \\
\text{When}  & \text{언제 수집됐는가? (시간적 분포)} \\
\text{Where} & \text{어디서 왔는가? (출처, 플랫폼)} \\
\text{Why}   & \text{왜 이 데이터가 연구 질문에 맞는가?} \\
\text{How}   & \text{어떻게 수집됐는가? (API/크롤링/설문/기존 데이터셋)}
\end{cases}
```
- 주석:
  - 논문 "Dataset" 섹션은 이 6가지를 모두 서술해야 함
  - Why가 가장 중요: 데이터 선택이 연구 질문과 aligned됨을 보여야 함
  - How를 빠뜨리면 리뷰어가 "데이터 누수 가능성"을 의심

**callout (warn)**:
- 내용: **데이터셋 누수(Data Leakage)**는 ML 논문에서 가장 치명적인 오류 중 하나입니다. 테스트 데이터에 있는 정보가 훈련 데이터에 포함되면 결과가 과대평가됩니다. 시계열 데이터에서 미래 데이터로 과거를 예측하거나, 데이터 정규화를 전체 데이터셋에 적용하면 발생합니다.

---

### 섹션 2: 데이터 편향 분석

**아이콘**: `⚖`  
**제목**: `데이터 편향 — 숨겨진 가정을 찾아내라`

**설명**: 편향이 없는 데이터는 존재하지 않습니다. 중요한 것은 편향을 숨기는 것이 아니라 분석하고 서술하는 것입니다. 리뷰어는 편향을 인정하고 분석한 논문을 더 신뢰합니다.

**표 (데이터 편향의 주요 유형)**:

| 편향 유형 | 설명 | 예시 | 완화 방법 |
|---------|------|------|---------|
| 선택 편향 | 특정 그룹만 표본에 포함 | 모바일 사용자만 수집 | 층화 샘플링 |
| 생존자 편향 | 성공한 사례만 분석 | 출시된 앱만 연구 | 실패 사례 포함 |
| 라벨 편향 | 주석자(annotator)의 편견 | 문화적 감성 차이 | 다수 주석자, κ 계수 |
| 측정 편향 | 측정 방식 자체의 편향 | 자기보고 설문 | 객관적 지표 병행 |
| 시간 편향 | 특정 시기의 데이터만 | 코로나 이전 데이터 | 시간 범위 명시 |
| 확인 편향 | 가설에 맞는 데이터만 선택 | 긍정 사례만 분석 | 부정 사례 명시 |

**수식 박스 2**:
- 레이블: `라벨 품질 측정 — Cohen's κ`
- 수식:
```
\kappa = \frac{p_o - p_e}{1 - p_e}
\quad\text{where}\quad
p_o = \text{관측 일치율},\quad p_e = \text{우연 일치 기대율}
```
- 주석:
  - κ > 0.8: 우수한 라벨 일치도
  - κ 0.6~0.8: 양호
  - κ < 0.6: 라벨 품질 낮음 → 재작업 필요
  - 논문에서 사람 주석 데이터를 쓰면 κ 값을 반드시 보고

---

### 섹션 3: 데이터 품질 관리 체크리스트

**아이콘**: `✅`  
**제목**: `데이터 정제 — 논문 수준의 데이터 품질 기준`

**설명**: "원본 데이터를 그냥 사용"은 논문에서 허용되지 않습니다. 어떤 기준으로 정제했는지 명확히 기술해야 합니다.

**표 (데이터 품질 체크리스트)**:

| 항목 | 확인 질문 | 논문 서술 방법 |
|------|---------|--------------|
| 중복 제거 | 완전 중복, 거의 같은 샘플 제거? | "We remove near-duplicate samples using MinHash (threshold=0.85)" |
| 결측치 | 결측 비율과 처리 방식? | "Samples with missing labels were excluded (n=143, 2.1%)" |
| 클래스 불균형 | 클래스별 분포 확인? | "Class distribution: positive 34%, negative 66%" |
| 외부값(outlier) | 통계적 이상값 처리? | "Samples outside 3σ range were flagged and manually reviewed" |
| 전처리 명시 | 토크나이저, 정규화 방식? | "Text normalized using unicode NFKC, tokenized with BPE (vocab=32K)" |
| train/val/test 분리 | 무작위? 층화? 시간 순? | "Stratified split: 70/15/15 maintaining class distribution" |

**수식 박스 3**:
- 레이블: `클래스 불균형 — Imbalance Ratio와 대응 전략`
- 수식:
```
\text{Imbalance Ratio (IR)} = \frac{N_{\text{majority}}}{N_{\text{minority}}}
\quad \Rightarrow \quad
\text{IR} > 10: \text{심각한 불균형} \to \text{oversampling/undersampling/class weight}
```
- 주석:
  - IR > 10이면 단순 accuracy가 의미 없음 → F1, AUROC 사용
  - SMOTE: 소수 클래스 오버샘플링 (합성 데이터 생성)
  - Class weight: loss function에 클래스 비율 역수로 가중치 부여
  - 논문에서 클래스 불균형 무시 → 리뷰어의 강력한 의문

---

### 섹션 4: 데이터 윤리와 논문 서술

**아이콘**: `⚡`  
**제목**: `데이터 윤리 — SCI 논문에서 필수 서술 항목`

**설명**: 현대 SCI 저널·학회는 데이터 윤리 서술을 점점 강하게 요구합니다. 개인정보, 동의, 저작권, 공정성을 서술하지 않으면 reject 사유가 됩니다.

**수식 박스 4**:
- 레이블: `데이터 윤리 서술 체크리스트`
- 수식:
```
\text{윤리 서술 필수 항목:}
\begin{cases}
\text{개인정보}  & \text{익명화/가명화 여부} \\
\text{동의}      & \text{IRB 승인/사용자 동의 여부} \\
\text{저작권}    & \text{공개 라이선스 확인} \\
\text{공정성}    & \text{인구통계 편향 분석} \\
\text{해악 가능성} & \text{오용 시나리오와 대응}
\end{cases}
```
- 주석:
  - IRB (Institutional Review Board): 인체 관련 연구의 필수 심사
  - 공개 데이터셋도 라이선스 확인 필요 (CC BY vs CC BY-NC vs 상업 불가)
  - NeurIPS/ICML은 "Broader Impact" 서술 의무화
  - 의료·금융·법률 데이터: 특히 강한 윤리 기준 적용

**callout (info)**:
- 내용: **논문에서 데이터 한계를 먼저 서술하면 더 강해진다.** 리뷰어가 "이 데이터에는 X 한계가 있다"고 지적하기 전에 저자가 먼저 인정하고 해결 방향을 제시하면, 리뷰어는 "연구자가 자기 데이터의 약점을 알고 있구나"고 신뢰합니다.

---

## 인터랙티브: 데이터 품질 진단기

**제목**: `🔢 데이터 품질 점수 — SCI 논문 수준 진단`

**JavaScript 로직**:
```javascript
const dataItems = [
  { id: 'd1', text: '데이터 출처가 명확히 문서화됨 (5W1H)', weight: 10 },
  { id: 'd2', text: '중복 샘플 제거 완료 (유사도 임계값 명시)', weight: 10 },
  { id: 'd3', text: '결측치 처리 방식과 비율 기록', weight: 8 },
  { id: 'd4', text: '클래스 분포 분석 및 불균형 대응', weight: 10 },
  { id: 'd5', text: '라벨 품질 검증 (κ 계수 또는 다수결)', weight: 12 },
  { id: 'd6', text: '전처리 파이프라인 완전 문서화', weight: 10 },
  { id: 'd7', text: 'train/val/test 분리 기준 명시', weight: 10 },
  { id: 'd8', text: '데이터 누수(leakage) 확인 완료', weight: 15 },
  { id: 'd9', text: '윤리 서술 (IRB/동의/개인정보) 포함', weight: 10 },
  { id: 'd10', text: '데이터 한계를 논문에 명시적으로 서술', weight: 5 },
];

function buildDataItems() {
  const container = document.getElementById('data-container');
  dataItems.forEach(c => {
    const div = document.createElement('div');
    div.style.cssText = 'display:flex;align-items:center;gap:8px;margin:6px 0;';
    div.innerHTML = `<input type="checkbox" id="${c.id}" onchange="calcDataScore()">
                     <label for="${c.id}">${c.text} <span style="color:var(--muted);font-size:.8rem">(${c.weight}점)</span></label>`;
    container.appendChild(div);
  });
}

function calcDataScore() {
  let score = 0;
  dataItems.forEach(c => { if (document.getElementById(c.id)?.checked) score += c.weight; });
  const level = score < 40 ? '❌ 부족 (데이터 섹션으로 reject 가능)' :
                score < 65 ? '⚠️ 보통 (major revision 수준)' :
                score < 85 ? '🔶 양호 (minor revision 수준)' :
                             '✅ 우수 (SCI 데이터 기준 충족)';
  document.getElementById('data-result').innerHTML =
    `데이터 품질 점수: <strong>${score}점</strong> / 100점<br/>${level}`;
}

buildDataItems();
calcDataScore();
```

---

## 퀴즈

**질문**: 논문에서 데이터 편향과 한계를 먼저 서술하는 것이 유리한 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 논문 분량을 채울 수 있기 때문이다 | ❌ |
| (b) 리뷰어가 지적하기 전에 인정하고 대응 방향을 제시하면 연구자의 신뢰성을 높인다 | ✅ |
| (c) 데이터 한계가 없다는 것을 증명하기 때문이다 | ❌ |
| (d) 윤리 심사를 면제받을 수 있기 때문이다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 38)

```
[ ] 데이터 수집 설계를 5W1H로 정리할 수 있다
[ ] 데이터 편향의 6가지 유형을 설명하고 자신의 데이터에 적용할 수 있다
[ ] Cohen's κ로 라벨 품질을 측정하고 해석할 수 있다
[ ] 데이터 품질 체크리스트 10항목을 논문 수준으로 수행할 수 있다
[ ] 클래스 불균형을 정량화하고 대응 전략을 선택할 수 있다
[ ] 논문의 데이터 윤리 서술 5가지 항목을 작성할 수 있다
[ ] 자신의 데이터셋에 대한 "Data" 섹션 초안을 작성할 수 있다
```
