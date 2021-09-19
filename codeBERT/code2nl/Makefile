lang=java
lr=5e-5
batch_size=24
beam_size=10
source_length=256
pruned_source_length=64
pruned_variable_source_length=150

target_length=128
data_dir=../data/code2nl/CodeSearchNet

output_dir=model/$(lang)
pruned_variable_output_dir=model/pruned_variable/$(lang)

train_file=$(data_dir)/$(lang)/train.jsonl
dev_file=$(data_dir)/$(lang)/valid.jsonl
eval_steps=5000 
train_steps=30000 
pretrained_model=microsoft/codebert-base 

train: output_dir=model/codebert_origin/$(lang)

test: beam_size=10
test: batch_size=128
test_file= $(data_dir)/$(lang)/test.jsonl
test: test_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

train-origin-python: lang=python
test-origin-python: lang=python


test-without-finetune:
	nohup python3 run_origin.py \
	--do_test \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path microsoft/codebert-base \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length $(source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_without_finetuning.log 2>&1 &

test-without-finetune-python:lang=python
test-without-finetune-python:
	nohup python3 run_origin.py \
	--do_test \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path microsoft/codebert-base \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length $(source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_without_finetuning_python.log 2>&1 &

train:
	nohup python3 run_origin.py --do_train --do_eval --model_type roberta \
	--model_name_or_path $(pretrained_model) --train_filename $(train_file) \
	--tokenizer_name microsoft/codebert-base \
	--dev_filename $(dev_file) --output_dir $(output_dir) --max_source_length $(source_length) \
	--max_target_length $(target_length) --beam_size $(beam_size) --train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) --learning_rate $(lr) --train_steps $(train_steps) --eval_steps $(eval_steps) > train_origin_sourcelength_256.log 2>&1 &

train-slim-tiny:lang=java
train-slim-tiny:output_dir=model/$(lang)/slim-tiny
train-slim-tiny:
	nohup python3 run.py --do_train --do_eval --model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) --train_filename $(train_file) \
	--prune_strategy slim \
	--dev_filename $(dev_file) --output_dir $(output_dir) --max_source_length 100 \
	--max_target_length $(target_length) --beam_size $(beam_size) --train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) --learning_rate $(lr) --train_steps $(train_steps) --eval_steps $(eval_steps) > train_slim.log 2>&1 &

test-slim-tiny:lang=java
test-slim-tiny:
test-slim-tiny:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path microsoft/codebert-base \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length 100 \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_slim.log 2>&1 &

train-pruned:
	nohup python3 run_pruned_method.py --do_train --do_eval --model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) --train_filename $(train_file) \
	--prune_strategy method-return \
	--dev_filename $(dev_file) --output_dir $(output_dir) --max_source_length $(pruned_source_length) \
	--max_target_length $(target_length) --beam_size $(beam_size) --train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) --learning_rate $(lr) --train_steps $(train_steps) --eval_steps $(eval_steps) > pruned_train.log 2>&1 &

train-origin-python: lang=python
train-origin-python: output_dir=model/$(lang)/origin

train-origin-python:
	nohup python3 run_origin.py \
	--do_train \
	--do_eval \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) \
	--train_filename $(train_file) \
	--dev_filename $(dev_file) \
	--output_dir $(output_dir) \
	--max_source_length $(source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) \
	--learning_rate $(lr) \
	--train_steps $(train_steps) \
	--eval_steps $(eval_steps) > train_origin_python.log 2>&1 &

test-origin-python: lang=python
test-origin-python: output_dir=model/$(lang)/origin
test-origin-python: load_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

test-origin-python:
	nohup python3 run_origin.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length $(source_length) \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > origin_train_256_and_test_256_python.log 2>&1 &

train-pruned-python:output_dir=model/$(lang)/pruned2
train-pruned-python:lang=python

train-pruned-python:
	nohup python3 run.py \
	--do_train \
	--do_eval \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) \
	--train_filename $(train_file) \
	--dev_filename $(dev_file) \
	--output_dir $(output_dir) \
	--prune_strategy method-return \
	--max_source_length $(pruned_source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) \
	--learning_rate $(lr) \
	--lang python \
	--train_steps $(train_steps) \
	--eval_steps $(eval_steps) > train_pruned_python.log 2>&1 &

test-prune-slim:output_dir=model/$(lang)/slim
test-prune_slim:lang=java
test-prune-slim:load_model=model/origin_train/$(lang)/checkpoint-best-bleu/pytorch_model.bin
test-prune-slim:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--prune_strategy slim \
	--lang java \
	--max_source_length 100 \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_slim_60.log 2>&1 &

test-prune-slim-python:lang=python
test-prune-slim-python:output_dir=model/$(lang)/slim
test-prune-slim-python:load_model=model/$(lang)/origin/checkpoint-best-bleu/pytorch_model.bin
test-prune-slim-python:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--prune_strategy slim \
	--lang python \
	--max_source_length 100 \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_slim_python.log 2>&1 &



test-pruned-python:output_dir=model/$(lang)/pruned2
test-pruned-python:lang=python
test-pruned-python:load_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

test-pruned-python:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--prune_strategy method-return \
	--max_source_length $(pruned_source_length) \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_python.log 2>&1 &

train-pruned-loop-python:output_dir=model/$(lang)/loop
train-pruned-loop-python:lang=python

train-pruned-loop-python:
	nohup python3 run.py \
	--do_train \
	--do_eval \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path $(pretrained_model) \
	--train_filename $(train_file) \
	--dev_filename $(dev_file) \
	--output_dir $(output_dir) \
	--max_source_length 120 \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--prune_strategy loop \
	--train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) \
	--learning_rate $(lr) \
	--lang python \
	--train_steps $(train_steps) \
	--eval_steps $(eval_steps) > train_pruned_loop_python.log 2>&1 &
test-pruned-loop-python:output_dir=model/$(lang)/loop
test-pruned-loop-python:lang=python
test-pruned-loop-python:load_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

test-pruned-loop-python:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length 120 \
	--prune_strategy loop \
	--lang python \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_loop_python.log 2>&1 &


test-pruned-random:output_dir=model/origin_train/$(lang)
test-pruned-random:lang=java
test-pruned-random:load_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

test-pruned-random:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length 256 \
	--prune_strategy random \
	--lang java \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_random.log 2>&1 &



test-pruned-random-python:output_dir=model/$(lang)/origin
test-pruned-random-python:lang=python
test-pruned-random-python:load_model=$(output_dir)/checkpoint-best-bleu/pytorch_model.bin

test-pruned-random-python:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(load_model) \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length 256 \
	--prune_strategy random \
	--lang python \
	--max_target_length $(target_length) \
	--tokenizer_name microsoft/codebert-base \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_random_python.log 2>&1 &



train-pruned-variable-declaration:
	nohup python3 run.py \
	--do_train \
	--do_eval \
	--model_type roberta \
	--model_name_or_path $(pretrained_model) \
	--train_filename $(train_file) \
	--dev_filename $(dev_file) \
	--tokenizer_name microsoft/codebert-base \
	--output_dir $(pruned_variable_output_dir) \
	--max_source_length $(pruned_variable_source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--prune_strategy variable \
	--train_batch_size $(batch_size) \
	--eval_batch_size $(batch_size) \
	--learning_rate $(lr) \
	--train_steps $(train_steps) \
	--eval_steps $(eval_steps) > train_pruned_variable_declaration.log 2>&1 &

test:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path model/java/checkpoint-best-bleu/pytorch_model.bin \
	--prune_strategy method-return \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--output_dir $(output_dir) \
	--max_source_length $(source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > origin_train_256_and_test_256.log 2>&1 &

test-pruned-variable-declaration:
	nohup python3 run.py \
	--do_test \
	--model_type roberta \
	--tokenizer_name microsoft/codebert-base \
	--model_name_or_path microsoft/codebert-base \
	--load_model_path $(pruned_variable_output_dir)/checkpoint-best-bleu/pytorch_model.bin \
	--dev_filename $(dev_file) \
	--test_filename $(test_file) \
	--prune_strategy variable \
	--output_dir $(pruned_variable_output_dir) \
	--max_source_length $(pruned_variable_source_length) \
	--max_target_length $(target_length) \
	--beam_size $(beam_size) \
	--eval_batch_size $(batch_size) > test_pruned_variable_declaration.log 2>&1 &
