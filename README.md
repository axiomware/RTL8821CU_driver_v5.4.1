# RTL8821CU_RTL8811CU_driver version v5.4.1
RTL8811CU driver

The Realtek RTL8811CU-CG is a highly integrated single-chip that supports 1-stream 802.11ac solutions with Multi-user MIMO (Multiple-Input, Multiple-Output) and Wireless LAN (WLAN) USB interface controller. It combines a WLAN MAC, a 1T1R capable WLAN baseband, and RF in a single chip. The RTL8811CU-CG provides an outstanding solution for a high-performance integrated wireless device.:
- USB high speed interface
- 802.11ac/abgn, 802.11ac
- 2.4 GHz Support
- 5.8 GHz Support
- Supports concurrent mode (operates as two virtual WLAN interfaces)
- MIMO config - 1x1
- MU-MIMO
- AC wave2
- 256 QAM

## Linux driver support
Linux uses 8821cu.ko for driver. Check if the USB sub-system recognizes the device.  
```bash
lsusb | grep Realtek
Bus 001 Device 003: ID 0bda:c811 Realtek Semiconductor Corp.
```
```bash
lsmod | grep 8821cu
8821cu               2260992  0
cfg80211              589824  1 8821cu
usbcore               253952  6 ehci_hcd,xhci_pci,btusb,8821cu,xhci_hcd,ehci_pci
```

If 8821cu is present, everything is good to go!

## Building the driver
If the driver is not present, it can be built using the included driver sources.  
 - [Driver sources](./driver) - version : rtl8821CU_WiFi_linux_v5.4.1_28754.20180921_COEX20180712-3232
 - [Documents](./document)
 - Build tools - Install build tools, if needed

```bash
# Install build tools
sudo apt-get install build-essential -y
sudo apt-get install bc -y
sudo apt-get install unzip git -y

# install kernel headers
sudo apt-get install linux-headers-$(uname -r)

# check
apt search linux-headers-$(uname -r)
ls -l /usr/src/linux-headers-$(uname -r)
```
#### Option 1 - Direct build

```bash
git clone https://github.com/axiomware/RTL8821CU_driver_v5.4.1.git
cd RTL8821CU_driver_v5.4.1/driver

# Modify Makefile, if needed (see installation document for details)
# default Makefile is for:
# x86 target
# Concurrent mode enabled
# Monitor mode enabled
# Mesh point

# Build
make

#install
sudo make install

# reboot to start the wireless module or use modprobe to load the driver
sudo modprobe 8821cu

#uninstall the driver using make
sudo make uninstall

# or unistall directly if make is not installed
KVER=$(uname -r)
MODFILE=/lib/modules/$KVER/kernel/drivers/net/wireless/8821cu.ko
sudo rm -f $MODFILE
sudo /sbin/depmod -a $KVER
```
#### Option 2 - DKMS build (verified on Debian 9)

DKMS is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the dkms package, which on Debian (based) systems is done like this:

```bash
sudo apt-get install dkms

DRV_NAME=rtl8821CU
DRV_VERSION=5.4.1

git clone https://github.com/axiomware/RTL8821CU_driver_v5.4.1.git

# Modify Makefile, if needed (see installation document for details)
# default Makefile is for:
# x86 target
# Concurrent mode enabled
# Monitor mode enabled
# Mesh point

sudo cp -r  RTL8821CU_driver_v5.4.1/driver /usr/src/${DRV_NAME}-${DRV_VERSION}

# Build and install
sudo dkms add -m ${DRV_NAME} -v ${DRV_VERSION}
sudo dkms build -m ${DRV_NAME} -v ${DRV_VERSION}
sudo dkms install -m ${DRV_NAME} -v ${DRV_VERSION}

# reboot to start the wireless module or use modprobe to load the driver
sudo modprobe 8821cu

# To remove a driver, do the following:
DRV_NAME=rtl8821CU
DRV_VERSION=5.4.1
sudo dkms remove ${DRV_NAME}/${DRV_VERSION} --all
```

## Test Driver

```bash
sudo iwconfig
enp3s0    no wireless extensions.

enp1s0    no wireless extensions.

wlx30eb1f04ecad  unassociated  Nickname:"<WIFI@REALTEK>"
          Mode:Auto  Frequency=2.412 GHz  Access Point: Not-Associated
          Sensitivity:0/0
          Retry:off   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality:0  Signal level:0  Noise level:0
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0

enp2s0    no wireless extensions.

lo        no wireless extensions.

wlp0s18u1u2  unassociated  Nickname:"<WIFI@REALTEK>"
          Mode:Auto  Frequency=2.412 GHz  Access Point: Not-Associated
          Sensitivity:0/0
          Retry:off   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality:0  Signal level:0  Noise level:0
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0
```
