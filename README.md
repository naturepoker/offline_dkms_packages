# .deb packages for offline dkms installation on Ubuntu 22.04 LTS

Selection of packages for offline installation of dkms (along with build-essential, make, gcc and etc) on Ubuntu LTS 22.04

For whatever the reason official Ubuntu LTS iso can ship without make/build-essential packages, rendering their installation with unsupported hardware more complicated than they need to be. This repository is a selection of dkms and dependency packages to allow quick, easy setup of compilation-to-kernel module tool set on an offline machine.  

I tested this out on an old HP Z280 workstation with Ubuntu LTS 22.04 running off a generic RTL88x2bu USB wifi card that required a separate driver installation while off-line. 

If you're running an offline **_Ubuntu 22.04 LTS_** machine, simply running below in /dkms_packages directory should setup necessary dependencies that will allow compilation and driver installation as a kernel module. If you're running some different version of 'buntu distro, instruction further down this file should work as well. 

```bash
git clone https://github.com/naturepoker/offline_dkms_packages
cd offline_dkms_packages
sudo dpkg -i *.deb
```

If there's an error on file permissions, you'll have to change them manually - 

```bash
sudo chown -R ubuntu:ubuntu *
```

For generating the packages I used below method with a live-USB environment on a separate machine with wifi connection. I highly recommend that you don't use a production machine for this:

Make sure libdpkg-perl_1.21.1ubuntu2.1_all.deb package is part of downloaded files! 
Ubuntu live USBs can sometimes download them or skip them depending on the machine; I haven't been able to figure out what causes the difference. 
If some necessary dependency isn't being downloaded properly, see if it's already installed on the live system, delete it and then re-download it using below commands again (which is one of the reasons why you really want to use a disposable live-USB environment for these steps). 
 
 ```bash
 sudo sed -i "s|^deb cdrom|# deb cdrom|g" /etc/apt/sources.list
 sudo apt-add-repository universe
 mkdir offline_dkms_packages
 cd offline_dkms_packages
 sudo apt --yes --download-only -o Dir::Cache::archives="./" install dkms gcc make build-essential libdpkg-perl
 ```
 
 A good way to save time is to run sudo dpkg -i *.deb on above downloaded files within the live-USB environment to see if everything installs without issues. If you already have a driver, it's a good idea to compile-install-dkms it within the live environment as well. 
 
 For reference, I used above method to install a dkms driver package available from here:
 https://github.com/morrownr/88x2bu-20210702
