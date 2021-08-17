# Multistage docker build, requires docker 17.05
#
# cd ~/Dockers/DebianHaven
# docker build -t blueyred/deb10:v10 .
# docker run -ti --rm blueyred/deb10:v10 /bin/bash
#
# builder stage Debian 10
FROM debian:10-slim as builder

ARG DEBIAN_FRONTEND=noninteractive

RUN set -ex && \
    apt-get update && \
    apt-get upgrade && \
    apt-get --no-install-recommends --yes install \
    build-essential \
    cmake \
    pkg-config \
    git \
	libboost-all-dev \
	libssl-dev \
	libzmq3-dev \
	libnorm-dev \
	libunbound-dev \
	libsodium-dev \
	libunwind8 \
	libunwind-dev \
	liblzma-dev \
	libreadline-dev \
	libreadline7 \
	libldns-dev \
	libexpat1-dev \
	doxygen \
	graphviz \
	libpgm-dev \
	qttools5-dev-tools \
	libhidapi-dev \
	libhidapi-libusb0 \
	libusb-dev \
	libprotobuf-dev \
	protobuf-compiler \
	libusb-1.0-0-dev \
	libudev-dev \
	libminiupnpc-dev \
	miniupnpc \
	libgss-dev \
	libsasl2-dev \
	librpcsecgss-dev \
	libglobus-gss-assist-dev \
	libglobus-gssapi-error-dev \
	libglobus-gssapi-gsi-dev \
	libgssdp-1.0-dev \
	libgssglue-dev \
	librpcsecgss-dev \
	libzmq5 \
	libgtest-dev \
	ccache \
	qt5-default \
	python3 \
	nodejs 

#java sdk needed for emscripten, it needs a few folders setup to not complain during install
RUN mkdir -p /usr/share/man/man1 /usr/share/man/man2

RUN set -ex && \ 
	apt-get --no-install-recommends --yes install default-jdk
    
# emscripten needs python3 to be the default
RUN set -ex \
&& update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1 \
&& update-alternatives --install /usr/bin/python python /usr/bin/python3.7 2

WORKDIR /usr/local

RUN set -ex \
    && git clone https://github.com/emscripten-core/emsdk.git \
    && cd emsdk \
    && ./emsdk install latest-upstream \
    && ./emsdk activate latest-upstream 
    
RUN /bin/bash -c "source /usr/local/emsdk/emsdk_env.sh"

ARG EMSCRIPTEN=/usr/local/emsdk/upstream/emscripten 


    
