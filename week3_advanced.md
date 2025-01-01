# Week3_advanced

## 1.어떤 Task를 선택하셨나요?

    1) MNLI: Premise와 Hypothesis 간의 중립 / 포함 / 모순 관계를 분류하는 자연어 추론(NLI) 문제입니다.
        - 과제 링크

    2) Translation: 영어(EN)에서 프랑스어(FR)로 번역하는 시퀀스-투-시퀀스(Seq2Seq) 문제입니다.
        - 과제 링크

## 2.모델은 어떻게 설계하셨나요? 설계한 모델의 입력과 출력 형태는 어떻게 되나요?

    1) MNLI:

        1) 모델 구조: DistilBERT 기반의 텍스트 분류 모델.
        
        2) 특징: 경량화된 인코더 모델인 DistilBERT를 사용하였으며, 분류 개수(3개 클래스)에 맞춰 
        
        3) Text Classifier를 추가하였습니다.
        
        4) 입력: Premise와 Hypothesis 두 텍스트.
        
        5) 출력: 중립 / 포함 / 모순 중 하나의 클래스 라벨.

    2) Translation:

        1) 모델 구조: T5 기반의 인코더-디코더 모델.
        
        2) 특징: 번역 문제를 처리하기 위해 T5를 사용하였습니다.
        
        3) 입력: 영어 문장 (EN).
        
        4) 출력: 번역된 프랑스어 문장 (FR).

## 3.데이터 입력 형태-Input의 형태가 어떻게 되나요?

    1) MNLI: 위 과제 링크의의 1.7번 셀 참조 (이미지 첨부 예정).

    2) Translation: 위 과제 링크의 1.3번 셀 참조 (이미지 첨부 예정).

## 4.Fine-tuning 결과 - Fine-tuning 전후의 결과 차이가 어떻게 되나요?

    1) MNLI: 링크의 4. 모델 결과 차트화 셀 참조 (이미지 첨부 예정).

    2) Translation: 링크의 4. 모델 결과 차트화 셀 참조 (이미지 첨부 예정).

## 5. 사용 Metric

    1) MNLI: Accuracy, F1 Score (weighted)

    2) Translation: BLEU Score

## 6. 한계 및 어려웠던 점

    1) MNLI: 

        1) 초반에 분류군 개수를 4개로 설정해서 코드는 오류 없이 돌아가는데, accuracy가 오르지 않는 문제가 있었다. 위와 같이 syntax 오류가 아닌 동작은 하는데 개념상 오류가 있는 것이 metric 상승을 가로막으면 이를 해결하는 것이 가장 어렵다.

        2) naive torch로 훈련시키는 코드를 huggingface trainer로 전환시키는데 어려움이 있었고 tokenize 함수와 data collator 함수의 관계를 이해하는데 시간이 걸렸다. 

    2) Machine Translation: 
    
        1) 모델이 t5로 바뀌고 seq2seq 구조를 다뤄야 하다보니, 이전에 배웠던 인코더 구조를 확장시켜 생각하는 것이 어려웠다. 

        2) t5의 파라미터 수가 많아 훈련 자체가 오래걸려 시행 착오 한 번이 과제 수행 시간을 너무 길게 늘리는 결과를 초래했다.

        3) 인코더 디코더 구조에서 어떤 layer까지 동결시키고 finetuning 해야 하는지 결정하는데 참고할 것이 없이 실험을 통해 알아야 해서, 참고할만한 것이 없다는 어려움이 있었다. 

        4) naive torch로 훈련시키는 코드를 huggingface trainer로 전환시키는데 어려움이 있었고 tokenize 함수와 data collator 함수의 관계를 이해하는데 시간이 걸렸다. 