# Note of Installing Tensorflow
> 2018-04-07

* [1]. [参考博客1](https://blog.csdn.net/qq_26815027/article/details/79629193)
* [2]. [参考博客2](https://www.cnblogs.com/zengcv/p/6564517.html)
* [3]. [参考博客3](https://www.linuxidc.com/Linux/2017-01/139300.htm)

## 1. System and Environment

* Ubuntu14.04 (64bit)
* gcc 4.8.4
* Python 2.7
* Nvidia GeForce GTX 750
```sh
# You can check gcc version by.
gcc --version
# You can use this to check Python version.
python --version
# You can use this to check GPU type.
lspci | grep VGA
```

## 2. Install diver for NVIDIA GPU

```sh
sudo nano /etc/modprobe.d/blacklist.conf
```

Add following script:
```sh
blacklist nouveau
options nouveau modeset=0
```

Check whether it has nouveau:
```sh
lsmod | grep nouveau
```

Download driver from the [NVIDIA](http://www.nvidia.cn/Download/index.aspx?lang=cn) web

![nvidia](https://raw.githubusercontent.com/RotatingSky/Rotating-Sky/master/Tensorflow/Note_Images/2.png)

```sh
cd ~/Download
sudo chmod +x NVIDIA-Linux-x86_64-390.48.run
sudo ./NVIDIA-Linux-x86_64-390.48.run -no-opengl-files
```

## 3. Install [CUDA](https://developer.nvidia.com/cuda-toolkit-archive)

1. Select the appropriate version.

![cuda](https://github.com/RotatingSky/Rotating-Sky/blob/master/Tensorflow/Note_Images/1.png?raw=true)

2. Then open a terminal and enter the path you save it.

```sh
sudo dpkg -i cuda-repo-ubuntu1404-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

3. Other type file to install.

```sh
sudo chmod +x cuda_8.0.61_375.26_linux.run
sudo ./cuda_8.0.61_375.26_linux.run
# And choose almost yes but not driver.
```

## 4. Install [cuDNN](https://developer.nvidia.com/rdp/cudnn-download)
```sh
tar xvzf cudnn-8.0-linux-x64-v6.0.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## 5. Install [Tensorflow](https://github.com/tensorflow)
```sh
# Install pip.
sudo apt-get install python-pip python-dev
# Install Tensorflow by download from https://pypi.python.org/pypi/tensorflow-gpu/1.7.0
sudo pip install tensorflow_gpu-1.4.1-cp27-none-linux_x86_64.whl
# Tensorflow_gpu-1.5 does not fit CUDA8.0
```

Test Tensorflow:
```python
import tensorflow as tf

hello = tf.constant('hello tensorflow')
sess = tf.Session()
print(sess.run(hello))
```

Test Tensorflow GPU:
```python
import tensorflow as tf

with tf.device('/cpu:0'):
    a = tf.constant([1.0,2.0,3.0],shape=[3],name='a')
    b = tf.constant([1.0,2.0,3.0],shape=[3],name='b')

with tf.device('/gpu:1'):
    c = a+b

sess = tf.Session(config=tf.ConfigProto(allow_soft_placement=True,log_device_placement=True))
sess.run(tf.global_variables_initializer())
print(sess.run(c))
```

Install python libraries:
```sh
sudo apt-get install python-numpy
sudo apt-get install python-scipy
sudo apt-get install python-matplotlib
```