FROM bethgelab/jupyter-deeplearning:cuda6.5

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
    g++ \
    libboost-all-dev \
    libdouble-conversion-dev \
    libedit-dev \
    libevent-dev \
    libgflags-dev \
    libgoogle-glog-dev \
    libiberty-dev \
    libjemalloc-dev \
    libkrb5-dev \
    liblz4-dev \
    liblzma-dev \
    libmatio-dev \
    libnuma-dev \
    libprotobuf-dev \
    libsasl2-dev \
    libsnappy-dev \
    libssl-dev \
    libssl-dev \
    libtool \
    pkg-config \
    protobuf-compiler \
    python-zmq \
    torch7-nv \
    unzip \
  && rm -rf /var/lib/apt/lists/*

# workaround
RUN ln -s /usr/lib/x86_64-linux-gnu/libTH* /usr/lib/

RUN git clone https://github.com/facebook/iTorch.git ~/itorch \
  && cd ~/itorch \
  && luarocks make \
  && cp -r /root/.ipython/* /usr/local/share/jupyter/

RUN luarocks install loadcaffe

RUN mkdir /tmp/build_fblualib \
  && cd /tmp/build_fblualib \
  && git clone -b v0.35.0 --depth 1 https://github.com/facebook/folly.git \
  && git clone -b v0.24.0 --depth 1 https://github.com/facebook/fbthrift.git \
  && git clone https://github.com/facebook/thpp \
  && git clone https://github.com/facebook/fblualib

RUN cd /tmp/build_fblualib/folly/folly \
  && autoreconf -ivf \
  && ./configure \
  && make -j`nproc` \
  && sudo make install \
  && sudo ldconfig

RUN cd /tmp/build_fblualib/fbthrift/thrift \
  && autoreconf -ivf \
  && ./configure \
  && make -j`nproc` \
  && sudo make install

RUN cd /tmp/build_fblualib/thpp/thpp \
  && ./build.sh \
  && cd /tmp/build_fblualib/fblualib/fblualib \
  && ./build.sh \
  && ldconfig \
  && cd / \
  && rm -rf /tmp/build_fblualib

