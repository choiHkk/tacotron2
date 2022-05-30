## Introduction
1. nvidia-tacotron2 오픈 소스와 한국어 데이터셋(KSS)을 사용해 빠르게 학습합니다.
2. KSS 데이터셋은 기본적으로 44kHz인 점을 감안하여 22kHz로 resampling할 수 있도록 utils.load_wav_to_torch()를 수정했습니다.
4. 기존의 nvidia-tacotron2 stft를 사용하면 변환되는 mel-spectrogram의 범위가 약 2 ~ -11 사이로 변환되어 학습하는데 많은 시간이 소요되기 때문에 torch version stft는 사용하지 않습니다.
5. mel-spectrogram 생성 과정이 변경되었기 때문에 data_utils.py 스크립트 일부가 수정되었습니다.
6. 학습 시 사용되는 nvidia 기반 amp(automatic mixed precison)은 별도의 설치가 필요하기 때문에 사용하지 않습니다. train.py 스크립트 대부분이 수정되었습니다.
7. tensorflow를 사용하지 않기 때문에 hparams.py 스크립트 대부분이 수정되었습니다.
8. conda 환경으로 진행해도 무방하지만 본 레포지토리에서는 docker 환경만 제공합니다. 기본적으로 ubuntu에 docker, nvidia-docker가 설치되었다고 가정합니다.
9. GPU, CUDA 종류에 따라 Dockerfile 상단 torch image 수정이 필요할 수도 있습니다.
10. 별도의 pre-processing 과정은 필요하지 않습니다.


## Dataset
1. download dataset - https://www.kaggle.com/datasets/bryanpark/korean-single-speaker-speech-dataset
2. `mkdir /path/to/the/tacotron2/data/dataset`
3. `mv /path/to/the/kss.zip /path/to/the/tacotron2/data/dataset`
4. `cd /path/to/the/tacotron2/data/dataset`
5. `unzip kss.zip -d kss`

## Docker build
1. `cd /path/to/the/tacotron2`
2. `docker build --tag tacotron2:latest .`

## Training
1. `nvidia-docker run -it --name 'tacotron2' -v /path/to/tacotron2:/home/work/tacotron2 --ipc=host --privileged tacotron2:latest`
2. `cd /home/work/tacotron2`
3. `python train.py -g 0 -o data -l kss_v1 -d /home/work/tacotron2/data/dataset/kss`
4. `python train.py -g 0 -o data -l kss_v1 -d /home/work/tacotron2/data/dataset/kss -c /home/work/tacotron2/data/kss_v1/model_state/checkpoint_<step>`
5. arguments
  * -g : gpu number
  * -o : output directory
  * -l : log directory
  * -d : data directory
  * -c : checkpoint path with step number
5. (OPTIONAL) `tensorboard --logdir=outdir/logdir`
