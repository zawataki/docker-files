FROM ubuntu:16.04 as base
ENV HOME=/root
ENV LLVM_VERSION=6.0.1
ENV LLVM_PATH=$HOME/clang+llvm
ENV CLANG_FORMAT=$LLVM_PATH/bin/clang-format


FROM base as build-clang
WORKDIR $HOME
RUN apt update -y \
 && apt install -y axel xz-utils \
 && axel -a -n 10 -o $LLVM_PATH.tar.xz \
    http://llvm.org/releases/$LLVM_VERSION/clang+llvm-$LLVM_VERSION-x86_64-linux-gnu-ubuntu-14.04.tar.xz \
 && mkdir $LLVM_PATH \
 && tar xf $LLVM_PATH.tar.xz -C $LLVM_PATH --strip-components=1


FROM base
LABEL maintainer="Yuki Takizawa <y.takizawa64@gmail.com>"
WORKDIR $HOME
COPY --from=build-clang $CLANG_FORMAT $CLANG_FORMAT
RUN apt -y update \
 && apt install -y automake pkg-config libpcre3-dev zlib1g-dev liblzma-dev git \
 && export PATH=$HOME/.local/bin:$PATH \
 && git clone https://github.com/ggreer/the_silver_searcher.git \
 && cd the_silver_searcher \
 && ./build.sh \
 && apt install -y python3-pip \
 && pip3 install cram \
 && make test

CMD apt -y update \
 && apt -y upgrade \
 && cd the_silver_searcher \
 && git pull --prune \
 && /bin/bash

