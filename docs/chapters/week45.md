# Week 45 — LM 구현 실습: 토크나이저·FLOPs·FlashAttention·Scaling Laws (Ch.45)

> **파일명**: `chapters/ch45.html`  
> **Phase**: Phase 3-D · LLM 심화 이론 (Unit 5 확장)  
> **인터랙티브**: ✅ Transformer FLOPs·메모리 계산기  
> **우선순위**: P2  
> **전제 챕터**: Ch.31 (Transformer Attention), Ch.41 (구현·재현성), Ch.12B (사전학습)  
> **기반 강의**:
> - [CS336 L1 — 개요·토크나이저](https://youtu.be/JuoVZkPBiKk) (Percy Liang)
> - [CS336 L2 — PyTorch·FLOPs·메모리](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) (Percy Liang)
> - [CS336 L6 — 커널·FlashAttention](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) (Percy Liang)
> - [CS336 L9/L11 — Scaling Laws](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) (Tatsu Hashimoto)

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.45 · LM 구현 (Phase 3-D)` |
| 제목 | `언어 모델 구현 실습 — 토크나이저·FLOPs·FlashAttention·Scaling Laws` |
| 서브타이틀 | `ChatGPT를 처음부터 만든다면 어디서부터 시작하는가 — BPE 토크나이저, Transformer 연산량 분석, FlashAttention I/O 최적화, Chinchilla 스케일링 법칙까지. Stanford CS336 "Language Modeling from Scratch"의 핵심.` |
| 이전 챕터 | `ch44.html` — Ch.44 LLM Alignment 이론 |
| 다음 챕터 | `index.html` — 홈 |

---

## 📺 강의 레퍼런스

- **CS336 L1**: [Overview, Tokenization](https://youtu.be/JuoVZkPBiKk) — Percy Liang (Stanford)
- **CS336 L2**: [PyTorch, FLOPs, Memory, Arithmetic Intensity](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)
- **CS336 L6**: [Kernels, Triton, FlashAttention](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)
- **CS336 L9+L11**: [Scaling Laws](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)
- **공식 사이트**: [cs336.stanford.edu](https://cs336.stanford.edu/)

---

## EASY BOX 내용

**핵심 비유**: "LEGO처럼 언어 모델을 직접 쌓아본다. 설명서(Transformer 논문)를 보고 브릭(토크나이저→어텐션→FFN→학습루프)을 하나씩 조립하면서 '이 브릭이 왜 필요한가'를 손으로 느끼는 것이 CS336의 철학이다."

**예시 리스트**:
- BPE 토크나이저: "hello world" → ["hell", "o", " world"] → [1234, 567, 8901] 숫자 시퀀스로
- FLOPs 계산: 7B 모델을 100B 토큰으로 학습하면 약 2×7B×100B = 1.4×10²¹ FLOPs 필요
- FlashAttention: 표준 Attention은 HBM 읽기·쓰기 O(N²) → Flash는 SRAM 타일링으로 10배 빠름
- AI에서: CS336 Assignment 1 = 실제 LLaMA 수준의 Transformer를 Python으로 구현

---

## 섹션 구성

### 섹션 1: BPE 토크나이저 — 텍스트에서 숫자로

**아이콘**: `🔤`  
**제목**: `BPE 토크나이저 — 언어 모델의 입력 단계`

**설명**: 모든 언어 모델은 텍스트를 직접 처리하지 않습니다. BPE(Byte Pair Encoding)는 자주 등장하는 문자 쌍을 반복적으로 합쳐 어휘집(vocabulary)을 만드는 알고리즘입니다. GPT-4는 약 100K 크기의 BPE 어휘집을 사용합니다.

**수식 박스 1**:
- 레이블: `BPE 알고리즘 (Sennrich et al., 2016)`
- 수식:
```
\text{merge}(a, b) \quad\text{s.t.}\quad
\text{freq}(ab) = \max_{\text{pair } (x,y)} \text{freq}(xy)
\quad\text{(반복 적용)}
```
- 주석:
  - 초기 어휘: 모든 개별 바이트 (256개)
  - 매 단계: 가장 자주 붙어 나오는 쌍 (a, b)를 새 토큰 ab로 합침
  - V번 반복 → 어휘 크기 = 256 + V (GPT-4: V ≈ 100,000)
  - 한국어: 글자 단위로 분리 후 BPE → "안녕하세요" = ["안녕", "하세요"] 등

**개념 카드**:
- 제목: `🔑 토크나이저 비교`
- 내용:
  - **BPE** (GPT, LLaMA): 빈도 기반 합치기, 언어 무관
  - **WordPiece** (BERT): 우도 기반 합치기, 서브워드 분리 특화
  - **SentencePiece** (T5, Gemma): BPE + WordPiece의 통합, raw text 처리
  - **Tiktoken** (OpenAI): BPE 기반, 매우 빠른 구현 (Rust 백엔드)

---

### 섹션 2: Transformer FLOPs·메모리 계산

**아이콘**: `⚡`  
**제목**: `LLM 연산량 분석 — 학습에 얼마나 걸리는가`

**설명**: "7B 파라미터 모델을 1조 토큰으로 학습하면 몇 시간이 걸리는가?" 이 질문에 답하려면 FLOPs(Floating Point Operations) 계산이 필수입니다. CS336 L2에서 Percy Liang이 강조하는 "Arithmetic Intensity" 분석의 핵심입니다.

**수식 박스 2**:
- 레이블: `Transformer 학습 총 FLOPs 근사 (Kaplan et al., 2020)`
- 수식:
```
C \approx 6ND
\quad\text{where}\quad N = \text{파라미터 수},\; D = \text{학습 토큰 수}
```
- 주석:
  - 계수 6의 유래: 순전파 2×ND + 역전파 4×ND ≈ 6ND (더 정확한 계산 가능)
  - 예시: GPT-3 (N=175B, D=300B) → C ≈ 6×175B×300B ≈ 3.1×10²³ FLOPs
  - A100 GPU: 312 TFLOP/s → GPT-3 학습 ≈ 355일 (단일 GPU)
  - 실제: A100 1,024개 사용 → 약 3.5개월

**수식 박스 3**:
- 레이블: `Transformer 레이어당 FLOPs (정밀 계산)`
- 수식:
```
\text{FLOPs/token} \approx 2 \cdot \underbrace{4d_{\text{model}}^2}_{\text{Attention QKV+O}} 
+ 2 \cdot \underbrace{2 \cdot 4d_{\text{model}}^2}_{\text{FFN (4× 확장)}} 
= 24 d_{\text{model}}^2 \text{ per layer}
```
- 주석:
  - `d_model`: 모델 차원 (LLaMA 3 8B: 4096)
  - Attention: Q, K, V 투영 (3×d²) + O 투영 (d²) = 4d²
  - FFN: 2개 선형 변환, 내부 차원 = 4×d_model → 2×4d² × 2 = 16d²
  - L레이어 전체: ≈ 24Ld² × 시퀀스 길이

**표 (주요 LLM FLOPs 비교)**:

| 모델 | N (파라미터) | D (토큰) | C (FLOPs) | GPU×시간 추정 |
|------|----------|---------|---------|------------|
| GPT-3 | 175B | 300B | 3.1×10²³ | A100 × 1024 × ~100일 |
| LLaMA 3 8B | 8B | 15T | 7.2×10²³ | H100 × 512 × ~30일 |
| LLaMA 3 70B | 70B | 15T | 6.3×10²⁴ | H100 × 2048 × ~60일 |

---

### 섹션 3: FlashAttention — I/O 최적화의 혁명

**아이콘**: `⚡`  
**제목**: `FlashAttention — 느린 메모리 읽기를 최소화하는 기법`

**설명**: 표준 Self-Attention은 N×N 크기의 Attention 행렬을 HBM(GPU DRAM)에 저장합니다. 시퀀스 길이 N이 길어지면 이 행렬이 수 GB에 달해 I/O 병목이 생깁니다. FlashAttention(Dao et al., 2022)은 이를 SRAM 내부에서 타일링하여 처리합니다.

**수식 박스 4**:
- 레이블: `표준 Attention vs FlashAttention I/O 복잡도`
- 수식:
```
\text{표준 Attention HBM 접근}: \quad O(N^2 d)
\quad\Rightarrow\quad
\text{FlashAttention HBM 접근}: \quad O\!\left(\frac{N^2 d}{M}\right)
```
- 주석:
  - `N`: 시퀀스 길이, `d`: 헤드 차원, `M`: SRAM 크기 (보통 수십 MB)
  - 표준: 중간 N×N 행렬을 HBM에 저장·읽기 반복
  - Flash: SRAM에 타일 단위로 로드 → 계산 → HBM에 최종 결과만 기록
  - 속도 향상: 2~4× (N이 클수록 더 큰 효과)
  - FlashAttention2 (2023): 더 효율적인 타일링, GPU 활용률 ↑

**callout (tip)**:
- 내용: **Arithmetic Intensity**: `FLOPs / (HBM 접근 bytes)` — 높을수록 메모리 병목 없이 연산만 함. MatMul: ~200 FLOP/byte (컴퓨트 바운드). Attention: ~3 FLOP/byte (메모리 바운드). FlashAttention은 타일링으로 Attention을 컴퓨트 바운드에 가깝게 만듦.

---

### 섹션 4: Scaling Laws — 얼마나 키워야 최적인가

**아이콘**: `📈`  
**제목**: `Chinchilla Scaling Laws — 최적 파라미터·토큰 비율`

**설명**: Kaplan et al.(2020)의 초기 스케일링 법칙은 "컴퓨트 고정 시 모델을 크게"를 추천했지만, Hoffmann et al.(2022) Chinchilla 논문이 "파라미터 수와 학습 토큰 수를 동시에 증가"시켜야 함을 보였습니다.

**수식 박스 5**:
- 레이블: `신경 언어 모델 손실 스케일링 (Kaplan et al., 2020)`
- 수식:
```
L(N, D) = \left(\frac{N_c}{N}\right)^{\alpha_N} + \left(\frac{D_c}{D}\right)^{\alpha_D} + L_\infty
```
- 주석:
  - `L(N, D)`: 파라미터 N, 학습 토큰 D에서의 검증 손실
  - `α_N ≈ 0.076, α_D ≈ 0.095`: 멱법칙 지수 (데이터 스케일이 약간 더 중요)
  - `L_∞`: 데이터·모델이 무한히 커도 사라지지 않는 하한 (데이터의 엔트로피)
  - `N_c, D_c`: 적합 상수

**수식 박스 6**:
- 레이블: `Chinchilla 최적 스케일링 (Hoffmann et al., 2022)`
- 수식:
```
N^* = \left(\frac{C}{6 a}\right)^{0.5},\quad D^* = \left(\frac{C}{6 b}\right)^{0.5}
\quad\Rightarrow\quad D^* \approx 20 \cdot N^*
```
- 주석:
  - `C = 6ND`: 컴퓨트 예산, `a, b`: 스케일링 계수
  - 핵심: 최적 토큰 수 = 파라미터 수 × 20 (Chinchilla 비율)
  - LLaMA 3 8B: Chinchilla 비율보다 훨씬 많은 15T 토큰 학습 → 추론 효율 최대화
  - 결론: 추론 비용 고려 시 "작은 모델 + 더 많은 데이터"가 이득

---

## 인터랙티브: Transformer FLOPs·메모리 계산기

**제목**: `⚡ LLM 학습 비용 계산기 — 파라미터·토큰·GPU`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `cs336-n` | 파라미터 수 (억) | 1 | 700 | 1 | 70 |
| `cs336-d` | 학습 토큰 (B) | 1 | 15000 | 10 | 1000 |
| `cs336-gpu-flops` | GPU TFLOP/s | 100 | 3000 | 100 | 1000 |
| `cs336-gpu-count` | GPU 수 | 1 | 4096 | 1 | 64 |

**JavaScript 로직**:
```javascript
function updateCS336() {
  const nB = parseInt(document.getElementById('cs336-n').value) * 1e9;
  const dB = parseInt(document.getElementById('cs336-d').value) * 1e9;
  const gpuFlops = parseInt(document.getElementById('cs336-gpu-flops').value) * 1e12;
  const gpuCount = parseInt(document.getElementById('cs336-gpu-count').value);
  document.getElementById('cs336-n-v').value = (nB/1e9).toFixed(0);
  document.getElementById('cs336-d-v').value = (dB/1e9).toFixed(0);
  document.getElementById('cs336-gpu-flops-v').value = (gpuFlops/1e12).toFixed(0);
  document.getElementById('cs336-gpu-count-v').value = gpuCount;

  const totalFlops = 6 * nB * dB;
  const totalGpuFlops = gpuFlops * gpuCount * 0.35; // MFU 35% 가정
  const trainDays = totalFlops / totalGpuFlops / 86400;

  // Chinchilla 최적 토큰 비율
  const chinchillaD = nB * 20;
  const chinchillaRatio = (dB / chinchillaD).toFixed(2);
  const chinchillaStatus = dB >= chinchillaD * 0.9 && dB <= chinchillaD * 1.1 
    ? '✅ 최적 범위' : dB < chinchillaD * 0.9 
    ? '⚠️ 데이터 부족 (학습 불충분)' : '📈 데이터 풍부 (추론 효율 최적화)';

  document.getElementById('cs336-res').innerHTML =
    `총 학습 FLOPs: <strong>${(totalFlops/1e21).toFixed(2)} × 10²¹</strong><br/>
     유효 GPU 연산: ${(totalGpuFlops/1e12).toFixed(0)} TFLOP/s × ${gpuCount}개 (MFU 35%)<br/>
     학습 소요 시간: <strong style="color:var(--accent3)">${trainDays.toFixed(1)}일</strong><br/>
     Chinchilla 최적 토큰: ${(chinchillaD/1e9).toFixed(0)}B 토큰<br/>
     현재 비율: ${(dB/1e9).toFixed(0)}B / ${(chinchillaD/1e9).toFixed(0)}B = ${chinchillaRatio}× → ${chinchillaStatus}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       MFU(Model FLOP Utilization) 35%: A100/H100 실제 효율 25~40% 가정.<br/>
       Chinchilla 비율: D* ≈ 20×N. 추론 배포용은 이 비율보다 더 많이 학습.
     </span>`;
}
updateCS336();
```

---

## 퀴즈

**질문**: FlashAttention이 표준 Attention보다 빠른 핵심 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) Attention 연산량(FLOPs)을 줄여서 | ❌ |
| (b) N×N Attention 행렬을 HBM에 저장하지 않고 SRAM 내 타일링으로 I/O를 줄여서 | ✅ |
| (c) Multi-Head 수를 줄여서 | ❌ |
| (d) 정밀도를 INT8로 낮춰서 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 45)

```
[ ] BPE 알고리즘 3단계 (초기화 → 빈도 계산 → 합치기)를 설명할 수 있다
[ ] Transformer 학습 총 FLOPs ≈ 6ND 공식을 설명하고 적용할 수 있다
[ ] Arithmetic Intensity 개념과 메모리 병목의 관계를 설명할 수 있다
[ ] FlashAttention이 표준 Attention보다 빠른 이유를 I/O 관점에서 설명할 수 있다
[ ] Chinchilla 스케일링 법칙에서 D* ≈ 20N의 의미를 설명할 수 있다
[ ] [CS336 L1 강의](https://youtu.be/JuoVZkPBiKk) 시청 완료
[ ] [CS336 플레이리스트](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) L2, L6, L9, L11 시청 완료
```
