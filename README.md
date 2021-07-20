# bananaExp
banana Pi 

## How To Setup Wifi on Armbian Ubuntu Orange Banana Raspberry Pi Quickest Out of The Box Guide
https://www.youtube.com/watch?v=TicU7hMdi64
After following this video instructions, reboot board and WiFi client works fine

http://wiki.banana-pi.org/Getting_Started_with_M2P#WiFi_on_M2P

https://unix.stackexchange.com/questions/283722/how-to-connect-to-wifi-from-command-line

## armbian
https://www.armbian.com/
#### Banana Pi M2 Ultra
https://www.armbian.com/bananapi-m2u/  
#### Banana Pi M2+
https://www.armbian.com/banana-pi-m2-plus/  
#### Armbian getting started
https://docs.armbian.com/User-Guide_Getting-Started/ 

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
#### Como conectarse a una red WPA en linea de ordenes con nmcli
https://www.wifi-libre.com/topic-604-como-conectarse-a-una-red-wpa-en-linea-de-ordenes-con-nmcli.html
```
nmcli dev status
nmcli dev wifi list
nmcli con add con-name wifistudio ifname wlx0013efa800e4 type wifi ssid TRENDnet410_2.4GHz_2Z43
nmcli con modify wifistudio wifi-sec.key-mgmt wpa-psk
nmcli con modify wifistudio wifi-sec.psk wFi@Lbr3?@!¡_kl
nmcli connection up wifistudio
nmcli -p c
nmcli -p con show wifistudio
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

### How to manage DNS in NetworkManager via console (nmcli)?
https://serverfault.com/questions/810636/how-to-manage-dns-in-networkmanager-via-console-nmcli
```
~ $ nmcli con mod <connectionName> ipv4.dns "8.8.8.8 8.8.4.4"
~ $ nmcli con down <connectionName>
~ $ nmcli con up <connectionName>
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
https://forum.armbian.com/topic/1331-armbian-sd-card-backup/ 
```
# https://forum.armbian.com/topic/1502-emmc-backup/
if=/dev/mmcblk1 | gzip > /mnt/mnt1/image.gz # where mnt1 is the mounted uSD
```

or 
```
# https://forum.armbian.com/topic/1331-armbian-sd-card-backup/
dd if=/dev/mmcblk0 of=/dev/sdb
```


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
# HTML5 Live Streaming with MPEG-DASH 
https://isrv.pw/html5-live-streaming-with-mpeg-dash 

# Audio
## arecord
Recording audio through arecord (It will record an audio of 5 secs of U8 formet and mp3 type named sample.mp3)
```
sudo arecord -d 15 -f U8 sample.mp3
parecord --device=alsa_input.usb-Microsoft_Microsoft___LifeCam_HD-3000-02.analog-mono --record sound2.wav

```
### How to test microphone with Audio Linux Sound Architecture
https://linuxconfig.org/how-to-test-microphone-with-audio-linux-sound-architecture-alsa
```
arecord -f S16_LE -d 10 -r 16000 --device="hw:1,0" /tmp/test-mic.wav
aplay /tmp/test-mic.wav

```
### How to play pcm audio in a browser
https://www.programmersought.com/article/3010498150/ 


### CAF Intallation log

```
Install the project...
-- Install configuration: ""
-- Up-to-date: /usr/local/include/caf
-- Up-to-date: /usr/local/include/caf/test
-- Up-to-date: /usr/local/include/caf/test/unit_test.hpp
-- Up-to-date: /usr/local/include/caf/test/io_dsl.hpp
-- Up-to-date: /usr/local/include/caf/test/bdd_dsl.hpp
-- Up-to-date: /usr/local/include/caf/test/dsl.hpp
-- Up-to-date: /usr/local/include/caf/test/unit_test_impl.hpp
-- Installing: /usr/local/lib/cmake/CAF/CAFTargets.cmake
-- Installing: /usr/local/lib/cmake/CAF/CAFTargets-noconfig.cmake
-- Installing: /usr/local/lib/cmake/CAF/CAFConfig.cmake
-- Installing: /usr/local/lib/cmake/CAF/CAFConfigVersion.cmake
-- Installing: /usr/local/include/caf/detail/build_config.hpp
-- Installing: /usr/local/lib/libcaf_core.so.0.18.2
-- Up-to-date: /usr/local/lib/libcaf_core.so
-- Up-to-date: /usr/local/include/caf
-- Up-to-date: /usr/local/include/caf/actor_system_config.hpp
-- Up-to-date: /usr/local/include/caf/attach_stream_stage.hpp
-- Up-to-date: /usr/local/include/caf/byte_span.hpp
-- Up-to-date: /usr/local/include/caf/dictionary.hpp
-- Up-to-date: /usr/local/include/caf/intrusive_ptr.hpp
-- Up-to-date: /usr/local/include/caf/is_timeout_or_catch_all.hpp
-- Up-to-date: /usr/local/include/caf/span.hpp
-- Up-to-date: /usr/local/include/caf/parser_state.hpp
-- Up-to-date: /usr/local/include/caf/catch_all.hpp
-- Up-to-date: /usr/local/include/caf/attach_stream_source.hpp
-- Up-to-date: /usr/local/include/caf/sec.hpp
-- Up-to-date: /usr/local/include/caf/deserializer.hpp
-- Up-to-date: /usr/local/include/caf/callback.hpp
-- Up-to-date: /usr/local/include/caf/scheduler.hpp
-- Up-to-date: /usr/local/include/caf/default_enum_inspect.hpp
-- Up-to-date: /usr/local/include/caf/variant.hpp
-- Up-to-date: /usr/local/include/caf/cow_tuple.hpp
-- Up-to-date: /usr/local/include/caf/config_value_writer.hpp
-- Up-to-date: /usr/local/include/caf/local_actor.hpp
-- Up-to-date: /usr/local/include/caf/scheduled_actor.hpp
-- Up-to-date: /usr/local/include/caf/actor_system.hpp
-- Up-to-date: /usr/local/include/caf/stream_sink_trait.hpp
-- Up-to-date: /usr/local/include/caf/load_inspector.hpp
-- Up-to-date: /usr/local/include/caf/node_id.hpp
-- Up-to-date: /usr/local/include/caf/unifyn.hpp
-- Up-to-date: /usr/local/include/caf/detail
-- Up-to-date: /usr/local/include/caf/detail/mask_bits.hpp
-- Up-to-date: /usr/local/include/caf/detail/variant_data.hpp
-- Up-to-date: /usr/local/include/caf/detail/enqueue_result.hpp
-- Up-to-date: /usr/local/include/caf/detail/select_all.hpp
-- Up-to-date: /usr/local/include/caf/detail/squashed_int.hpp
-- Up-to-date: /usr/local/include/caf/detail/padded_size.hpp
-- Up-to-date: /usr/local/include/caf/detail/behavior_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/json.hpp
-- Up-to-date: /usr/local/include/caf/detail/ieee_754.hpp
-- Up-to-date: /usr/local/include/caf/detail/move_if_not_ptr.hpp
-- Up-to-date: /usr/local/include/caf/detail/safe_equal.hpp
-- Up-to-date: /usr/local/include/caf/detail/get_root_uuid.hpp
-- Up-to-date: /usr/local/include/caf/detail/blocking_behavior.hpp
-- Up-to-date: /usr/local/include/caf/detail/local_group_module.hpp
-- Up-to-date: /usr/local/include/caf/detail/make_meta_object.hpp
-- Up-to-date: /usr/local/include/caf/detail/make_unique.hpp
-- Up-to-date: /usr/local/include/caf/detail/abstract_worker_hub.hpp
-- Up-to-date: /usr/local/include/caf/detail/size_based_credit_controller.hpp
-- Up-to-date: /usr/local/include/caf/detail/cas_weak.hpp
-- Up-to-date: /usr/local/include/caf/detail/default_invoke_result_visitor.hpp
-- Up-to-date: /usr/local/include/caf/detail/raw_access.hpp
-- Up-to-date: /usr/local/include/caf/detail/monotonic_buffer_resource.hpp
-- Up-to-date: /usr/local/include/caf/detail/type_list.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_stage_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/config_consumer.hpp
-- Up-to-date: /usr/local/include/caf/detail/init_fun_factory.hpp
-- Up-to-date: /usr/local/include/caf/detail/assign_inspector_try_result.hpp
-- Up-to-date: /usr/local/include/caf/detail/get_process_id.hpp
-- Up-to-date: /usr/local/include/caf/detail/pp.hpp
-- Up-to-date: /usr/local/include/caf/detail/mtl_util.hpp
-- Up-to-date: /usr/local/include/caf/detail/optional_message_visitor.hpp
-- Up-to-date: /usr/local/include/caf/detail/pretty_type_name.hpp
-- Up-to-date: /usr/local/include/caf/detail/print.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser
-- Up-to-date: /usr/local/include/caf/detail/parser/read_number.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_ipv6_address.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_uri.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_floating_point.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/add_ascii.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/fsm_undef.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_unsigned_integer.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_config.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/ascii_to_int.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_string.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_ipv4_address.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/sub_ascii.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/chars.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/is_char.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/is_digit.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_number_or_timespan.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_signed_integer.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_bool.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/read_timespan.hpp
-- Up-to-date: /usr/local/include/caf/detail/parser/fsm.hpp
-- Up-to-date: /usr/local/include/caf/detail/tail_argument_token.hpp
-- Up-to-date: /usr/local/include/caf/detail/sync_request_bouncer.hpp
-- Up-to-date: /usr/local/include/caf/detail/offset_at.hpp
-- Up-to-date: /usr/local/include/caf/detail/type_traits.hpp
-- Up-to-date: /usr/local/include/caf/detail/log_level.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_sink_driver_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/typed_actor_util.hpp
-- Up-to-date: /usr/local/include/caf/detail/try_serialize.hpp
-- Up-to-date: /usr/local/include/caf/detail/profiled_send.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_distribution_tree.hpp
-- Up-to-date: /usr/local/include/caf/detail/type_id_list_builder.hpp
-- Up-to-date: /usr/local/include/caf/detail/serialized_size.hpp
-- Up-to-date: /usr/local/include/caf/detail/private_thread.hpp
-- Up-to-date: /usr/local/include/caf/detail/parse.hpp
-- Up-to-date: /usr/local/include/caf/detail/token_based_credit_controller.hpp
-- Up-to-date: /usr/local/include/caf/detail/pseudo_tuple.hpp
-- Up-to-date: /usr/local/include/caf/detail/tick_emitter.hpp
-- Up-to-date: /usr/local/include/caf/detail/spawn_fwd.hpp
-- Up-to-date: /usr/local/include/caf/detail/as_mutable_ref.hpp
-- Up-to-date: /usr/local/include/caf/detail/behavior_stack.hpp
-- Up-to-date: /usr/local/include/caf/detail/select_integer_type.hpp
-- Up-to-date: /usr/local/include/caf/detail/ringbuffer.hpp
-- Up-to-date: /usr/local/include/caf/detail/network_order.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_source_driver_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/append_percent_encoded.hpp
-- Up-to-date: /usr/local/include/caf/detail/spawnable.hpp
-- Up-to-date: /usr/local/include/caf/detail/meta_object.hpp
-- Up-to-date: /usr/local/include/caf/detail/arg_wrapper.hpp
-- Up-to-date: /usr/local/include/caf/detail/unordered_flat_map.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_stage_driver_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/int_list.hpp
-- Up-to-date: /usr/local/include/caf/detail/glob_match.hpp
-- Up-to-date: /usr/local/include/caf/detail/consumer.hpp
-- Up-to-date: /usr/local/include/caf/detail/path_state.hpp
-- Up-to-date: /usr/local/include/caf/detail/unique_function.hpp
-- Up-to-date: /usr/local/include/caf/detail/worker_hub.hpp
-- Up-to-date: /usr/local/include/caf/detail/append_hex.hpp
-- Up-to-date: /usr/local/include/caf/detail/shared_spinlock.hpp
-- Up-to-date: /usr/local/include/caf/detail/ripemd_160.hpp
-- Up-to-date: /usr/local/include/caf/detail/thread_safe_actor_clock.hpp
-- Up-to-date: /usr/local/include/caf/detail/apply_args.hpp
-- Up-to-date: /usr/local/include/caf/detail/scope_guard.hpp
-- Up-to-date: /usr/local/include/caf/detail/set_thread_name.hpp
-- Up-to-date: /usr/local/include/caf/detail/split_join.hpp
-- Up-to-date: /usr/local/include/caf/detail/type_pair.hpp
-- Up-to-date: /usr/local/include/caf/detail/double_ended_queue.hpp
-- Up-to-date: /usr/local/include/caf/detail/limited_vector.hpp
-- Up-to-date: /usr/local/include/caf/detail/delegate_serialize.hpp
-- Up-to-date: /usr/local/include/caf/detail/implicit_conversions.hpp
-- Up-to-date: /usr/local/include/caf/detail/gcd.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_source_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/comparable.hpp
-- Up-to-date: /usr/local/include/caf/detail/simple_actor_clock.hpp
-- Up-to-date: /usr/local/include/caf/detail/is_complete.hpp
-- Up-to-date: /usr/local/include/caf/detail/is_one_of.hpp
-- Up-to-date: /usr/local/include/caf/detail/algorithms.hpp
-- Up-to-date: /usr/local/include/caf/detail/message_data.hpp
-- Up-to-date: /usr/local/include/caf/detail/message_builder_element.hpp
-- Up-to-date: /usr/local/include/caf/detail/get_mac_addresses.hpp
-- Up-to-date: /usr/local/include/caf/detail/tbind.hpp
-- Up-to-date: /usr/local/include/caf/detail/private_thread_pool.hpp
-- Up-to-date: /usr/local/include/caf/detail/stream_sink_impl.hpp
-- Up-to-date: /usr/local/include/caf/detail/abstract_worker.hpp
-- Up-to-date: /usr/local/include/caf/detail/bounds_checker.hpp
-- Up-to-date: /usr/local/include/caf/detail/test_actor_clock.hpp
-- Up-to-date: /usr/local/include/caf/detail/invoke_result_visitor.hpp
-- Up-to-date: /usr/local/include/caf/detail/overload.hpp
-- Up-to-date: /usr/local/include/caf/detail/encode_base64.hpp
-- Up-to-date: /usr/local/include/caf/detail/group_tunnel.hpp
-- Up-to-date: /usr/local/include/caf/detail/base64.hpp
-- Up-to-date: /usr/local/include/caf/detail/functor_attachable.hpp
-- Up-to-date: /usr/local/include/caf/detail/stringification_inspector.hpp
-- Up-to-date: /usr/local/include/caf/ip_subnet.hpp
-- Up-to-date: /usr/local/include/caf/term.hpp
-- Up-to-date: /usr/local/include/caf/actor_storage.hpp
-- Up-to-date: /usr/local/include/caf/abstract_actor.hpp
-- Up-to-date: /usr/local/include/caf/spawn_options.hpp
-- Up-to-date: /usr/local/include/caf/others.hpp
-- Up-to-date: /usr/local/include/caf/inspector_access.hpp
-- Up-to-date: /usr/local/include/caf/broadcast_downstream_manager.hpp
-- Up-to-date: /usr/local/include/caf/response_promise.hpp
-- Up-to-date: /usr/local/include/caf/inbound_path.hpp
-- Up-to-date: /usr/local/include/caf/stream_manager.hpp
-- Up-to-date: /usr/local/include/caf/outbound_path.hpp
-- Up-to-date: /usr/local/include/caf/after.hpp
-- Up-to-date: /usr/local/include/caf/blocking_actor.hpp
-- Up-to-date: /usr/local/include/caf/downstream_msg.hpp
-- Up-to-date: /usr/local/include/caf/intrusive_cow_ptr.hpp
-- Up-to-date: /usr/local/include/caf/abstract_channel.hpp
-- Up-to-date: /usr/local/include/caf/message_builder.hpp
-- Up-to-date: /usr/local/include/caf/stream_stage_driver.hpp
-- Up-to-date: /usr/local/include/caf/config_value_reader.hpp
-- Up-to-date: /usr/local/include/caf/json_writer.hpp
-- Up-to-date: /usr/local/include/caf/binary_deserializer.hpp
-- Up-to-date: /usr/local/include/caf/default_downstream_manager.hpp
-- Up-to-date: /usr/local/include/caf/stream_finalize_trait.hpp
-- Up-to-date: /usr/local/include/caf/tag
-- Up-to-date: /usr/local/include/caf/tag/boxing_type.hpp
-- Up-to-date: /usr/local/include/caf/actor_control_block.hpp
-- Up-to-date: /usr/local/include/caf/make_message.hpp
-- Up-to-date: /usr/local/include/caf/attach_continuous_stream_source.hpp
-- Up-to-date: /usr/local/include/caf/thread_hook.hpp
-- Up-to-date: /usr/local/include/caf/ip_address.hpp
-- Up-to-date: /usr/local/include/caf/sum_type_token.hpp
-- Up-to-date: /usr/local/include/caf/group.hpp
-- Up-to-date: /usr/local/include/caf/attach_continuous_stream_stage.hpp
-- Up-to-date: /usr/local/include/caf/stream_source_driver.hpp
-- Up-to-date: /usr/local/include/caf/no_stages.hpp
-- Up-to-date: /usr/local/include/caf/ipv4_subnet.hpp
-- Up-to-date: /usr/local/include/caf/behavior_policy.hpp
-- Up-to-date: /usr/local/include/caf/json_reader.hpp
-- Up-to-date: /usr/local/include/caf/config_value.hpp
-- Up-to-date: /usr/local/include/caf/buffered_downstream_manager.hpp
-- Up-to-date: /usr/local/include/caf/stream.hpp
-- Up-to-date: /usr/local/include/caf/stream_source.hpp
-- Up-to-date: /usr/local/include/caf/credit_controller.hpp
-- Up-to-date: /usr/local/include/caf/make_source_result.hpp
-- Up-to-date: /usr/local/include/caf/byte.hpp
-- Up-to-date: /usr/local/include/caf/replies_to.hpp
-- Up-to-date: /usr/local/include/caf/none.hpp
-- Up-to-date: /usr/local/include/caf/stream_source_trait.hpp
-- Up-to-date: /usr/local/include/caf/forwarding_actor_proxy.hpp
-- Up-to-date: /usr/local/include/caf/typed_actor_pointer.hpp
-- Up-to-date: /usr/local/include/caf/illegal_message_element.hpp
-- Up-to-date: /usr/local/include/caf/type_erased_value.hpp
-- Up-to-date: /usr/local/include/caf/byte_buffer.hpp
-- Up-to-date: /usr/local/include/caf/attach_stream_sink.hpp
-- Up-to-date: /usr/local/include/caf/actor_proxy.hpp
-- Up-to-date: /usr/local/include/caf/timeout_definition.hpp
-- Up-to-date: /usr/local/include/caf/string_algorithms.hpp
-- Up-to-date: /usr/local/include/caf/ipv6_address.hpp
-- Up-to-date: /usr/local/include/caf/actor_addr.hpp
-- Up-to-date: /usr/local/include/caf/logger.hpp
-- Up-to-date: /usr/local/include/caf/behavior.hpp
-- Up-to-date: /usr/local/include/caf/delegated.hpp
-- Up-to-date: /usr/local/include/caf/policy
-- Up-to-date: /usr/local/include/caf/policy/upstream_messages.hpp
-- Up-to-date: /usr/local/include/caf/policy/select_all.hpp
-- Up-to-date: /usr/local/include/caf/policy/single_response.hpp
-- Up-to-date: /usr/local/include/caf/policy/work_stealing.hpp
-- Up-to-date: /usr/local/include/caf/policy/urgent_messages.hpp
-- Up-to-date: /usr/local/include/caf/policy/select_any.hpp
-- Up-to-date: /usr/local/include/caf/policy/arg.hpp
-- Up-to-date: /usr/local/include/caf/policy/downstream_messages.hpp
-- Up-to-date: /usr/local/include/caf/policy/normal_messages.hpp
-- Up-to-date: /usr/local/include/caf/policy/scheduler_policy.hpp
-- Up-to-date: /usr/local/include/caf/policy/categorized.hpp
-- Up-to-date: /usr/local/include/caf/policy/unprofiled.hpp
-- Up-to-date: /usr/local/include/caf/policy/work_sharing.hpp
-- Up-to-date: /usr/local/include/caf/tracing_data.hpp
-- Up-to-date: /usr/local/include/caf/actor_clock.hpp
-- Up-to-date: /usr/local/include/caf/typed_message_view.hpp
-- Up-to-date: /usr/local/include/caf/actor_registry.hpp
-- Up-to-date: /usr/local/include/caf/invoke_message_result.hpp
-- Up-to-date: /usr/local/include/caf/tracing_data_factory.hpp
-- Up-to-date: /usr/local/include/caf/event_based_actor.hpp
-- Up-to-date: /usr/local/include/caf/uri_builder.hpp
-- Up-to-date: /usr/local/include/caf/pec.hpp
-- Up-to-date: /usr/local/include/caf/decorator
-- Up-to-date: /usr/local/include/caf/decorator/splitter.hpp
-- Up-to-date: /usr/local/include/caf/decorator/sequencer.hpp
-- Up-to-date: /usr/local/include/caf/raise_error.hpp
-- Up-to-date: /usr/local/include/caf/default_attachable.hpp
-- Up-to-date: /usr/local/include/caf/type_id_list.hpp
-- Up-to-date: /usr/local/include/caf/binary_serializer.hpp
-- Up-to-date: /usr/local/include/caf/typed_response_promise.hpp
-- Up-to-date: /usr/local/include/caf/monitorable_actor.hpp
-- Up-to-date: /usr/local/include/caf/system_messages.hpp
-- Up-to-date: /usr/local/include/caf/make_config_option.hpp
-- Up-to-date: /usr/local/include/caf/scheduler
-- Up-to-date: /usr/local/include/caf/scheduler/coordinator.hpp
-- Up-to-date: /usr/local/include/caf/scheduler/test_coordinator.hpp
-- Up-to-date: /usr/local/include/caf/scheduler/worker.hpp
-- Up-to-date: /usr/local/include/caf/scheduler/abstract_coordinator.hpp
-- Up-to-date: /usr/local/include/caf/scheduler/profiled_coordinator.hpp
-- Up-to-date: /usr/local/include/caf/is_typed_actor.hpp
-- Up-to-date: /usr/local/include/caf/group_manager.hpp
-- Up-to-date: /usr/local/include/caf/atom.hpp
-- Up-to-date: /usr/local/include/caf/invalid_stream.hpp
-- Up-to-date: /usr/local/include/caf/byte_address.hpp
-- Up-to-date: /usr/local/include/caf/default_sum_type_access.hpp
-- Up-to-date: /usr/local/include/caf/is_error_code_enum.hpp
-- Up-to-date: /usr/local/include/caf/fwd.hpp
-- Up-to-date: /usr/local/include/caf/typed_actor_view_base.hpp
-- Up-to-date: /usr/local/include/caf/downstream_manager.hpp
-- Up-to-date: /usr/local/include/caf/inspector_access_type.hpp
-- Up-to-date: /usr/local/include/caf/stream_stage_trait.hpp
-- Up-to-date: /usr/local/include/caf/memory_managed.hpp
-- Up-to-date: /usr/local/include/caf/ipv4_endpoint.hpp
-- Up-to-date: /usr/local/include/caf/config_option_set.hpp
-- Up-to-date: /usr/local/include/caf/expected.hpp
-- Up-to-date: /usr/local/include/caf/composed_type.hpp
-- Up-to-date: /usr/local/include/caf/uuid.hpp
-- Up-to-date: /usr/local/include/caf/function_view.hpp
-- Up-to-date: /usr/local/include/caf/timestamp.hpp
-- Up-to-date: /usr/local/include/caf/const_typed_message_view.hpp
-- Up-to-date: /usr/local/include/caf/upstream_msg.hpp
-- Up-to-date: /usr/local/include/caf/serializer.hpp
-- Up-to-date: /usr/local/include/caf/fused_downstream_manager.hpp
-- Up-to-date: /usr/local/include/caf/message.hpp
-- Up-to-date: /usr/local/include/caf/actor_companion.hpp
-- Up-to-date: /usr/local/include/caf/actor_traits.hpp
-- Up-to-date: /usr/local/include/caf/weak_intrusive_ptr.hpp
-- Up-to-date: /usr/local/include/caf/result.hpp
-- Up-to-date: /usr/local/include/caf/actor.hpp
-- Up-to-date: /usr/local/include/caf/make_copy_on_write.hpp
-- Up-to-date: /usr/local/include/caf/type_id.hpp
-- Up-to-date: /usr/local/include/caf/typed_event_based_actor.hpp
-- Up-to-date: /usr/local/include/caf/typed_actor_view.hpp
-- Up-to-date: /usr/local/include/caf/exec_main.hpp
-- Up-to-date: /usr/local/include/caf/stream_priority.hpp
-- Up-to-date: /usr/local/include/caf/ipv6_subnet.hpp
-- Up-to-date: /usr/local/include/caf/static_visitor.hpp
-- Up-to-date: /usr/local/include/caf/resumable.hpp
-- Up-to-date: /usr/local/include/caf/uri.hpp
-- Up-to-date: /usr/local/include/caf/execution_unit.hpp
-- Up-to-date: /usr/local/include/caf/inspector_access_base.hpp
-- Up-to-date: /usr/local/include/caf/prohibit_top_level_spawn_marker.hpp
-- Up-to-date: /usr/local/include/caf/error.hpp
-- Up-to-date: /usr/local/include/caf/skip.hpp
-- Up-to-date: /usr/local/include/caf/attachable.hpp
-- Up-to-date: /usr/local/include/caf/deduce_mpi.hpp
-- Up-to-date: /usr/local/include/caf/stream_sink.hpp
-- Up-to-date: /usr/local/include/caf/actor_pool.hpp
-- Up-to-date: /usr/local/include/caf/unsafe_behavior_init.hpp
-- Up-to-date: /usr/local/include/caf/abstract_group.hpp
-- Up-to-date: /usr/local/include/caf/save_inspector_base.hpp
-- Up-to-date: /usr/local/include/caf/stateful_actor.hpp
-- Up-to-date: /usr/local/include/caf/allowed_unsafe_message_type.hpp
-- Up-to-date: /usr/local/include/caf/settings.hpp
-- Up-to-date: /usr/local/include/caf/actor_factory.hpp
-- Up-to-date: /usr/local/include/caf/config_option_adder.hpp
-- Up-to-date: /usr/local/include/caf/typed_behavior.hpp
-- Up-to-date: /usr/local/include/caf/sum_type_access.hpp
-- Up-to-date: /usr/local/include/caf/may_have_timeout.hpp
-- Up-to-date: /usr/local/include/caf/save_inspector.hpp
-- Up-to-date: /usr/local/include/caf/proxy_registry.hpp
-- Up-to-date: /usr/local/include/caf/caf_main.hpp
-- Up-to-date: /usr/local/include/caf/extend.hpp
-- Up-to-date: /usr/local/include/caf/stream_aborter.hpp
-- Up-to-date: /usr/local/include/caf/config.hpp
-- Up-to-date: /usr/local/include/caf/check_typed_input.hpp
-- Up-to-date: /usr/local/include/caf/exit_reason.hpp
-- Up-to-date: /usr/local/include/caf/infer_handle.hpp
-- Up-to-date: /usr/local/include/caf/hash
-- Up-to-date: /usr/local/include/caf/hash/sha1.hpp
-- Up-to-date: /usr/local/include/caf/hash/fnv.hpp
-- Up-to-date: /usr/local/include/caf/deep_to_string.hpp
-- Up-to-date: /usr/local/include/caf/init_global_meta_objects.hpp
-- Up-to-date: /usr/local/include/caf/scoped_execution_unit.hpp
-- Up-to-date: /usr/local/include/caf/make_stage_result.hpp
-- Up-to-date: /usr/local/include/caf/config_option.hpp
-- Up-to-date: /usr/local/include/caf/stream_stage.hpp
-- Up-to-date: /usr/local/include/caf/all.hpp
-- Up-to-date: /usr/local/include/caf/intrusive
-- Up-to-date: /usr/local/include/caf/intrusive/inbox_result.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/task_result.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/new_round_result.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/drr_queue.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/wdrr_fixed_multiplexed_queue.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/task_queue.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/lifo_inbox.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/drr_cached_queue.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/wdrr_dynamic_multiplexed_queue.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/forward_iterator.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/singly_linked.hpp
-- Up-to-date: /usr/local/include/caf/intrusive/fifo_inbox.hpp
-- Up-to-date: /usr/local/include/caf/downstream_manager_base.hpp
-- Up-to-date: /usr/local/include/caf/scoped_actor.hpp
-- Up-to-date: /usr/local/include/caf/make_actor.hpp
-- Up-to-date: /usr/local/include/caf/input_range.hpp
-- Up-to-date: /usr/local/include/caf/send.hpp
-- Up-to-date: /usr/local/include/caf/actor_profiler.hpp
-- Up-to-date: /usr/local/include/caf/actor_config.hpp
-- Up-to-date: /usr/local/include/caf/ref_counted.hpp
-- Up-to-date: /usr/local/include/caf/mtl.hpp
-- Up-to-date: /usr/local/include/caf/optional.hpp
-- Up-to-date: /usr/local/include/caf/group_module.hpp
-- Up-to-date: /usr/local/include/caf/locks.hpp
-- Up-to-date: /usr/local/include/caf/unit.hpp
-- Up-to-date: /usr/local/include/caf/is_message_sink.hpp
-- Up-to-date: /usr/local/include/caf/message_priority.hpp
-- Up-to-date: /usr/local/include/caf/mixin
-- Up-to-date: /usr/local/include/caf/mixin/actor_widget.hpp
-- Up-to-date: /usr/local/include/caf/mixin/requester.hpp
-- Up-to-date: /usr/local/include/caf/mixin/subscriber.hpp
-- Up-to-date: /usr/local/include/caf/mixin/sender.hpp
-- Up-to-date: /usr/local/include/caf/mixin/behavior_changer.hpp
-- Up-to-date: /usr/local/include/caf/sum_type.hpp
-- Up-to-date: /usr/local/include/caf/ipv4_address.hpp
-- Up-to-date: /usr/local/include/caf/defaults.hpp
-- Up-to-date: /usr/local/include/caf/downstream.hpp
-- Up-to-date: /usr/local/include/caf/error_code.hpp
-- Up-to-date: /usr/local/include/caf/actor_ostream.hpp
-- Up-to-date: /usr/local/include/caf/response_type.hpp
-- Up-to-date: /usr/local/include/caf/is_actor_handle.hpp
-- Up-to-date: /usr/local/include/caf/timespan.hpp
-- Up-to-date: /usr/local/include/caf/mailbox_element.hpp
-- Up-to-date: /usr/local/include/caf/stream_sink_driver.hpp
-- Up-to-date: /usr/local/include/caf/ipv6_endpoint.hpp
-- Up-to-date: /usr/local/include/caf/interface_mismatch.hpp
-- Up-to-date: /usr/local/include/caf/load_inspector_base.hpp
-- Up-to-date: /usr/local/include/caf/message_handler.hpp
-- Up-to-date: /usr/local/include/caf/typed_actor.hpp
-- Up-to-date: /usr/local/include/caf/response_handle.hpp
-- Up-to-date: /usr/local/include/caf/string_view.hpp
-- Up-to-date: /usr/local/include/caf/actor_cast.hpp
-- Up-to-date: /usr/local/include/caf/stream_slot.hpp
-- Up-to-date: /usr/local/include/caf/make_counted.hpp
-- Up-to-date: /usr/local/include/caf/message_id.hpp
-- Up-to-date: /usr/local/include/caf/make_sink_result.hpp
-- Up-to-date: /usr/local/include/caf/ip_endpoint.hpp
-- Up-to-date: /usr/local/include/caf/telemetry
-- Up-to-date: /usr/local/include/caf/telemetry/metric.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/metric_type.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/label_view.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/metric_family.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/metric_registry.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/int_gauge.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/counter.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/timer.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/dbl_gauge.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/collector
-- Up-to-date: /usr/local/include/caf/telemetry/collector/prometheus.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/metric_impl.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/histogram.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/gauge.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/metric_family_impl.hpp
-- Up-to-date: /usr/local/include/caf/telemetry/label.hpp
-- Up-to-date: /usr/local/include/caf/meta
-- Up-to-date: /usr/local/include/caf/meta/annotation.hpp
-- Up-to-date: /usr/local/include/caf/meta/type_name.hpp
-- Up-to-date: /usr/local/include/caf/meta/hex_formatted.hpp
-- Up-to-date: /usr/local/include/caf/meta/omittable_if_none.hpp
-- Up-to-date: /usr/local/include/caf/meta/load_callback.hpp
-- Up-to-date: /usr/local/include/caf/meta/omittable_if_empty.hpp
-- Up-to-date: /usr/local/include/caf/meta/save_callback.hpp
-- Up-to-date: /usr/local/include/caf/meta/omittable.hpp
-- Installing: /usr/local/include/caf/detail/core_export.hpp
-- Installing: /usr/local/lib/libcaf_io.so.0.18.2
-- Up-to-date: /usr/local/lib/libcaf_io.so
-- Set runtime path of "/usr/local/lib/libcaf_io.so.0.18.2" to ""
-- Up-to-date: /usr/local/include/caf
-- Up-to-date: /usr/local/include/caf/detail
-- Up-to-date: /usr/local/include/caf/detail/call_cfun.hpp
-- Up-to-date: /usr/local/include/caf/detail/socket_guard.hpp
-- Up-to-date: /usr/local/include/caf/detail/remote_group_module.hpp
-- Up-to-date: /usr/local/include/caf/detail/prometheus_broker.hpp
-- Up-to-date: /usr/local/include/caf/policy
-- Up-to-date: /usr/local/include/caf/policy/udp.hpp
-- Up-to-date: /usr/local/include/caf/policy/tcp.hpp
-- Up-to-date: /usr/local/include/caf/io
-- Up-to-date: /usr/local/include/caf/io/basp
-- Up-to-date: /usr/local/include/caf/io/basp/version.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/connection_state.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/message_queue.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/message_type.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/worker.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/fwd.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/instance.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/remote_message_handler.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/header.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/all.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/endpoint_context.hpp
-- Up-to-date: /usr/local/include/caf/io/basp/routing_table.hpp
-- Up-to-date: /usr/local/include/caf/io/publish_local_groups.hpp
-- Up-to-date: /usr/local/include/caf/io/middleman.hpp
-- Up-to-date: /usr/local/include/caf/io/network
-- Up-to-date: /usr/local/include/caf/io/network/acceptor_manager.hpp
-- Up-to-date: /usr/local/include/caf/io/network/multiplexer.hpp
-- Up-to-date: /usr/local/include/caf/io/network/doorman_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/network/stream_manager.hpp
-- Up-to-date: /usr/local/include/caf/io/network/rw_state.hpp
-- Up-to-date: /usr/local/include/caf/io/network/default_multiplexer.hpp
-- Up-to-date: /usr/local/include/caf/io/network/stream_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/network/pipe_reader.hpp
-- Up-to-date: /usr/local/include/caf/io/network/stream.hpp
-- Up-to-date: /usr/local/include/caf/io/network/acceptor_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/network/operation.hpp
-- Up-to-date: /usr/local/include/caf/io/network/manager.hpp
-- Up-to-date: /usr/local/include/caf/io/network/receive_buffer.hpp
-- Up-to-date: /usr/local/include/caf/io/network/datagram_handler_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/network/event_handler.hpp
-- Up-to-date: /usr/local/include/caf/io/network/interfaces.hpp
-- Up-to-date: /usr/local/include/caf/io/network/test_multiplexer.hpp
-- Up-to-date: /usr/local/include/caf/io/network/native_socket.hpp
-- Up-to-date: /usr/local/include/caf/io/network/acceptor.hpp
-- Up-to-date: /usr/local/include/caf/io/network/datagram_manager.hpp
-- Up-to-date: /usr/local/include/caf/io/network/protocol.hpp
-- Up-to-date: /usr/local/include/caf/io/network/datagram_handler.hpp
-- Up-to-date: /usr/local/include/caf/io/network/scribe_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/network/ip_endpoint.hpp
-- Up-to-date: /usr/local/include/caf/io/network/datagram_servant_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/doorman.hpp
-- Up-to-date: /usr/local/include/caf/io/open.hpp
-- Up-to-date: /usr/local/include/caf/io/basp_broker.hpp
-- Up-to-date: /usr/local/include/caf/io/connect.hpp
-- Up-to-date: /usr/local/include/caf/io/accept_handle.hpp
-- Up-to-date: /usr/local/include/caf/io/remote_group.hpp
-- Up-to-date: /usr/local/include/caf/io/system_messages.hpp
-- Up-to-date: /usr/local/include/caf/io/datagram_servant.hpp
-- Up-to-date: /usr/local/include/caf/io/receive_policy.hpp
-- Up-to-date: /usr/local/include/caf/io/fwd.hpp
-- Up-to-date: /usr/local/include/caf/io/broker.hpp
-- Up-to-date: /usr/local/include/caf/io/middleman_actor_impl.hpp
-- Up-to-date: /usr/local/include/caf/io/publish.hpp
-- Up-to-date: /usr/local/include/caf/io/connection_helper.hpp
-- Up-to-date: /usr/local/include/caf/io/handle.hpp
-- Up-to-date: /usr/local/include/caf/io/scribe.hpp
-- Up-to-date: /usr/local/include/caf/io/connection_handle.hpp
-- Up-to-date: /usr/local/include/caf/io/close.hpp
-- Up-to-date: /usr/local/include/caf/io/typed_broker.hpp
-- Up-to-date: /usr/local/include/caf/io/middleman_actor.hpp
-- Up-to-date: /usr/local/include/caf/io/all.hpp
-- Up-to-date: /usr/local/include/caf/io/broker_servant.hpp
-- Up-to-date: /usr/local/include/caf/io/datagram_handle.hpp
-- Up-to-date: /usr/local/include/caf/io/remote_actor.hpp
-- Up-to-date: /usr/local/include/caf/io/abstract_broker.hpp
-- Up-to-date: /usr/local/include/caf/io/unpublish.hpp
-- Installing: /usr/local/include/caf/detail/io_export.hpp
-- Installing: /usr/local/lib/libcaf_openssl.so.0.18.2
-- Up-to-date: /usr/local/lib/libcaf_openssl.so
-- Set runtime path of "/usr/local/lib/libcaf_openssl.so.0.18.2" to ""
-- Up-to-date: /usr/local/include/caf
-- Up-to-date: /usr/local/include/caf/openssl
-- Up-to-date: /usr/local/include/caf/openssl/manager.hpp
-- Up-to-date: /usr/local/include/caf/openssl/session.hpp
-- Up-to-date: /usr/local/include/caf/openssl/publish.hpp
-- Up-to-date: /usr/local/include/caf/openssl/middleman_actor.hpp
-- Up-to-date: /usr/local/include/caf/openssl/all.hpp
-- Up-to-date: /usr/local/include/caf/openssl/remote_actor.hpp
-- Up-to-date: /usr/local/include/caf/openssl/unpublish.hpp
-- Installing: /usr/local/include/caf/detail/openssl_export.hpp
-- Up-to-date: /usr/local/share/caf/examples/./aout.cpp
-- Up-to-date: /usr/local/share/caf/examples/./hello_world.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/calculator.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/cell.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/dancing_kirby.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/delegating.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/divider.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/fan_out_request.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/fixed_stack.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/promises.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/request.cpp
-- Up-to-date: /usr/local/share/caf/examples/message_passing/typed_calculator.cpp
-- Up-to-date: /usr/local/share/caf/examples/streaming/integer_stream.cpp
-- Up-to-date: /usr/local/share/caf/examples/dynamic_behavior/skip_messages.cpp
-- Up-to-date: /usr/local/share/caf/examples/dynamic_behavior/dining_philosophers.cpp
-- Up-to-date: /usr/local/share/caf/examples/custom_type/custom_types_1.cpp
-- Up-to-date: /usr/local/share/caf/examples/custom_type/custom_types_2.cpp
-- Up-to-date: /usr/local/share/caf/examples/custom_type/custom_types_3.cpp
-- Up-to-date: /usr/local/share/caf/examples/custom_type/custom_types_4.cpp
-- Up-to-date: /usr/local/share/caf/examples/testing/ping_pong.cpp
-- Up-to-date: /usr/local/share/caf/examples/remoting/group_chat.cpp
-- Up-to-date: /usr/local/share/caf/examples/remoting/group_server.cpp
-- Up-to-date: /usr/local/share/caf/examples/remoting/remote_spawn.cpp
-- Up-to-date: /usr/local/share/caf/examples/remoting/distributed_calculator.cpp
-- Up-to-date: /usr/local/share/caf/examples/broker/simple_broker.cpp
-- Up-to-date: /usr/local/share/caf/examples/broker/simple_http_broker.cpp
-- Up-to-date: /usr/local/share/caf/tools/caf-vec.cpp
-- Up-to-date: /usr/local/share/caf/tools/caf-run.cpp
make: Leaving directory '/home/sergio/actor-framework/build'

```
