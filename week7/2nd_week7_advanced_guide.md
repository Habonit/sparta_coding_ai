## 준비

------

이전 챕터에서 작성한 GPT pre-training과 instruction-tuning 코드를 활용하시면 됩니다.

## 목표

------

이번 과제에서는 LLM instruction-tuning을 HuggingFace hub의 data를 활용하는 것이 아닌 자체 제작한 data를 활용하여 학습하는 것이 목표입니다.

자세한 요구사항들은 다음과 같습니다:

- [ ]  Instruction-data 준비
  - 먼저 text corpus를 `corpus.json`의 이름으로 준비합니다.
  - Corpus의 형식은 제한이 없고, 100개 이상의 sample들로 구성되어 있으면 됩니다.
- [ ]  Train 및 validation data 준비
  - 먼저 `corpus.json`를 불러옵니다.
  - 그 다음 8:2 비율로 나눠, train과 validation data를 나눕니다.
  - 그 다음 기존의 data 전처리 코드를 적절히 수정하여 불러온 train과 validation data를 전처리합니다.
- [ ]  학습 결과 공유
  - 직접 만든 data로 GPT를 fine-tune한 후 저장된 `train/loss`과 `valid/loss`에 대한 wandb log를 공유해주시면 됩니다.

## 제출자료

------

위의 요구사항들이 만족된 `train.py`을 public github repository에 업로드하여 공유해주시면 됩니다. 그리고 공유할 wandb log 링크를 README.md를 만들어 적어놓으시면 됩니다.