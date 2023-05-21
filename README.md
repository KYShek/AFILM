# AFiLM

为了鼠大的信号2项目给原repo加了数据下载来方便在云gpu上跑，现archive

Add data folder from [kuleshov/audio-super-res](https://github.com/kuleshov/audio-super-res) . And now archive after my course finish.

## 宗文似用方法
写给哈哈群和sbu-scie的你们

### 环境准备 
python版本为3.7

命令行输入
```
git clone https://https://github.com/KYShek/AFILM
```
```
cd AFILM
```
```
pip install -r requirements.txt
```
### 数据集准备
命令行输入
```
cd ./data/vctk
```
```
make
```
```
cd speaker1
```
```
make
```
*若数据集生成不成功*
执行以下命令：
```
python ../prep_vctk.py \
  --file-list  speaker1-train-files.txt \
  --in-dir ../VCTK-Corpus/wav48/p225 \
  --out vctk-speaker1-train.4.16000.8192.4096.h5 \
  --scale 4 \
  --sr 16000 \
  --dimension 8192 \
  --stride 4096 \
  --interpolate \
  --low-pass

python ../prep_vctk.py \
  --file-list speaker1-val-files.txt \
  --in-dir ../VCTK-Corpus/wav48/p225 \
  --out vctk-speaker1-val.4.16000.8192.4096.h5 \
  --scale 4 \
  --sr 16000 \
  --dimension 8192 \
  --stride 4096 \
  --interpolate \
  --low-pass

python ../prep_vctk.py \
  --file-list  speaker1-train-files.txt \
  --in-dir ../VCTK-Corpus/wav48/p225 \
  --out vctk-speaker1-train.4.16000.-1.4096.h5 \
  --scale 4 \
  --sr 16000 \
  --dimension -1 \
  --stride 4096 \
  --interpolate \
  --low-pass

python ../prep_vctk.py \
  --file-list speaker1-val-files.txt \
  --in-dir ../VCTK-Corpus/wav48/p225 \
  --out vctk-speaker1-val.4.16000.-1.4096.h5 \
  --scale 4 \
  --sr 16000 \
  --dimension -1 \
  --stride 4096 \
  --interpolate \
  --low-pass
```
### 训练
```
cd ../../../
```
```
cd codes
```
```
python train.py \
   --model afilm \
   --train ../data/vctk/speaker1/vctk-speaker1-train.4.22050.8192.4096.h5 \
   --val ../data/vctk/speaker1/vctk-speaker1-val.4.22050.8192.4096.h5 \
   -e 2 \
   --batch-size 64 \
   --logname singlespeaker \
   --layers 4 \
   --lr 3e-4 \
   --save_path model.h5 \
   --r 4 \
   --pool_size 2 \
   --strides 2
```
项目软件部分指路：[KYShek/audio_sup_res_python](https://github.com/KYShek/audio_sup_res_python)

###

##
### [Paper arXiv](https://arxiv.org/abs/2108.11637)
## MLSP 2021 Self-Attention for Audio Super-resolution 

### Dependencies

The model is implemented in Python 3.7.12 \
Additional packages: 
- `tensorflow==2.6.0`
- `keras==2.6.0`
- `h5py==3.1.0`
- `numpy==1.19.5`
- `scipy==1.4.1`
- `librosa==0.9.1`


### Data
Data preparation is the same as [TFiLM](https://github.com/kuleshov/audio-super-res) . The main dataset is VCTK speech. The model expects `.h5` archives contaning pairs of high and low resolution sound patches. 

### Pre-trained models
The shared models were trained in a similar way to previous audio super-resolution models for fair comparison purposes. You will be able to get better results by training them longer. Moreover, the models do not generalize to other domains. If you recordings are very different from the dataset, you should collect additional data. 

| Scale |   Dataset   |   Model  |
|-------|:-----------:|:--------:|
| 2     | VCTK Single | [Download](https://drive.google.com/file/d/1aT72PhsFJILvdauAh5g8O87qj3Fisfis/view?usp=sharing) |
| 2     |  VCTK Multi | [Download](https://drive.google.com/file/d/1Uw06eSpOHA_qZWX6DZxa1I6UgbU9lpqI/view?usp=sharing) |
| 4     | VCTK Single | [Download](https://drive.google.com/file/d/1-AR-Cn1q1QeuDSeaP4RTcBSZ3s63j5rO/view?usp=sharing) |
| 4     | VCTK Multi  | [Download](https://drive.google.com/file/d/1-5JfveMwPWS-3C2V8pJx7QJ4PzG24N55/view?usp=sharing) |
| 8     | VCTK Single | [Download](https://drive.google.com/file/d/1-AlWbNt1M8UgqdRpMC41C42zLHymGGwG/view?usp=sharing) |
| 8     | VCTK Multi  | [Download](https://drive.google.com/file/d/1-AfiNBinwq1y52JWQ4J9l4Z0j0bCKQgQ/view?usp=sharing) |

### Training
Run the `codes/train.py` script.
```
usage: train.py [-h] [--model {afilm,tfilm}] --train TRAIN --val VAL
                [-e EPOCHS] [--batch-size BATCH_SIZE] [--logname LOGNAME]
                [--layers LAYERS] [--alg ALG] [--lr LR]
                [--save_path SAVE_PATH] [--r R] [--pool_size POOL_SIZE]
                [--strides STRIDES]

optional arguments:
  -h, --help            show this help message and exit
  --model {afilm,tfilm}
                        model to train
  --train TRAIN         path to h5 archive of training patches
  --val VAL             path to h5 archive of validation set patches
  -e EPOCHS, --epochs EPOCHS
                        number of epochs to train
  --batch-size BATCH_SIZE
                        training batch size
  --logname LOGNAME     folder where logs will be stored
  --layers LAYERS       number of layers in each of the D and U halves of the
                        network
  --alg ALG             optimization algorithm
  --lr LR               learning rate
  --save_path SAVE_PATH
                        path to save the model
  --r R                 upscaling factor
  --pool_size POOL_SIZE
                        size of pooling window
  --strides STRIDES     pooling stide
```

### Testing 
Run the `codes/test.py` script.
```
usage: test.py [-h] --pretrained_model PRETRAINED_MODEL
               [--out-label OUT_LABEL] [--wav-file-list WAV_FILE_LIST]
               [--layers LAYERS] [--r R] [--sr SR] [--patch_size PATCH_SIZE]

optional arguments:
  -h, --help            show this help message and exit
  --pretrained_model PRETRAINED_MODEL
                        path to pre-trained model
  --out-label OUT_LABEL
                        append label to output samples
  --wav-file-list WAV_FILE_LIST
                        list of audio files for evaluation
  --layers LAYERS       number of layers in each of the D and U halves of the
                        network
  --r R                 upscaling factor
  --sr SR               high-res sampling rate
  --patch_size PATCH_SIZE
                        Size of patches over which the model operates
```




