# Installing [raspberian onto sd card step by step

1. Download [raspbian image](https://www.raspberrypi.org/downloads/raspbian/)
2. Find sd card ```$ diskutil list```
3. Unmount ```diskutil unmountDisk /dev/disk<disk# from diskutil>```
4. Copy iage ```sudo dd bs=1m if=image.img of=/dev/rdisk<disk# from diskutil>```
5. Put sd card into raspberry pi and connect via ethernet
6. ssh into pi with ```ssh pi@raspberrypi.local```
7. setup wifi ```wpa_passphrase "wifi name" "pass" >> /etc/wpa_supplicant/wpa_supplicant.conf```
8. disconnect ethernet cable and restart pi
9. ssh into pi with ```ssh pi@raspberrypi.local``` - it should use wifi now
10. expand system ```sudo raspi-config```
11. passwordless login ```ssh-copy-id pi@raspberrypi.local```
12. disable passwords login ```sudo vi /etc/ssh/sshd_config```

    ```
    PermitRootLogin no
    PasswordAuthentication no
    ```
13. Install watchdog 

    ```
    sudo modprobe bcm2708_wdog
    sudo bash -c 'echo "bcm2708_wdog" >> /etc/modules'
    sudo apt-get install watchdog
    sudo update-rc.d watchdog defaults
    sudo vi /etc/watchdog.conf
    ```
    Uncomment fallowing lines
    
    ```
    max-load-1
    watchdog-device
    ```
    
    Start it ```sudo service watchdog start```
14. Update system ```sudo apt-get update && sudo apt-get -y upgrade```
15. Configure timezone using `sudo raspi-config`
