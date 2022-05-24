## Dataset
1. download dataset - https://www.kaggle.com/datasets/bryanpark/korean-single-speaker-speech-dataset
2. move dataset to /path/to/the/tacotron2/data/dataset
3. cd /path/to/the/tacotron2/data/dataset
4. unzip kss.zip -d kss

## Docker build
1. cd /path/to/the/tacotron2
2. docker build --tag tacotron2:latest .

## Training
1. nvidia-docker run -it -n 'tacotron2' -v /path/to/tacotron2:/home/work/tacotron2 --ipc=host --privileged tacotron2:latest
2. cd /home/work/tacotron2
3. `python train.py -g 0 -o data -l kss_v1 -d /home/work/tacotron2/data/dataset/kss`
4. (OPTIONAL) `tensorboard --logdir=outdir/logdir`
