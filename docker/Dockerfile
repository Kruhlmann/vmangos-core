FROM ubuntu:18.04
LABEL title="vmangos docker" maintainer="Andreas Krühlmann" version="1.0"

RUN apt-get update -qq && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    automake \
    git-core \
    autoconf \
    make \
    patch \
    libmysql++-dev \
    mysql-server \
    libtool \
    libssl-dev \
    grep \
    binutils \
    zlibc \
    libc6 \
    libbz2-dev \
    cmake \
    subversion \
    libboost-all-dev \
    libace-dev \
    libtbb-dev

WORKDIR /opt/vmangos
COPY ./CMakeLists.txt/ ./CMakeLists.txt
COPY ./cmake/ ./cmake/
COPY ./dep/ ./dep/
COPY ./contrib/ ./contrib/
COPY ./src/ ./src/
RUN pwd
RUN ls
RUN cmake .
RUN make
RUN make install
RUN ln -sf /opt/server/bin/mangosd /mangosd
RUN ln -sf /opt/server/bin/realmd /realmd
