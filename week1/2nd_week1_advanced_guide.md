1. ## 준비

   ------

   기본 과제를 구현한 notebook에서 진행하시면 됩니다.

   ## 목표

   ------

   Classification model을 MNIST에 적용한 코드에서 다음 부분들을 변경하거나 추가해주시면 됩니다:

   - [ ]  Dataset 및 activation 함수 변경

     - Dataset을 MNIST에서 [CIFAR10](https://pytorch.org/vision/stable/generated/torchvision.datasets.CIFAR10.html)으로 변경해줍니다.
     - Activation 함수를 `nn.ReLU`에서 `nn.LeakyReLU`로 변경해줍니다.
     - 학습 인자는 `n_epochs` = 50, `batch_size` = 256로 설정합니다.

   - [ ]  CIFAR10의 입력 shape 확인

     - CIFAR10은 MNIST와 다른 입력 shape을 가지고 있습니다.
     - 입력 shape은 model을 선언할 때 중요하기 때문에 MNIST 실습 자료에서 사용한 방식과 똑같이 shape을 확인해주시면 됩니다.

   - [ ]  SGD와 Adam 성능 비교

     - 먼저 [Adam optimizer](https://pytorch.org/docs/stable/generated/torch.optim.Adam.html)을 사용하여 학습하는 코드를 구현합니다.
     - (Plot 1) SGD와 Adam을 학습시킨 후 각각의 epoch에 대한 train 정확도를 plot합니다.

   - [ ]  Leaky ReLU와 Sigmoid 성능 비교

     - Activation 함수가 `nn.Sigmoid`인 class를 새로 정의합니다.
     - (Plot 2) Adam optimizer를 가지고 sigmoid와 leaky ReLU 모델들을 학습한 후, epoch에 따른 train 정확도를 비교합니다.

   - [ ]  Dropout을 적용한 이후의 generalization error 확인

     - PyTorch [dropout](https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html)을 leaky ReLU를 사용하는 MLP의 모든 layer에 적용한 class를 새로 정의합니다. Dropout 확률은 0.1로 설정합니다.

     - 학습 코드에서 다음 부분들을 추가해줍니다:

       - `model.train()`을 `for data in trainloader:` 이전 줄에 둡니다.

       - `trainloader`와 `testloader`에 대한 정확도를 계산하는 코드를 다음과 같이 변경합니다:

         ```python
         with torch.no_grad():
           model.eval()
           <기존 정확도 계산 코드>
         ```

     - (Plot 3) Adam optimizer를 가지고 dropout을 적용한 모델을 학습한 후, epoch에 따른 train과 test 정확도를 비교합니다.

   ## 제출자료

   ------

   제약 조건은 전혀 없으며, 위의 사항들을 구현하고 plot이 3개 포함된 notebook을 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**).

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
   4. 주의 : ipynb 파일 링크가 아닌, Repository 링크 또는 파이썬 파일을 제출하면 채점이 불가합니다.