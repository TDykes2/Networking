# Cisco Router Setup and Deployment  

##### This tutorial will be completed on a Windows 10 Machine, connected to a Cisco Router via a Cisco Console Cable

### Based upon Avery's Networking's video on YouTube.
##### https://youtu.be/721SvUGi36U?si=VnUJWLz06hrlt_P0

#### Other Works Cited:
#### - David Bombal on YouTube, Connecting to a Cisco Router Through PuTTy. (What, I'm New to this)
##### + https://youtu.be/HKCYs-UKU4Y?si=ocp5Srk7AY7_lKXw

#
## Connecting through PuTTy: Putty.org
##### 
#### 1. Find COM port that router is on.
##### Unplug Router > Device Manager > Open Ports > Plug in Router > Note which COM port is new, this is your cisco router. 
#### 2. Connect Via PuTTy
##### PuTTy > Select "Serial" under "Connection Type" (Fig. 1) > Input COM Port from step 1 in "Serial Line" > Select "Open"
![image](https://github.com/TDykes2/Networking/assets/105371918/8c6152db-5622-4d82-ac17-d65353f979e6)
###### Figure 1   

### Would you like to enter the initial configuration dialog?
##### > No  

## Configuring Gigbabit 0/0 and 0/1 ports
#### 1. > enable
#### 2. > configure terminal
#### 3. > int g0/0 (Starting work into g0/0 "input" port)
  ##### This is the port that will be accepting your incoming signal from your modem
#### 4. > ip address dhcp
#### 5. > ip nat outside
#### 6. > no shut (leaving g0/0 workspace)
#### 7. > int g0/1 (Starting work into g0/1 "output" port) 
  ##### This is the port that will be sending your signal to devices downstream: Switch, Server, PC, etc
#### 8. > ip nat inside
#### 9. > exit
#### Ports 0/0 and 0/1 setup complete  

### Setting up packet sending to outside network. This is the code that will point towards your Service Providers Router/Modem (ISP Router). 
#### 1. > ip route 0.0.0.0 0.0.0.0 xxx.xxx.xxx.xxx
##### - Replace X's with your ISP Router's Gateway (inside ip address)
#### 2. > ip dhcp pool home

### Setup Network Connection
#### 1. > network 192.158.125.0 255.255.255.0

### Setup Default Router
#### 1. > default-router 192.168.125.1
##### Makes the cisco router the default

### DNS Server Selection
#### 1. > dns-server 8.8.8.8

#### > exit

## Setup fixed ip addresses
#### 1. > ip dhcp excluded-address XXX.XXX.XXX.XXX
##### Replace X's with ip address you have or plan to have devices be fixed on.
##### Also recommended to exclude the first 10 ip adresses so DHCP will not mess with these. > ip dchp xxx.xxx.xxx.xxx xxx.xxx.xxx.xxx
##### Replace x's with your first ip adress in the sequence and the last, of the list you want to exclude.


### Setup Access List
#### 1. > ip access-list standard 1
#### 2. > permit 192.168.125.0 0.0.0.255
#### 3. < exit

#### ip nat inside source list 1 int g0/0 overload
# Configuration now completed. Try connecting devices
