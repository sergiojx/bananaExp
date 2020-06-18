# bananaExp
banana Pi 

## How To Setup Wifi on Armbian Ubuntu Orange Banana Raspberry Pi Quickest Out of The Box Guide
https://www.youtube.com/watch?v=TicU7hMdi64
After following this video instructions, reboot board and WiFi client works fine

http://wiki.banana-pi.org/Getting_Started_with_M2P#WiFi_on_M2P

https://unix.stackexchange.com/questions/283722/how-to-connect-to-wifi-from-command-line

## armbian
https://www.armbian.com/

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
If eth0 is down, wlan0 gets up. Nest command will show wlan0 connection preogres/status
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

## Download, install, and set up the Linux workload 
### Download, install, and set up the Linux workload
https://docs.microsoft.com/en-us/cpp/linux/download-install-and-setup-the-linux-development-workload?view=vs-2019  
```
~ $ sudo apt-get install openssh-server g++ gdb make ninja-build rsync zip
```

### Create a new Linux project
https://docs.microsoft.com/en-us/cpp/linux/create-a-new-linux-project?view=vs-2019  


## eMMC copy-clone

https://forum.armbian.com/topic/11404-how-to-clone-emmc-nanopi-neo-core/  


