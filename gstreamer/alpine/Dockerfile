FROM smilemakc/opencv:4.1.1-alpine

LABEL maintainer="Maks Balashov <maksbalashov@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive

RUN apk update && apk upgrade --update-cache --available
RUN apk add cmake wget \
    boost-dev \
    pkgconfig \
    gstreamer-dev gst-plugins-base-dev\
    libmicrohttpd-dev gtest-dev gmock \
    gst-plugins-bad \
    gst-plugins-good \
    gst-plugins-ugly \
    gst-plugins-base \
    gstreamer-tools \
    ffmpeg \
    gst-libav libressl-dev \
    autoconf automake gcc g++ make libffi-dev openssl-dev libtool \
    build-base boost-program_options boost-static

RUN cp -r /usr/include/gstreamer-1.0/* /usr/include/

WORKDIR /tmp
RUN wget -O /tmp/0.17.5.tar.gz https://github.com/etr/libhttpserver/archive/0.17.5.tar.gz
RUN mkdir -p /usr/src/libhttpserver && tar -xvf 0.17.5.tar.gz && cp -R libhttpserver-0.17.5/* /usr/src/libhttpserver
WORKDIR /usr/src/libhttpserver
RUN ./bootstrap && mkdir build && cd build && ../configure --enable-static && make && make install
CMD export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
RUN rm -rf /tmp/0.17.5.tar.gz /tmp/libhttpserver-0.17.5