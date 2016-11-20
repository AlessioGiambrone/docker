# cuda7-keras

This is my first docker image, and it is almost completely based on Kaixhin's cuda-theano image.

It uses nvidia-docker-plugin.

## Purposes

- having a reproducible environment to run computation using Keras and CUDA with all the dependencies needed already present
- Python 3.4
- Theano and (obviously) Keras
- having a container that could be run as a command

## Usage

Build the image as usual with something like `docker build -t cuda7-keras .`

Before starting it is needed to run `nvidia-docker-plugin` (as root; 
please see details at https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker-plugin)

The container will mount a volume in its /opt directory and will search
for a file called `main.py`, that will be executed.

The command is
```bash
nvidia-docker run -v $PYTHONFILE_DIRECTORY:/opt/ -t cuda7-keras
```
working in the current directory became, for example
```bash
nvidia-docker run -v `pwd`:/opt/ -t giambrox/cuda7-keras
```
It is convenient to define an alias in `.bashrc`, for example:
```bash
echo alias keras=\'nvidia-docker run -v \`pwd\`:/opt/ -t giambrox/cuda7-keras\'  >> ~/.bashrc
```
Then usage is simply:
```bash
keras
```

## Test

Executing the code from [Testing Theano with GPU](http://deeplearning.net/software/theano/tutorial/using_gpu.html) from the host machine using CPU took 4s with an AMD Athlon II x2 270 CPU (using a single thread); 0.8s with the container using an Nvidia GeForce GT 630 mk.2 (384 cores).
