# Week 6: Correctly Project Midterm Review

---

## 프로젝트 개요
- **환경 가정**
  - Python 3.8+, Docker & Docker Compose, Airflow, MLflow  
  - 모델: Gemma3 1B (교정), GPT-4o-mini (검증)  
  - DB: PostgreSQL, Chroma/FAISS (메타데이터 조회)  
  - UI 검증: OpenWebUI  
- **목표**
  1. 어색한 영어 문장을 교정하고 이유를 설명하는 챗봇 개발   
  2. 훈련 파이프라인에 MLOps Level 1.5 구성 및 자동화  
  3. 훈련 전후 성능 검증 및 GPT-4o-mini 비교  
  4. Task 고정 시 작은 모델의 성능 한계 및 가능성 평가 :contentReference[oaicite:0]{index=0}:contentReference[oaicite:1]{index=1}

---

## 1. Part 1: 프로젝트 목표
### 1.1 Chatbot Concept
- 입력된 문장의 어색한 부분을 교정하고, 수정 이유를 자연어로 설명  
- 예시  
  ```text
  Before: If I had to assess someone like a job interview…
  After : If I were evaluating someone in a job interview…
  Reason: 자연스러운 가정법 구현을 위해 ‘were evaluating’, 전치사 ‘in’ 추가 :contentReference[oaicite:2]{index=2}:contentReference[oaicite:3]{index=3}

### 1.2 MLOps & Validation
- **MLOps Level 1.5**  
  - 코드·데이터 버전 관리(Git, DVC)  
  - Airflow DAG로 데이터 생성 및 배치 학습 자동화  
  - MLflow로 실험 추적 및 모델 관리  
- **검증 전략**  
  - OpenWebUI 기반 정성 평가(사용자 플로우 시뮬레이션)  
  - GPT-4o-mini와 성능·응답 비교  

---

### 2. Part 2: 주의사항
- 민감 정보(PII) 제거 및 프라이버시 보호 필터링  
- 프롬프트 일관성 유지: 토큰 길이 제한 및 형식 검증  
- 부적절·비속어 필터링 정책 수립  
- 리소스(CPU/GPU) 사용량 모니터링  

---

### 3. Part 3: 달성 사항 (4가지)
#### 3.1 검증 방법: OpenWebUI 정성 평가
- 사용자 시나리오 기반 인터랙션 테스트  
- 교정 품질 및 설명 자연스러움 평가  

#### 3.2 MLOps 1.5 단계 구성
- Airflow DAG 자동화: 데이터 파이프라인 구축  
- MLflow 실험 추적: 하이퍼파라미터·메트릭 기록  
- Docker & Compose로 일관된 개발·배포 환경 확보  

#### 3.3 범용적 DB 스키마 구성
- **Text Repository** (공통 속성): Form, Tone/Sentiment, Length 등  
- **Correction Script** (개별 속성): Style, Key Expressions, Speaker Details  
- **프롬프트 정책**:  
  - One‐shot 예시 기반 데이터 생성  
  - `{form_form}`, `{mapping_emotion_form_tone}`, `{example_example}` 플레이스홀더 설계  

#### 3.4 Airflow DAG 기반 데이터 생성
- `example` 테이블용 레코드 자동 삽입 DAG 구현  
- Docker 환경 내에서 스케줄링 테스트 완료  
- 참고: Docker / Docker Compose 설정  

---

### 4. Part 4: 미달성 사항 (2가지)
1. 사용자 대상 Web UI/서비스 배포 파이프라인 미구현  
2. 모델 재학습 자동화(Trigger 기반) 및 모니터링 시스템 미완료  

---

### 5. Part 5: 결론 및 향후 계획
- **결론**  
  - 텍스트 교정 챗봇의 기본 구조 및 MLOps 파이프라인 토대 마련  
  - 작은 모델로도 일관된 교정 성능 도출 가능성 확인  
- **향후 계획**  
  - Web UI 연동 및 실사용 테스트  
  - MLOps Level 2: 실시간 재학습·배포 자동화  
  - 성능 모니터링 대시보드 구축  

### 요약: 달성 vs 미달성
| 구분    | 주요 내용                                                |
|--------|----------------------------------------------------------|
| 달성   | OpenWebUI 검증, MLOps 1.5 자동화, DB 스키마, DAG 데이터 생성 |
| 미달성 | Web UI 배포, 실시간 재학습·모니터링                        |

### 실험 인사이트
- 예시 기반 One‐shot 프롬프트가 데이터 품질 일관성에 핵심  
- MLOps 1.5는 개발-운영 브릿지 역할, Level 2 전환 시 모니터링이 관건  
- 범용 DB 스키마로 언어·테스크 확장 용이  
- Airflow DAG로 배치 처리 자동화, 실시간 처리 고려 필요  

### 질문
> **Q1. Gemma3 1B와 대형 LLM 간 성능·자원 효율 비교 시 핵심 평가 지표는 무엇인가?**  
> **Q2. MLOps Level 2로 진화하기 위해 필수적인 구성 요소는 어떤 것들이 있는가?** 
