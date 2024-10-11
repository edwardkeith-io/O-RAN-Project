#tech #aut #programming #howto #srsRAN #4GLTE #RnD 

---
# <mark style="background: #D2B3FFA6;">Executive Summary</mark>


*What is this document?*

## Desired Outcome:


*Insert goal here*


---
# <mark style="background: #FFB86CA6;">System and hardware requirements</mark>

There are two main parts of a mobile base-station – the radio access network (RAN) and the mobile core network. These parts are typically bundled and sold as proprietary hardware and software. However, the advance in virtualization technologies and commercial off-the-shelf (COTS) hardware has made it possible to implement such a base station using open-source software and Software Defined Radios (SDR). 

These installation and operation steps specifically apply to the equipment used in our testbed. If the SDR module you are using is different, the driver steps may not be applicable. Please refer to the manufacturer's driver installation guide for your SDR. For our specific testing configuration, we used the following:

## Ettus B205mini USRP

B205mini external connections and respective power information:

| Component ID | Description                           | Details                                        |
| ------------ | ------------------------------------- | ---------------------------------------------- |
| USB3         | USB Connector                         | USB 3.0                                        |
| J1 Antenna   | TRX (Transmission and Reception)      | TX power +20 dBm max  <br>RX power -15 dBm max |
| J2 Antenna   | RX2 (Reception)                       | RX power -15 dBm max                           |
| J3 Antenna   | External 10 MHz/PPS Reference (Clock) | +15 dBm max                                    |

## Laptops

*Insert laptop specs here*

## Operating System

Linux Ubuntu 22.04 

## Software Elements

Required dependencies and build tools:

- [cmake](https://cmake.org/)
- [gcc](https://gcc.gnu.org/) (v9.4.0 or later) **OR** [Clang](https://clang.llvm.org/) (v10.0.0 or later)
- [libfftw](https://www.fftw.org/)
- [libsctp](https://github.com/sctp/lksctp-tools)
- [yaml-cpp](https://github.com/jbeder/yaml-cpp)
- [mbedTLS](https://www.trustedfirmware.org/projects/mbed-tls/)     
- [googletest](https://github.com/google/googletest/)

srsRAN Package:

- srsEPC
- srsENB
- srsUE

UHD Package:

- UHD RF drivers
- GNU Radio

## Minimum hardware configuration for 4G/5G NSA network implementation:

| MODE           | USRPS        | CPU CORES | THREADS | RAM  | NIC     | RF CHANNELS | MTU         |
| -------------- | ------------ | --------- | ------- | ---- | ------- | ----------- | ----------- |
| 4G             | B-2xx        | 2         | 4       | 4GB  | USB 3.0 | 1x1         | ~1500 Bytes |
| 4G             | X3xx, N3xx   | 4         | 8       | 8GB  | 1Gbps   | 1x1         | ~1500 Bytes |
| 5G             | X3xxs, N3xxs | 8         | 16      | 16GB | 10Gbps  | 2x2         | ~9000 Bytes |
| 5G (Simulated) | None         | 2         | 4       | 4GB  | None    | None        | None        |

---
# <mark style="background: #ADCCFFA6;"> Native Installation</mark>

## Stage 1: Install UHD drivers for the SDR

1. **Update and Install Required Packages**:
   This step ensures the system has all necessary libraries and tools for building UHD.

      ```bash
   sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget
   ```

2. **Create a Working Directory**:
   This step creates the necessary working directories and prepares the system for UHD installation.
   
   ```bash
   mkdir -p /root/workarea && cd /root/workarea
   ```

3. **Clone the UHD Repository**:
   Download the UHD repository from GitHub, specifically checking out version 4.7.0.0.
   
   ```bash
   git clone https://github.com/EttusResearch/uhd.git && \
   cd uhd && git checkout v4.7.0.0
   ```

4. **Build and Install UHD**:
   Now, move into the host directory, build the UHD host libraries, and run the tests.
   
   ```bash
   cd host && mkdir build && cd build && \
   cmake .. && make && make test
   ```

5. **Install UHD Libraries**:
   Finally, install the compiled UHD libraries and update the system library path.
   
   ```bash
   sudo make install && sudo ldconfig
   ```

6. **Configure Environment Variable**:
   Add the library path to the bash configuration file to ensure it's available for future sessions.
   
   ```bash
   echo 'export LD_LIBRARY_PATH=/usr/local/lib' >> ~/.bashrc
   ```

7. **Download UHD Firmware Images**:
   This step ensures the necessary UHD firmware is downloaded.
   
   ```bash
   sudo uhd_images_downloader
   ```

Did it work? 
- Yes - Continue to Native Installation, Stage 2
- No - Refer to troubleshooting part ?
## Stage 2: Test and Verify the Operation of the USRP

- Connect the USRP to the Host Computer
The included USB 3.0 cable provides power and data connectivity for the USRP Bus Series. The host-side of the cable must be plugged into either a USB 2.0 or 3.0 port. Note that a USB 2.0 link provides less bandwidth, and USB 3.0 is recommended. 

Once the software tools are installed on the host computer, verify the correct operation of the USRP by running the utility programs on the host computer.

Devices attached to your system can be discovered using the `uhd_find_devices` program. This program scans your system for supported devices and prints out an enumerated list of discovered devices and their addresses. The list of discovered devices can be narrowed down by specifying device address args.

```bash
uhd_find_devices
```

Device address arguments can be supplied to narrow the scope of the search.

```bash
uhd_find_devices --args="type=usrp1"

-- OR --

uhd_find_devices --args="serial=12345678"
```

Did it work? 
- Yes - Continue to Native Installation, Stage 3
- No - Refer to troubleshooting part ?
## Stage 3: Install srs-RAN software

1. **Install Dependencies for srsRAN**
    - Installs essential tools and libraries for compiling and running srsRAN.
    ```bash
    apt-get update && apt-get install -y 
        build-essential cmake libfftw3-dev libmbedtls-dev 
        libboost-program-options-dev libconfig++-dev libsctp-dev
    ```

2. **Add Software Radio Systems Repository**
    - Adds the SRS PPA for accessing the required packages.
    ```bash
    sudo add-apt-repository ppa:softwareradiosystems/srsran && apt-get update
    ```

3. **Install Additional Libraries for srsGUI**
    - Installs libraries required for the srsGUI interface.
    ```bash
    apt-get install -y 
        libboost-system-dev libboost-test-dev libboost-thread-dev
        libqwt-qt5-dev qtbase5-dev
    ```

4. **Clone and Build srsGUI**
    - Clones the **srsGUI** repository and builds the graphical user interface.
    ```bash
    git clone https://github.com/srsLTE/srsGUI.git && 
    cd srsGUI && mkdir build && cd build && cmake ../ && make
    ```

5. **Clone and Build srsRAN**
    - Clones the **srsRAN 4G** repository, builds the source, and installs it.
    ```bash
    cd /root && git clone https://github.com/srsRAN/srsRAN_4G.git && 
    cd srsRAN_4G && mkdir build && cd build && cmake ../ && make && sudo make install
    ```

6. **Install srsRAN Configuration Files**
    - Installs configuration files for both user and service mode.
    ```bash
    sudo ./srsran_install_configs.sh user && sudo ./srsran_install_configs.sh service
    ```

In the event that the above configuration install command fails, a srsUE example configuration file can be found [here](https://github.com/srsRAN/srsRAN_4G/blob/master/srsue/ue.conf.example)

Did it work? 
- Yes - Continue to Operation, Stage 1
- No - Refer to troubleshooting part ?


---
# <mark style="background: #ADCCFFA6;">Docker Installation</mark>

## How to install Docker

Notes on what operating system we recommend go here, and reasoning (usb compatiblity, SDR funkiness etc)
### Install Docker On Windows

### Install Docker On Mac

### Install Docker On Linux 

Did it work? 
- Yes - Continue to Docker Operation, Stage 1
- No - Refer to troubleshooting part ?

# Docker Operation

How to run the docker container

Then refer to Operation, Stage 1

---
# <mark style="background: #BBFABBA6;">Operation</mark>

## Stage 1: Run srs-RAN software and verify network

Did it work? 
- Yes - Continue to Stage 2
- No - Refer to troubleshooting part ?

## Stage 2: Configure srs-RAN settings to desired parameters

Did it work? 
- Yes - Continue to ???
- No - Refer to troubleshooting part ?

---
# <mark style="background: #D2B3FFA6;">Performance Testing</mark>

*Insert performance testing steps here*


---
# <mark style="background: #FFB86CA6;">Troubleshooting</mark>

## Native Installation Stage 1

*Insert*

## Native Installation Stage 2

If the USRP device does not show when running the `uhd_find_devices` command, try these additional troubleshooting steps.

Properties of devices attached to your system can be probed with the `uhd_usrp_probe` program. This program constructs an instance of the device and prints out its properties, such as detected daughterboards, frequency range, gain ranges, etc...

```bash
uhd_usrp_probe --args <device-specific-address-args>
```

**Naming a USRP Device**

For convenience purposes, users may assign a custom name to their USRP device once it is found. The USRP device can then be identified via name, rather than a difficult to remember serial or address. A name has the following properties:

- is composed of ASCII characters
- is 0-20 characters
- is not required to be unique

Run the following commands:

```bash
cd <install-path>/lib/uhd/utils
./usrp_burn_mb_eeprom --args=<optional device args> --values="name=lab1_xcvr"
```

**Discovery via name**

The keyword `name` can be used to narrow the scope of the search. Example with the find devices utility:

```bash
uhd_find_devices --args="name=lab1_xcvr"

-- OR --

uhd_find_devices --args="type=usrp1, name=lab1_xcvr"
```


**Below is a table of the B200mini LED indicators and their meanings:**

| Component ID | Description       | Details                                                                                                                     |
| ------------ | ----------------- | --------------------------------------------------------------------------------------------------------------------------- |
| PWR LED      | Power Indicator   | off = no power applied  <br>on = power applied (external or USB)                                                            |
| TRX LED      | TX/RX Activity    | off = no activity  <br>green = receiving  <br>red = transmitting  <br>orange = switching between transmitting and receiving |
| RX2 LED      | RX2 Activity      | off = no activity  <br>green = receiving                                                                                    |
| S0 LED       | Reference Lock    | off = no activity  <br>green = locked                                                                                       |
| S1 LED       | Reference Present | off = reference level low or not present  <br>green = reference level high                                                  |

## Section 2 

## Section 3


---

# <mark style="background: #CACFD9A6;">References:</mark>

https://files.ettus.com/manual/page_install.html
https://files.ettus.com/manual/page_identification.html
https://files.ettus.com/manual/page_usrp_b200.html
https://kb.ettus.com/B200/B210/B200mini/B205mini_Getting_Started_Guides

https://docs.srsran.com/projects/4g/en/latest/usermanuals/source/1_setup.html
https://docs.srsran.com/projects/project/en/latest/user_manuals/source/installation.html
https://github.com/srsRAN/srsRAN_4G/blob/master/srsue/ue.conf.example