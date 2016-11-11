# based on Kaixhin's cuda-theano image
# Start with cuDNN base image
# Theano will support cuDNN 5 in version 0.8.2 (not stable yet).
FROM nvidia/cuda:7.0-cudnn4-devel
MAINTAINER Alessio Giambrone <giambrox@gmail.com>
# Install git, wget, python-dev, pip, BLAS + LAPACK and other dependencies
RUN apt-get update && apt-get install -y \
	gfortran \
	git \
	wget \
	liblapack-dev \
	libopenblas-dev \
	python3-dev \
	python3-pip \
	python3-nose \
	python3-numpy \
	python3-scipy
# Set CUDA_ROOT
ENV CUDA_ROOT /usr/local/cuda/bin
# Install bleeding-edge Theano
RUN pip3 install --upgrade --no-deps git+git://github.com/Theano/Theano.git
# Install Keras
RUN pip3 install keras
# Set up .theanorc for CUDA
RUN echo "[global]\ndevice=gpu\nfloatX=float32\noptimizer_including=cudnn\n[lib]\ncnmem=0.1\n[nvcc]\nfastmath=True" > /root/.theanorc
# Set theano as Keras backend
RUN mkdir /root/.keras && echo '{\n"image_dim_ordering": "th",\n"backend": "theano",\n"floatx": "float32",\n"epsilon": 1e-07\n}' > /root/.keras/keras.json
# Set the init script; it moves to root directory and executes "python main.py"
RUN echo "#!/bin/bash\n\ncd /opt\npython3 main.py\n" > /root/init_script.sh
ENTRYPOINT ["sh","/root/init_script.sh"]
