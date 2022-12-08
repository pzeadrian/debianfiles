# DEBIAN
- I'm an Archer since ever, but I'm a little tired of cope with the usual troubles that a rolling release distro usually brings. So in order of search something more "stable" I decided to move my configs to Debian...

- Now, let's see how to install and configure it to leave as my Arch dotfiles, or at least as similar as possible:

## 1. Download your ISO:
   1.1. You can download the standard netinstall ISO from [Debian](https://www.debian.org/download), but you must take into account that if you want to configure it only using wifi, you will probably need a ISO with included non-free firmware, so you could take it from [here](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/)
   
## 2. Install your ISO
  2.1. No matter which iso you have chosen, start the installation normally, either graphical or non-graphical, but when you arrive to the Software selection section, be sure that the only marked option is "Standard System Utilities" (unless you need SSH or web server).
  2.2. When iinstallation had finished, reboot, and be ready...
  
--> If you chose Standard ISO install or you have an ethernet connection, just skip step 3.
## 3. Debian does not include NetworkManager by default like Arch. Instead, it uses wpa_supplicant, which involves a somewhat more complex process.
  3.1. First, login as root
  Now enable and start wpa_supplicant:
  ```bash
  systemctl enable wpa_supplicant
  systemctl start wpa_supplicant
  ```
  Then, run
  ```bash
  ip a
  ```
  And copy your wifi device name. It isn't "lo".
  Run
  ```bash
  ip link set dev %Your device name% up
  ```
  And
  ```bash
  wpa_passphrase your-ESSID your-wifi-passphrase | tee -a /etc/wpa_supplicant/wpa_supplicant.conf
  ```
  After that
  ```bash
  wpa_supplicant -B -c /etc/wpa_supplicant.conf -i %Your device name%
  ```
  Then
  ```bash
  dhclient %Your device name%
  ```
  Now your wireless interface has a private IP address, which can be shown with
  ```bash
  ip addr show %Your device name%
  ```
  Nice.. You are connected to internet via Wifi.
