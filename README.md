<h2 align="center">LLMOPT: Learning to Define and Solve <br> General Optimization Problems from Scratch </h2>
<p align="center">
    <a href=""><strong>Caigao Jiang</strong></a><sup>*</sup>
    ·
    <a href=""><strong>Xiang Shu</strong></a><sup>*</sup>
    ·
    <a href=""><strong>Hong Qian</strong></a><sup>†</sup>
    ·
    <a href=""><strong>Xingyu Lu</strong></a><sup>†</sup>
    <br>
    <a href=""><strong>Jun Zhou</strong></a>
    ·
    <a href=""><strong>Aimin Zhou</strong></a>
    ·
    <a href=""><strong>Yang Yu</strong></a>
    <div align='center'>
        <sup>*</sup>Equal Contribution, <sup>†</sup>Corresponding Authors.
    </div>
    <p align="center">
        <b>East China Normal University    |    Ant Group   |   Nanjing University  </b></p> 
    <p align="center">
        <a href="https://openreview.net/pdf?id=9OMvtboTJg"><img src='https://img.shields.io/badge/Paper-LLMOPT-red'></a>
        <a href='https://huggingface.co/ant-opt/LLMOPT-Qwen2.5-14B'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-Model-yellow'></a>
        <!-- <a href=''><img src='https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-Dataset-yellow'></a> -->
        <a href='https://github.com/antgroup/LLMOPT/tree/main/data/testset'><img src='https://img.shields.io/badge/Dataset-Testset-blue'></a>
        <a href='https://github.com/antgroup/LLMOPT'><img src='https://img.shields.io/badge/GitHub-Repo-blue'></a>
  </p>  
</p>

This repository contains the code for LLMOPT, enabling the reproduction of data generation, model learning, and automated testing as described in the accompanying paper. The running shell are in the `script` folder with the deepspeed training config in `config`.

## 🔥News

- [2025/04/22]: We have release the latest [LLMOPT-Qwen2.5-14B](https://huggingface.co/ant-opt/LLMOPT-Qwen2.5-14B) model, which is fine-tuned based on [Qwen2.5-14B-Instruct](https://huggingface.co/Qwen/Qwen2.5-14B-Instruct). Comprehensive evaluations are conducted across all testsets, and the [results](🤖Model-Release) is published.
- [2025/04/22]: We review and correct errors in the testsets, and update the questions of NLP4LP dataset. Additionally, evaluations are conducted on three new datasets: OptiBench, OptMath, and ICML_Competition. All 9 latest [datasets](📊Dataset-Release) is released.
- [2025/01/22]: [Our paper](https://openreview.net/pdf?id=9OMvtboTJg) "LLMOPT: Learning to Define and Solve General Optimization Problems from Scratch" accepted by ICLR2025! 🔥
- [2024/09/30]: We firstly release our training and test [code](🔧Usage), as well as data generation code for LLMOPT.

## 🌆Overview

![LLMOPT Framework](./docs/fw.png)

- In this paper, we present LLMOPT, a learning-based framework designed to tackle the challenges of optimization generalization in current LLM-based methods. To enhance the generalization of our approach, we first introduce a learning-to-define module that constructs a five-element formulation as a universal framework for various optimization types. To improve performance, we employ multi-instruction fine-tuning and model alignment for both problem formulation and solver code generation. Finally, we incorporate a self-correction mechanism to further enhance system performance during testing. Under an extensive and realistic experimental setting, LLMOPT demonstrates significantly superior performance compared to all competitors.

## 🤖Model Release

We release the [LLMOPT-Qwen2.5-14B](https://huggingface.co/ant-opt/LLMOPT-Qwen2.5-14B) model on Hugging Face and conduct comprehensive performance evaluations. We have updated the model evaluation results as shown in the following table, where the original results correspond to Table 1 and Table 2 in the paper. The differences in results stem from two reasons. Firstly, we exclude all Mamo EasyLP and ComplexLP datasets from the training process, reserving them exclusively for the test. Additionally, unlike the version described in our paper which used [Qwen1.5-14B](https://huggingface.co/Qwen/Qwen1.5-14B), this release is fine-tuned from the latest [Qwen2.5-14B-Instruct](https://huggingface.co/Qwen/Qwen2.5-14B-Instruct) model. The performance metrics for [LLMOPT-Qwen2.5-14B](https://huggingface.co/ant-opt/LLMOPT-Qwen2.5-14B) are as follows:

|              Dataset              |      NL4Opt      |    Mamo Easy    |   Mamo Complex   |      NLP4LP      |    ComplexOR    |    IndustryOR    | ICML Competition |    OptiBench    |     OptMath     |       AVG       |
| :-------------------------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: |
|            #Questions            |       230       |       652       |       211       |       242       |        18        |       100       |       410       |       605       |       166       |        -        |
|      ER with self-correction      |     100.00%     |     100.00%     |      99.05%      |     100.00%     |     100.00%     |      94.00%      |      99.66%      |      82.31%      |      75.30%      |      94.48%      |
| **SA with self-correction** | **97.31%** | **95.31%** | **85.78%** | **86.49%** | **76.47%** | **44.00%** | **95.76%** | **66.44%** | **40.00%** | **76.40%** |
|     AST with self-correction     |       1.38       |       1.13       |       2.13       |       1.50       |       3.46       |       2.14       |       1.47       |       1.54       |       4.06       |       2.09       |
|      ER w/o self-correction      |      97.42%      |      98.29%      |      77.73%      |      97.93%      |      88.89%      |      61.00%      |      93.90%      |      73.22%      |      31.93%      |      80.03%      |
|      SA w/o self-correction      |      80.28%      |      89.53%      |      44.08%      |      73.42%      |      35.29%      |      29.00%      |      75.35%      |      53.83%      |      12.50%      |      54.81%      |

In the experiment, we use three performance metrics to comprehensively evaluate the optimization generalization of the algorithm, namely, **Execution Rate (ER), Solving Accuracy (SA), and Average Solving Times (AST)**. Specifically, **ER** refers to the proportion of solutions whose code can run without any errors and has running results output. **SA** refers to the proportion of solutions that correctly solve the optimization problem, i.e., find the optimal solution. **AST** refers to the average number of times the self-correction process is performed during the test. 

## 📊Dataset Release

All the 9 testing datasets is released in the `./data` folder. The following is an introduction.

### Data Structure

To facilitate the evaluation, we process all datasets into a unified data structure. Specifically, each dataset is organized in a `jsonl` file, and each line is an independent piece of data. Each data includes four attributes, `question`, `answer`, `ori`, and `index`. The `question` field is a complete string description of the optimization problem, including complete data that can solve a problem. The `answer` field is a `float` type value, which indicates the objective function value corresponding to the optimal solution of the problem, i.e., the ground truth. The `ori` field indicates the source of the problem, that is, the name of the dataset. In order to facilitate statistical results, we use the `index` field to number the data in each dataset.

An example: (The first data of the NL4Opt dataset)

```json
{
    "question": "There has been an oil spill in the ocean and ducks need to be taken to shore to be cleaned either by boat or by canoe. A boat can take 10 ducks per trip while a canoe can take 8 ducks per trip. Since the boats are motor powered, they take 20 minutes per trip while the canoes take 40 minutes per trip. In order to avoid further environmental damage, there can be at most 12 boat trips and at least 60% of the trips should be by canoe. If at least 300 ducks need to be taken to shore, how many of each transportation method should be used to minimize the total amount of time needed to transport the ducks?", 
    "answer": 1160, 
    "ori": "5_nl4opt_test", 
    "index": 1
}
```

### Dataset Source

Here we explain the sources of all data sets and the detailed data processing process. For ground truth values with more than two decimal places, they will be rounded to two decimal places. If you find any omissions in manual labeling, please feel free to correct them.

##### 1. NL4Opt

The data for this testset comes from the competition, [NL4Opt](https://nl4opt.github.io/). We only used the test split. We manually labeled these 230 optimization problems. The [original dataset](https://huggingface.co/datasets/CardinalOperations/NL4OPT) contains 245 problems, of which 15 were found to be unsolvable after manual inspection, so we manually removed these problems. The sorted data can be found in the `./data/testset/nl4opt_test.jsonl`.

##### 2. Mamo Easy

This testset comes from the paper [Mamo: a Mathematical Modeling Benchmark with Solvers](https://arxiv.org/pdf/2405.13144v1). We obtained the original dataset of 652 data from [huggingface](https://huggingface.co/datasets/CardinalOperations/MAMO/viewer/default/easy_lp?views%5B%5D=easy_lp). Since we found some wrong ground truth value in the open-source data, we manually checked and re-labeled all the data. The manually checked data is stored in `./data/testset/mamo_easy_test.jsonl`.

##### 3. Mamo Complex

This testset comes from the paper [Mamo: a Mathematical Modeling Benchmark with Solvers](https://arxiv.org/pdf/2405.13144v1). We sorted out 211 original problems from the `complex_lp` spilt of the [huggingface](https://huggingface.co/datasets/CardinalOperations/MAMO/viewer/default/complex_lp?views%5B%5D=complex_lp) and stored the original data in a unified format in `./data/testset/mamo_complex_test.jsonl`.

##### 4. NLP4LP

This testset comes from the paper [OptiMUS: Optimization Modeling Using MIP Solvers and large language models](https://arxiv.org/abs/2310.06116). We sorted out these 242 feasible original problems from [huggingface](https://huggingface.co/datasets/udell-lab/NLP4LP) and stored the original data in a unified format in `./data/testset/nlp4lp.jsonl`.

##### 5. ComplexOR

This testset comes from the paper [Chain-of-Experts: When LLMs Meet Complex Operation Research Problems](https://openreview.net/pdf?id=HobyL1B9CZ). We sorted out these 18 feasible original problems from the [github repo](https://github.com/xzymustbexzy/Chain-of-Experts/tree/main/dataset/ComplexOR) and stored the original data in a unified format in `./data/testset/complexor.jsonl`.

##### 6. IndustryOR

This testset comes from the paper [ORLM: A Customizable Framework in Training Large Models for Automated Optimization Modeling](https://arxiv.org/abs/2405.17743). We sorted out these 100 original problems from [huggingface](https://huggingface.co/datasets/CardinalOperations/IndustryOR) and stored the original data in a unified format in `./data/testset/industryor.jsonl`.

##### 7. ICML Competition

The data for this testset comes from the competition, [ICML 2024 Challenges on Automated Math Reasoning - Track 3: Automated Optimization Problem-Solving with Code](https://www.codabench.org/competitions/2438/). We only used the test split. Since the competition organizer did not open source the ground truth of the testset, we manually labeled these 410 problems. The original dataset contains 421 problems, of which 11 were found to be unsolvable after manual inspection, so we manually removed these problems. The sorted data can be found in the `./data/testset/task3_test.jsonl`.

##### 8. OptiBench

This testset comes from the paper [OptiBench Meets ReSocratic: Measure and Improve LLMs for Optimization Modeling](https://arxiv.org/pdf/2407.09887v2). We sorted out these 605 original problems from the [repository](https://github.com/yangzhch6/ReSocratic/blob/main/data/OptiBench.json) and stored the original data in a unified format in `./data/testset/optibench.jsonl`.

##### 9. OptMath

This testset comes from the paper [OptMATH: A Scalable Bidirectional Data Synthesis Framework for Optimization Modeling](https://arxiv.org/pdf/2502.11102). We sorted out these 165 original problems from the [repository](https://github.com/AuroraLHL/OptMATH/blob/main/benchmark/OptMATH_Bench.json) and stored the original data in a unified format in `./data/testset/optmath.jsonl`.

## 🔧Usage

### Requirements

Necessary python libraries and versions are in the `requirements.txt`:

```bash
pip install -r requirements.txt
```

with `python>=3.6`.

### Installation

For development, you can clone the repository and install it locally.

```bash
git clone https://github.com/antgroup/LLMOPT.git
cd LLMOPT
```

**Attention**: `Pyomo` is a modeling language, you should alse install the solvers to execute the `Pyomo` code! 

For example, install some open-source solvers:

```bash
pip install pyomo
conda install -c conda-forge glpk coincbc ipopt scip pulp
```

### User Case 1: SFT training

The hyperparameter setting and method for our Multi-Instruction Supervised Fine-Tuning(MISFT):

```bash
torchrun $DISTRIBUTED_ARGS ../sft/sft.py \
    --model_name_or_path $MODEL \
    --data_path $DATA \
    --bf16 True \
    --output_dir "./output_dir" \
    --num_train_epochs 1000 \
    --per_device_train_batch_size 4 \
    --per_device_eval_batch_size 1 \
    --gradient_accumulation_steps 8 \
    --evaluation_strategy "no" \
    --save_strategy "steps" \
    --save_steps 100 \
    --save_total_limit 1 \
    --learning_rate 3e-4 \
    --weight_decay 0.01 \
    --adam_beta2 0.95 \
    --warmup_ratio 0.01 \
    --lr_scheduler_type "cosine" \
    --logging_dir ./logs_v0 \
    --logging_strategy "steps"\
    --logging_steps 1 \
    --report_to "tensorboard" \
    --model_max_length 1500 \
    --lazy_preprocess True \
    --use_lora ${USE_LORA} \
    --q_lora ${Q_LORA} \
    --gradient_checkpointing \
    --save_only_model \
    --deepspeed ${DS_CONFIG_PATH}
```

The complete MISFT code can be found in `./script/run_sft.sh`, just run the following command:

```bash
bash run_sft.sh
```

### User Case 2: KTO Training

The hyperparameter setting and method for KTO training is as follows:

```bash
torchrun $DISTRIBUTED_ARGS ../kto/kto.py \
    --deepspeed ${DS_CONFIG_PATH} \
    --per_device_train_batch_size 4 \
    --num_train_epochs 100 \
    --evaluation_strategy "no" \
    --learning_rate 1e-4 \
    --lr_scheduler_type=cosine \
    --per_device_eval_batch_size 1 \
    --gradient_accumulation_steps 8 \
    --save_steps 100 \
    --save_total_limit 1 \
    --logging_dir ./logs_v0 \
    --logging_strategy "steps"\
    --logging_steps 10 \
    --warmup_ratio 0.1 \
    --weight_decay 0.01 \
    --adam_beta2 0.95 \
    --report_to "tensorboard" \
    --bf16 \
    --logging_first_step \
    --use_peft \
    --lora_target_modules=all-linear \
    --lora_r=16 \
    --lora_alpha=16 \
    --save_only_model \
    --output_dir "./output_dir"
```

The complete KTO code can be found in `./script/run_kto.sh`, just run the following command:

```bash
bash run_kto.sh
```

## ⚙️Inference

The following example code for model inference in getting the experiement data:

```python
model = AutoModelForCausalLM.from_pretrained(path,torch_dtype="auto",device_map="auto")
tokenizer = AutoTokenizer.from_pretrained(path_t)
prompt = "Give me a short introduction to large language model."
messages = [{"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": prompt}
            ]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)
generated_ids = model.generate(model_inputs.input_ids,max_new_tokens=512)
generated_ids = [output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids generated_ids)]
response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
print(response)
```

## ⌛️Future Work

With the remarkable progress and rapid development of reasoning models (like DeepSeek R1 and OpenAI O1-3) in solving complex mathematical problems, we have also developed the LLMOPT Reasoning model. We will soon release our LLMOPT Reasoning version along with a new benchmarking effort.

## 📄Citation

If you encounter any question about our work, please do not hesitate to submit an issue. If you do find our resources helpful, please cite our paper.

```
@inproceedings{JiangShu2025llmopt,
  title     = {LLMOPT: Learning to Define and Solve General Optimization Problems from Scratch},
  author    = {Caigao Jiang and Xiang Shu and Hong Qian and Xingyu Lu and Jun Zhou and Aimin Zhou and Yang Yu},
  booktitle = {Proceedings of the Thirteenth International Conference on Learning Representations (ICLR)},
  year      = {2025},
  address   = {Singapore, Singapore},
  url       = {https://openreview.net/pdf?id=9OMvtboTJg}
}
```
