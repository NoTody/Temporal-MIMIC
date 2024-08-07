# Temporal-MIMIC
This repository contains code for generating time-series radiology images and reports data by linking MIMIC-CXR and 
MIMIC-IV with train/evaluation code for aggregating and fusing representations from both modalities.

## Dependencies
Check requirements.txt for dependencies of this repository.

## Generation Dataset
Following README in [Generation Dataset](./data_generation) to generate dataset.

## To Run
To run code on one gpu for one machine
```
torchrun --nnodes 1 --nproc_per_node 1 --master_port 12323 train.py --model_name "vitb16" --batch_size 8 \
    --max_epoch 15 --save_suffix "VIT_early_width768_1hr_imglen1_textlen50_decoder_rope_ep15_s42" --seed 42 \
    --method "decoder" --num_workers 8 --mode "mm_early" \
    --train_path "./dataset/train_impressions_1hr.csv" \
    --val_path "./dataset/val_impressions_1hr.csv" \
    --test_path "./dataset/test_impressions_1hr.csv" \
    --section "impression" --local_rank 0 --pos_encoding "rope" --use_time \
    --img_lr 1e-5 --unpre_lr 1e-4 --text_lr 1e-5 --decoder_layers 3 --patient 15 \
    --run_name "VIT_early_width768_1hr_imglen1_textlen50_decoder_rope_ep15_s42" --project "HAIM" --text_len 200 \
    --text_time_series --img_max_len 1 --text_max_len 50 --grad_clip 3.0 --d_model 768 
```
where fusion method is Block (--fusion_method "Block"), early multi-modal fusion (--mode "mm_early") is used,  
impression section is used for text (--section "impression"), text time series is used (--text_time_series)
with text maximum length to be 50 (--text_max_len 50) and image maximum length to be 1 (--img_max_len 1). 
See arguments in train.py for all argument options.

## Reference
Haoxu Huang, Cem M. Deniz, Kyunghyun Cho, Sumit Chopra, Divyam Madaan. "Temporal Fine-tuning of Medical Vision-Language Representations". In: NeurIPS Workshop on Medical Imaging Meets NeurIPS, New Orleans, LA, USA, 2023