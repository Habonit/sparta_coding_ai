# Week 4: Transformer Fine-tuning & Prompt-based Classification

---

## 프로젝트 개요
- **환경 가정**
  - Python 3.8+
  - PyTorch · Transformers · Datasets · scikit-learn
  - OpenAI API 키 설정 (.env)
- **목표**
  - DistilBERT 모델을 Huggingface Trainer로 Fine-tuning
  - Prompt 기반 분류 및 Retrieval-Augmented Generation(RAG) 구조 이해

---

## 1. Basic Assignment: DistilBERT Fine-tuning by Huggingface Trainer
### 1.1 Preparation
- 모델 및 데이터 설정  
  - `model_name="distilbert-base-uncased"`, `dataset="nyu-mll/glue"`(`mnli`)  
  - `batch_size=64`, `max_len=400`, `n_labels=3`
- 토크나이저 로드 & 토크나이징  
- EDA: 입력 길이 분포, 레이블 분포 확인

### 1.2 Training
- DataCollator 정의 (Dynamic Padding)  
- `TrainingArguments` 설정 (`learning_rate`, `epochs`, `logging_steps`)  
- `Trainer` 인스턴스 생성 및 학습 실행

### 1.3 Metric Check
- Initial / Train 로그 확인  
- `accuracy`, `f1`, `precision`, `recall` 계산  
- 학습 곡선 시각화

### 1.4 Classification by LLM
- Prompt Template 정의  
- Qwen / Mistral / SmallThinker 래퍼 클래스 구현  
- OpenAI API 호출로 분류 수행

### 1.5 Conclusion
- Fine-tuning vs Prompt-based 분류 결과 비교  
- 주요 성능 지표 요약

---

## 2. Advanced Assignment: Prompt-based Classification & RAG
### 2.1 Preparation
- `data/2023_11_KICE.json` 로드  
- Dataset 빌드 (paragraph-질문 쌍 변환)  
- Prompt 템플릿 로드

### 2.2 Prediction by Prompt
- 단순 Prompt 호출로 분류 수행  
- 결과 로그 및 예측 분포 확인

### 2.3 Prediction - RAG
- Paragraph Retrieval (유사도 기반 검색)  
- Context + Prompt 결합 후 RAG 분류  
- RAG 성능 로그 기록

### 2.4 Conclusion
- Prompt-only vs RAG 결과 비교  
- RAG 적용 시 정확도 및 정보 보완 효과

---

## 요약: 과제별 주요 구성
| 과제 구분            | 주요 내용                                          |
|---------------------|----------------------------------------------------|
| Basic Assignment    | DistilBERT Fine-tuning 및 LLM을 활용한 분류         |
| Advanced Assignment | Prompt 기반 분류 및 Retrieval-Augmented Generation |

---

## 실험 인사이트
- Fine-tuning은 높은 정확도를 보이지만 학습 비용 발생  
- RAG는 Context 보강으로 희소 정보 문제 완화 가능  
- Prompt 설계와 RAG 파이프라인 구조 이해 필수

---

## 질문
> **Q1. 왜 [CLS] 토큰을 분류기에 사용하는가?**  
> **Q2. Prompt 기반 분류와 Fine-tuning 중 어떤 방식이 더 비용-효율적인가?**

---
발표자료

본 발표에서 다룬 F1 점수와 ROC AUC 관련 심화 내용은 Velog 포스트 “플러스 AI 4. F1과 ROC AUC”(https://velog.io/@paradeigma/플러스-AI-4.-F1과-ROC-AUC)에서 확인할 수 있습니다

