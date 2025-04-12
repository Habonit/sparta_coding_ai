# Knowledge Distillation 이해와 DeepSeek-R1 사례 정리

본 글은 Knowledge Distillation(KD)의 개념적 기초부터 최근의 활용 사례인 DeepSeek-R1까지를 종합적으로 정리한 자료입니다. KD의 원리를 이해하고, 현대 언어모델에서의 적용 방식의 차이를 파악하는 데 초점을 맞춥니다.

---

## 1. 개괄

2025년 초 공개된 **DeepSeek 모델**에서 가장 주목할 만한 점은 첫째, R1이라는 고성능 추론 모델의 소스가 공개되었다는 점과 둘째, 671B 규모의 모델을 distillation 기법을 통해 효과적으로 경량화했다는 점이었습니다. 특히 LLM 시장의 경쟁이 치열해지는 상황에서 모델 경량화는 중요한 돌파구가 될 수 있다는 판단 아래, 이번 글에서는 **Knowledge Distillation 개념**을 다시 정리해보고자 합니다.![](https://velog.velcdn.com/images/paradeigma/post/0a51a9a0-f1e7-432a-aefb-768f0876eaeb/image.png)

---

## 2. KD의 시작

Knowledge Distillation은 **Geoffrey Hinton이 2014년 NIPS Deep Learning Workshop에서 제안한 개념**으로, 복잡하고 큰 모델(또는 앙상블)의 지식을 더 작고 경량화된 모델로 전이시키기 위한 학습 방법입니다.

> 복잡한 모델이 훈련된 후에는, 우리는 '지식 증류(distillation)'라고 부르는 다른 종류의 훈련을 사용하여, 그 복잡한 모델로부터 더 배포에 적합한 작은 모델로 지식을 전달할 수 있습니다.

### 2.1 KD의 이유

- 복잡하고 성능 좋은 모델은 추론 속도가 느리고 배포에 부적합
- 작은 모델은 추론이 빠르고 가볍지만 성능이 부족함
- KD는 성능과 효율성 간 **트레이드오프**를 해결하기 위한 전략으로 등장

### 2.2 예시

- `bert-base-uncased` → `distilbert-base-uncased`  
- 약 109M → 약 67M 파라미터 감소, 학습 대상은 0.59M 수준

![](https://velog.velcdn.com/images/paradeigma/post/923d139c-6f78-4152-ad71-d8ad8fc9470b/image.png)

---

## 3. 초기 이론

![](https://velog.velcdn.com/images/paradeigma/post/1ae772a4-919b-49f8-a1fc-42063f09cf1a/image.png)

- **손실 구성**: KL Divergence + Cross Entropy
- **Temperature 사용**: 확률 분포를 평탄하게 만들어 부드러운 label 생성

![](https://velog.velcdn.com/images/paradeigma/post/7cee6f48-2a91-4397-8638-b33d943df213/image.png)

### 3.1 Temperature가 학습에 사용됨

![](https://velog.velcdn.com/images/paradeigma/post/59b469cc-6495-422c-94b5-ab49b9e75b4d/image.png)

- T ↑ → softmax 분포가 부드러워짐 → 비주류 클래스 정보도 학습 가능

### 3.2 정리

| 구분           | 목적                                       |
| -------------- | ------------------------------------------ |
| KD에서의 역할  | Soft target 분포 평탄화로 미세한 차이 학습 |
| LLM에서의 역할 | 다양한 출력 유도(top-k, sampling 등)       |

---

## 4. 언어모델에서의 Temperature

![](https://velog.velcdn.com/images/paradeigma/post/b569c06f-d3b2-4f04-b548-dc4450d8404f/image.png)

- 추론 시점에서 사용, 생성 다양성 확보 목적
- Top-k, Top-p와 연계되면 더욱 창의적인 결과 가능

![](https://velog.velcdn.com/images/paradeigma/post/c8a358d0-0cf8-4cd8-89c1-16c8347be258/image.png)

---

## 5. 언어모델에서의 Distillation

> DeepSeek-R1은 DeepSeek-R1이 생성한 80만 개의 샘플을 활용하여 Qwen 및 Llama와 같은 오픈소스 모델들을 SFT 방식으로 학습했다.

![](https://velog.velcdn.com/images/paradeigma/post/5d9691fc-b138-4a50-bf78-257a06f32c1e/image.png)

- Teacher: 사전학습된 DeepSeek-R1
- Student: LLaMA, Qwen 계열 모델
- 방법: Teacher의 출력을 정답으로 간주한 SFT

![](https://velog.velcdn.com/images/paradeigma/post/f9ae7607-1f77-40a2-a3fc-6791b8e55e5b/image.png)

---

## 6. 현재의 KD 예시

- GPT 결과를 정답처럼 사용해 소형 모델을 학습시키는 현대적 KD 구조
- DeepSeek-R1은 이 전략을 대규모로 확장하여 성공한 사례

---

## 7. 결론

- **초기 KD**: KL + CE + Temperature로 soft label 학습
- **현대 KD**: GPT 등 LLM의 응답을 정답으로 간주한 SFT 방식
- Temperature의 의미도 학습(초기) → 추론 다양성(현재)으로 전이됨

---

## 🔗 참고 링크

- [Distillation 논문 (Hinton, 2015)](https://arxiv.org/abs/1503.02531)
- [DeepSeek-R1 Distillation 사례](https://arxiv.org/abs/2501.12948)
- [Knowledge Distillation 발표자료 (PPTX)](week3/Knowledge%20Distillation.pptx)