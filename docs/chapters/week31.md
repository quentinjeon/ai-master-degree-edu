# Week 31 — Transformer Attention 완전 유도 (Ch.31)

> **파일명**: `chapters/ch31.html`  
> **Phase**: Phase 3-B · 딥러닝 이론 심화  
> **인터랙티브**: ✅ Attention 가중치 계산기  
> **우선순위**: P0

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.31 · 딥러닝 이론 심화 (Phase 3 — 5주차)` |
| 제목 | `Transformer Attention 완전 유도` |
| 서브타이틀 | `Scaled Dot-Product Attention, Multi-Head, Positional Encoding의 수학. "Attention is All You Need" 논문을 수식 수준으로 읽습니다.` |
| 이전 챕터 | `ch30.html` — Ch.30 딥러닝 일반화 미스터리 |
| 다음 챕터 | `ch32.html` — Ch.32 정보 기하학 |

---

## EASY BOX 내용

**핵심 비유**: "도서관에서 책을 찾을 때 '쿼리(찾는 키워드)'로 '키(책 제목)'를 검색하면 관련도(attention score)가 나오고, 그 점수에 따라 '값(책 내용)'을 가져온다 — 이것이 Attention의 전부다."

**예시 리스트**:
- Query: 현재 처리하는 단어 ("the cat sat")
- Key: 모든 단어의 검색 가능 표현
- Value: 정보를 담은 실제 내용 — 가중치 합으로 문맥 정보를 가져옴
- AI에서: ChatGPT, BERT, GPT-4 모두 이 메커니즘을 수십 레이어 쌓은 것 — 수식 수준으로 이해하면 어떤 논문도 읽을 수 있다

---

## 섹션 구성

### 섹션 1: Scaled Dot-Product Attention

**아이콘**: `⊙`  
**제목**: `Attention의 수학적 정의 — 완전 유도`

**설명**: Attention 메커니즘은 Query(Q), Key(K), Value(V) 행렬을 사용하여 각 위치에서 다른 모든 위치의 정보를 가중합합니다. Scaling이 없으면 소프트맥스의 그라디언트가 소멸합니다.

**수식 박스 1**:
- 레이블: `Scaled Dot-Product Attention`
- 수식:
```
\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V
```
- 주석:
  - `Q ∈ ℝ^{n×dₖ}`: 쿼리 행렬 (n = 시퀀스 길이, dₖ = 키 차원)
  - `K ∈ ℝ^{m×dₖ}`: 키 행렬 (m = 키/값 길이)
  - `V ∈ ℝ^{m×dᵥ}`: 값 행렬
  - `√dₖ`: 스케일링 인자 — QKᵀ의 분산이 dₖ에 비례 → 1/√dₖ로 표준화
  - 결과: `ℝ^{n×dᵥ}` — 각 쿼리 위치에서의 문맥화된 표현

**수식 박스 2** (Attention 점수 분석):
- 레이블: `스케일링의 필요성 — 분산 분석`
- 수식:
```
\mathrm{Var}[q_i \cdot k_j] = d_k \cdot \mathrm{Var}[q] \cdot \mathrm{Var}[k] = d_k
\quad\Rightarrow\quad
\frac{q_i \cdot k_j}{\sqrt{d_k}} \sim \mathcal{N}(0, 1) \text{ (근사)}
```
- 주석:
  - q와 k의 각 차원이 N(0,1)이면 내적의 분산 = dₖ
  - √dₖ로 나누면 분산 = 1 → softmax 포화(saturation) 방지
  - dₖ가 크면 스케일 없을 때 softmax가 극단적으로 집중됨 → 그라디언트 소멸

---

### 섹션 2: Multi-Head Attention

**아이콘**: `H`  
**제목**: `Multi-Head Attention — 여러 관점에서 동시에`

**설명**: 단일 Attention 대신 h개의 헤드로 서로 다른 서브공간에서 동시에 Attention을 계산합니다. 각 헤드는 다른 종류의 관계(구문, 의미, 지시 등)를 포착합니다.

**수식 박스 3**:
- 레이블: `Multi-Head Attention`
- 수식:
```
\mathrm{MultiHead}(Q,K,V) = \mathrm{Concat}(\text{head}_1, \ldots, \text{head}_h) W^O
\quad\text{where}\quad
\text{head}_i = \mathrm{Attention}(QW_i^Q, KW_i^K, VW_i^V)
```
- 주석:
  - `W_i^Q ∈ ℝ^{d_{model}×dₖ}`: i번째 헤드의 쿼리 투영 행렬
  - `W_i^K, W_i^V`: 키, 값 투영 (dₖ = dᵥ = d_model/h)
  - `W^O ∈ ℝ^{hd_v×d_model}`: 출력 투영
  - h개 헤드를 연결 후 선형 투영 → d_model 차원 출력

**표 (Transformer 원논문 하이퍼파라미터)**:

| 파라미터 | 값 | 의미 |
|---------|---|----|
| d_model | 512 | 모델 전체 차원 |
| h | 8 | 헤드 수 |
| dₖ = dᵥ | 64 | 헤드당 차원 (= 512/8) |
| d_ff | 2048 | FFN 내부 차원 |
| N (레이어) | 6 | 인코더/디코더 레이어 수 |

---

### 섹션 3: Positional Encoding — 순서 정보 주입

**아이콘**: `📍`  
**제목**: `Positional Encoding — Attention의 순서 무관성 해결`

**설명**: Attention 자체는 위치 정보가 없습니다 (집합 연산처럼 동작). 위치 정보를 Embedding에 더하는 것이 Positional Encoding입니다.

**수식 박스 4**:
- 레이블: `Sinusoidal Positional Encoding`
- 수식:
```
PE_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)
\quad
PE_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)
```
- 주석:
  - `pos`: 시퀀스에서의 위치 (0부터)
  - `i`: 차원 인덱스 (0 ~ d_model/2)
  - 각기 다른 주파수의 sin/cos — 멀리 떨어진 위치는 다른 패턴
  - 장점: 학습 시 보지 못한 긴 시퀀스도 외삽 가능 (상대 위치 정보 내포)
  - 최신: 대부분 학습 가능한 Positional Encoding 또는 RoPE 사용

**callout (tip)**:
- 내용: **RoPE (Rotary Position Embedding)** (Su et al., 2021): 위치를 복소수 회전으로 표현. 상대 위치 정보를 Attention 점수 자체에 직접 주입 → LLaMA, GPT-NeoX 등에서 사용. `qᵢ · kⱼ = Re(q_R · k_R*)` where R은 회전 행렬.

---

### 섹션 4: Transformer 블록의 완전한 구조

**아이콘**: `🏗`  
**제목**: `Transformer 블록 — 전체 계산 그래프`

**수식 박스 5**:
- 레이블: `Transformer Encoder 블록`
- 수식:
```
\begin{aligned}
\mathbf{Z} &= \mathrm{LayerNorm}(\mathbf{X} + \mathrm{MultiHead}(\mathbf{X},\mathbf{X},\mathbf{X})) \\
\mathbf{Y} &= \mathrm{LayerNorm}(\mathbf{Z} + \mathrm{FFN}(\mathbf{Z})) \\
\mathrm{FFN}(\mathbf{z}) &= \max(0, \mathbf{z}W_1 + b_1)W_2 + b_2
\end{aligned}
```
- 주석:
  - Self-Attention: Q=K=V=X (자기 자신에 대한 Attention)
  - Residual connection: X를 더해 그라디언트 흐름 보장
  - LayerNorm: 각 위치의 특징 차원으로 정규화 (BatchNorm과 다름)
  - FFN: 2레이어 MLP — 각 위치에 독립적으로 적용 (위치 간 정보 교환 없음)

**수식 박스 6** (자기회귀 디코더의 Causal Mask):
- 레이블: `Masked Self-Attention — 미래 토큰 차단`
- 수식:
```
\mathrm{score}_{ij} = \begin{cases} \frac{q_i \cdot k_j}{\sqrt{d_k}} & j \leq i \\ -\infty & j > i \end{cases}
```
- 주석:
  - 자기회귀 생성: 위치 i는 위치 j ≤ i의 정보만 볼 수 있음
  - j > i에 -∞을 넣으면 softmax 후 가중치가 0
  - GPT 계열: 디코더 전용, 항상 Causal Mask 사용
  - BERT: 마스킹 없는 Bidirectional Attention

---

## 인터랙티브: Attention 가중치 계산기

**제목**: `🔢 Scaled Dot-Product Attention — 3토큰 예시`

**파라미터**:

| ID | 이름 | min | max | step | default |
|----|------|-----|-----|------|---------|
| `att-dk` | 키 차원 dₖ | 1 | 16 | 1 | 4 |
| `att-temp` | 온도 (1/√dₖ 조절) | 0.1 | 3.0 | 0.1 | 1.0 |

**JavaScript 로직**:
```javascript
function updateAtt() {
  const dk = parseInt(document.getElementById('att-dk').value);
  const temp = parseFloat(document.getElementById('att-dk').value); // placeholder
  const tempV = parseFloat(document.getElementById('att-temp').value);
  document.getElementById('att-dk-v').value = dk;
  document.getElementById('att-temp-v').value = tempV.toFixed(1);

  // 간단한 3×dk QK 예시 (고정 값)
  // q = [1,0,...], k1 = [1,0,...], k2 = [0,1,...], k3 = [-1,0,...]
  const scale = tempV / Math.sqrt(dk);
  const scores = [1.0 * dk * 0.3, 0.0, -1.0 * dk * 0.3].map(s => s * scale);

  // Softmax
  const maxS = Math.max(...scores);
  const exps = scores.map(s => Math.exp(s - maxS));
  const sumExp = exps.reduce((a, b) => a + b, 0);
  const weights = exps.map(e => e / sumExp);

  document.getElementById('att-res').innerHTML =
    `dₖ=${dk}, 온도 배율=${tempV.toFixed(1)} (스케일 = ${scale.toFixed(3)})<br/>
     Attention 점수 (qKᵀ/√dₖ·온도): [${scores.map(s => s.toFixed(3)).join(', ')}]<br/>
     Softmax 가중치: [<strong>${weights.map(w => w.toFixed(3)).join(', ')}</strong>]<br/>
     해석: 토큰1(유사)=${(weights[0]*100).toFixed(1)}%, 토큰2(직교)=${(weights[1]*100).toFixed(1)}%, 토큰3(반대)=${(weights[2]*100).toFixed(1)}%<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       온도를 높이면 가중치가 균등해지고, dₖ가 커지면 스케일이 중요해짐
     </span>`;
}
updateAtt();
```

---

## 퀴즈

**질문**: Transformer에서 √dₖ로 나누는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 학습 속도를 높이기 위해 | ❌ |
| (b) QKᵀ의 분산이 dₖ에 비례하므로, softmax 포화를 막기 위해 표준화 | ✅ |
| (c) Attention 가중치 합이 1이 되도록 정규화하기 위해 | ❌ |
| (d) 위치 정보를 인코딩하기 위해 | ❌ |

**정답 위치**: (b)

---

## 주간 체크리스트 (Week 31)

```
[ ] Scaled Dot-Product Attention 수식을 쓸 수 있다
[ ] √dₖ 스케일링의 필요성을 분산 분석으로 설명할 수 있다
[ ] Multi-Head Attention에서 각 헤드가 하는 역할을 설명할 수 있다
[ ] Sinusoidal Positional Encoding의 수식을 설명할 수 있다
[ ] Transformer 블록(Self-Attention + FFN + LayerNorm + Residual)을 그릴 수 있다
[ ] Causal Mask가 GPT에서 왜 필요한지 설명할 수 있다
[ ] "Attention is All You Need" 논문의 수식 섹션을 독립적으로 읽을 수 있다
```
