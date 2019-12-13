FROM smilemakc/opencv:4.1.1-ubuntu-18

LABEL maintainer="Maks Balashov <maksbalashov@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt upgrade -y
RUN apt-get install -y cmake wget \
    libboost-all-dev \
    pkg-config \
    libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \
    libmicrohttpd-dev

RUN apt-get install -y libgtest-dev
WORKDIR /usr/src/gtest
RUN cmake . && make
RUN cp /usr/src/gtest/*.a /usr/lib

RUN apt-get install -y google-mock
WORKDIR /usr/src/gmock
RUN cmake . && make
RUN cp /usr/src/gmock/*.a /usr/lib

RUN apt-get update && apt-get install -y \
      gstreamer1.0-plugins-bad \
      gstreamer1.0-plugins-good \
      gstreamer1.0-plugins-ugly \
      gstreamer1.0-plugins-base \
      gstreamer1.0-plugins-bad-videoparsers \
      gstreamer1.0-tools \
      ffmpeg \
      gstreamer1.0-libav
RUN apt install -y libssl-dev
RUN cp -r /usr/include/gstreamer-1.0/* /usr/include/

WORKDIR /usr/src/libhttpserver
RUN wget -O /usr/src/libhttpserver/0.17.5.tar.gz https://github.com/etr/libhttpserver/archive/0.17.5.tar.gz
RUN tar -xvf 0.17.5.tar.gz  && cp -R libhttpserver-0.17.5/* ./
RUN ./bootstrap && mkdir build && cd build && ../configure --enable-static && make && make install
CMD export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib