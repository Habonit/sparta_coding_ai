# Week 5: Vector Search & Local LangChain Chatbot

---

## 프로젝트 개요
- **환경 가정**
  - Python 3.8+
  - LangChain · Chroma · SentenceTransformers · faiss-cpu · Matplotlib
  - OpenAI API 키 및 로컬 모델 체크포인트 (.env)
- **목표**
  - Chroma DB 기반 텍스트 벡터 검색 파이프라인 구축
  - LangChain 사용해 로컬 모델 기반 질의응답 챗봇 구현

---

## 1. Basic Assignment: 텍스트 검색 파이프라인
### 1.1 Data Preparation
- API 키 로드: `load_dotenv(); os.getenv("OPENAI_API_KEY")`
- 모델 로드: `SentenceTransformer("all-MiniLM-L6-v2")` 또는 로컬 대체 모델
- 데이터 로드 및 전처리: JSON → DataFrame → 텍스트 클리닝 및 토큰화

### 1.2 Text Retrieval
- Chroma DB 초기화 및 컬렉션 생성
- 텍스트 임베딩 인덱싱: `add_texts()`를 통해 대량 데이터 색인
- 임베딩 메타데이터 확인 및 요약 통계 출력

### 1.3 Embedding Visualization
- PCA 또는 t-SNE 적용하여 고차원 벡터 차원 축소
- 2D 산점도로 주요 토픽 클러스터 시각화
- 시각화 결과 해석: 유사 토픽별 군집 효과 분석

### 1.4 Conclusion
- Chroma 기반 검색 정확도 및 응답 속도 평가 결과 요약
- 시각화로 데이터 구조 이해도 향상

---

## 2. Local LangChain Implementation: 로컬 챗봇 구축
### 2.1 Data Preparation
- Basic Assignment와 동일한 데이터 및 전처리 파이프라인 재사용
- Embedding 모델 선언 및 VectorStore 구성

### 2.2 Embedding & VectorStore
- `Embedding` 클래스 초기화 (OpenAIEmbeddings 또는 로컬 모델)
- VectorStore 생성: Chroma 또는 FAISS 설정 및 인덱싱 메커니즘 구현

### 2.3 Query & Generation
- LangChain 체인 구성: `RetrievalQA` 또는 `ConversationalRetrievalChain`
- 질의 및 응답 처리, 내부 Memory 관리
- 예시 대화 시나리오 및 출력 포맷 설명

### 2.4 Conclusion
- 검색 기반 질의응답 vs 챗봇 응답 비교
- 실시간 응답률, 토픽 일관성 및 정확도 평가

---

## 3. Advanced Assignment: 심화 과제
### 3.1 Preparation
- 데이터 로드 및 `RecursiveCharacterTextSplitter` 설정으로 문서 분할
- 고급 임베딩 모델 선택: `HuggingFaceEmbeddings`
- 사용자 정의 프롬프트 템플릿 설계 (시스템 메시지 / 유저 메시지 분리)

### 3.2 Chatbot Pipeline
- `LLMChain` 및 `ChatVectorDBChain` 구성 세부사항
- 컨텍스트 윈도우 관리 전략 및 역할 기반 프롬프트 적용
- 오류 처리 및 예외 관리 방안

### 3.3 Performance Analysis
- 응답 속도 벤치마킹 (지연 시간 측정 및 비교)
- 대화 품질 평가: BLEU, ROUGE 등의 지표 활용
- 성능 개선 포인트 및 최적화 제안

### 3.4 Conclusion
- Advanced Assignment 결과 요약
- 로컬 모델 vs 클라우드 모델 비교 인사이트

---

## 요약: 과제별 주요 구성
| 구분        | 주요 내용                                   |
|------------|---------------------------------------------|
| Basic      | Chroma 검색 및 벡터 시각화                  |
| LangChain  | 로컬 챗봇 파이프라인                        |
| Advanced   | 고급 텍스트 분할, 임베딩, 챗봇 성능 분석    |

---

## 실험 인사이트
- 벡터 DB 색인 시 응답 시간과 검색 정확도 간 트레이드오프 존재
- 차원 축소 기반 시각화로 데이터 분포 직관 확보 가능
- LangChain 체인 설계 시 Memory 관리와 Prompt 전략이 핵심
- RecursiveCharacterTextSplitter는 긴 문서 처리 효율성 개선

---

## 질문
> **Q1. Chroma DB와 FAISS 중 어떤 벡터 스토어가 더 적합한가?**  
> **Q2. 로컬 모델 챗봇에서 메모리 관리 전략은 어떻게 최적화할 수 있는가?**

---

