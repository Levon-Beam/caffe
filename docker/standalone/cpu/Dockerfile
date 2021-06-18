FROM ubuntu:18.04
MAINTAINER caffe-maint@googlegroups.com

ENV TC=ASIA/SINGAPORE
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        libopenblas-dev \
        protobuf-compiler \
        python3-dev \
        python3-numpy \
        python3-pip \
        python3-scipy \
	curl \
	nano && \
    rm -rf /var/lib/apt/lists/*


ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT

# FIXME: clone a specific git tag and use ARG instead of ENV once DockerHub supports this.
ENV CLONE_TAG=master

RUN git clone --depth 1 -b ssd https://github.com/Levon-Beam/caffe.git .
RUN pip3 install wheel
RUN for req in $(cat python/requirements.txt) pydot; do pip3 install $req; done
RUN    make all -j3
RUN    make py -j3
RUN    make test -j3
#    mkdir build && cd build && \
#    cmake -DCPU_ONLY=1 .. 

RUN export PYTHONPATH="$CAFFE_ROOT/python":$PYTHONPATH

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR /workspace
