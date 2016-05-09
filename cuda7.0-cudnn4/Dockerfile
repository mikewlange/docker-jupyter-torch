FROM bethgelab/jupyter-deeplearning:cuda7.0-cudnn4

MAINTAINER Bethge Lab <opensource@bethgelab.org>

# Install Torch
# -------------
# including Lua, iTorch, loadcaffe, fblualib

RUN apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    autoconf \
    autoconf-archive \
    automake \
    binutils-dev \
    bison \
    cmake \
    flex \
    git-core \
    gnuplot \
    gnuplot-x11 \
    g++ \
    imagemagick \
    libboost-all-dev \
    libdouble-conversion-dev \
    libedit-dev \
    libevent-dev \
    libfftw3-dev \
    libgflags-dev \
    libgoogle-glog-dev \
    libgraphicsmagick1-dev \
    libiberty-dev \
    libjemalloc-dev \
    libjpeg-dev \
    libkrb5-dev \
    liblz4-dev \
    liblzma-dev \
    libmatio-dev \
    libnuma-dev \
    libpng-dev \
    libprotobuf-dev \
    libqt4-core \
    libqt4-gui \
    libqt4-dev \
    libreadline-dev \ 
    libsasl2-dev \
    libsdl2-dev \
    libsnappy-dev \
    libsox-dev \
    libsox-fmt-all \
    libssl-dev \
    libtool \
    libzmq3-dev \
    ncurses-dev \
    pkg-config \
    protobuf-compiler \
    python-zmq \
    sox \
    # torch7-nv \
    unzip \
    zlib1g-dev \
  && rm -rf /var/lib/apt/lists/*

# install torch
RUN git clone https://github.com/torch/distro.git usr/local/torch --recursive \
&& git config --global url.https://github.com/.insteadOf git://github.com/ \
&& cd usr/local/torch && ./install.sh

ENV PATH /usr/local/torch/install/bin:$PATH

# workaround
# RUN ln -s /usr/lib/x86_64-linux-gnu/libTH* /usr/lib/

RUN git clone https://github.com/facebook/iTorch.git ~/itorch \
  && cd ~/itorch \
  && luarocks make \
  && cp -r /root/.ipython/* /usr/local/share/jupyter/

RUN luarocks install loadcaffe
RUN luarocks install nninit

RUN mkdir /tmp/build_fblualib \
  && cd /tmp/build_fblualib \
  && git clone -b v0.35.0 --depth 1 https://github.com/facebook/folly.git \
  && cd /tmp/build_fblualib/folly/folly \
  && autoreconf -ivf \
  && ./configure \
  && make -j`nproc` \
  && sudo make install \
  && sudo ldconfig \
  && cd / \
  && rm -rf /tmp/build_fblualib

RUN mkdir /tmp/build_fblualib \
  && cd /tmp/build_fblualib \
  && git clone -b v0.24.0 --depth 1 https://github.com/facebook/fbthrift.git \
  && cd /tmp/build_fblualib/fbthrift/thrift \
  && autoreconf -ivf \
  && ./configure \
  && make -j`nproc` \
  && sudo make install \
  && cd / \
  && rm -rf /tmp/build_fblualib

RUN mkdir /tmp/build_fblualib \
  && cd /tmp/build_fblualib \
  && git clone -b v1.0 https://github.com/facebook/thpp \
  && cd /tmp/build_fblualib/thpp/thpp \
  && ./build.sh \
  && cd / \
  && rm -rf /tmp/build_fblualib \
  && mkdir /tmp/build_fblualib \
  && cd /tmp/build_fblualib \
  && git clone -b v1.0 https://github.com/facebook/fblualib \
  && cd /tmp/build_fblualib/fblualib/fblualib \
  && ./build.sh \
  && ldconfig \
  && cd / \
  && rm -rf /tmp/build_fblualib

