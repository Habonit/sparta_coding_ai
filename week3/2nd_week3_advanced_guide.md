<aside> 💡 **이번 심화과제에서는 3주차에서 다룬 감정분석과 같은 task가 아닌 새로운 종류의 task들에 transfer learning을 적용해봅니다. 입력/출력 형태가 달라질 것이기 때문에 모델 구조를 다시 생각해봐야 하며, 특정 task들에서는 새로운 종류의 pre-trained 모델을 찾아서 활용해야 합니다.**

</aside>

## 준비

------

이번 주차에서 사용한 코드들을 모두 활용하시면 됩니다. 전체적인 구조는 아래 실습 코드와 유사하게 구현하시면 됩니다

[Google Colab](https://colab.research.google.com/drive/1Q8Co2FWHxjftQw3hZmk4SjF3lyse4MZR?usp=drive_link)

문서 하단의 템플릿을 사용하여 과제를 진행하시되, 팀원들과 함께 해결 하는 것을 추천드립니다.

## 목표

------

이번 심화과제에서는 직접 NLP task와 pre-trained 모델을 선정한 다음 학습 결과를 보고하시면 됩니다. 요구사항은 다음과 같습니다:

### 1. Task 설계

- 먼저 어떤 NLP task를 풀 것인지 정하셔야 합니다. 아래 세 가지 task 중 하나를 선택하시면 됩니다:

  - **Named entity recognition(NER):** NER은 문장의 단어들 중에서 개체명인지, 개체명이라면 어떤 종류인지(e.g., 사람 이름, 장소, 시간 등등) 분류하는 문제입니다. 즉, 감정 분석과 같은 sentence classification이 아니라 token classification 문제입니다. Dataset은 [이 링크](https://www.kaggle.com/datasets/debasisdotcom/name-entity-recognition-ner-dataset)를 활용하시면 됩니다. Test data는 주어진 train data를 split하셔서 만드셔야 합니다.

  - Multi-genre natural language inference(MNLI):

     MNLI는 두 문장이 주어졌을 때 논리적으로 연결이 되어 있는지, 서로 모순되는지, 아니면 아예 무관한지 분류하는 문제입니다. 감정 분석과 같은 sentence classification이지만 문장이 두 개 주어집니다. 

    이 링크

    의 data를 활용하시면 됩니다. Test data는 

    ```
    validation_matched.csv
    ```

    를 활용하시면 됩니다.

    - **MNLI는 training data의 양이 많아, 위 data 중 10,000개만 사용해 진행해 주세요!**

  - **Machine translation:** 기계 번역은 하나의 언어로 쓰여진 문장이 주어졌을 때 이를 원하는 언어의 문장으로 변환하는 문제입니다. 분류 문제가 아닌 sequence-to-sequence 문제입니다. 2주차에서 배운 encoder-decoder를 구현하셔야 합니다. Dataset는 [영어-프랑스어 번역 data](https://www.kaggle.com/datasets/devicharith/language-translation-englishfrench)를 사용하시면 됩니다. 기계 번역을 구현하시는 경우에는 train data에 대한 성능만 보고하셔도 됩니다.

### 2. Pre-trained 모델 선정

- [PyTorch](https://pytorch.org/hub/huggingface_pytorch-transformers/)에서 위에서 정한 task에 맞는 pre-trained 모델을 선정하시면 됩니다.
- 예를 들어 단순한 언어 이해 능력만을 필요로 한다면 이번 주차에서 사용한 BERT나 DistillBERT를 그대로 활용하시면 됩니다.
- 또는 기계 번역과 같이 영어 이외의 언어에 대한 이해 능력을 요구한다면 그에 맞는 pre-trained 모델을 찾으시면 됩니다.

### 3. 모델 학습

- 선택한 task의 data들을 가지고 pre-trained 모델을 fine-tuning하고 학습 결과를 확인하셔야 합니다.
- Pre-train하지 않은 Transformer를 똑같은 data로 학습하여 fine-tuning 결과와 비교해보셔야 합니다.
  - 비교 metric은 loss curve, accuracy, 또는 test data에 대한 generalization 성능 등을 활용하시면 됩니다.
  - 이외에도 기계 번역 같은 문제에서 활용하는 BLEU 등의 metric을 마음껏 활용 가능합니다.

<aside> 💡

**Task 선택과 과제 접근이 어려운 분들을 위한 Tip!**

- 추천 Task: MNLI
- 모델 설계: MNLI은 문장이 두 개 주어지는 Task입니다. 이를 문장 하나로 바꾸면 기존의 모델을 그대로 쓸 수 있는데, 여러가지 쉬운 방법들이 있을 것입니다. 두 문장을 한 문장으로 바꾸는 것은 `collate_fn`을 잘 수정하면 됩니다.
- Pre-trained 모델: 일반적인 영어 이해 Task이기 때문에 기존에 사용하던 distilbert를 그대로 사용해도 됩니다. </aside>

### 4. 요구사항 정리

위의 사항들을 구현한 뒤, 다음 질문들에 대한 답들을 보고서에 작성하여 제출하시면 됩니다:

- [ ]  어떤 task를 선택하셨나요?
- [ ]  모델은 어떻게 설계하셨나요? 설계한 모델의 입력과 출력 형태가 어떻게 되나요?
- [ ]  어떤 pre-trained 모델을 활용하셨나요?
- [ ]  실제로 pre-trained 모델을 fine-tuning했을 때 loss curve은 어떻게 그려지나요? 그리고 pre-train 하지 않은 Transformer를 학습했을 때와 어떤 차이가 있나요?

### 5. 예시

- 어떤 task를 선택하셨나요? → 저의 경우 NER task를 사용하였습니다.
- 모델은 어떻게 설계하셨나요? 설계한 모델의 입력과 출력 형태가 어떻게 되나요? → NER은 token classification 문제입니다. 그래서 3주차에서 활용한 구조를 그대로 사용하되, 마지막 출력이 token 하나가 아니라 입력 문장의 token 수 만큼 되도록 구조를 변경하였습니다.
- 어떤 pre-trained 모델을 활용하셨나요? → 영어 이해 능력과 제한된 GPU에서 빠르게 굴러갈 수 있는 모델이 필요하여 DistillBERT를 사용하였습니다.

## 제출자료

------

제약 조건은 전혀 없습니다. 위의 사항들을 구현하고 나온 결과들을 정리한 보고서를 [README.md](http://README.md) 형태로, 그리고 코드 및 실행 결과는 jupyter notebook 형태로 같이 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**).

## 심화과제 [Read.md](http://Read.md) 템플릿

------

```markdown
## Q1) 어떤 task를 선택하셨나요?
> NER, MNLI, 기계 번역 셋 중 하나를 선택

## Q2) 모델은 어떻게 설계하셨나요? 설계한 모델의 입력과 출력 형태가 어떻게 되나요?
> 모델의 입력과 출력 형태 또는 shape을 정확하게 기술

## Q3) 어떤 pre-trained 모델을 활용하셨나요?
> [PyTorch](<https://pytorch.org/hub/huggingface_pytorch-transformers/>)에서 위에서 정한 task에 맞는 pre-trained 모델을 선정

## Q4) 실제로 pre-trained 모델을 fine-tuning했을 때 loss curve은 어떻게 그려지나요? 그리고 pre-train 하지 않은 Transformer를 학습했을 때와 어떤 차이가 있나요? 
> 비교 metric은 loss curve, accuracy, 또는 test data에 대한 generalization 성능 등을 활용.
> +)이외에도 기계 번역 같은 문제에서 활용하는 BLEU 등의 metric을 마음껏 활용 가능
- 
-  
-  
- 이미지 첨부시 : ![이미지 설명](경로) / 예시: ![poster](./image.png)

### 위의 사항들을 구현하고 나온 결과들을 정리한 보고서를 README.md 형태로 업로드
### 코드 및 실행 결과는 jupyter notebook 형태로 같이 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**) 
```