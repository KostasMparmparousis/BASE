## Setup Environment
1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) if not already installed.
2. Create and activate the environment:
   ```bash
   conda env create -f environment.yml
   conda activate base
   ```

## Training Guide

1. Configure the path of your training workload in `train_base.py` (line 44):
   ```python
   sql_directory = "/your/path/here"  # Update this path to where you wish for the checkpoints to be saved
   ```

2. Run the training script:
   ```bash
   python3 train_base.py --path ./Models/now.pth --epoch_start 1 \
   --epoch_end 100 --epsilon_decay 0.95 \--epsilon_end 0.02 --capacity 60000 \
    --batch_size 512 --sync_batch_size 50 --steps_per_epoch 1000 \ 
    --max_lr 0.0008 --learning_rate 0.0003
   ```

## Testing guide

1. Configure the path of your testing workload in `Transfer_Active_job.py` (line 44):
   ```python
   sql_directory = "/your/path/here"  # Update this path to where you wish for the checkpoints to be saved
   ```

2. Run the training script:
   ```bash
   python3 Transfer_Active_job.py
   ```

---

# BASE: Bridging the Gap between Cost and Latency for Query Optimization

BASE is a two-stage RL-based framework that bridges the gap between cost and latency for training a learned query optimizer efficiently. For more details, check out the VLDB 2023 paper: [BASE: Bridging the Gap between Cost and Latency for Query Optimization.](https://www.vldb.org/pvldb/vol16/p1958-chen.pdf)

## Dataset

The pre-training queries for IMDB are available at this [link](https://drive.google.com/drive/folders/16Dguw7xDWR19K_B7ZPUscfdCRg5r5mQ4?usp=drive_link).

## Usage

BASE is a two stage training framework for learned query optimizer:  

* First stage: policy pretraining with cost from PostgreSQL cost model.

```bash
python -u RLGOOTest_NEW.py --path ./Models/now.pth --epoch_start 1 --epoch_end 100 --epsilon_decay 0.95 --epsilon_end 0.02 --capacity 60000 --batch_size 512 --sync_batch_size 50 --steps_per_epoch 1000 --max_lr 0.0008 --learning_rate 0.0003 > log_NEW.txt 2>&1
```

* Second stage (starting from the first stage): latency finetuning with latency from actual execution. 

```bash
python Transfer_Active.py
```



## Contact

xuchen.2019@outlook.com
