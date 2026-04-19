# Week 25 — SVD 완전 유도 (Ch.25)

> **파일명**: `chapters/ch25.html`  
> **Phase**: Phase 2-D · 선형대수 심화  
> **인터랙티브**: ✅ SVD 저랭크 근사 시각화  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.25 · 선형대수 심화 (Phase 2 — 11주차)` |
| 제목 | `SVD 완전 유도 — 저랭크 근사와 LoRA` |
| 서브타이틀 | `모든 행렬을 3개의 행렬로 분해합니다. 차원 축소, 추천 시스템, LoRA의 수학적 기반.` |
| 이전 챕터 | `ch24.html` — Ch.24 고유값 분해 |
| 다음 챕터 | `ch26.html` — Ch.26 행렬 미분 |

---

## EASY BOX 내용

**핵심 비유**: "사진을 JPEG로 압축할 때 중요한 정보만 남기고 나머지를 버린다. SVD는 행렬에서 '중요한 방향과 크기'를 내림차순으로 뽑아내어, 상위 k개만 남기면 원본의 근사가 된다."

**예시 리스트**:
- 1000×1000 이미지 행렬을 SVD하면 상위 50개 성분만으로도 90% 이상 복원 가능
- 추천 시스템: 유저×영화 행렬을 SVD로 분해 → 숨겨진 선호도(잠재 요인) 발굴
- AI에서: LoRA는 대형 모델의 가중치 행렬을 저랭크 행렬 두 개(AB)로 근사하여 파라미터를 99% 이상 줄인다

---

## 섹션 구성

### 섹션 1: SVD 정의와 기하학적 의미

**아이콘**: `Σ`  
**제목**: `특이값 분해 (Singular Value Decomposition)`

**설명**: 고유값 분해(A = VΛV⁻¹)는 정방 행렬에만 적용되지만, SVD는 임의의 m×n 행렬에 적용됩니다. 어떤 행렬도 "회전 × 스케일 × 회전"으로 분해됩니다.

**수식 박스 1**:
- 레이블: `SVD 분해`
- 수식:
```
A = U \Sigma V^\top
\quad\text{where}\quad
\begin{cases}
U \in \mathbb{R}^{m \times m}: & \text{좌 특이벡터 (입력 공간 정규직교 기저)} \\
\Sigma \in \mathbb{R}^{m \times n}: & \text{특이값 대각 행렬 } \sigma_1 \geq \sigma_2 \geq \cdots \geq 0 \\
V \in \mathbb{R}^{n \times n}: & \text{우 특이벡터 (출력 공간 정규직교 기저)}
\end{cases}
```
- 주석:
  - U, V: 직교 행렬 (UᵀU = I, VᵀV = I)
  - Σ: 대각 원소 σᵢ가 "특이값" (크기 내림차순 정렬)
  - σᵢ = √λᵢ(AᵀA) — AᵀA의 고유값의 제곱근
  - 기하학: A가 하는 일 = V가 회전 → Σ가 스케일 → U가 다시 회전

**수식 박스 2** (SVD와 고유값 분해 연결):
- 레이블: `SVD를 고유값 분해로 유도`
- 수식:
```
A^\top A = V \Sigma^\top U^\top U \Sigma V^\top = V \Sigma^2 V^\top
\quad\Rightarrow\quad \sigma_i = \sqrt{\lambda_i(A^\top A)}
```
- 주석:
  - AᵀA는 n×n 대칭 양반정치 행렬 → 스펙트럼 정리 적용 가능
  - V의 열 = AᵀA의 고유벡터 (우 특이벡터)
  - U의 열 = Avᵢ / σᵢ (좌 특이벡터)

---

### 섹션 2: Eckart-Young 정리 — 최적 저랭크 근사

**아이콘**: `k`  
**제목**: `Eckart-Young 정리 — 상위 k 성분이 최적 근사`

**설명**: SVD의 가장 강력한 성질입니다. 상위 k개의 특이값/벡터만 사용한 근사가 프로베니우스 노름(Frobenius norm) 기준으로 최적의 랭크-k 근사임을 보장합니다.

**수식 박스 3**:
- 레이블: `Eckart-Young 정리`
- 수식:
```
A_k = \sum_{i=1}^{k} \sigma_i \mathbf{u}_i \mathbf{v}_i^\top
= U_k \Sigma_k V_k^\top
\quad\Rightarrow\quad
A_k = \underset{\mathrm{rank}(B) \leq k}{\arg\min} \|A - B\|_F
```
- 주석:
  - `Aₖ`: 상위 k개 특이값·벡터로 구성한 랭크-k 근사 행렬
  - `‖A - Aₖ‖_F² = σₖ₊₁² + ... + σᵣ²` — 제거된 특이값들의 제곱합
  - 랭크-k 행렬 중 A를 가장 잘 근사하는 것 = Aₖ
  - 이 정리가 PCA, SVD 압축, LoRA의 이론적 보장

---

### 섹션 3: 응용 1 — 행렬 완성 (추천 시스템)

**아이콘**: `🎬`  
**제목**: `행렬 완성 — 넷플릭스 알고리즘`

**수식 박스 4**:
- 레이블: `저랭크 행렬 완성 (Matrix Completion)`
- 수식:
```
\min_{U, V} \sum_{(i,j) \in \Omega} (M_{ij} - [\mathbf{u}_i^\top \mathbf{v}_j])^2 + \lambda(\|U\|_F^2 + \|V\|_F^2)
```
- 주석:
  - `Ω`: 관측된 (유저, 영화) 쌍
  - `uᵢ`: 유저 i의 잠재 요인 벡터 (k차원)
  - `vⱼ`: 영화 j의 잠재 요인 벡터 (k차원)
  - `uᵢᵀvⱼ`: 예측 평점 (내적)
  - 관측되지 않은 항목의 평점을 저랭크 분해로 예측

---

### 섹션 4: 응용 2 — LoRA (Large Language Model Fine-tuning)

**아이콘**: `🤖`  
**제목**: `LoRA — SVD 기반 효율적 파인튜닝`

**설명**: 거대 언어모델을 파인튜닝할 때 전체 가중치(수십억 파라미터)를 업데이트하는 대신, 변화량 ΔW를 저랭크로 근사합니다.

**수식 박스 5**:
- 레이블: `LoRA (Low-Rank Adaptation)`
- 수식:
```
W' = W_0 + \Delta W = W_0 + BA
\quad\text{where}\quad B \in \mathbb{R}^{d \times r},\ A \in \mathbb{R}^{r \times d},\ r \ll d
```
- 주석:
  - `W₀`: 원래 사전학습 가중치 (고정, 학습 안 함)
  - `ΔW = BA`: 변화량을 랭크-r 행렬로 분해 (r ≪ d)
  - 학습 파라미터: B와 A만 업데이트 → (r×d + d×r) 개 = 2rd 개
  - 기존 파라미터 d²에서 2rd로 감소: r=4, d=4096이면 99.8% 감소
  - 이론적 근거: Eckart-Young — 충분히 좋은 랭크-r 근사 존재

---

## 인터랙티브: SVD 저랭크 근사 오차 계산기

**제목**: `🔢 랭크-k 근사 오차 — 상위 k개 특이값만 사용`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `svd-k` | 사용 랭크 k | 1 | 5 | 1 | 2 |

**JavaScript 로직**:
```javascript
function updateSVD() {
  const k = parseInt(document.getElementById('svd-k').value);
  document.getElementById('svd-k-v').value = k;

  // 예시: 특이값 [10, 5, 2, 1, 0.5] (5×5 행렬)
  const sigmas = [10, 5, 2, 1, 0.5];
  const totalVar = sigmas.reduce((s, v) => s + v * v, 0);
  const usedVar  = sigmas.slice(0, k).reduce((s, v) => s + v * v, 0);
  const errVar   = sigmas.slice(k).reduce((s, v) => s + v * v, 0);
  const explainedRatio = (usedVar / totalVar * 100).toFixed(1);
  const errorNorm = Math.sqrt(errVar).toFixed(4);

  const terms = sigmas.slice(0, k).map((s, i) => `σ${i+1}=${s}`).join(', ');

  document.getElementById('svd-res').innerHTML =
    `특이값: [${sigmas.join(', ')}]<br/>
     랭크-${k} 근사 사용: ${terms}<br/>
     설명 분산 비율: <strong>${explainedRatio}%</strong><br/>
     근사 오차 ‖A - Aₖ‖_F = √(σ${k+1}² + ...) = <strong>${errorNorm}</strong><br/>
     <span style="font-size:.78rem;color:var(--muted)">
       k를 늘릴수록 오차 감소, 설명 분산 증가
     </span>`;
}
updateSVD();
```

---

## 퀴즈

**질문**: Eckart-Young 정리가 보장하는 것은?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) SVD는 항상 유일하다 | ❌ |
| (b) 랭크-k 행렬 중 SVD의 상위 k성분 Aₖ가 Frobenius 노름 최적 근사 | ✅ |
| (c) 랭크-k 근사 오차는 항상 0이다 | ❌ |
| (d) 모든 행렬은 SVD로 역행렬을 계산할 수 있다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 25)

```
[ ] A = UΣVᵀ의 각 행렬(U, Σ, V)의 의미를 설명할 수 있다
[ ] 특이값이 고유값과 어떻게 연결되는지 설명할 수 있다
[ ] Eckart-Young 정리를 수식과 함께 설명할 수 있다
[ ] SVD로 랭크-k 근사 오차를 계산할 수 있다
[ ] LoRA가 SVD와 어떻게 연결되는지 설명할 수 있다
[ ] 논문에서 "low-rank decomposition"을 보면 어떤 수학인지 안다
[ ] PCA와 SVD의 관계를 설명할 수 있다 (공분산의 SVD = PCA)
```
