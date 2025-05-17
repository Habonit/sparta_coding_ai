# Week 7: SFT vs DPO Instruction Tuning Experiment
## 목적

SFT(Supervised Fine-Tuning)와 DPO(Direct Preference Optimization)가 어떻게 다른 학습 결과를 보여주는지 비교하는 것이 본 실험의 목적입니다.

실험에 사용된 instruction 주제는 다음과 같습니다:

> 통한 세 개의 키워드를 마지막으로 포함하여 한 편의 문학적인 글을 생성하는 것

이 타스크는 단일 정답이 존재하지 않고, 다양성과 표현력이 요구되는 유형으로 SFT와 DPO의 차이를 검증하기에 적합합니다.

---

## 실험 구성

| 항목            | 내용                             |
| -------------- | -------------------------------- |
| **모델**        | `gemma-3b-it` (1B 파라미터)       |
| **데이터 수**    | 전체 175개 (학습 140 / 검증 35)   |
| **Batch Size** | 8                                |
| **Epochs**     | 5                                |
| **SFT 데이터**   | `input` / `output`               |
| **DPO 데이터**   | `prompt` / `chosen` / `rejected` |

### 🔗 코드 및 데이터

* [SFT 코드](https://drive.google.com/file/d/1JOomO7meDWxhtmPyT0rWkCughDj4yq7S/view?usp=sharing)
* [DPO 코드](https://drive.google.com/file/d/1SK3tY_XnDfLBtxmgGnErL67KQONkGyus/view?usp=sharing)
* [데이터셋](https://drive.google.com/file/d/1vce0jzN1R9iinUkDp2SosnsgilTEo1-s/view?usp=sharing)

---

## 시험 결과 요약

### 사전 출력

* 학습 전 출력 결과는 키워드 미포함, 일관성 부족 등 전형적인 **1B 모델 수준**의 한계 존재

### SFT 결과

* **악성 반복 현상** 발생
* 다양한 표현을 포착하지 못하고, 학습이 이루어지지 않은 등형 패턴
* 단일 정답만을 강제로 학습시키는 결과로 해석됨

### DPO 결과

* 완성도는 낮지만, **문학적 구성과 통에 대한 방향성 있는 학습** 이루어지냈음
* 제한된 학습량에도 불구한것치고 **출력 구성 유지**
* 선호 기반 학습이 창의성 타스크에 더 적합함을 시상

---

## 결론

* 동일한 모델, 데이터, 조건에서 **SFT와 DPO는 전혀 다른 학습 결과**를 유동
* 특히 \*\*창의적 생성(task with multiple valid outputs)\*\*에서는 DPO가 더 효과적인 선택지임
* 후일에는 SFT를 기반으로 모델을 정력한 뒤, **DPO를 적용해 미세한 정력과 자연스러운 화면을 강화하는 하이브리드 방식**이 더욱 일반화될 것으로 예상

---

## 주요 인사이트

* SFT는 “정답이 하나일 때” 강력하지만, 창의성이 요구되는 테스크에서는 불적합 가능
* DPO는 “선호” 기반의 정답이 가능해 표현 다양성이 중요한 테스크에서 유리
* `gemma3-it-1b`  기분 모델도 DPO 적용 시 일정 수준의 구성적 일관성 학습 가능
