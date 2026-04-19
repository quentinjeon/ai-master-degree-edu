# Week 12B — LLM 사전학습·파인튜닝·LoRA (Ch.12B)

> **파일명**: `chapters/ch12b.html`  
> **Phase**: Unit 3 · AI & ML 연결 (고도화)  
> **인터랙티브**: ✅ LoRA 파라미터 효율 계산기  
> **우선순위**: P0  
> **기반 강의**: [Stanford CME295 L4 — LLM Training](https://www.youtube.com/watch?v=VlA_jt_3Qc4&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=4) (1:47:27)  
> **슬라이드**: [fall25-cme295-lecture4.pdf](https://cme295.stanford.edu/slides/fall25-cme295-lecture4.pdf)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.12B · LLM 학습 (Unit 3 고도화)` |
| 제목 | `LLM 사전학습·파인튜닝·LoRA` |
| 서브타이틀 | `ChatGPT가 어떻게 학습되는지 — 사전학습, 지도 파인튜닝(SFT), 그리고 LoRA로 1% 파라미터만 바꿔 성능을 끌어올리는 비결.` |
| 이전 챕터 | `ch12.html` — Ch.12 딥러닝·LLM 기초 |
| 다음 챕터 | `ch12c.html` — Ch.12C RLHF·선호도 학습·DPO |

---

## 📺 강의 레퍼런스

- **Stanford CME295 L4**: [LLM Training (1:47:27)](https://www.youtube.com/watch?v=VlA_jt_3Qc4&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=4)
- **슬라이드**: [강의자료 PDF](https://cme295.stanford.edu/slides/fall25-cme295-lecture4.pdf)

---

## EASY BOX 내용

**핵심 비유**: "요리사를 키우는 과정과 같다. 먼저 수백만 권의 요리책을 읽혀서 '일반 요리 감각'을 익히고(사전학습), 그다음 특정 식당 스타일에 맞게 짧게 추가 훈련한다(파인튜닝). LoRA는 요리책 전체를 새로 쓰는 대신 '핵심 레시피 메모지'만 추가하는 방법이다."

**예시 리스트**:
- GPT-4는 인터넷 전체 텍스트로 사전학습 → 다음 토큰 예측을 수조 번 반복
- SFT: "의사처럼 대화하는 방법"을 수천 개 예시로 추가 학습
- LoRA: 1750억 파라미터 모델을 파인튜닝할 때 단 0.1%만 업데이트 → GPU 비용 1/100
- AI에서: LLaMA 3, Mistral, Gemma 등이 모두 LoRA로 도메인 특화

---

## 섹션 구성

### 섹션 1: 사전학습 — LLM의 기반

**아이콘**: `📚`  
**제목**: `사전학습 — 조 단위 토큰으로 언어 구조 흡수`

**설명**: 사전학습(Pretraining)은 LLM이 특정 태스크 없이 인터넷 텍스트 전체를 학습하는 단계입니다. 목표는 단순합니다: "다음 토큰을 예측하라." 하지만 이 단순한 목표가 수학, 코딩, 논문 독해 능력을 자연스럽게 만들어냅니다.

**수식 박스 1**:
- 레이블: `사전학습 목적함수 — 자기회귀 NLL`
- 수식:
```
\mathcal{L}_{\text{PT}}(\theta) = -\sum_{t=1}^{T} \log P_\theta(w_t \mid w_1, \ldots, w_{t-1})
```
- 주석:
  - `w_t`: t번째 토큰 (단어, 서브워드 단위)
  - `θ`: LLM 파라미터 (GPT-4는 약 1.8T 파라미터로 추정)
  - 모든 위치에서 다음 토큰을 맞히도록 경사하강법으로 최적화
  - 사전학습 후 모델 = "언어를 이해하는 만능 기반" (Foundation Model)

**개념 카드**:
- 제목: `🔑 사전학습 스케일의 비밀`
- 내용:
  - **데이터**: 수조 토큰 (Common Crawl + 책 + 코드 + 논문)
  - **규모**: GPT-3 = 1750억 파라미터, LLaMA 3 = 700억 파라미터
  - **비용**: GPT-4 사전학습 추정 ~$100M 이상
  - **Scaling Law**: 파라미터 수↑, 데이터↑, 컴퓨트↑ → 성능이 **멱법칙**으로 향상 (Chinchilla 최적 비율: 토큰 수 ≈ 파라미터 수 × 20)

---

### 섹션 2: 지도 파인튜닝 (SFT)

**아이콘**: `🎯`  
**제목**: `지도 파인튜닝 — "이렇게 대화해" 가르치기`

**설명**: SFT(Supervised Fine-Tuning)는 사전학습된 모델에게 "올바른 대화 방식"을 가르치는 단계입니다. (질문, 이상적 대답) 쌍을 수천~수만 개 준비하여 같은 NLL 손실로 학습합니다. ChatGPT의 경우 OpenAI 직원들이 직접 작성한 수만 개의 예시 대화를 사용했습니다.

**수식 박스 2**:
- 레이블: `SFT 목적함수`
- 수식:
```
\mathcal{L}_{\text{SFT}}(\theta) = -\sum_{(x,y) \in \mathcal{D}_{\text{SFT}}} \sum_{t=1}^{|y|} \log P_\theta(y_t \mid x, y_{<t})
```
- 주석:
  - `x`: 사용자 질문 (prompt)
  - `y`: 이상적 응답 (response), `y_t`는 t번째 응답 토큰
  - `𝒟_SFT`: (질문, 응답) 쌍으로 구성된 데이터셋
  - 사전학습과 수식은 동일하지만 **응답 부분**의 토큰만 손실 계산

**표 (사전학습 vs SFT 비교)**:

| 항목 | 사전학습 | SFT |
|------|---------|-----|
| 데이터 | 인터넷 전체 (수조 토큰) | 큐레이션된 대화 (수만 쌍) |
| 목적 | 언어 구조 학습 | 지시 따르기 |
| 비용 | 매우 높음 ($M~$B) | 상대적으로 낮음 |
| 효과 | 능력 부여 | 행동 방향 설정 |

---

### 섹션 3: LoRA — 효율적 파인튜닝의 혁명

**아이콘**: `⚡`  
**제목**: `LoRA — 1%만 업데이트해도 충분하다`

**설명**: 전체 파인튜닝은 700억 파라미터를 모두 업데이트하여 엄청난 GPU 메모리가 필요합니다. LoRA(Low-Rank Adaptation)는 가중치 업데이트 행렬 ΔW를 **두 개의 작은 행렬 B·A**로 분해하여, 실제로 업데이트할 파라미터를 99% 줄입니다.

**수식 박스 3**:
- 레이블: `LoRA — 저차원 업데이트 분해 (Hu et al., 2022)`
- 수식:
```
W' = W_0 + \Delta W = W_0 + BA
\quad\text{where}\quad B \in \mathbb{R}^{d \times r},\; A \in \mathbb{R}^{r \times k},\; r \ll \min(d, k)
```
- 주석:
  - `W₀ ∈ ℝ^{d×k}`: 사전학습된 원본 가중치 (고정, 업데이트 안 함)
  - `B, A`: 학습할 작은 행렬. B는 0으로, A는 랜덤으로 초기화
  - `r`: 랭크 (보통 4~64). r이 작을수록 파라미터가 적고 r=k이면 전체 파인튜닝과 동일
  - 파라미터 수: 원본 d×k → LoRA: r×(d+k) → r=8, d=k=4096이면 **약 99.8% 절감**

**수식 박스 4**:
- 레이블: `LoRA 파라미터 효율 계산`
- 수식:
```
\text{절감률} = 1 - \frac{r(d + k)}{dk} \approx 1 - \frac{r}{\min(d,k)} \quad (d \approx k \text{ 일 때})
```
- 주석:
  - r=8, d=k=4096: 절감률 = 1 - 8×8192/(4096²) ≈ 99.6%
  - 실제 LoRA는 Attention의 Q, K, V, O 행렬에만 적용하는 경우가 많음
  - QLoRA: INT4 양자화 + LoRA = 소비자 GPU에서도 70B 모델 파인튜닝 가능

**callout (tip)**:
- 내용: **QLoRA (Dettmers et al., 2023)**: 4비트 양자화(INT4) + LoRA를 결합. 단일 RTX 3090 GPU (24GB VRAM)로도 LLaMA 70B 파인튜닝이 가능하게 만든 혁신. Hugging Face `transformers` + `peft` 라이브러리로 몇 줄의 코드만으로 적용 가능.

---

### 섹션 4: 양자화 — 모델 압축의 수학

**아이콘**: `🗜`  
**제목**: `양자화 — FP32에서 INT4로, 메모리 1/8`

**설명**: LLM의 가중치는 보통 FP32(32비트 부동소수점)로 저장됩니다. 양자화(Quantization)는 이를 INT8(8비트) 또는 INT4(4비트)로 압축하여 메모리와 연산 비용을 줄입니다.

**수식 박스 5**:
- 레이블: `선형 양자화`
- 수식:
```
w_{\text{quant}} = \text{round}\!\left(\frac{w - z}{s}\right)
\quad\text{역양자화:}\quad \hat{w} = s \cdot w_{\text{quant}} + z
```
- 주석:
  - `s` (scale): 양자화 간격 = (max - min) / (2^bits - 1)
  - `z` (zero-point): 0이 어떤 정수에 매핑되는지
  - INT8: 2^8=256 구간 → FP32 대비 4배 압축
  - INT4: 2^4=16 구간 → FP32 대비 8배 압축 (정확도 약간 손실)

---

## 인터랙티브: LoRA 파라미터 효율 계산기

**제목**: `⚡ LoRA 파라미터 절감률 계산기`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `lora-r` | 랭크 r | 1 | 64 | 1 | 8 |
| `lora-d` | 행 차원 d (d_model) | 512 | 8192 | 512 | 4096 |
| `lora-k` | 열 차원 k | 512 | 8192 | 512 | 4096 |

**JavaScript 로직**:
```javascript
function updateLoRA() {
  const r = parseInt(document.getElementById('lora-r').value);
  const d = parseInt(document.getElementById('lora-d').value);
  const k = parseInt(document.getElementById('lora-k').value);
  document.getElementById('lora-r-v').value = r;
  document.getElementById('lora-d-v').value = d;
  document.getElementById('lora-k-v').value = k;

  const fullParams = d * k;
  const loraParams = r * (d + k);
  const savings = ((1 - loraParams / fullParams) * 100).toFixed(2);

  document.getElementById('lora-res').innerHTML =
    `원본 파라미터: <strong>${fullParams.toLocaleString()}</strong> (${d}×${k})<br/>
     LoRA 파라미터: <strong>${loraParams.toLocaleString()}</strong> (r×(d+k) = ${r}×${d+k})<br/>
     절감률: <strong style="color:var(--accent3)">${savings}%</strong><br/>
     메모리 절감: ${(fullParams * 4 / 1e6).toFixed(1)}MB → ${(loraParams * 4 / 1e6).toFixed(1)}MB (FP32 기준)<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       r을 줄이면 파라미터는 줄지만 표현력도 감소. 보통 r=8~16이 좋은 균형점.
     </span>`;
}
updateLoRA();
```

---

## 퀴즈

**질문**: LoRA에서 랭크(r)를 낮추면 어떤 일이 일어나는가?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 학습 파라미터가 증가하고 표현력이 높아진다 | ❌ |
| (b) 학습 파라미터가 감소하고 메모리 효율이 높아지지만 표현력도 줄어든다 | ✅ |
| (c) 사전학습된 가중치 W₀가 업데이트된다 | ❌ |
| (d) 모델의 사전학습 지식이 사라진다 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 12B)

```
[ ] 사전학습이 "다음 토큰 예측"을 통해 이루어짐을 안다
[ ] SFT와 사전학습의 차이 (데이터 규모, 목적)를 설명할 수 있다
[ ] LoRA의 수식 W' = W₀ + BA를 쓰고 각 행렬의 차원을 설명할 수 있다
[ ] LoRA 파라미터 절감률을 계산할 수 있다 (r=8, d=k=4096)
[ ] QLoRA가 왜 소비자 GPU에서 가능한지 설명할 수 있다
[ ] INT8/INT4 양자화의 trade-off를 설명할 수 있다
[ ] [CME295 L4 강의](https://www.youtube.com/watch?v=VlA_jt_3Qc4&list=PLoROMvodv4rOCXd21gf0CF4xr35yINeOy&index=4) 시청 완료
```
