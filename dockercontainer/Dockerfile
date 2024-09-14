# Use the official Ubuntu 22.04 as a base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages
RUN apt-get update && apt-get install -y \
    sudo \
    vim \
    curl \
    gnupg \
    lsb-release \
    software-properties-common \
    net-tools \
    iputils-ping \
    && rm -rf /var/lib/apt/lists/*

# Install Docker inside the container (optional, if needed)
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
    && apt-get update && apt-get install -y docker-ce docker-ce-cli containerd.io

# Add your project-specific setup here
# For example, install ORAN-related packages, copy project files, etc.
# RUN apt-get install -y oran-related-packages
# COPY . /path/to/project

#UHD setup
RUN sudo apt-get update \
    sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget \
    cd $HOME \
    mkdir workarea \
    cd workarea \
    git checkout v4.7.0.0 \
    cd host \
    mkdir build \
    cd build \
    cmake .. \
    make \
    make test \
    sudo make install \
    sudo ldconfig \
    nano ~/.bashrc \
    echo 'export LD_LIBRARY_PATH=/usr/local/lib' >> ~/.bashrc \
    sudo uhd_images_downloader \
    
    # install a udev rule so that non-root users may access the device.
    cd $HOME/workarea/uhd/host/utils \
    sudo cp uhd-usrp.rules /etc/udev/rules.d/ \
    sudo udevadm control --reload-rules \
    sudo udevadm trigger \

#srsRAN including srsue, srsenb, and srsepc setup
    # need to reboot here
    sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev \
    sudo add-apt-repository ppa:softwareradiosystems/srsran \
    sudo apt-get update \
    
    # need to reboot here
    sudo apt-get install libboost-system-dev libboost-test-dev libboost-thread-dev libqwt-qt5-dev qtbase5-dev \
    git clone https://github.com/srsLTE/srsGUI.git \
    cd srsGUI \
    mkdir build \
    cd build \
    cmake ../ \
    make \ 
    cd $HOME \
    git clone https://github.com/srsRAN/srsRAN_4G.git \
    cd srsRAN_4G \
    mkdir build \
    cd build \
    cmake ../ \ 
    make \
    sudo make install \
    sudo ./srsran_install_configs.sh user \
    sudo ./srsran_install_configs.sh service \

#edits we need for enb.conf and epc.conf
#[gui] enable = false
#tx_gain = 50
#comment out rx_gain to enable AGC, makes automatic adjustments to the module's RF gain, in order to maintain an overall stable output level
#device_name = UHD

# Expose necessary network ports (if any)
# EXPOSE 1234

# Define entry point
# CMD ["/path/to/executable"]
