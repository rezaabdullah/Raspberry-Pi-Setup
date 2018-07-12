# Raspberry Pi Headless Setup Instructions
**Following methods are verified in Windows 10**<br/>
*Complexities of the methods are in ascending order.*

#### **Method 1**
**Requirements:**
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++
* [Bonjour Application](https://support.apple.com/downloads/bonjour)<br/>


**Steps**<br/>
1. [Download Raspberry Pi OS Image](https://www.raspberrypi.org/downloads/raspbian/)
2. Extract the image file
3. Burn image file to SD card with [Etcher](https://etcher.io/) or [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
4. Once done, navigate to ***boot*** folder of the SD card
5. Create an empty file with Notepad or similar text editor and name it ***ssh* (NO file extension i.e. file type)** *ssh file can also be downloaded from here.*
6. Mount SD Card on Pi
7. Connect ethernet cable to pi and laptop
8. Turn on the pi
9. Open [putty](https://www.putty.org/)<br/>
   a. Connection Type: SSH<br/>
   b. Host Name: raspberrypi.local
10. Login as (User Name): `pi`
11. Password: `raspberry`
12. At Terminal:<br/>
   a. Type `sudo raspi-config`<br/>
   b. Choose `5 Interfacing Options`<br/>
   c. Choose `P3 VNC`<br/>
   d. Use arrow key to enable it
13. [Install VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)
14. At Address Bar enter: `raspberrypi.local`. Use info from *10 & 11* when prompted to enter username and password.

#### **Method 2**
**Requirements:**
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++<br/>


**Steps**<br/>
1. Follow *1-5* from Method 1
6. Find and open ***cmdline.txt*** with text editor. The file can be found inside ***boot*** folder
7. Edit the file. Add `ip=192.168.1.200::192.168.1.1:255.255.255.0:rpi:eth0:off` at the end of the existing content of the file.<br/>
   *The file can also be found here. Replace the original file with the file from this repository.*
8. Disconnect computer from the internet
9. Connect ethernet cable to Pi and computer
10. Turn on the Pi
11. Open ***Network & Internet Settings*** from Control Panel
12. Click on the network name next to ***Connections:***
13. Under ***Activity* select *Properties***<br/>
14. Select ***Internet Protocol Version 4 (TCP/IPv4)***
15. Select ***Properties***<br/>
16. Select ***Use the following IP address***<br/>
    a. IP address: `192.168.1.201`<br/>
    b. Subnet mask: `255.255.255.0`<br/>
    c. Default gateway: `192.168.1.1`<br/>
17. Connect Pi to the router via ethernet cable
18. Turn on the pi
19. Obtain Pi's IP address from the router
20. Follow *9-14* from Method 1

#### **Method 3**
**Requirements:**
* Internet connection
* Router
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++
* [Bonjour Application](https://support.apple.com/downloads/bonjour)<br/>

**Steps**<br/>
1. Follow *1-6* from Method 1
7. Connect Pi to the router via ethernet cable
8. Turn on the pi
9. Obtain Pi's IP address from the router
10. Follow *9-14* from Method 1

## Enable VNC and SSH
#### **Method 1 (Preferred)**
1. Click Raspberry Pi icon
2. Go to **Preferences**
3. Select **Raspberry Pi Configuration**
4. Got to **Interfaces** Tab
5. Enable **SSH** and **VNC**

#### **Method 2**
1. At terminal enter `sudo raspi-config`
2. Select **5 Interfacing Options**
3. Select **P2** (SSH) and **P3** (VNC)

## Set up Teamviewer
1. **Download *Teamviewer Host* from Teamviewer Website**
2. Click the downloaded file and follow the instructions from the prompt window

# Set up Serial Port (***Swap serial ports***)
Raspberry Pi 3 has two serial ports, named `ttyS0` and `ttyAMA0`. By default, `ttyAMA0` is mapped for Bluetooth while `ttyS0` is reserved for hardware GPIO. Since `ttyAMA0` has higher performance and more reliable compared to `ttyS0`, it is necessary to swap the ports. The steps to swap the ports are listed below:
1. Edit **config.txt:** At terminal enter `sudo nano /boot/config.txt`
2. At the last line add `enable_uart=1` and save the file
3. Stop **Getty(console)** Service: At terminal enter `sudo systemctl stop serial-getty@ttyS0.service`
4. Disable **Getty(console)** Service: At terminal enter `sudo systemctl disbale serial-getty@ttyS0.service`
5. Edit **cmdline.txt:** At terminal enter `sudo nano /boot/cmdline.txt`
6. Remove `console=serial0,115200` and save the file
7. Reboot RPi to take effect
8. Reedit **config.txt:** At terminal enter `sudo nano /boot/config.txt`
9. To point Bluetooth to `ttyS0` add `dtoverlay=pi3-miniuart-bt` or to disable Bluetooth add `pi3-disable-bt` at the end of the file
10. Reboot RPi
11. At terminal enter `ls -l /dev` : `serial0` should point to `ttyAMA0` while `serial1` should point to `ttyS0`

# Set up NodeJS
1. Go to NodeJS Download page
2. Select **ARMv7** (RPi-3) or **ARMv6** (RPi-Zero)
3. Extract the downloaded file
4. At terminal change directory to go inside the extracted folder `cd <NodeJS_Folder_Path>` where *<NodeJS_Folder_Path>* is the path of the extracted folder
5. Enter `sudo cp -R * /usr/local/`
6. Enter `node -v` and `npm -v` to check the version of the NodeJS and NPM
7. Create new directory `NodeApp`
8. Download NPM Modules:<br/>
   i. `serialport`<br/>
   ii. `modbus-serial`<br/>
   iii. `firebase`<br/>