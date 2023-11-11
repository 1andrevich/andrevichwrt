# Personal OpenWrt OPKG Server (fork of Burhan7610/Repo-Paket-Openwrt)
Install and upgrade OpenWrt modified community application packages (such as: OpenClash, Passwall, ShadowSocksR+ Plus, Wegare STL, Tiny File Manager, Xderm Mini, v2rayA, Modeminfo, etc.) easily.

The advantages of installing and updating using a custom server like this are:
1. No need to bother using wget and curl which are very long and complicated.
2. Installing the ipk package can use `opkg install package-name`.
3. Installing the IPK package can also use the **System - Software** feature in LuCI OpenWrt.

List of contents:
- [Architecture List](#architecture-list)
- [How to Add Repository to Software Update OpenWrt](#how-to-add-repository-to-software-update-openwrt)
- [How to Install and Update Packages](#how-to-install-and-update-packages)
- [How to check whether a package is installed or not](#how-to-check-whether-a-package-has-been-installed-or-not)

## Architectural List
This repository supports the following architecture:

```
aarch64_cortex-a53
aarch64_cortex-a72
aarch64_generic
arm_arm1176jzf-s_vfp
arm_cortex-a7_neon-vfpv4
i386_pentium4
mips_24kc
mipsel_24kc
x86_64
```

## How to Add a Repository to the OpenWrt Update Software
There are 2 ways to add this repository to the firmware, namely:
- [Using LuCI](#using-luci)
- [Using Terminal](#using-terminal) such as JuiceSSH/Termius/Termux


### Using LuCI

   1. Enter LuCI IP (example: 192.168.1.1), Login, Open **System -> Software -> Configuration**
  
   2. Add a # sign (fence) in front of the ```option check_signature``` line, example below
  
       change the text below
      
       ```
       option check_signature
       ```
      
       become like this
      
       ```
       # option check_signature
       ```

   3. In the custom feeds section, add the list below

       ```
       src/gz custom_generic https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/generic
       src/gz custom_arch https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/aarch64_cortex-a53
       ```
         change **aarch64_cortex-a53** and adjust the CPU architecture of your OpenWrt router

Alternatively:


      sed -i 's/option check_signature/# option check_signature/g' /etc/opkg.conf
      echo "src/gz custom_generic https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/generic" >> /etc/opkg/customfeeds.conf
      echo "src/gz custom_arch https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/$(grep "OPENWRT_ARCH" /etc/os-release | awk -F '"' '{print $2}')" >> /etc/opkg/customfeeds.conf 


   ![](https://raw.githubusercontent.com/Burhan7610/Repo-Paket-Openwrt/main/preview/preview1.gif)
 
### Using Terminal
   1. Use one of the recommended Terminal applications below
       - TTYD Terminal (OpenWrt Package)
       - JuiceSSH
       - Termius
       - Termux
      
       > Note: Users can use terminal applications other than those mentioned above
  
   2. Copy and paste below in the terminal, it will automatically adjust the router's CPU architecture type
      
       ```
       sed -i 's/option check_signature/# option check_signature/g' /etc/opkg.conf
       echo "src/gz custom_generic https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/generic" >> /etc/opkg/customfeeds.conf
       echo "src/gz custom_arch https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/$(grep "OPENWRT_ARCH" /etc/os-release | awk -F '"' '{print $2} ')" >> /etc/opkg/customfeeds.conf
       ```

       > Note: for OpenWrt 19.07 firmware there are still things that have to be installed manually, such as `kcptun-client`, `xray-core` and `libcap-bin`.
    
       ![](https://raw.githubusercontent.com/1andrevich/andrevichwrt/main/preview/preview2.gif)
    

## How to Install and Update Packages
There are 2 ways to install this repository, namely
- [Using LuCI](#install-and-update-packages-using-luci)
- [Using Terminal](#install-and-update-packages-using-terminal) such as JuiceSSH/Termius/Termux

### Install and Update Packages Using LuCI
   1. Enter LuCI IP (example: 192.168.1.1), Login, Open **System -> Software -> Configuration**
   2. Press the **Update Lists** button.
   3. Look for the package name (such as: `luci-app-passwall`) in the **Filter** column.
   4. Press the **Find Package** button.
   5. Scroll a little, see below the **Installed packages** and **Available packages** tabs:
       - Installed packages: the packages in the list are already installed.
       - Available packages: the packages in the list are not yet installed.
   6. Click **Available packages**, then look for the package name written in the filter earlier.
   7. Click **Install** in the line containing the package name, then wait until the package installation is complete.
 
### Install and Update Packages Using Terminal
   1. Open the preferred terminal application
   2. Run the command below to update the list of packages available on the server
       ```
       opkg update
       ```
  
   3. Run the command `opkg install package-name`, changing `package-name` to the existing package name (this example will use the `luci-app-passwall` package).
      
       ```
       opkg install luci-app-passwall
       ```

## How to check whether a package has been installed or not
There are 2 ways to install this repository, namely
- [Using LuCI](#how-to-check-package-status-with-luci)
- [Using Terminal](#how-to-check-package-status)
