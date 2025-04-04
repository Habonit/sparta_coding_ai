<aside> 💡 **이번 과제에서는 pre-trained 된 DistilBERT를 뉴스 기사 분류 문제에 적용합니다.**

</aside>

## 준비

------

DistilBERT 실습 위에서 진행합니다:

[Google Colab](https://colab.research.google.com/drive/1Q8Co2FWHxjftQw3hZmk4SjF3lyse4MZR?usp=drive_link)

## 목표

------

- [ ]  AG_News dataset 준비

  - Huggingface dataset의 `fancyzhx/ag_news`를 load합니다.

  - ```
    collate_fn
    ```

     함수에 다음 수정사항들을 반영하면 됩니다.

    - Truncation과 관련된 부분들을 지웁니다.

- [ ]  Classifier output, loss function, accuracy function 변경

  - 뉴스 기사 분류 문제는 binary classification이 아닌 일반적인 classification 문제입니다. **MNIST 과제(링크)**에서 했던 것 처럼 `nn.CrossEntropyLoss` 를 추가하고 `TextClassifier`의 출력 차원을 잘 조정하여 task를 풀 수 있도록 수정하시면 됩니다.
  - 그리고 정확도를 재는 `accuracy` 함수도 classification에 맞춰 수정하시면 됩니다.

- [ ]  학습 결과 report

  - DistilBERT 실습과 같이 매 epoch 마다의 train loss를 출력하고 최종 모델의 test accuracy를 report합니다.

## 제출자료

------

제약 조건은 전혀 없으며, 위의 사항들을 구현하고, epoch마다의 train loss와 최종 모델의 test accuracy가 print된 notebook을 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**).

## 과제 제출 방법 상세

------

1. 과제 목표에 대한 체크 리스트에 따라 Colab 노트북에서 과제를 수행합니다.

2. 노트북 중간 중간 텍스트 블록(마크 다운)을 추가해 아래 항목을 표기합니다.

   1. 텍스트는 모두 ##(제목2) 형식으로 기재합니다.

      ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/e8830910-0593-477b-914d-5ca1c6c16666/image.png)

   2. 아래 3가지를 형식에 맞추어 기재합니다.

| 항목                  | 제목 형식  | 내용                                   | 예시                                                         |
| --------------------- | ---------- | -------------------------------------- | ------------------------------------------------------------ |
| 수행한 부분           | [MY CODE]  | 수행한 내용에 대한 설명                | [MY CODE] Test data 준비하기                                 |
| 출력 결과가 남은 부분 | [LOG]      | 출력 로그 설명                         | [LOG] 학습 과정에서의 Epoch별 손실값 출력                    |
| 피드백 요청 부분      | [FEEDBACK] | 질문 또는 개선 요청 내용을 간결히 정리 | [FEEDBACK] 정확도를 더 높이려면 어떻게 해야 할지 궁금합니다! |

1. 개인 GitHub에 Public Repository를 하나 생성합니다.

   1. 반드시 접근이 가능하도록  Public으로 생성해 주세요!

2. 과제 진행이 모두 완료되면 GitHub에 사본을 가져옵니다.

   1. 파일 > GitHub에 사본 저장 > 저장소 선택 > 확인

3. GitHub으로 가져온 ipynb 파일 링크를 과제 제출 페이지에 제출합니다.

   ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/7dbc6e75-0567-4036-8ee8-e14c3989c783/image.png)

4. 주의 : ipynb 파일 링크가 아닌, Repository 링크 또는 파이썬 파일을 제출하면 채점이 불가합니다.