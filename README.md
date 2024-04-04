## Intra-Class Similarity-Guided Feature Distillation
#### This repository contains the code for the paper [Intra-Class Similarity-Guided Feature Distillation](https://ca-roll.github.io/downloads/Intra-Class%20Similarity-Guided%20Feature%20Distillation.pdf) (NeurIPS-ENLSP-Workshop-2023).


#### For more details about the technical details, please refer to our paper.

**Installation**

Run command below to install the environment (using python3.9):

```
pip install -r requirements.txt
```

**Data Preparation**

Run command below to get GLUE data:

```
python download_glue_data.py --data_dir glue_data --task all
```

**Student Initialization**

Download the 6-layer [general TinyBERT](https://huggingface.co/huawei-noah/TinyBERT_General_6L_768D) model as the initial student model, and unzip it to directory ```./tinybert/TinyBERT_General_6L_768D```

**Fine-tuning**

Run command below to get fine-tuned teacher model for every task of GLUE:

```
# GLUE (e.g., RTE)
python run_glue.py \
  --model_type bert \
  --model_name_or_path bert-base-uncased \
  --task_name RTE \
  --data_dir ./glue_data/RTE/ \
  --do_lower_case \
  --max_seq_length 128 \
  --do_train \
  --per_gpu_train_batch_size 32 \
  --per_gpu_eval_batch_size 32 \
  --learning_rate 5e-5 \
  --num_train_epochs 5 \
  --output_dir ./model/RTE/teacher/ \
  --overwrite_output_dir

 
```

**Distillation**

Run command below to get distilled student model for every task of GLUE:

```
# GLUE (e.g., RTE)
python run_glue_with_distillation.py \
  --model_type bert \
  --teacher_model ./model/RTE/teacher/ \
  --student_model ./tinybert/TinyBERT_General_6L_768D/ \
  --task_name RTE \
  --data_dir ./glue_data/RTE/ \
  --do_lower_case \
  --max_seq_length 128 \
  --do_train \
  --per_gpu_train_batch_size 32 \
  --per_gpu_eval_batch_size 32 \
  --teacher_learning_rate 5e-5 \
  --student_learning_rate 5e-5 \
  --num_train_epochs 5 \
  --output_dir ./model/RTE/student/ \
  --overwrite_output_dir \
  --alpha 0.5 \
  --num_hidden_layers 6 \
  --temperature 5.0 \
  --strategy skip

```

**Results**

On the GLUE development set:

**Cite**

```
@article{saadi2024Intra,
  title={Intra-Class Similarity-Guided Feature Distillation},
  author={Saadi, Khouloud and Mitrovic, Jelena and Granitzer, Michael},
  journal={NeurIPS ENLSP},
  year={2023}
}
```

**Evaluation on Hate-Speech Tasks**
