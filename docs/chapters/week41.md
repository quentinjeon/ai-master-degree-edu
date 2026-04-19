# Week 41 — 구현·재현성 엔지니어링 (Ch.41)

> **파일명**: `chapters/ch41.html`
> **위치**: Phase 3-D · 실전 역량 (Ch.40 이후, 신규 Phase)
> **인터랙티브**: ✅ 실험 재현성 체크리스트 + Seed 고정 시뮬레이터
> **우선순위**: P0 (레벨 3 핵심 실전 갭)
> **선행 조건**: `ch40.html` (리뷰 대응·Response Letter) 완료 또는 `ch00f.html` (ML 실습 입문) 이후 언제든 참조 가능

---

## 이 챕터가 필요한 이유 (갭 분석)

현재 커리큘럼은 **수학 이론과 논문 작성법**에 집중되어 있다.
그러나 SCI급 논문은 **실험이 재현 가능해야** 통과된다.

| 기존 커리큘럼에서 하는 것 | 이 챕터에서 하는 것 |
|----------------------|------------------|
| 수식과 알고리즘 이해 | PyTorch로 알고리즘 직접 구현 |
| 실험 설계 이론 | Config 파일로 실험을 관리하는 방법 |
| 결과 해석 | 실험 로그를 남기고 추적하는 방법 |
| 논문 쓰기 | 다른 사람이 내 코드로 같은 결과를 낼 수 있는 구조 |

**100단계 연구 로드맵 커버 항목**:
- 섹션 7 (구현·재현성) 7-4 ~ 7-10 전체
  - 7-4: PyTorch 핵심 사용법
  - 7-5: 데이터 로더와 전처리 파이프라인 분리
  - 7-6: 설정 파일(config)로 실험값 관리
  - 7-7: 실험 로그 기록 방법
  - 7-8: Seed 고정 방법과 이유
  - 7-9: 그래프·표 자동 생성
  - 7-10: 폴더 구조와 버전 관리

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| 뱃지 | `Ch.41 · 구현 실전 (Phase 3-D — 실전 역량 1주차)` |
| 제목 | `구현·재현성 엔지니어링` |
| 서브타이틀 | `"좋은 실험은 한 번 잘 나온 것이 아니라 누구나 반복할 수 있는 것이다." Seed 고정, Config 관리, 실험 로그, 폴더 구조 — SCI급 논문의 코드 재현성 표준.` |
| 이전 챕터 | `ch40.html` — Ch.40 리뷰 대응·Response Letter·재투고 |
| 다음 챕터 | `ch42.html` — Ch.42 효과크기와 결과 해석 심화 |

---

## EASY BOX 내용

**핵심 비유**: "요리 레시피가 정확하지 않으면 다른 사람이 똑같이 만들 수 없다. 코드도 마찬가지다. Seed, 파라미터, 환경이 모두 기록돼야 '재현'이 된다."

**예시 리스트**:
- Seed 없이 실험: 모델을 새로 학습할 때마다 정확도가 85%, 84%, 86%로 다르다 → 논문에 몇 %를 쓸 것인가?
- Config 없이 실험: 6개월 뒤 "이때 learning rate가 얼마였지?"를 기억할 수 없다
- 폴더 구조 없이 실험: `model_final2_real_fix.pt`, `model_best_v3_copy.pt` → 어느 파일이 최종인지 모른다
- AI에서: NeurIPS 2023부터 코드 공개 + 재현성 체크리스트를 의무화한 학회가 늘고 있다

---

## 섹션 구성

### 섹션 1: PyTorch 핵심 패턴 — 실험 구현의 최소 단위

**아이콘**: `🔥`
**제목**: `PyTorch로 실험을 구현하는 표준 패턴`

**설명**: PyTorch는 연구 구현의 사실상 표준입니다. 전체 라이브러리를 다 알 필요는 없습니다. 논문 실험을 구현하는 데 필요한 핵심 5개 패턴만 익히면 됩니다.

**표 (PyTorch 핵심 5패턴)**:

| 패턴 | 역할 | 코드 스니펫 |
|------|------|-----------|
| Dataset + DataLoader | 데이터 로딩 파이프라인 | `DataLoader(dataset, batch_size=32, shuffle=True)` |
| nn.Module | 모델 정의 | `class Model(nn.Module): def forward(self, x):...` |
| Optimizer + Loss | 학습 루프 | `optimizer.zero_grad(); loss.backward(); optimizer.step()` |
| Checkpoint 저장 | 모델 저장 | `torch.save({'epoch':e, 'state':model.state_dict()}, path)` |
| device 관리 | GPU/CPU 자동화 | `device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')` |

**수식 박스 1**:
- 레이블: `PyTorch 학습 루프 구조`
- 수식:
```
\text{for epoch in range}(E): \quad
\theta_{t+1} = \theta_t - \eta \cdot \nabla_\theta \mathcal{L}(\theta_t; \mathcal{B}_t)
```
- 주석:
  - `E`: 전체 에포크 수 (config에서 관리)
  - `η`: 학습률 (learning rate, config에서 관리)
  - `∇ₜL`: 미니배치 `Bₜ`에 대한 손실 그라디언트 → `loss.backward()`
  - `θt+1 = θt - η∇L` → `optimizer.step()`

**callout (tip)**:
- 내용: **Dataset 클래스를 먼저 분리하라.** 전처리 로직이 모델 코드와 섞이면 나중에 데이터를 바꿀 때 코드 전체를 수정해야 한다. `class MyDataset(Dataset)`으로 독립 파일을 만드는 것이 재현성의 시작이다.

---

### 섹션 2: Config 파일 — 하드코딩을 없앤다

**아이콘**: `⚙`
**제목**: `Config 파일로 모든 실험 파라미터를 관리한다`

**설명**: 코드에 숫자를 직접 쓰는 것(하드코딩)은 실험 관리의 최대 실수입니다. Learning rate, batch size, epoch 수, 모델 구조 등 모든 파라미터는 config 파일에서 관리해야 합니다.

**표 (하드코딩 vs Config 관리 비교)**:

| 항목 | ❌ 하드코딩 | ✅ Config 관리 |
|------|-----------|-------------|
| 학습률 | `lr = 0.001` (코드 내부) | `config.yaml: lr: 0.001` |
| 에포크 | `for i in range(100):` | `config.yaml: epochs: 100` |
| 실험 재현 | 코드를 뒤져야 파라미터 파악 | config 파일 하나로 실험 전체 복원 |
| 파라미터 탐색 | 코드 수정 → 다시 실행 | config 파일만 바꾸고 실행 |
| 논문 Table | "어느 파라미터로 돌렸는지 모름" | config 파일 = 논문 Appendix 초안 |

**수식 박스 2**:
- 레이블: `Config YAML 예시 구조`
- 수식:
```
\text{config.yaml:} \quad
\begin{cases}
\texttt{model:}\ \{ \texttt{hidden\_dim}: 256,\ \texttt{num\_layers}: 4 \} \\
\texttt{train:}\ \{ \texttt{lr}: 0.001,\ \texttt{epochs}: 100,\ \texttt{batch\_size}: 32 \} \\
\texttt{seed}: 42 \\
\texttt{data:}\ \{ \texttt{dataset}: \text{"squad"},\ \texttt{split}: 0.8 \}
\end{cases}
```
- 주석:
  - Python에서 `yaml.safe_load(f)` 로 dict로 읽음
  - `argparse` + YAML 조합으로 실험별 config 파일 관리
  - 각 실험은 별도 config 파일로 → `exp01.yaml`, `exp02.yaml`
  - 논문 Experiment Setting 섹션 = config 파일의 핵심 파라미터 목록

---

### 섹션 3: Seed 고정 — 재현 가능한 결과의 기초

**아이콘**: `🌱`
**제목**: `Seed를 고정하지 않으면 결과는 매번 다르다`

**설명**: 딥러닝 실험은 가중치 초기화, 데이터 셔플, 드롭아웃 등에서 난수를 사용합니다. Seed를 고정하지 않으면 같은 코드로도 매번 다른 결과가 나옵니다. SCI 논문은 "동일 seed에서 재현 가능"해야 합니다.

**수식 박스 3**:
- 레이블: `Seed 고정 표준 코드 패턴`
- 수식:
```
\text{seed\_everything}(42): \quad
\begin{aligned}
&\texttt{random.seed}(42) \\
&\texttt{np.random.seed}(42) \\
&\texttt{torch.manual\_seed}(42) \\
&\texttt{torch.cuda.manual\_seed\_all}(42) \\
&\texttt{torch.backends.cudnn.deterministic} = \texttt{True}
\end{aligned}
```
- 주석:
  - Seed 42: 관습적으로 가장 많이 쓰이는 값 (통계적 의미는 없음)
  - 논문에서 "We use seed 42 for reproducibility"처럼 명시
  - 단일 seed가 아니라 여러 seed 평균 ± 표준편차가 더 신뢰도 높음
  - 예: seed [42, 123, 456]에서 실험 후 "85.3 ± 0.4%" 형식으로 보고

**callout (warn)**:
- 내용: **Seed 고정만으로 완전한 재현성이 보장되지 않는다.** CUDA 비결정론적 연산, 병렬 처리 순서, 라이브러리 버전 차이 등이 결과에 영향을 줄 수 있다. 논문에서는 seed + 환경(Python/PyTorch/CUDA 버전)을 함께 공개하는 것이 표준이다.

---

### 섹션 4: 실험 로그와 폴더 구조 — 6개월 뒤의 자신을 위해

**아이콘**: `📁`
**제목**: `실험실 정리 능력이 논문 품질을 결정한다`

**설명**: 실험이 쌓일수록 "어느 실험이 어느 결과였지?"를 찾는 데 시간을 낭비하게 됩니다. 처음부터 실험 로그와 폴더 구조를 정리하는 습관이 논문 완성 속도를 결정합니다.

**표 (권장 폴더 구조)**:

| 폴더/파일 | 역할 |
|----------|------|
| `src/` | 핵심 소스 코드 (model.py, dataset.py, trainer.py) |
| `configs/` | 실험별 YAML config 파일 |
| `experiments/` | 실험 결과 저장 (`exp_001/`, `exp_002/`) |
| `experiments/exp_001/` | checkpoint/, logs/, config.yaml, results.json |
| `notebooks/` | 분석용 Jupyter 노트북 (실험 코드 아님) |
| `scripts/` | 학습 실행 스크립트 (train.sh) |
| `README.md` | 설치 방법, 실험 재현 명령어 |
| `requirements.txt` | 라이브러리 버전 고정 |

**표 (실험 로그 최소 기록 항목)**:

| 기록 항목 | 이유 |
|----------|------|
| 실험 날짜·시간 | 어느 실험이 먼저인지 파악 |
| Config 파라미터 전체 | 재현에 필요한 모든 설정 |
| 에포크별 train/val loss | 과적합 시점 파악 |
| 최종 test metric (평균 ± 표준편차) | 논문 Table 직접 사용 |
| 소요 시간, GPU 메모리 | 논문 Appendix: 계산 비용 |
| 사용 라이브러리 버전 | 재현성 보장 |

**callout (info)**:
- 내용: **Weights & Biases (wandb), MLflow, TensorBoard** 중 하나를 선택해 자동 로그를 남기는 것을 강력히 권장한다. `wandb.log({'train_loss': loss, 'val_acc': acc})`만으로 모든 실험이 웹에서 비교 가능해진다. SCI 논문 저자의 70% 이상이 wandb 또는 유사 도구를 사용한다.

---

### 섹션 5: 논문 재현성 표준 — 학회 요구사항

**아이콘**: `📋`
**제목**: `NeurIPS / ICML 재현성 체크리스트 — 논문 제출 전 필수 확인`

**설명**: 최근 주요 학회들은 논문 제출 시 재현성 체크리스트를 요구합니다. 이를 미리 알고 실험을 설계하면 나중에 재실험할 필요가 없습니다.

**표 (재현성 체크리스트 핵심 항목)**:

| 항목 | 설명 | 논문에서의 위치 |
|------|------|--------------|
| ✅ 모든 hyperparameter 공개 | 실험에 사용된 모든 파라미터 | Appendix: Experimental Details |
| ✅ Seed 명시 | 사용한 random seed 값 | Experimental Setup |
| ✅ 환경 공개 | Python, PyTorch, CUDA, GPU 사양 | Appendix |
| ✅ 데이터셋 버전 | 사용한 데이터셋 버전 및 전처리 | Experimental Setup |
| ✅ 코드 공개 URL | GitHub 링크 | Abstract 또는 Footnote |
| ✅ 통계적 유의성 | 여러 seed 결과 ± 표준편차 | Results Table |
| ✅ 계산 비용 | 학습 시간, GPU 수 | Appendix |

---

## 인터랙티브: 실험 재현성 점수 진단기

**제목**: `🔢 내 실험은 재현 가능한가? — 체크리스트 점수 진단`

**JavaScript 로직**:
```javascript
const checks = [
  { id: 'c1', label: 'Seed를 고정하고 코드에 명시했다', weight: 15 },
  { id: 'c2', label: 'Config 파일로 모든 파라미터를 관리한다', weight: 15 },
  { id: 'c3', label: '실험 결과를 날짜와 함께 폴더별로 저장한다', weight: 10 },
  { id: 'c4', label: '에포크별 loss를 로그로 남긴다', weight: 10 },
  { id: 'c5', label: 'requirements.txt 또는 conda env 파일이 있다', weight: 10 },
  { id: 'c6', label: '여러 seed에서 실험하고 평균±표준편차를 계산한다', weight: 15 },
  { id: 'c7', label: 'GPU/CPU 환경을 문서화했다', weight: 5 },
  { id: 'c8', label: '데이터셋 버전과 전처리 과정을 기록했다', weight: 10 },
  { id: 'c9', label: 'GitHub에 코드를 공개하거나 공개 계획이 있다', weight: 10 },
];

function calcReproScore() {
  let total = 0;
  checks.forEach(c => {
    if (document.getElementById(c.id).checked) total += c.weight;
  });

  const level = total >= 80 ? '🏆 NeurIPS/ICML 제출 가능 수준' :
                total >= 60 ? '🟡 학회 제출 전 보완 필요' :
                total >= 40 ? '🟠 실험 로그·seed 관리부터 시작' :
                              '🔴 기초 재현성 설정이 필요합니다';

  document.getElementById('repro-res').innerHTML =
    `재현성 점수: <strong>${total}/100</strong><br/>
     ${level}<br/>
     <span style="font-size:.78rem;color:var(--muted)">
       각 체크 항목을 구현할수록 논문의 신뢰도가 올라갑니다.<br/>
       NeurIPS 2024부터 재현성 체크리스트가 필수화됐습니다.
     </span>`;
}
```

**HTML 구조 (calc 블록 내)**:
```html
<!-- checks 배열의 각 항목을 checkbox + label로 렌더링 -->
<!-- 예시: -->
<div class="cr">
  <input type="checkbox" id="c1" onchange="calcReproScore()"/>
  <label for="c1">Seed를 고정하고 코드에 명시했다 (15점)</label>
</div>
<!-- ... 나머지 8개 동일 구조 ... -->
<div class="cres" id="repro-res">위 항목을 체크해서 재현성 점수를 확인하세요.</div>
```

---

## 퀴즈

**질문**: SCI 논문에서 "실험 결과 정확도 87.3% ± 0.4%"처럼 쓰는 이유는?

| 선택지 | 정답 여부 |
|--------|----------|
| (a) 소수점을 더 쓸수록 논문이 정확해 보이기 때문에 | ❌ |
| (b) 여러 seed에서 실험한 결과의 평균과 표준편차로, 결과의 통계적 안정성을 보여주기 위해 | ✅ |
| (c) 학회마다 형식이 다르기 때문에 | ❌ |
| (d) GPU 성능에 따른 오차 범위를 표시하기 때문에 | ❌ |

**정답 위치**: (b)
**해설**: ± 표준편차 표기는 단일 seed의 우연한 결과가 아니라 여러 seed에서 안정적으로 나오는 결과임을 증명한다. seed 수가 많을수록 (최소 3~5개) 결과의 신뢰도가 높아진다.

---

## 주간 체크리스트 (Week 41)

```
[ ] 로컬에 PyTorch를 설치하고 기본 학습 루프를 실행할 수 있다
[ ] Dataset 클래스를 모델 코드와 분리해서 작성할 수 있다
[ ] config.yaml을 만들고 Python에서 읽어올 수 있다
[ ] seed_everything(42) 함수를 직접 구현하고 사용할 수 있다
[ ] 실험 폴더 구조를 위 표대로 만들 수 있다
[ ] 에포크별 loss를 파일로 저장하는 로깅 코드를 작성할 수 있다
[ ] requirements.txt를 생성할 수 있다 (pip freeze > requirements.txt)
[ ] 재현성 점수 인터랙티브에서 80점 이상 달성
[ ] 본인 실험 하나를 위 체크리스트 기준으로 정리 완료
```
