# Batch Size & Optimization Strategy 실험 (MNIST)

본 프로젝트는 MNIST 손글씨 데이터셋을 기반으로, 학습 시 사용되는 배치 사이즈(batch size) 및 최적화 방식(Batch / Mini-Batch / SGD)이 모델의 학습 성능과 효율성에 미치는 영향을 비교 실험한 결과를 정리한 것입니다.

---

## 프로젝트 개요

- 데이터셋: MNIST (28x28 흑백 이미지, 10 클래스)
- 모델 구조: MLP 기반 선형 분류기
- 목표: 동일한 모델 구조와 학습 조건에서 배치 사이즈에 따른 최적화 성능 차이 분석
- 실험 대상:
  - Batch Gradient Descent (batch size = 1024)
  - Mini-Batch Gradient Descent (batch size = 32)
  - Stochastic Gradient Descent (batch size = 1)

---

## 모델 구조

```python
Model(
  (hidden_layers): ModuleDict(
    (layer1_layer): Linear(784 → 1024)
    (layer1_activation): ReLU
    (layer2_layer): Linear(1024 → 1024)
    (layer2_activation): ReLU
  )
  (final_layer): Linear(1024 → 10)
)
```

## 모델 구성

MNIST 이미지를 1D 벡터(784차원)로 변환한 뒤, ReLU 활성화 함수를 포함한 두 개의 은닉층을 거쳐 10차원의 출력값으로 분류합니다. CNN은 사용하지 않고 순수한 MLP 구조를 기반으로 하여, 배치 사이즈에 따른 최적화 방식의 영향을 집중적으로 실험하기 위한 모델입니다.

---

## 실험 구성

| 실험 이름 | 배치 크기 | 특징                                      |
|-----------|-----------|-------------------------------------------|
| case1     | 1024      | 안정적이나 업데이트 적음, 느린 성능 향상 |
| case2     | 32        | 실무에서 일반적, 빠르고 안정적인 학습    |
| case3     | 1         | 가장 많은 업데이트, 불안정성 높음        |

- 모든 실험은 동일한 모델 구조, 옵티마이저(SGD), 손실 함수(CrossEntropyLoss)를 사용합니다.
- 에폭 수는 30으로 고정하며, 각 실험의 train/val 성능 및 학습 시간 기록을 수집합니다.

---

## 주요 결과 요약

### case1 (Batch size = 1024)
- **장점**: GPU 연산 효율이 높고, 안정적인 수렴 경향을 보임
- **단점**: 업데이트 횟수가 적어 학습 속도 느림

### case2 (Batch size = 32)
- **결과**: 가장 빠른 수렴과 높은 정확도를 보이며
- **평가**: 성능과 효율성 모두에서 균형 잡힌 결과를 달성

### case3 (Batch size = 1, SGD)
- **현상**: 학습 초반부터 `loss = NaN`, `accuracy ≈ 10%`로 학습 실패
- **원인 분석**:
  - 작은 배치로 인한 gradient 분산 증가
  - 높은 learning rate로 인해 파라미터 발산
  - CrossEntropyLoss 내부의 log(softmax)에서 수치적 불안정성 발생

---

## 실험 인사이트

- 배치 사이즈가 작을수록 gradient의 노이즈가 커지므로 학습률을 더 세밀하게 조정해야 함
- 배치 사이즈가 클수록 1 에폭당 소요 시간은 짧지만, 충분한 학습을 위해 더 많은 에폭이 필요함
- 실무에서는 mini-batch 방식이 학습 효율성과 성능 면에서 가장 안정적이고 실용적인 선택임