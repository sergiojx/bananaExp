# bananaExp
banana Pi 

## How To Setup Wifi on Armbian Ubuntu Orange Banana Raspberry Pi Quickest Out of The Box Guide
https://www.youtube.com/watch?v=TicU7hMdi64
After following this video instructions, reboot board and WiFi client works fine

http://wiki.banana-pi.org/Getting_Started_with_M2P#WiFi_on_M2P

https://unix.stackexchange.com/questions/283722/how-to-connect-to-wifi-from-command-line

## armbian
https://www.armbian.com/
### Banana Pi M2 Ultra
https://www.armbian.com/bananapi-m2u/  
### Banana Pi M2+
https://www.armbian.com/banana-pi-m2-plus/  

## FTDI setup on MAC
https://learn.sparkfun.com/ftdiDriversMac
https://www.ftdichip.com/Drivers/VCP.htm
https://www.ftdichip.com/Support/Documents/InstallGuides/Mac_OS_X_Installation_Guide.pdf
```
~ $ screen tty.usbserial-A505AN1O 115200
```
## Network Manager
https://wiki.archlinux.org/index.php/NetworkManager  

### eth0 static setup  
```
root@bananapim2ultra:/# nmcli con add type ethernet con-name fradio ifname eth0 ip4 110.168.0.14/24 gw4 110.168.0.1  
Connection 'fradio' (b0fabb6f-06a3-4d9a-be57-4757dca14cfe) successfully added.  
root@bananapim2ultra:/# nmcli con up fradio
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/29)

```
#### Ups wlan0 is no longer the default interface
If eth0 is down, wlan0 gets up. Next command will show wlan0 connection preogres/status
```
root@bananapim2ultra:/# nmcli d
DEVICE  TYPE      STATE         CONNECTION 
wlan0   wifi      connected     UP 1       
eth0    ethernet  disconnected  --         
lo      loopback  unmanaged     --   
```
This command shows default gateway
```
ip route show
```
One way to solve this isse
```
ip route delete default
ip route add default via 192.168.1.1
```
OR change wlan0 priorety
```
root@bananapim2ultra:/# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    600    0        0 wlan0
192.168.0.0     0.0.0.0         255.255.255.0   U     600    0        0 wlan0
```
after enabling eth0
```
root@bananapim2ultra:/# nmcli con up fradio
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/10)
root@bananapim2ultra:/# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         110.168.0.1     0.0.0.0         UG    100    0        0 eth0
0.0.0.0         192.168.0.1     0.0.0.0         UG    600    0        0 wlan0
110.168.0.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
192.168.0.0     0.0.0.0         255.255.255.0   U     600    0        0 wlan0

root@bananapim2ultra:/# nmcli d
DEVICE  TYPE      STATE      CONNECTION 
eth0    ethernet  connected  fradio     
wlan0   wifi      connected  UP 1

```
With this commnad. And reboot
```
root@bananapim2ultra:~# nmcli con mod UP ipv4.route-metric 98
```
This command wont work is actual connection name is "UP 1" or "UP 2", etc.  Delete connections if required  
```
nmcli connection delete id <connection name>
```  
Go to /etc/NetworkManager/system-connections and check no other similar name connection exists.  
New output will be:
```
root@bananapim2ultra:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    98     0        0 wlan0
0.0.0.0         110.168.0.1     0.0.0.0         UG    100    0        0 eth0
110.168.0.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
192.168.0.0     0.0.0.0         255.255.255.0   U     98     0        0 wlan0
```
## Download, install, and set up the Linux workload 
### Download, install, and set up the Linux workload
https://docs.microsoft.com/en-us/cpp/linux/download-install-and-setup-the-linux-development-workload?view=vs-2019  
```
~ $ sudo apt-get install openssh-server g++ gdb make ninja-build rsync zip
```
```
root@bananapim2ultra:~# g++ --version
g++ (Debian 8.3.0-6) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
```
root@bananapim2ultra:~# gdb --version
GNU gdb (Debian 8.2.1-2+b3) 8.2.1
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
### Create a new Linux project
https://docs.microsoft.com/en-us/cpp/linux/create-a-new-linux-project?view=vs-2019  

## Cmake
```
root@bananapim2ultra:~# apt-get install cmake
root@bananapim2ultra:~# cmake --version
cmake version 3.13.4
```
## eMMC copy-clone

https://forum.armbian.com/topic/11404-how-to-clone-emmc-nanopi-neo-core/  


## OpenCV
### How to Install OpenCV on Linux
https://linuxize.com/post/how-to-install-opencv-on-debian-10/  
https://docs.opencv.org/4.4.0/d7/d9f/tutorial_linux_install.html
### Live video streaming over network with OpenCV and ImageZMQ
https://www.pyimagesearch.com/2019/04/15/live-video-streaming-over-network-with-opencv-and-imagezmq/
### imagezmq
https://github.com/jeffbass/imagezmq
### ZeroMQ
https://zeromq.org/get-started/
### YOLOv3
https://www.learnopencv.com/deep-learning-based-object-detection-using-yolov3-with-opencv-python-c/ 
https://github.com/spmallick/learnopencv/tree/master/ObjectDetection-YOLO 
### Setting Eclipse OpenCV project
https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_eclipse/linux_eclipse.html 




# Drogon C++ web framewok
https://github.com/an-tao/drogon  
https://drogon.docsforge.com/  
## Installing Drogon dependencies
### Ubuntu 18.04 
#### Environment
```
sudo apt install git
sudo apt install gcc
sudo apt install g++
sudo apt install cmake
```
#### jsoncpp
```
sudo apt install libjsoncpp-dev
```
#### uuid
```
sudo apt install uuid-dev
```
#### OpenSSL
```
sudo apt install openssl
sudo apt install libssl-dev
```
#### zlib
```
sudo apt install zlib1g-dev
```

# IXWebSocket  
https://github.com/machinezone/IXWebSocket  
https://machinezone.github.io/IXWebSocket/  
### Installed set  
https://github.com/machinezone/IXWebSocket/issues/37
### Compilling IXWebSocket basic example
```
g++ --std=c++14  testIxws.cpp -lixwebsocket -lz -lssl -lcrypto -lpthread

g++ --std=c++14  testIxws.cpp -lixwebsocket -lz -lssl -lcrypto -lpthread -ljsoncpp
```

# Device could not set format, VIDIOC_S_FMT: Device or resource busy. This was not the probles. Adding Virtual Box USB extension fixed!
Xwindows is using the camera
```
~$/dev$ fuser /dev/input/by-id/usb-Microsoft_Microsoft®_LifeCam_HD-3000-event-if00
dev/input/event7:    1277
~$/dev$ ps axl | grep 1277
~$kill -9 1277

```
