# ioBroker-Standby

## Description

This is not an adapter!

This linux shell files provides the possibility for a ioBroker "redundancy". 
The script has to be installed on a second linux system, which will work as hot standby. In case of a total breakdown of the first system (it is not longer reachable per network ping), the second system will get active and overtake the full 
(full??? hardware/io limitations not clear) functionality.


## Solution

My (master and only instance) of ioBroker is running on a pretty buggy Windows installation. This Windows installation runs on an
old Barebone PC. Due to Windows or hardware problems, the system sporadic breaks down. Because of my limited time, and no 
other ioBroker sollution, I created this workaround.


## Technical details

In general, there is only one script to check the network connection to master system and enable/disable the ioBroker instance on the Standby. 


## Prerequirements

### Master system:

* A common running Master ioBroker instance (in my script productive IP 192.168.2.251). No changes are needed  

* The Master ioBroker has to be backed up (iobroker backup)

* The Master System needs a second IP address (best: in an other IP range, in my script 192.168.3.251)


### Standby system:

* I'm using a Raspberry Pi 3 (image from ioBroker samples) on a 16GB SD Card (IP in my script 192.168.2.108). 

* Restore the latest Master System backup to Standby System

* Remove IoBroker autostart ("update-rc.d -f iobroker.sh remove") from Standby System

*  For Standby System 3 IP addresses are needed

** eth0 for ssh etc. in your network (script: 192.168.2.108). 

** eth0:1 in the subnet of the second master IP (scropt: 192.168.3.107). It is used for  connection check. It is configured/set by the scripts

** eth0:2 it is the same IP as the productive master system (script: 192.168.2.251). It is configured/set by the scripts (to avoid ip conflicts, normally this interface ist blocked by iptables)



## Installation

* Copy script to /opt/ioBrokerStandby

* Set execution attribute to script (chmod 0777 checkMaster)

* add cronjob for execution (or run it manually)

** "crontab -e"

** add 

"#check every minute the connection to iobroker"

"* * * * * /opt/ioBrokerStandby/checkMaster"  


* Configuration (just adjust the variables at the top of the script


* Manually Test


** dmesg will show information of the ioBroker Standby script

** Unplug your master ioBroker from network, or shut the system down


## Limitations and Known Issues


* No back synchronisation of archives (or data, which was engineered on standby system)

* Archive data ist not includes in backup
