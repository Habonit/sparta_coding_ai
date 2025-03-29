1. 1. # [1주차] 기본과제: MNIST를 분류 모델로 학습하기

      ### MNIST 분류(classification) 모델 구현

      <aside> 💡 이번 과제에서는 MNIST를 regression model이 아닌 classification model로 구현합니다. 그리고 train과 test data에 대한 모델의 정확도를 plot하여 generalization error를 살펴봅니다.

      </aside>

      ## 준비

      ------

      MNIST 실습을 진행한 notebook 위에서 과제를 수행하시면 됩니다:

      

      ## 목표

      ------

      MNIST 실습과 똑같은 task와 model을 사용합니다. 대신 다음의 목표들을 notebook에서 마저 구현해주시면 됩니다:

      - [ ]  Test data 준비하기

        - Test data는 MNIST의 train data를 load하는 코드에서 `train=False`로 두면 됩니다.
        - Train data와 마찬가지로 test data에 대한 data loader를 생성해주시면 됩니다(batch size는 동일하게 적용).
          - Test data는 랜덤하게 섞일 필요가 없기 때문에 `shuffle=False`로 설정합니다.

      - [ ]  

        ```
        nn.CrossEntropyLoss
        ```

         적용하기

        - 현재 코드는 regression model을 구현한 상태로, MSE를 loss로 사용하고 있습니다.

        - 하지만 MNIST와 같은 분류 문제에서는 MSE는 적합하지 않습니다.

          - MSE에 따르면 1에 해당하는 손글씨 이미지는 7에 해당하는 손글씨 이미지보다 0에 해당하는 손글씨 이미지가 더 가깝게 여겨집니다.
          - 하지만 1은 실제로 0보다 7과 더 비슷하게 생겼습니다.

        - 일반적으로 분류 문제는 MSE 대신 cross entropy loss를 사용합니다.

        - PyTorch에서의 

          cross entropy loss 문서

           또는 

          웹 자료

          들을 통해 이해한 후, MSE를 대체하는 코드를 구현하시면 됩니다.

          - 변경되어야 할 점은 2가지로 i) `Model`의 최종 output의 dimension과 ii) `loss` 계산 부분입니다.

      - [ ]  학습을 진행한 후, epoch에 따른 model의 train과 test data에 대한 정확도 plot하기

        - 다음 조건들 아래에서 학습을 진행하면 됩니다.

          - `n_epochs`=100, `batch_size`=256, `lr`=0.001.

        - 어떤 dataloader에 대한 model의 정확도를 측정하는 코드는 다음 함수를 사용하시면 됩니다:

          ```python
          def accuracy(model, dataloader):
            cnt = 0
            acc = 0
          
            for data in dataloader:
              inputs, labels = data
              inputs, labels = inputs.to('cuda'), labels.to('cuda')
          
              preds = model(inputs)
              preds = torch.argmax(preds, dim=-1)
          
              cnt += labels.shape[0]
              acc += (labels == preds).sum().item()
          
            return acc / cnt
          ```

        - 추가적으로 구현되어야 하는 부분들을 요약하면 다음과 같습니다:

          - 매 epoch가 끝난 뒤의 model의 `trainloader`와 `testloader`에 대한 정확도를 각각 list로 저장해둡니다.

          - Epoch에 따른 train과 test data에 대한 model의 정확도를 다음 코드를 사용하여 plot합니다:

            ```python
            def plot_acc(train_accs, test_accs, label1='train', label2='test'):
              x = np.arange(len(train_accs))
            
              plt.plot(x, train_accs, label=label1)
              plt.plot(x, test_accs, label=label2)
              plt.legend()
              plt.show()
            ```

      ## 제출자료

      ------

      제약 조건은 전혀 없으며, 위의 사항들을 구현하고 plot이 1개 포함된 notebook을 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**).

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