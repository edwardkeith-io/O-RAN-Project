# Use the official Ubuntu 22.04 as a base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Pacific/Auckland

# Install necessary packages and configure timezone non-interactively
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive TZ=Pacific/Auckland apt-get -y install tzdata && \
    apt-get install -y \
    sudo \
    vim \
    curl \
    gnupg \
    lsb-release \
    software-properties-common \
    net-tools \
    iputils-ping \
    usbutils \
    && rm -rf /var/lib/apt/lists/*

# Install Docker inside the container (optional, if needed)
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
    && apt-get update && apt-get install -y docker-ce docker-ce-cli containerd.io

# UHD setup
RUN apt-get update && apt-get -y install \
    autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git \
    gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev \
    libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 \
    libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 \
    libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click \
    python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps \
    python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools \
    python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget && \
    mkdir -p /root/workarea && \
    cd /root/workarea && \
    git clone https://github.com/EttusResearch/uhd.git && \
    cd uhd && git checkout v4.7.0.0 && \
    cd host && \
    mkdir build && cd build && \
    cmake .. && make && make test && sudo make install && sudo ldconfig && \
    echo 'export LD_LIBRARY_PATH=/usr/local/lib' >> ~/.bashrc && \
    sudo uhd_images_downloader

# Copy the udev rule (do NOT reload udev during build)
RUN cd /root/workarea/uhd/host/utils && \
    sudo cp uhd-usrp.rules /etc/udev/rules.d/

# srsRAN setup
RUN apt-get update && apt-get install -y \
    build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev && \
    sudo add-apt-repository ppa:softwareradiosystems/srsran && \
    apt-get update && apt-get install -y \
    libboost-system-dev libboost-test-dev libboost-thread-dev libqwt-qt5-dev qtbase5-dev && \
    git clone https://github.com/srsLTE/srsGUI.git && \
    cd srsGUI && mkdir build && cd build && cmake ../ && make && \
    cd /root && git clone https://github.com/srsRAN/srsRAN_4G.git && \
    cd srsRAN_4G && mkdir build && cd build && cmake ../ && make && sudo make install && \
    sudo ./srsran_install_configs.sh user && sudo ./srsran_install_configs.sh service

# Replace the udevadm reload and trigger commands with a shell script that can be executed at runtime
CMD /bin/bash
