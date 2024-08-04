
## <mark style="background: #FFB8EBA6;">For UHD driver install:</mark>

`sudo apt-get update`

Reboot

```
sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget
```

Reboot

Test UHD device recognition with `lsusb` - make sure it is connected directly to the USB port of the computer, not through a USB hub. 

If it is present as `Ettus Research LLC USRP B205-mini` then proceed to srsRAN steps.

https://github.com/srsran/srsRAN_4G

## <mark style="background: #FFB8EBA6;">For srsRAN 4G dependencies install:</mark>

````
sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev

sudo add-apt-repository ppa:softwareradiosystems/srsran

sudo apt-get update
````

Reboot

````
sudo apt-get install libboost-system-dev libboost-test-dev libboost-thread-dev libqwt-qt5-dev qtbase5-dev

git clone https://github.com/srsLTE/
cd srsGUI
mkdir build
cd build
cmake ../
make 

make test
````

## <mark style="background: #FFB8EBA6;">For srsRAN 4G install:</mark>

````
git clone https://github.com/srsRAN/srsRAN_4G.git
cd srsRAN_4G  
mkdir build  
cd build  
cmake ../  
make  
sudo make install  
sudo ./srsran_install_configs.sh user
sudo ./srsran_install_configs.sh service
````

This installs srsRAN 4G and also copies the default srsRAN 4G config files to _~/.config/srsran_.

## <mark style="background: #FFB8EBA6;">Test srsRAN:</mark>


### <mark style="background: #BBFABBA6;">srsEPC</mark>

On machine 1, open a terminal and run srsEPC as follows:

`sudo srsepc`

Using the default configuration (made in the last section), this creates a virtual network interface named `“srs_spgw_sgi”` on machine 1 with the IP address 172.16.0.1. All connected machines running srsUE will be assigned an IP in this network.

### <mark style="background: #BBFABBA6;">srsENB</mark>

Also on machine 1, but in another terminal, run srsENB as follows:

`sudo srsenb`

### <mark style="background: #BBFABBA6;">srsUE</mark>

On machine 2, run srsUE as follows:

`sudo srsue`

Using the default configuration, this creates a virtual network interface named `“tun_srsue”` on machine 2 with an IP in the network 172.16.0.x. Assuming the UE has been assigned IP 172.16.0.2, you may now exchange IP traffic with machine 1 over the LTE link. For example, run a ping to the default SGi IP address using the command:

`ping 172.16.0.1`

## <mark style="background: #D2B3FFA6;">END</mark>

