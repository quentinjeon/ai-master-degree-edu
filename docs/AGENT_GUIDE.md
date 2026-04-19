# AGENT GUIDE — 멀티에이전트 운영 안내

> **읽는 대상**: 이 프로젝트에 처음 투입된 모든 에이전트  
> **먼저 읽을 것**: `docs/PRD.md` → 이 파일 → 자신의 역할 MDC

---

## 1. 전체 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                ORCHESTRATOR AGENT                        │
│  MDC: agent-orchestrator.mdc                             │
│  파일 소유: docs/PROGRESS.md (전체 쓰기)                  │
│  역할: 상태 확인 → 할당 → 웨이브 조율                     │
└──────────┬──────────────────────────┬────────────────────┘
           │ 할당                      │ 완료 보고
  ┌────────▼──────────┐     ┌─────────▼───────────────────┐
  │  CHAPTER AGENT    │     │        QA AGENT              │
  │  (N개 병렬 가능)   │     │  MDC: agent-qa.mdc           │
  │  MDC:             │     │  파일 소유: QA_REPORT.md     │
  │  agent-chapter    │     │  역할: 체크리스트 검증        │
  │  파일 소유:        │     │  → 수정 필요 목록 출력       │
  │  chapters/chNN    │     └─────────────┬───────────────┘
  │  (담당 1개만)      │                   │ 전체 PASS
  └────────┬──────────┘     ┌─────────────▼───────────────┐
           │ DONE            │    INTEGRATION AGENT         │
           └────────────────▶│  MDC: agent-integration.mdc  │
                             │  파일 소유: index.html       │
                             │  역할: 사이드바·링크 통합    │
                             └─────────────────────────────┘
```

---

## 2. 에이전트별 파일 소유권 (충돌 방지 규칙)

> **황금 규칙**: 자신이 소유한 파일만 쓴다. 다른 파일은 읽기 전용.

| 파일 | Orchestrator | Chapter | QA | Integration |
|------|:---:|:---:|:---:|:---:|
| `docs/PROGRESS.md` | **W** | 내 행만 | R | R |
| `docs/PRD.md` | R | R | R | R |
| `docs/DESIGN_SYSTEM.md` | R | R | R | R |
| `docs/CHAPTER_TEMPLATE.md` | — | R | — | — |
| `docs/chapters/weekNN.md` | R | R | — | — |
| `chapters/chNN.html` | — | **W** (내 것만) | R | R |
| `docs/QA_REPORT.md` | R | — | **W** | — |
| `index.html` | — | ❌ | — | **W** |

`W` = 쓰기 가능, `R` = 읽기 전용, `—` = 사용 안 함, `❌` = 절대 금지

---

## 3. 프롬프트 템플릿

### Orchestrator에게

```
너는 이 프로젝트의 Orchestrator Agent다.
docs/PROGRESS.md를 확인하고 현재 TODO 상태인 챕터를 파악해라.
agent-orchestrator.mdc 규칙을 따라 웨이브 계획대로 Chapter Agent를 할당해라.
직접 챕터 HTML을 만들지 않는다.
```

### Chapter Agent에게

```
너는 Chapter-Agent-[A-N]이다.
담당 챕터: Ch.[NN] [챕터명]
명세 파일: docs/chapters/week[NN].md
출력 파일: chapters/ch[NN].html (이 파일만 생성/수정한다)

[시작 전 필수] docs/LOCKS.md 확인
  → chapters/ch[NN].html이 🔒 LOCKED이면 즉시 중단
  → 🔓 FREE이면:
      LOCKS.md: 🔒 LOCKED | 내 이름 | 시각 으로 업데이트
      PROGRESS.md: 락 열 🔒, 상태 🔒 IN_PROGRESS 로 업데이트

[완료 후 필수] 락 해제
      LOCKS.md: 🔓 FREE | — | — 으로 복원
      PROGRESS.md: 락 열 🔓, 상태 ✅ DONE, 완료일 기록

따라야 할 규칙:
- .cursor/rules/lock-protocol.mdc  ← 최우선
- .cursor/rules/agent-chapter.mdc
- .cursor/rules/chapter-dev.mdc
- .cursor/rules/html-structure.mdc
- .cursor/rules/design-system.mdc
- .cursor/rules/new-chapter.mdc

index.html과 다른 챕터 파일은 절대 수정하지 않는다.
```

### QA Agent에게

```
너는 QA Agent다.
docs/PROGRESS.md에서 DONE 상태인 챕터를 모두 검증해라.
검증 결과를 docs/QA_REPORT.md에 기록해라.
챕터 HTML 파일을 직접 수정하지 않는다.

따라야 할 규칙: .cursor/rules/agent-qa.mdc
```

### Integration Agent에게

```
너는 Integration Agent다.
docs/PROGRESS.md에서 모든 챕터가 QA_PASS인지 확인해라.
하나라도 QA_PASS가 아니면 멈추고 보고해라.
모두 통과하면 index.html의 사이드바와 챕터 링크를 업데이트해라.

따라야 할 규칙: .cursor/rules/agent-integration.mdc
```

---

## 4. 웨이브 실행 순서

```
Wave 1 (동시 실행 가능)
  ├── Chapter-Agent-A → ch01.html
  ├── Chapter-Agent-B → ch02.html
  ├── Chapter-Agent-C → ch03.html
  └── Chapter-Agent-D → ch04.html
        ↓ (모두 DONE 확인 후)
Wave 2 (동시 실행 가능)
  ├── Chapter-Agent-E → ch05.html
  ├── Chapter-Agent-F → ch06.html
  ├── Chapter-Agent-G → ch07.html
  └── Chapter-Agent-H → ch08.html
        ↓ (모두 DONE 확인 후)
Wave 3 (동시 실행 가능)
  ├── Chapter-Agent-I  → ch09.html
  ├── Chapter-Agent-J  → ch10.html
  ├── Chapter-Agent-K  → ch11.html
  ├── Chapter-Agent-L  → ch12.html
  ├── Chapter-Agent-M  → ch13.html
  └── Chapter-Agent-N  → ch14.html
        ↓ (모두 DONE 확인 후)
Wave 4
  ├── QA-Agent → docs/QA_REPORT.md (모든 챕터 검증)
  └── Integration-Agent → index.html (모두 QA_PASS 후)
```

---

## 5. PROGRESS.md 상태값 의미

| 상태 | 의미 | 다음 액션 |
|------|------|---------|
| `TODO` | 아직 시작 안 함 | Orchestrator가 할당 가능 |
| `IN_PROGRESS` | 에이전트 작업 중 | 다른 에이전트 건드리지 않음 |
| `DONE` | 챕터 HTML 완성 | QA Agent 검증 대기 |
| `QA_FAIL` | 검증 실패 | 원래 Chapter Agent가 수정 후 재DONE |
| `QA_PASS` | 검증 통과 | Integration Agent 통합 대상 |

---

## 6. 충돌 시나리오와 해결법

### 시나리오 1: 두 에이전트가 같은 챕터 시도
→ PROGRESS.md에서 먼저 `IN_PROGRESS`로 바꾼 에이전트가 작업.  
나중에 온 에이전트는 `IN_PROGRESS` 상태 확인 후 다른 `TODO` 챕터 선택.

### 시나리오 2: 에이전트가 중간에 멈춤 (IN_PROGRESS 고착)
→ Orchestrator가 24시간 이상 IN_PROGRESS인 챕터 발견 시  
해당 행을 다시 `TODO`로 초기화 후 재할당.

### 시나리오 3: QA 실패 후 수정
→ QA Agent가 `QA_FAIL` + 수정 항목을 QA_REPORT.md에 기록  
→ 원래 Chapter Agent (또는 새 에이전트)가 해당 챕터만 수정  
→ 완료 후 PROGRESS.md를 다시 `DONE`으로 변경  
→ QA Agent가 재검증

---

## 7. 읽어야 할 MDC 규칙 파일

```
.cursor/rules/
├── chapter-dev.mdc       ← 모든 에이전트: 프로젝트 전체 규칙
├── design-system.mdc     ← HTML 파일 작업 시: CSS 변수/컴포넌트
├── html-structure.mdc    ← 챕터 HTML 작업 시: 블록 구조
├── content-spec.mdc      ← 명세 파일 읽을 때: 변환 매핑
├── new-chapter.mdc       ← 챕터 생성 시: Step 1~8 워크플로우
├── agent-orchestrator.mdc ← Orchestrator만
├── agent-chapter.mdc     ← Chapter Agent만
├── agent-qa.mdc          ← QA Agent만
└── agent-integration.mdc ← Integration Agent만
```
