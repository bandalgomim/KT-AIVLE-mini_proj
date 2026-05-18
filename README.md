# 김성준 (Kim SeongJun)

**"관계를 묻는 사고방식과 효율을 추구하는 태도, 철학도이자 개발자인 저의 모습입니다."**

철학을 배운다는 것은 특정 분야의 지식을 익히는 것이기도 하지만, **추상적 대상**을 어떻게 다뤄야 하는지를 훈련하는 것입니다. 저에게 설계라는 것은 **관계맺음** 입니다. 코드 속 추상적 결과물들이 **"어떤 관계를 맺고 있는가, 꼭 이런 관계여야 하는가?"** 이런 의문으로 작업에 임합니다.

공학이 제게 다가온 방식은 **효율성의 극대화**였습니다. 발상의 착안, 설계, 완성. 심지어 작업 방식과 도구를 고르는 것 조차 생산성과 효율성의 관계 속에서 작동합니다. 당연한 이 사실이 철학도였던 저에게는 신선한 충격이었습니다. **"어떤 것이 효율적인가, 생산성을 높이려면 어떻게 해야하는가?"** 이것은 이제 저에게도 지상과제입니다.


---

## 🛠 기술 스택

**AI / ML**
`Python` `TensorFlow/Keras` `Scikit-learn` `LangChain` `LangGraph` `OpenAI API` `LangSmith`

**Backend**
`Node.js` `Express` `MySQL` `SQLite`

**Frontend**
`React` `React Native`

**Tools**
`Git` `Streamlit` `Gradio`

---

## 💡 핵심 역량

**구조적 사고**
문제를 거시적 관점에서 먼저 파악하고, 데이터와 시스템 흐름을 설계하는 데 강점이 있습니다.

**AI 파이프라인 설계**
데이터 수집부터 모델 학습, 추론, API 제공까지 전체 파이프라인을 직접 구축한 경험이 있습니다.

**멀티에이전트 시스템**
LangGraph 기반 Supervisor 패턴, Critic QA 루프, State 설계 등 에이전트 아키텍처를 실습 프로젝트에서 직접 구현했습니다.

---

## 📁 프로젝트

### 🛫 AI 기반 항공사 고객 만족도 예측 시스템
> 2026.04.14 ~ 2026.04.15 · KT AIVLE 미니프로젝트 1차

항공사 고객 설문 데이터를 분석하여 만족도 예측 딥러닝 모델을 구축하고, 비즈니스 개선안을 도출했습니다.

- 카이제곱 검정으로 고객군(비즈니스 목적·충성 고객)과 만족도 간 통계적 유의성 검증
- Base Model(Accuracy 97%) → Fine-tuning 후 불만족 고객 Recall 0.81 → 0.96 향상
- 타 팀과 달리 거시적 비즈니스 관점에서 분석, 고객 구조를 이원화하여 개선 방향 제시

`Python` `TensorFlow/Keras` `Scikit-learn` `Seaborn`

---

### 🎓 강사와 수강자 두 주체를 고려한 교육용 AI Agent
> 2026.04.27 ~ 2026.04.28 · KT AIVLE 미니프로젝트 2차

PPT 업로드만으로 강의 영상·퀴즈·설문을 자동 생성하고, 수강생 피드백을 다음 강의에 자동 반영하는 LangGraph 기반 AI 에이전트입니다.

- 강사-수강자 피드백 루프 구조 기획 제안 (수강자 중심 → 양방향 구조로 전환)
- 멀티모달 입력(텍스트+이미지), Tavily 검색 연동, SQLite 피드백 DB, OpenAI TTS 통합
- Pydantic 구조화 출력으로 퀴즈 생성 안정성 확보

`Python` `LangGraph` `LangChain` `OpenAI API` `SQLite` `Gradio`

---

### 💄 화장품 리뷰 분석 커스터마이징 에이전트
> 2026.05.11 ~ 2026.05.13 · KT AIVLE 미니프로젝트 3차

화장품 리뷰를 자동 분석하는 멀티에이전트 시스템. Supervisor 패턴과 Critic 기반 QA 루프로 분석 품질을 자동 검증합니다.

- 조장으로서 8인 팀 총괄 및 노드 출력 구조 통일 설계 (`node_result / node_evidence / node_reason`)
- 단일 Critic 노드가 모든 분석 노드를 재사용 가능하도록 확장성 있는 구조 설계
- LangSmith로 전체 실행 흐름 트레이싱 및 디버깅
- Streamlit 대시보드로 제품군별 감성 분석 결과 시각화

`Python` `LangGraph` `LangChain` `OpenAI GPT-4.1-mini` `SQLite` `Streamlit` `LangSmith`

---

## 🎓 교육

**KT AIVLE School** AI 부트캠프 (2026 수료 예정)
- ML/DL · 단일 에이전트 · 멀티에이전트 시스템

**한국외국어대학교** 철학과 (주전공) · 컴퓨터공학과 (복수전공)

---

## 🔗 링크

[![GitHub](https://img.shields.io/badge/GitHub-bandalgomim-181717?style=flat&logo=github)](https://github.com/bandalgomim)
