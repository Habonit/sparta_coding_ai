1. 1. ## 준비

      ------

      이번 주차에서 작성한 GPT fine-tuning 코드를 활용하시면 됩니다.

      - 코드 통합 버전

        ```python
        import os
        import sys
        import math
        import torch
        import wandb
        import logging
        import datasets
        import argparse
        import evaluate
        import transformers
        
        from typing import Optional
        from itertools import chain
        from dataclasses import dataclass, field
        
        from datasets import load_dataset
        from transformers import (
            AutoConfig,
            AutoModelForCausalLM,
            AutoTokenizer,
            HfArgumentParser,
            Trainer,
            TrainingArguments,
            default_data_collator
        )
        from transformers.trainer_utils import get_last_checkpoint
        
        wandb.init(project='Hanghae99')
        wandb.run.name = 'gpt-finetuning'
        
        @dataclass
        class Arguments:
            model_name_or_path: Optional[str] = field(default=None)  # HuggingFace hub에서 pre-trained 모델로 사용할 모델의 이름
            torch_dtype: Optional[str] = field(default=None, metadata={'choices': ['auto', 'bfloat16', 'float16', 'float32']})  # 우리 모델의 precision(data type이라고 이해하시면 됩니다)
        
            dataset_name: Optional[str] = field(default=None)  # Fine-tuning으로 사용할 huggingface hub에서의 dataset 이름
            dataset_config_name: Optional[str] = field(default=None)  # Fine-tuning으로 사용할 huggingface hub에서의 dataset configuration
            block_size: int = field(default=1024)  # Fine-tuning에 사용할 input text의 길이
            num_workers: Optional[int] = field(default=None)  # Data를 업로드하거나 전처리할 때 사용할 worker 숫자
            
        parser = HfArgumentParser((Arguments, TrainingArguments))
        args, training_args = parser.parse_args_into_dataclasses()
        
        logger = logging.getLogger()
        
        logging.basicConfig(
            format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
            datefmt="%m/%d/%Y %H:%M:%S",
            handlers=[logging.StreamHandler(sys.stdout)],
        )
        
        if training_args.should_log:
            transformers.utils.logging.set_verbosity_info()  # log level을 INFO로 변경 
        
        log_level = training_args.get_process_log_level()
        
        # 우리가 가지고 있는 logger와 HuggingFace의 logger의 log level 설정
        logger.setLevel(log_level)
        datasets.utils.logging.set_verbosity(log_level)
        transformers.utils.logging.set_verbosity(log_level)
        
        # 기타 HuggingFace logger option들을 설정
        transformers.utils.logging.enable_default_handler()
        transformers.utils.logging.enable_explicit_format()
        
        logger.info(f"Training/evaluation parameters {training_args}")
        
        raw_datasets = load_dataset(
            args.dataset_name,
            args.dataset_config_name
        )
        
        config = AutoConfig.from_pretrained(args.model_name_or_path)
        tokenizer = AutoTokenizer.from_pretrained(args.model_name_or_path)
        model = AutoModelForCausalLM.from_pretrained(
            args.model_name_or_path,
            config=config,
            torch_dtype=args.torch_dtype
        )
        
        tokenizer.pad_token_id = tokenizer.eos_token_id
        tokenizer.chat_template = "{% for message in messages %}{{'<|im_start|>' + message['role'] + '\\n' + message['content'] + '<|im_end|>' + '\\n'}}{% endfor %}"
        
        embedding_size = model.get_input_embeddings().weight.shape[0]
        if len(tokenizer) > embedding_size:
            model.resize_token_embeddings(len(tokenizer))
        
        column_names = list(raw_datasets["train"].features)
        text_column_name = "text" if "text" in column_names else column_names[0]
        
        def tokenize_function(examples):
            output = tokenizer(examples[text_column_name])
            return output
            
        with training_args.main_process_first(desc="dataset map tokenization"):
            tokenized_datasets = raw_datasets.map(
                tokenize_function,
                batched=True,
                num_proc=args.num_workers,
                remove_columns=column_names
            )
        
        max_pos_embeddings = config.max_position_embeddings if hasattr(config, "max_position_embeddings") else 1024
        block_size = args.block_size if tokenizer.model_max_length is None else min(args.block_size, tokenizer.model_max_length)
        
        def group_texts(examples):
            # 주어진 text들을 모두 concat 해줍니다. 
            # 예를 들어 examples = {'train': [['Hello!'], ['Yes, that is great!']]}이면 결과물은 {'train': ['Hello! Yes, that is great!']}가 됩니다.
            concatenated_examples = {k: list(chain(*examples[k])) for k in examples.keys()}
            
            # 전체 길이를 측정합니다.
            total_length = len(concatenated_examples[list(examples.keys())[0]])
            total_length = (total_length // block_size) * block_size
            
            # block_size로 text를 쪼갭니다.
            # 예를 들어 block_size=3일 때 {'train': ['Hello! Yes, that is great!']}는
            # {'train': ['Hel', 'lo!', ' Ye', 's, ', 'tha', ...]}가 됩니다. 
            result = {
                k: [t[i : i + block_size] for i in range(0, total_length, block_size)]
                for k, t in concatenated_examples.items()
            }
            
            # Next token prediction이니 label은 자기 자신으로 설정합니다.
            result["labels"] = result["input_ids"].copy()
            return result
            
        with training_args.main_process_first(desc="grouping texts together"):
            lm_datasets = tokenized_datasets.map(
                group_texts,
                batched=True,
                num_proc=args.num_workers
            )
            
        train_dataset = lm_datasets["train"]
        
        trainer = Trainer(
            model=model,
            args=training_args,
            train_dataset=train_dataset,
            tokenizer=tokenizer,
            data_collator=default_data_collator
        )
        
        checkpoint = None
        last_checkpoint = get_last_checkpoint(training_args.output_dir)  # 만약 output_dir에 checkpoint가 남아있으면 이를 사용하고, 없으면 None이 return됩니다.
        if training_args.resume_from_checkpoint is not None:  # output_dir이 아닌 다른 위치에서의 checkpoint를 resume_from_checkpoint로 지정할 수 있습니다.
            checkpoint = training_args.resume_from_checkpoint
        else:  # 아니면 last_checkpoint로 checkpoint를 지정합니다.  
            checkpoint = last_checkpoint
            
        train_result = trainer.train(resume_from_checkpoint=checkpoint)
        
        trainer.save_model()
        
        metrics = train_result.metrics
        trainer.log_metrics("train", metrics)
        trainer.save_metrics("train", metrics)
        trainer.save_state()
        ```

      ## 목표

      ------

      이번 과제에서는 GPT fine-tuning을 할 때 validation data를 두어 validation loss도 같이 측정하는 코드를 구현하면 됩니다.

      자세한 요구사항들은 다음과 같습니다:

      - [ ]  Validation data 준비

      - [ ]  학습 시 validation loss 계산

        - Trainer를 정의할 때 validation data를 추가하고 validation data에 대한 evaluation을 진행하도록 수정합니다. 이전 주차들의 코드를 참고하시면 쉽게 구현할 수 있습니다.

        - 실제로 학습 후, 

          ```
          train/loss
          ```

          와 

          ```
          eval/loss
          ```

           에 해당하는 wandb log를 공유해주시면 됩니다. 공유 방법은 다음과 같습니다:

          1. 공유하고자 하는 log의 “Share panel” 메뉴를 누릅니다.

             ![화면 캡처 2024-10-01 025733.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/a47b2c61-1ae8-4c29-9c2f-438e4df8963f/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2024-10-01_025733.png)

          2. “Share” 탭으로 가서 왼쪽 아래의 “Copy link”를 누르면 끝입니다.

             ![화면 캡처 2024-10-01 025803.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/acc83608-4ca2-45a1-abaf-004412bbb932/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2024-10-01_025803.png)

      ## 제출자료

      ------

      위의 요구사항들이 만족된 `train.py`을 public github repository에 업로드하여 공유해주시면 됩니다. 그리고 공유할 wandb log 링크를 README.md를 만들어 적어놓으시면 됩니다.