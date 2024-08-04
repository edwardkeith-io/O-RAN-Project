
## <mark style="background: #FFB8EBA6;">If needed: Mac OS Environment Prep</mark>

Install Xcode from the Mac App Store (it's free), open it to be able to agree to the T&C pop-ups, and download the base environment stuff for Mac OS. Then open a terminal to install the Apple Command Line Developer Tools:

```
~ % xcode-select --install
```

If you already have Xcode and the Apple Command Line Developer Tools installed, make sure Xcode is fully up-to-date and repoint the Apple Command Line Developer Tools it with the following command (they seem to cache the old version of Xcode and it'll confuse any package managers trying to using it):

```
~ % sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

The UHD device driver library has some helpful commands that can be utilized from the command line/terminal of the host PC. The first of which being the `uhd_find_devices` command. This will prompt any USRP connected to the host to respond back with an identifier name (ie - B205mini) and the connection address of the USRP whether it be a serial or network connection.

As mentioned above, any UHD devices connected to the host will respond with an identifier name and its connection address.

```
~ % uhd_find_devices---------------------------------------------------- UHD Device 2--------------------------------------------------Device Address:serial: 31DDAD8name: B205iproduct: B205minitype: b200
```

It's worth noting that there isn't a need to pay attention to how each UHD device is indexed in the system (my B205mini was indexed as "UHD Device 2" on my system) because that index isn't a dependency to connect/interface to the UHD device. The value listed under **Device Address:** is what is used in GNU Radio to target a USRP device.

## <mark style="background: #FFB8EBA6;">Specific OSX + USRP info:</mark>

[https://www.hackster.io/whitney-knitter/ettus-b205mini-on-mac-os-part-1-install-setup-b1346d](https://www.hackster.io/whitney-knitter/ettus-b205mini-on-mac-os-part-1-install-setup-b1346d "https://www.hackster.io/whitney-knitter/ettus-b205mini-on-mac-os-part-1-install-setup-b1346d")

[https://www.hackster.io/whitney-knitter/rtl-sdr-on-mac-os-part-1-install-setup-4d23da](https://www.hackster.io/whitney-knitter/rtl-sdr-on-mac-os-part-1-install-setup-4d23da "https://www.hackster.io/whitney-knitter/rtl-sdr-on-mac-os-part-1-install-setup-4d23da")

## <mark style="background: #FFB8EBA6;">This is for installing on OSX but can be applied to other operating systems</mark>

### <mark style="background: #BBFABBA6;">First install UHD (this is the driver for the USRP radio unit - B205mini)</mark>

https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_OS_X

### <mark style="background: #BBFABBA6;">After UHD, install the mandatory requirements:</mark>

cmake [https://cmake.org/](https://cmake.org/ "https://cmake.org/")

libfftw [http://www.fftw.org/](http://www.fftw.org/ "http://www.fftw.org/")

mbedTLS [https://tls.mbed.org/](https://tls.mbed.org/ "https://tls.mbed.org/")

Boost [http://www.boost.org/](http://www.boost.org/ "http://www.boost.org/")

Iksctp [http://lksctp.sourceforge.net/](http://lksctp.sourceforge.net/ "http://lksctp.sourceforge.net/")

config [http://www.hyperrealm.com/libconfig/](http://www.hyperrealm.com/libconfig/ "http://www.hyperrealm.com/libconfig/")

### <mark style="background: #BBFABBA6;">Then install srsGUI for real time plotting</mark> 

[https://github.com/srsran/srsgui](https://github.com/srsran/srsgui "https://github.com/srsran/srsgui")

### <mark style="background: #BBFABBA6;">Then download and build srsRAN 4G using this command:</mark>

`git clone https://github.com/srsRAN/srsRAN_4G.gitcd srsRAN_4Gmkdir buildcd buildcmake ../makemake test`

### Then install srsRAN 4G using this command:

`sudo make installsrsran_install_configs.sh user`

This installs srsRAN 4G and also copies the default srsRAN 4G config files to _~/.config/srsran_.

### <mark style="background: #BBFABBA6;">Default config file:</mark>

https://github.com/srsRAN/srsRAN_4G/blob/master/srsue/ue.conf.example