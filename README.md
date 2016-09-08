# Caffe Compilation
This manuscript describes how to compile Caffe on Ubuntu.

Created by Jingming Dong (12/10/2015).

## Tested Platform
Ubuntu 14.04.3 LTS (3.19.0-25-generic)

python2.7

Matlab 2015b

Caffe version (master branch on 12/10/2015)

## Prerequisites

### CUDA:
Download from https://developer.nvidia.com/cuda-downloads and run the installer.

Install the latest GPU driver again as recommend by Caffe instruction.

(Note that sometimes the latest driver crashes Ubuntu, then use the one that comes with the cuda installer.)

### cuDNN:
Download from https://developer.nvidia.com/cudnn and copy to system library path as mentioned later.

### Prerequisites installed from terminal:

```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install python-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

### Prerequisites installed from synaptic:
Search and install python-numpy, python-scipy, python-yaml

### Set up Bash:
```
export PATH=/usr/local/MATLAB/R2015b/bin:/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUBLAS_PATH=/usr/local/cuda/lib64
export CUDNN_PATH=/usr/local/cudnn
export PYTHONPATH=/path/to/caffe/python:$PYTHONPATH
```

### Copy cudnn.h to /usr/local/include, and cudnn libraries to /usr/local/lib and /usr/lib
```
sudo cp /usr/local/cudnn/cudnn.h /usr/local/include
sudo cp /usr/local/cudnn/libcudnn.so /usr/local/lib
sudo cp /usr/local/cudnn/libcudnn.so.7.0 /usr/local/lib
sudo cp /usr/local/cudnn/libcudnn.so.7.0.64 /usr/local/lib
sudo cp /usr/local/cudnn/libcudnn_static.a /usr/local/lib
sudo cp /usr/local/cudnn/libcudnn.so /usr/lib
sudo cp /usr/local/cudnn/libcudnn.so.7.0 /usr/lib
sudo cp /usr/local/cudnn/libcudnn.so.7.0.64 /usr/lib
sudo cp /usr/local/cudnn/libcudnn_static.a /usr/lib
```
(Note /usr/local/cudnn is where I put my cudnn library. Caffe search for default location only.)

(A better solution is needed as modifying /usr/lib is a bad idea.)

## Compilation

### Caffe

Edit configuration file and compile
```
cp Makefile.config.example Makefile.config
```
Edit configuration file. An example compatible with the above prerequisites is provided in this repository (Makefile.config). Note that it uses OpenCV 3.x by installing libopencv-dev. If you are using OpenCV 2.x, change accordingly.
```
make all
make test
make runtest
```

### Python interface
```
make pycaffe
sudo pip install -U scikit-image
sudo pip install protobuf
```

### Matlab interface
```
make matcaffe
make mattest
```
You need to add path in Matlab to use the matcaffe
```
addpath(genpath([<path_to_caffe_root> ‘/matlab’]));
```

## Download models

```
scripts/download_model_binary.py <dirname> 
```
where `<dirname>` is specified below:

BVLC Reference CaffeNet in models/bvlc_reference_caffenet: AlexNet trained on ILSVRC 2012, with a minor variation from the version as described in ImageNet classification with deep convolutional neural networks by Krizhevsky et al. in NIPS 2012. (Trained by Jeff Donahue @jeffdonahue)

BVLC AlexNet in models/bvlc_alexnet: AlexNet trained on ILSVRC 2012, almost exactly as described in ImageNet classification with deep convolutional neural networks by Krizhevsky et al. in NIPS 2012. (Trained by Evan Shelhamer @shelhamer)

BVLC Reference R-CNN ILSVRC-2013 in models/bvlc_reference_rcnn_ilsvrc13: pure Caffe implementation of R-CNN as described by Girshick et al. in CVPR 2014. (Trained by Ross Girshick @rbgirshick)

BVLC GoogLeNet in models/bvlc_googlenet: GoogLeNet trained on ILSVRC 2012, almost exactly as described in Going Deeper with Convolutions by Szegedy et al. in ILSVRC 2014. (Trained by Sergio Guadarrama @sguada)

You can find more models in Caffe Model Zoo (https://github.com/BVLC/caffe/wiki/Model-Zoo).
