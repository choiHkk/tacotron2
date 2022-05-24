## Training
1. nvidia-docker run -it -n 'tacotron2' -v /path/to/tacotron2:/home/work/tacotron2 --ipc=host --privileged tacotron2:latest
2. cd tacotron2
3. `python train.py -g 0 -o data -l kss_v1 -d /home/work/tacotron2/data/dataset/kss`
4. (OPTIONAL) `tensorboard --logdir=outdir/logdir`
