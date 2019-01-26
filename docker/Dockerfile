ARG cuda_version=9.0
ARG cudnn_version=7
FROM continuumio/miniconda3

# Install system packages
RUN apt-get update && apt-get install -y --no-install-recommends \
      bzip2 \
      g++ \
      git \
      graphviz \
      libgl1-mesa-glx \
      libhdf5-dev \
      openmpi-bin \
      wget && \
    rm -rf /var/lib/apt/lists/*

# Install conda
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH

# Install Python packages and keras
ENV NB_USER keras
ENV NB_UID 1000

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    chown $NB_USER $CONDA_DIR -R && \
    mkdir -p /src && \
    chown $NB_USER /src

USER $NB_USER

ARG python_version=3.6

RUN conda config --append channels conda-forge
RUN conda install -y python=${python_version} && \
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade pip && \
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple \
      sklearn_pandas \
      tensorflow \
      cntk-gpu && \
    conda install \
      bcolz \
      h5py \
      matplotlib \
      mkl \
      nose \
      notebook \
      Pillow \
      pandas \
      pydot \
      pygpu \
      pyyaml \
      scikit-learn \
      six \
      theano \
      mkdocs \
      && \
    git clone git://github.com/keras-team/keras.git /src && pip install -e /src[tests] && \
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple git+git://github.com/keras-team/keras.git && \
    conda clean -yt

ADD theanorc /home/keras/.theanorc

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

ENV PYTHONPATH='/src/:$PYTHONPATH'

WORKDIR /data

EXPOSE 8888

CMD jupyter notebook --port=8888 --ip=0.0.0.0
