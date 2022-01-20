FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y \
    python3-opencv ca-certificates python3-dev git wget sudo jupyter ffmpeg python3-empy vim \
    cmake ninja-build protobuf-compiler libprotobuf-dev python3-pip python-pip

## create a non-root user
ARG USER_ID=1000
RUN useradd -m --no-log-init --system  --uid ${USER_ID} ncrl -g sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER ncrl
WORKDIR /home/ncrl

ENV PATH="/home/ncrl/.local/bin:${PATH}"
RUN wget https://bootstrap.pypa.io/get-pip.py && \
    python3 get-pip.py --user && \
    rm get-pip.py


RUN sudo apt-get -o Acquire::ForceIPv4=true update && sudo apt-get -o Acquire::ForceIPv4=true install -q -y \
    build-essential \
    git \
    zlib1g-dev \
    libsdl1.2-dev \
    automake* \
    autoconf* \
    libtool \
    libpixman-1-dev \
    lib32gcc1 \
    lib32ncurses5 \
    # libc6:i386 \
    # libncurses5:i386 \
    # libstdc++6:i386 \
    libusb-1.0.0-dev 



RUN git clone git://git.code.sf.net/p/openocd/code openocd
WORKDIR /home/ncrl/openocd
RUN ./bootstrap
RUN ./configure --prefix=/usr/local  --enable-jlink --enable-amtjtagaccel --enable-buspirate  --enable-stlink   --disable-libftdi
RUN echo -e "all:\ninstall:" > doc/Makefile
RUN make -j4
RUN sudo make install
RUN wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2
RUN tar jxf ./gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2
RUN rm gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2


RUN git clone 