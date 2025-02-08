<div align="center">
  <h1>Joint Discriminator and Transfer Based Fast Domain Adaptation For End-To-End Speech Recognition</h1>
</div>

## 1. Introduction
Adapting End-to-End (E2E) models to unseen domains is still a big challenge since training E2E models requires lots of paired audio and text training data. We propose a novel domain adaptation framework for the E2E model, which only uses the text of the target domain. Moreover, the proposed methods can keep the performance on the source domain intact while greatly improving the performance on the target domain. The proposed framework consists of two parts: the discriminator and the transfer which were optimized separately. Finally, optimized discriminator and transfer were combined and evaluated on two domain adaption tasks. In the experiments of adapting the English Librispeech to Gigaspeech, we obtained an average relative 11.6% and 11.8% on word error rate (WER) reduction for the target domain dev and test sets, respectively, while almost without WER degradation on the source domain. For the inhouse Chinese corpus aviation and TV, the character error rate (CER) of the source domain increased within 5%, while the CER on the target domain achieved around relative 85% and 42% improvement, respectively. In addition, our approach is also more effective in the mixed domain scenarios in the evaluation.

## 2. Usage
### 2.1 数据准备

---

**将目标域文本放在data/target_domain_text目录下用于nnlm训练**<br>
**将目标域测试集数据集放在data/test下**<br>
**如果目标域文本数量小于100w条就将它repeat到100w级别**<br>
**feats.scp为伪造feats**<br>
**对应的脚本是scripts/data_process.sh**<br>

### 2.2 nnlm训练
**目标领域nnlm training**

```
bash ./scripts/train.sh --expdir exp/grulm2L_target_domain_aviage2L128h --conf conf/grulm2L128h.yaml
```
### 2.3 解码测试集

```
./scripts/decode_e2e_ngram.sh --dir ${exp} --data ${dir} --sets ${dataset} --suffix "_${expname}" --nj ${job} --pp ${pp} --target_nnlm exp/lstmlm2L_target_domain_tv/checkpoint
```
**dir为存放模型checkpoint的实验目录，data为数据目录，sets为数据目录里面具体的子集如test，target_nnlm为目标域lm的路径**


## 3. Experiment results
<img width="689" alt="Clipboard_Screenshot_1738985201" src="https://github.com/user-attachments/assets/9d2e7e53-ecca-44c2-81a6-5f66be1841e1" />
<img width="335" alt="Clipboard_Screenshot_1738985222" src="https://github.com/user-attachments/assets/c05a8bde-8fe3-4761-8f4e-1c59d867285e" />
<img width="331" alt="Clipboard_Screenshot_1738985234" src="https://github.com/user-attachments/assets/a7f77397-779e-470a-997c-5950b9fc1491" />
<img width="331" alt="Clipboard_Screenshot_1738985243" src="https://github.com/user-attachments/assets/d863c9b0-2c65-4c30-97e1-75cbb231e7d0" />


## 4. Usage Example

### 准备数据：
```
bash run.sh --stage -1 --target_domain_text target_domain_textdir/tv_text --stop_stage -1 --setname tv
```
### 目标域nnlm训练：
```
bash run.sh --stage 0 --target_domain_text target_domain_textdir/tv_text --stop_stage 0 --setname tv 
```
### 解码测试集：

**decode source domain when target domain is tv**
```
bash run.sh --setname aitrans --stage 1 --target_domain tv
```
**decode target domain tv**
```
bash run.sh --setname tv --stage 1 --target_domain tv
```

### 总的流程：
```
bash run.sh --target_domain_text target_domain_textdir/aviage_text --target_domain aviage 
```

## 5. Citation
```
@INPROCEEDINGS{
    author={Shao, Hang and Tan, Tian and Wang, Wei and Gong, Xun and Qian, Yanmin},
    booktitle={ICASSP 2023 - 2023 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)}, 
    title={Joint Discriminator and Transfer Based Fast Domain Adaptation For End-To-End Speech Recognition}, 
    year={2023},
    pages={1-5},
    keywords={Degradation;Training;Adaptation models;TV;Error analysis;Training data;Speech recognition;end-to-end speech recognition;domain adaptation;discriminator and transfer;log-likelihood ratio},
    doi={10.1109/ICASSP49357.2023.10095910}
}
```


