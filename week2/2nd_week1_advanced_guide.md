<aside> 💡 **이번 과제에서는 Transformer encoder의 완전한 형태를 구현합니다. Self-attention을 multi-head attention으로 확장하고 layer normalization, dropout, residual connection 등의 technique을 적용하여 감정 분석 성능을 확인해봅니다.**

</aside>

## 준비

------

Transformer 실습을 진행한 notebook 위에서 진행해주시면 됩니다:



## 목표

------

- [ ]  Multi-head attention(MHA) 구현

  - Self-attention module을 MHA로 확장해주시면 됩니다. 여기서 MHA는 다음과 같이 구현합니다.
    1. 기존의 $W_q, W_k, W_v$를 사용하여 $Q, K, V$를 생성합니다. 이 부분은 코드 수정이 필요 없습니다.
    2. $Q, K, V \in \mathbb{R}^{S \times D}$가 있을 때, 이를 $Q, K, V \in \mathbb{R}^{S \times H \times D’}$으로 reshape 해줍니다. 여기서 $H$는 `n_heads`라는 인자로 받아야 하고, $D$가 $H$로 나눠 떨어지는 값이여야 하는 제약 조건이 필요합니다. $D = H \times D’$입니다.
    3. $Q, K, V$를 $Q, K, V \in \mathbb{R}^{H \times S \times D’}$의 shape으로 transpose해줍니다.
    4. $A = QK^T/\sqrt{D'} \in \mathbb{R}^{H \times S \times S}$를 기존의 self-attention과 똑같이 계산합니다. 이 부분은 코드 수정이 필요 없습니다.
    5. Mask를 더합니다. 기존과 $A$의 shape이 달라졌기 때문에 dimension을 어떻게 맞춰줘야할지 생각해줘야 합니다.
    6. $\hat{x} = \textrm{Softmax}(A)V \in \mathbb{R}^{H \times S \times D'}$를 계산해주고 transpose와 reshape을 통해 $\hat{x} \in \mathbb{R}^{S \times D}$의 shape으로 다시 만들어줍니다.
    7. 기존과 똑같이 $\hat{x} = \hat{x} W_o$를 곱해줘서 마무리 해줍니다. 이 또한 코드 수정이 필요 없습니다.

- [ ]  Layer normalization, dropout, residual connection 구현

  - 다시 `TransformerLayer` class로 돌아와서 과제를 진행하시면 됩니다.

  - Attention module을 $MHA$, feed-forward layer를 $FFN$이라고 하겠습니다.

  - 기존의 구현은 다음과 같습니다:

    ```python
    # x, mask is given
    
    x1 = MHA(x, mask)
    x2 = FFN(x1)
    
    return x2
    ```

  - 다음과 같이 수정해주시면 됩니다.

    ```python
    # x, mask is given
    
    x1 = MHA(x, mask)
    x1 = Dropout(x1)
    x1 = LayerNormalization(x1 + x)
    
    x2 = FFN(x1)
    x2 = Dropout(x2)
    x2 = LayerNormalization(x2 + x1)
    
    return x2
    ```

  - 여기서 `x1 + x`와 `x2 + x1`에 해당하는 부분들은 residual connection이라고 부릅니다.

- [ ]  5-layer 4-head Transformer

  - 기존 실습에서 사용한 hyper-parameter들과 위에서 구현한 Transformer를 가지고 5-layer 4-head Transformer의 성능 결과를 report해주시면 됩니다.

## 제출자료

------

제약 조건은 전혀 없으며, 위의 사항들을 구현하고 epoch마다의 train accuracy와 test accuracy가 print된 notebook을 public github repository에 업로드하여 공유해주시면 됩니다(**반드시 출력 결과가 남아있어야 합니다!!**).

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