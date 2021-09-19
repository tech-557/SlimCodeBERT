train:
	nohup python3 run.py \
	--output_dir=./saved_models \
	--model_type=roberta \
	--config_name=microsoft/codebert-base \
	--model_name_or_path=microsoft/codebert-base \
	--tokenizer_name=microsoft/codebert-base \
	--do_train \
	--train_data_file=../dataset/train.txt \
	--eval_data_file=../dataset/valid.txt \
	--test_data_file=../dataset/test.txt \
	--epoch 2 \
	--block_size 300 \
	--train_batch_size 10 \
	--eval_batch_size 24 \
	--learning_rate 5e-5 \
	--max_grad_norm 1.0 \
	--evaluate_during_training \
	--seed 123456 > train.log 2>&1 &

train-pruned:
	nohup python3 run.py \
	--output_dir=./saved_models/pruned \
	--model_type=roberta \
	--config_name=microsoft/codebert-base \
	--model_name_or_path=microsoft/codebert-base \
	--tokenizer_name=microsoft/codebert-base \
	--do_train \
	--train_data_file=../dataset/train.txt \
	--eval_data_file=../dataset/valid.txt \
	--test_data_file=../dataset/test.txt \
	--epoch 2 \
	--block_size 300 \
	--train_batch_size 10 \
	--eval_batch_size 24 \
	--learning_rate 5e-5 \
	--max_grad_norm 1.0 \
	--evaluate_during_training \
	--seed 123456 > pruned_train.log 2>&1 &

test-pruned:
	nohup python3 run.py \
	--output_dir=./saved_models/pruned \
	--model_type=roberta \
	--config_name=microsoft/codebert-base \
	--model_name_or_path=microsoft/codebert-base \
	--tokenizer_name=roberta-base \
	--do_eval \
	--do_test \
	--train_data_file=../dataset/train.txt \
	--eval_data_file=../dataset/valid.txt \
	--test_data_file=../dataset/test.txt \
	--epoch 2 \
	--block_size 400 \
	--train_batch_size 16 \
	--eval_batch_size 32 \
	--learning_rate 5e-5 \
	--max_grad_norm 1.0 \
	--evaluate_during_training \
	--seed 123456 > pruned_test.log 2>&1 &

test:
	nohup python3 run.py \
	--output_dir=./saved_models \
	--model_type=roberta \
	--config_name=microsoft/codebert-base \
	--model_name_or_path=microsoft/codebert-base \
	--tokenizer_name=roberta-base \
	--do_eval \
	--do_test \
	--train_data_file=../dataset/train.txt \
	--eval_data_file=../dataset/valid.txt \
	--test_data_file=../dataset/test.txt \
	--epoch 2 \
	--block_size 400 \
	--train_batch_size 16 \
	--eval_batch_size 32 \
	--learning_rate 5e-5 \
	--max_grad_norm 1.0 \
	--evaluate_during_training \
	--seed 123456 > test.log 2>&1 &
