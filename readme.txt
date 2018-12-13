Support Linux kernel version
2.6.18-4.16

CPU support
x86

Ubuntu 14.04.4 LTS

Install and active module:
1. Unzip and change directory
>tar zxvf rtl8821CU_WiFi_linux_v5.4.1_28754.20180921_COEX20180712-3232.tar.gz
>cd rtl8821CU_WiFi_linux_v5.4.1_28754.20180921_COEX20180712-3232
2. Compile module
>make 
3. Install module
>sudo make install
4. Unplug adapter and re-plugin adapter to active module
Please note that DWA-171C support auto install driver function in Windwos OS,
user should exit Flash disk mode(default) to active adapter mode.

Uninstall module:
1. Change directory
>cd rtl8821CU_WiFi_linux_v5.4.1_28754.20180921_COEX20180712-3232
2. Uninstall module
>sudo make uninstall
3. Clean object code
>make clean