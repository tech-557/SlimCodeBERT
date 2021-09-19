lang = java
pretrained_model = microsoft/codebert-base
test: idx = 0
test_origin: idx = 0
test_layer: idx = 0
test_pruned_variable_declaration: idx = 0
export: idx = 0
export: lang=python
train-origin-python:lang=python
train-pruned-python:lang=python
test-origin-python: lang=python

clean:
	rm -rf ../data/codesearch/train_valid/java/cached_train_train_pytorch_model.bin_200_codesearch
	rm -rf ../data/codesearch/test/java/cached*

train-slim:lang=java
train-slim:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 95 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--output_attention \
	--lang java \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--prune_strategy slim \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/slim_tiny \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_slim_tiny.log 2>&1  &

test-slim:lang=java
test-slim:idx=0
test-slim:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_attention \
	--prune_strategy slim \
	--output_dir ./models/$(lang)/slim_origin_5 \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 86 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/slim/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/slim_origin_5/$(idx)_batch_result.txt > test_slim_2.log 2>&1 &

test-slim-python:lang=python
test-slim-python:idx=0
test-slim-python:
	lang=python
	pretrained_model=microsoft/codebert-base
	idx=0 nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_attention \
	--prune_strategy slim \
	--output_dir ./models/$(lang)/slim_origin \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 95 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--lang python \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/origin/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/slim_origin/$(idx)_batch_result.txt > test_slim_python_2.log 2>&1 &



train:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/prev  \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_origin.log 2>&1 &

train-origin-python:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--lang python \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--output_attention \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/origin \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_origin_python.log 2>&1 &

train-pruned-python:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--lang python \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 64 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--output_attention \
	--prune_strategy method-return \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/pruned \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_pruned_python.log 2>&1 &

test-pruned-python:idx=0
test-pruned-python:lang=python

test-pruned-python:
	lang=python
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/pruned \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 64 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--output_attention \
	--lang python \
	--prune_strategy method-return \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/pruned/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/pruned/$(idx)_batch_result.txt > test_pruned_python.log 2>&1 &


train-pruned:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 64 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--prune_strategy method-return \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/pruned  \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_pruned.log 2>&1  &

train-pruned-token:lang=java
train-pruned-token:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 150 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--prune_strategy token \
	--lang java \
	--output_attention \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/pruned_token  \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_pruned_token.log 2>&1  &

test-pruned-token:lang=java
test-pruned-token:idx=0
test-pruned-token:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/pruned_token_80 \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 140\
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--lang java \
	--output_attention \
	--prune_strategy token \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/prev/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/pruned_token_80/$(idx)_batch_result.txt > test_pruned_token.log 2>&1 &



train-pruned-token-python:lang=python
train-pruned-token-python:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 150 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--prune_strategy token \
	--output_attention \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/pruned_token  \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_pruned_token_python.log 2>&1  &

train-pruned-variable-declaration:
	nohup python3 run_classifier.py \
	--model_type roberta \
	--task_name codesearch \
	--do_train \
	--do_eval \
	--output_attention \
	--eval_all_checkpoints \
	--train_file train.txt \
	--dev_file valid.txt \
	--max_seq_length 150 \
	--per_gpu_train_batch_size 64 \
	--per_gpu_eval_batch_size 64 \
	--learning_rate 1e-5 \
	--num_train_epochs 4 \
	--prune_strategy variable \
	--gradient_accumulation_steps 1 \
	--overwrite_output_dir \
	--data_dir ../data/codesearch/train_valid/$(lang) \
	--output_dir ./models/$(lang)/pruned_variable_declaration  \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) > train_pruned_variable_declaration.log 2>&1  &


test_pruned_variable_declaration:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/pruned_variable_decalration \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 150 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
    --output_attention \
	--prune_strategy variable \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/pruned_variable_declaration/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/pruned_variable_declaration/$(idx)_batch_result.txt > test_pruned_variable_declaration.log 2>&1 &


test:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_attention \
	--prune_strategy method-return \
	--output_dir ./models/$(lang)/mr_origin \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 64 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/prev/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/mr_origin/$(idx)_batch_result.txt > train_pruned_64_and_test_pruned_64.log 2>&1 &


export:
	lang=python
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier_export.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/export \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/origin/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/export/$(idx)_batch_result.txt > export_python.log 2>&1 &


test_origin:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--output_attention \
	--do_predict \
	--output_dir ./models/$(lang)/prev \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/new/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/prev/$(idx)_batch_result.txt > train_origin_and_test_origin_200.log 2>&1 &

test-origin-python:idx=0

test-origin-python:
	lang=python
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--output_attention \
	--do_predict \
	--output_dir ./models/$(lang)/origin \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--lang=python \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/origin/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/origin/$(idx)_batch_result.txt > train_origin_and_test_origin_200_python.log 2>&1 &

test-without-finetuning:idx=0
test-without-finetuning:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--output_attention \
	--do_predict \
	--output_dir ./models/$(lang)/without-finetuning \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/without-finetuning/$(idx)_batch_result.txt > test_without_finetuning.log 2>&1 &


test_layer:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier_layer.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/new \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/new/checkpoint-best/ \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/new/$(idx)_batch_result.txt > test-layer.log 2>&1 &

test-pruned-random:idx=0
test-pruned-random:lang=java
test-pruned-random:
	lang=java
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/random \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 190 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--lang java \
	--output_attention \
	--prune_strategy random \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/prev/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/random/$(idx)_batch_result.txt > test_pruned_random.log 2>&1 &

test-pruned-random-python:idx=0
test-pruned-random-python:lang=python
test-pruned-random-python:
	lang=python
	pretrained_model=microsoft/codebert-base
	idx=0
	nohup python3 run_classifier.py \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--task_name codesearch \
	--do_predict \
	--output_dir ./models/$(lang)/random \
	--data_dir ../data/codesearch/test/$(lang) \
	--max_seq_length 200 \
	--per_gpu_train_batch_size 32 \
	--per_gpu_eval_batch_size 32 \
	--learning_rate 1e-5 \
	--lang python \
	--output_attention \
	--prune_strategy random \
	--num_train_epochs 8 \
	--test_file batch_$(idx).txt \
	--pred_model_dir ./models/$(lang)/origin/checkpoint-best \
	--tokenizer_name microsoft/codebert-base \
	--test_result_dir ./results/$(lang)/random/$(idx)_batch_result.txt > test_pruned_random_python.log 2>&1 &


output_weights:
	python3 analyse.py
