FROM ubuntu:18.04

LABEL maintainer="Maks Balashov <maksbalashov@gmail.com>"

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

ARG OPENCV_VERSION=4.1.1

RUN apt update && apt install -y software-properties-common && \
    add-apt-repository -y ppa:deadsnakes/ppa && apt install -y \
    # Build dependencies
    build-essential clang libclang-dev cmake pkgconf wget libopenblas-dev \
    linux-headers-generic \
    # Image IO packages
    libjpeg-dev libpng-dev \
    libwebp-dev \
    libtiff-dev \
    jasper \
    libopenexr-dev \
    # Video depepndencies
    ffmpeg \
    libavc1394-dev \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev \
    libgphoto2-dev libtbb-dev \
    python3.7 python3.7-dev python3-numpy && \
    # Make Python3 as default
    ln -vfs /usr/bin/python3 /usr/local/bin/python && \
    ln -vfs /usr/bin/pip3 /usr/local/bin/pip && \
    # Fix libpng path
    ln -vfs /usr/include/libpng16 /usr/include/libpng && \
    ln -vfs /usr/include/locale.h /usr/include/xlocale.h

# Download OpenCV source
RUN cd /tmp && \
    wget -O contib.$OPENCV_VERSION.tar.gz https://github.com/opencv/opencv_contrib/archive/$OPENCV_VERSION.tar.gz && \
    tar -xvzf contib.$OPENCV_VERSION.tar.gz && \
    rm -vrf contib.$OPENCV_VERSION.tar.gz && \
    wget https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz && \
    tar -xvzf $OPENCV_VERSION.tar.gz && \
    rm -vrf $OPENCV_VERSION.tar.gz && \
    # Configure
    mkdir -vp /tmp/opencv-$OPENCV_VERSION/build && \
    cd /tmp/opencv-$OPENCV_VERSION/build && \
    cmake \
        # Compiler params
        -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_C_COMPILER=/usr/bin/clang \
        -D CMAKE_CXX_COMPILER=/usr/bin/clang++ \
        -D CMAKE_INSTALL_PREFIX=/usr \
        # Add modules
        -D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv_contrib-$OPENCV_VERSION/modules \
        -D OPENCV_GENERATE_PKGCONFIG=ON \
        # No examples
        -D INSTALL_PYTHON_EXAMPLES=NO \
        -D INSTALL_C_EXAMPLES=NO \
        # Support
        -D WITH_IPP=NO \
        -D WITH_1394=NO \
        -D WITH_LIBV4L=NO \
        -D WITH_V4l=YES \
        -D WITH_TBB=YES \
        -D WITH_FFMPEG=YES \
        -D WITH_GPHOTO2=YES \
        -D WITH_GSTREAMER=YES \
        # NO doc test and other bindings
        -D BUILD_DOCS=NO \
        -D BUILD_TESTS=NO \
        -D BUILD_PERF_TESTS=NO \
        -D BUILD_EXAMPLES=NO \
        -D BUILD_opencv_java=NO \
        -D BUILD_opencv_python2=NO \
        -D BUILD_ANDROID_EXAMPLES=NO \
        # Build Python3 bindings only
        -D PYTHON3_LIBRARY=`find /usr -name libpython3.so` \
        -D PYTHON_EXECUTABLE=`which python3` \
        -D PYTHON3_EXECUTABLE=`which python3` \
        -D BUILD_opencv_python3=YES .. && \
    # Build
    make -j`grep -c '^processor' /proc/cpuinfo` && \
    make install && \
    # Cleanup
    cd / && rm -vrf /tmp/opencv-$OPENCV_VERSION && \
    cd / && rm -vrf /tmp/opencv_contrib-$OPENCV_VERSION

RUN apt purge -y build-essential clang libclang-dev cmake pkgconf wget libopenblas-dev \
                    libjpeg-dev libpng-dev \
                    libwebp-dev \
                    libtiff-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \
                    python3.7-dev python3-numpy && \
                    rm -vrf /var/cache/apt/*

ENV LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH