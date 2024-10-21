# <mark style="background: #FFB8EBA6;">Mac OS Environment Prep</mark>

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