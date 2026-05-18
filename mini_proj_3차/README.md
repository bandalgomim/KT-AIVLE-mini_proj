# 💄 화장품 리뷰 분석 커스터마이징 에이전트

## 개요

| 항목 | 내용 |
|------|------|
| **기간** | 2026.05.11 ~ 2026.05.13 |
| **목표** | 화장품 리뷰를 자동 분석하는 멀티 AI 에이전트 구축 |
| **역할** | 조장 · 프로젝트 총괄 · LangSmith 실행 트레이싱 |
| **기술 스택** | Python · LangGraph · LangChain · OpenAI GPT-4.1-mini · SQLite · Streamlit · LangSmith |
| **산출물** | 노드 추가로 확장이 용이한 화장품 리뷰 분석 멀티에이전트 + Streamlit 대시보드 |

---

## 프로젝트 소개

화장품 리뷰 텍스트를 입력받아 제품군 분류 · 재구매 의향 판단 · 속성별 감성 분석(ABSA)을 자동으로 수행하는 멀티에이전트 시스템이다. Supervisor 패턴을 중심으로 Critic 기반 QA 루프를 구성하여 분석 결과의 신뢰도를 자동 검증한다. 분석 결과는 SQLite에 저장되고 Streamlit 대시보드로 시각화된다.

---

## 시스템 아키텍처 (LangGraph)

| 노드 | 역할 |
|------|------|
| `supervisor` | 전체 실행 흐름 제어 — 다음 노드 결정 (분석/검수/종료) |
| `categorize` | 리뷰 분석 → 8개 허용 제품군 중 1개 분류 |
| `repurchase` | 리뷰 분석 → 재구매 의향 판단 (0/1) |
| `analyzer` | ABSA 수행 → 4개 속성(보습/가격/향/포장) 감성 분류 |
| `critic` | 전체 분석 결과 검증 → 승인/반려 + 재시도 노드 지정 |

---

## 에이전트 상세 스펙

### Supervisor Agent
- 현재 State를 확인하여 다음 실행 노드를 결정
- 직접 분석/평가 수행 없이 **흐름 제어만** 담당
- 실패 시 리스크: 잘못된 흐름 제어 → 무한 루프 가능

### Categorize Agent
- 허용 제품군: 스킨케어 · 메이크업 · 클렌징 · 선케어 · 헤어케어 · 바디케어 · 프래그런스 · 위생 및 기타 퍼스널케어
- 출력: `categorize` + `categorize_evidence` + `categorize_reason`

### Repurchase Agent
- 명확한 재구매 의사 표현 시 1, 단순 만족 표현은 0
- 출력: `repurchase` + `repurchase_evidence` + `repurchase_reason`

### Analyzer Agent (ABSA)
- 4개 속성(보습/가격/향/포장)에 대해서만 감성 분류
- Pydantic 구조화 출력, 환각 방지를 위해 원문 evidence 필수
- 출력: `aspect` + `label(0/1)` + `evidence` + `reason`

### Critic Agent (QA)
- 형식 검증 · 범위 검증 · 근거 검증 · 품질 검증 4단계 수행
- 모든 노드 출력을 **단일 구조(`node_result` / `node_evidence` / `node_reason`)로 통일**하여 처리
- 반려 시 `retry_nodes` 지정 → Supervisor가 해당 노드만 재실행

---

## 분기 정책 (Supervisor 제어 로직)

| 조건 | 동작 |
|------|------|
| `next_node == None` | 초기 실행 — Categorize → Repurchase → Analyzer 순차 실행 |
| `len(next_node) == 0` | 분석 완료 — Critic 실행 |
| `verdict == 1` | 적합 판정 — 종료 |
| `verdict != 1` AND `retry_count < max_retries` | 부적합 — 문제 노드만 재실행, retry_count +1 |
| `verdict != 1` AND `retry_count >= max_retries` | 재시도 초과 — 현재 결과 기준 종료 |

---

## 내가 기여한 부분

### 조장 역할
- 8인 팀 프로젝트 총괄 — 역할 분배 및 일정 조율
- 조원들과 업무 방식 차이를 조율하며 팀 협업 방식 정립
- **노드 출력 구조 통일 설계** — 모든 노드 출력을 `node_result / node_evidence / node_reason` 구조로 통일하여 Critic이 단일 로직으로 모든 노드를 검증할 수 있도록 확장성 확보

### LangSmith 트레이싱
- 에이전트 실행 흐름을 LangSmith로 추적
- 각 노드별 실행 시간 · 입출력 · LLM 호출 내역 모니터링
- 오류 발생 노드 특정 및 디버깅에 활용

---

## 산출물

### Streamlit 대시보드
- 제품군별 필터링
- 긍정/부정 요인 Top 10 시각화
- 재구매 확정 요인 분석
- 주요 속성 만족도 차트
- 재구매 성공 요인 도넛 차트
- 제품군별 속성 만족도 히트맵

### DB 스키마 (SQLite)
```sql
CREATE TABLE reviews (
    id           INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    review       TEXT NOT NULL,
    aspect       TEXT,    -- predicted aspects as JSON string
    label        TEXT,    -- predicted labels as JSON string
    categorize   TEXT,
    repurchase   TEXT,
    insert_time  DATETIME DEFAULT (datetime('now', 'localtime'))
);
```

---

## 회고

**잘한 점**
- Critic + Supervisor 패턴으로 분석 품질 자동 검증 루프 구현
- 노드 출력 구조 통일로 Critic의 재사용성과 확장성 확보
- LangSmith로 실행 흐름 전체 추적 및 디버깅
- 조장으로서 8인 팀 총괄 경험

**보완할 점**
- 제품군별 개선사항을 LLM으로 분석하는 기능 추가 필요
- 리뷰 분석 속도 최적화 (병렬 처리 도입 검토)
- 도메인 연동 및 계정 기반 DB 재구성으로 실서비스화
- 다음 프로젝트에서는 Streamlit 대시보드 구현까지 직접 참여할 것

---

## 향후 확장 방향

- 재구매 고객 성향 기반 추천 제품 사은품 증정 → 브랜드 충성도 증대
- 1차 리뷰 데이터 + 에이전트 가공 2차 데이터로 신규 서비스 제공
- 도메인 계정 연동 → 개인화 분석 도구로 발전
