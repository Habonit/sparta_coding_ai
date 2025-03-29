1. 1. ## 준비

      ------

      이번 주차의 LoRA 코드를 활용하시면 됩니다.

      ## 목표

      ------

      이번 실습에서는 LoRA에서 rank를 변화시켰을 때, 성능 및 메모리 사용량 차이를 살펴볼 것입니다. 기존의 LoRA 실습 코드를 그대로 사용하되, 다음 부분들을 report 하시면 됩니다:

      - [ ]  `lora_r`를 `[8, 128, 256]`로 변화시켜가며 학습

        - Deepspeed 없이 순수 LoRA만을 가지고 기존과 같은 LLM(`facebook/opt-350m`)과 dataset(`lucasmccabe-lmi/CodeAlpaca-20k`)를 활용합니다.

        - Rank를 8, 128, 256로 바꿔가며 학습을 진행해봅니다.

        - SFTTrainer는 다음과 같이 변경합니다:

          ```python
          trainer = SFTTrainer(
              model,
              train_dataset=dataset,
              args=SFTConfig(output_dir="/tmp/clm-instruction-tuning", **max_seq_length=128**),
              formatting_func=formatting_prompts_func,
              data_collator=collator,
          )
          trainer.train()
          ```

      - [ ]  Rank에 따른 loss, 학습 속도, 그리고 메모리 점유율 공유

        - Loss는 wandb를 활용하여 다음과 같은 log를 공유합니다.

          ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/b3073297-aa25-459d-bee9-0ed6843b447b/image.png)

        - 학습 속도 또한 wandb의 `Runtime` 항목을 공유합니다.

        - 메모리 점유율은 다음 코드를 적절히 추가하여 print한 후, 공유합니다.

          ```python
          print('Max Alloc:', round(torch.cuda.max_memory_allocated(0)/1024**3, 1), 'GB')
          ```

      - [ ]  LoRA의 장단점 분석

        - 위에서 공유한 loss, 학습 속도, 메모리 점유율 각각을 보고 LoRA의 장단점을 분석하시면 됩니다.

      ## 제출자료

      ------

      위의 요구사항들을 포함하고 있는 `README.md`를 공유해주시면 됩니다.