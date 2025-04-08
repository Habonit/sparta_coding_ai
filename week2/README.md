# Transformer 파라미터 수 계산 실험

본 프로젝트는 Transformer 모델의 구조를 수치적으로 깊이 이해하기 위해, 각 레이어에서 생성되는 파라미터 수를 **직접 계산**해보는 실험입니다. 단순히 구조를 이해하는 데서 그치지 않고, 학습 가능한 가중치가 실제로 어떤 층에서 얼마나 발생하는지를 확인함으로써 모델 구조에 대한 직관을 키우는 데 목적이 있습니다.

---

## 프로젝트 개요

- **환경 가정**
  - Batch Size: 1
  - Vocab Size: 30,522
  - Max Sequence Length: 400
  - d_model: 32
  - dff (Feed Forward 차원): 32
  - Attention Heads: 4
  - 레이어 수: 5
- **목표**: 각 레이어별 파라미터 수를 직접 계산하여 구조적 이해 강화

---

## 1. Transformer의 장단점

### 1.1 장점
- 토큰 간의 관계를 학습할 수 있는 Attention 구조
- 병렬 처리 가능 (순차적 처리 X)

### 1.2 단점
- 연산량 및 파라미터 수가 많아 복잡함

---

## 2. Embedding

### 2.1 Token Embedding
- `nn.Embedding(vocab_size, d_model)` 사용
- 파라미터 수: `30522 × 32 = 976,704`

### 2.2 Positional Encoding
- Sin/Cos 기반 위치 인코딩 (학습 불가)
- 학습 파라미터 수: `0`
- 발생 파라미터 수: `400 x 32 = 12,800`

---

## 3. Multi-Head Attention

### 3.1 Q, K, V 선형 레이어
- 각 가중치: `32×32 + 32 = 1056`
- 총: `1056 × 3 (Q, K, V) = 3168`

### 3.2 최종 Dense 레이어
- 파라미터 수: `1056`

### 총 파라미터 수: `4224`

---

## 4. Layer Normalization

- γ, β 각각 d_model 차원만큼 생성
- 파라미터 수: `32 × 2 = 64`

---

## 5. Feed Forward Network (FFN)

- 구조: `Linear(32 → 32) → ReLU → Linear(32 → 32)`
- 각 레이어: `32×32 + 32 = 1056`
- 총 파라미터 수: `1056 × 2 = 2112`

---

## 6. 두 번째 Layer Normalization

- 파라미터 수: `64` (위와 동일)

---

## 7. Classifier

- [CLS] 토큰 사용 (`x[:, 0]`)
- Linear(32 → 1)
- 파라미터 수: `32×1 + 1 = 33`

---

## 요약: 주요 레이어별 파라미터 수

| 구성 요소            | 파라미터 수 |
|---------------------|-------------|
| Token Embedding     | 976,704     |
| Positional Encoding | 12,800(학습 = 0) |
| Multi-Head Attention| 4,224       |
| LayerNorm (1)       | 64          |
| FFN                 | 2,112       |
| LayerNorm (2)       | 64          |
| Classifier          | 33          |
| **총합 (1 Layer 기준)**   | **983,201** |

---

## 실험 인사이트

- Self-Attention은 Q, K, V 선형 변환으로 가장 많은 파라미터를 사용
- Positional Encoding은 수치적으로는 0개의 학습 파라미터
- FFN 또한 많은 파라미터를 차지하며, 성능 향상에 핵심적 기여
- LayerNorm은 파라미터 수는 적지만 학습 안정성에 중요한 역할
- Classifier는 구조상 간단하나 최종 출력에 결정적 영향

---

## 질문

> **Q1. 왜 [CLS] 토큰을 분류기에 사용하는가? [SEP] 토큰이 아닌 이유는?**  

> **Q2. 왜 Batch Normalization이 아닌 Layer Normalization을 사용하는가?**  

---

## 발표자료

- [Transformer 파라미터 발표자료 PPT](https://docs.google.com/presentation/d/14cJSaRBWkv99sa3OY4heF9m9S-RU3eCHt43yOUqupWk/edit?usp=drive_link)
