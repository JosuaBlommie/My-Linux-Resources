# My-Linux-Resources
This is where I store all my Linux knowledge, tips and tricks accrued over the years

**Find RPi IP address for lan and wifi:**

	sudo apt install net-tools
	ifconfig eth0 | grep inet | awk '{ print $2 }'
	ifconfig wlan0 | grep inet | awk '{ print $2 }'

**Stop asking for sudo Password**
	sudo visudo
 then append "NOPASSWD:ALL" to the end of each line where "ALL" is present

# Python Version Control  

**List all versions of python:**

	ls /usr/bin/python*

**Install a specific version of python:**
	
	sudo apt install software-properties-common
	sudo apt-add-repository ppa:deadsnakes/ppa
	sudo apt-get update
	sudo apt-get install python#.#
	
Set the default version of python to a specific version when "python" is used (this may break apt-get for system updates so switch back to main python version after installing specific package)
	
	sudo update-alternatives --list python
	sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
	sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 2
	sudo update-alternatives --config python
	sudo update-alternatives --remove python /usr/bin/python2.7

**Remove python versions:**
	
	sudo apt-get remove --purge python#.#

# Installing of various packages

**For Ubuntu:**
	
	sudo apt install openssh-server
	sudo dpkg-reconfigure openssh-server

**Check Ubuntu version:**

	lsb_release -a

**Install pip3 specifically for Python3:**

	sudo apt-get install python3-pip

**To install pip 3 for a different version of Python3, download the get-pip.py file**

	https://bootstrap.pypa.io/get-pip.py
	python3.7 get-pip.py
	sudo python3.7 -m pip install <package_name> #specific package into that version of python

**Install matplotlib for default version of system python**

	pip install matplotlib


**Install GPIO packages on ubuntu:**
https://sourceforge.net/p/raspberry-gpio-python/wiki/Examples/

	sudo python3.7 -m pip install RPi.GPIO	

To install for specific version of python3.#, ensure update-alternative version is selected

**Virtual Environments:**
Awesome guide here: https://realpython.com/python-virtual-environments-a-primer/

	sudo pip3 install virtualenv
	virtualenv -p /usr/bin/python#.# venv (to create virtual environment in that version of python)
	source /venv/bin/activate

**Install Xubuntu Desktop on Pi**

	sudo apt-get install subuntu-desktop

# File Management in Terminal

**Remove files:**

	rm -r /home/swapnil/Documents/cio/stories
	rm -r /home/swapnil/Documents/cio/stories/*
	sudo rm <filename> OR sudo rm *.jpg (all files with that extension)

**Make a new folder:**

	mkdir -p /home/swapnil/Downloads/cio/stories 
	
('p' creates parent and child directories, otherwise just mkdir)

**Copy folder:**

	cp -r /path_of_current_folder /path_of_destination
	cp -r /home/swapnil/Documents/cio/stories /media/file/
	cp -r /home/swapnil/Documents/cio/stories/* /media/file/stories/ (the * copies the contents of the folder, not the whole folder)

**Move folder:**

	mv -r /home/swapnil/Documents/cio/stories /media/file/
	mv -r /home/swapnil/Documents/cio/stories/* /media/file/stories/

# Storage and Harddrives

**Mount and access flash drive:**
Check specs of drive connected, UASP - Driver = uas

	lsusb -t					
see all drives, USB usually /dev/sda1
	
	sudo fdisk -l 					
create mount point with name USB

	sudo mkdir /media/USB 				
to mount device, can now be acess via /media/

	sudo mount /dev/sda1 /media/USB		
	
	sudo umount /dev/sda1
	sudo umount /media/USB

**Benchmark storage write/read speed:**

	curl https://raw.githubusercontent.com/geerlingguy/raspberry-pi-dramble/master/setup/benchmarks/microsd-benchmarks.sh | sudo bash

**add a line to your /boot/config.txt to stop pollong for SD card:**

	dtoverlay=sdtweak,poll_once


# Setting up remote access on Buster 64 bit

Follow this guide: https://github.com/raspberrypi/Raspberry-Pi-OS-64bit/issues/3#issuecomment-636544190

	sudo apt-get install x11vnc
	sudo x11vnc -storepasswd yourVNCpasswordHERE /etc/x11vnc.pass
	sudo nano /etc/systemd/system/x11vnc.service (edit according to link above)

	sudo systemctl daemon-reload
	sudo systemctl start x11vnc

	sudo systemctl enable x11vnc

Set resolution in raspi-config

check status of server:

	sudo systemctl status x11vnc

# Setting up crontab start up shell script

Following this tutorial: https://www.dexterindustries.com/howto/auto-run-python-programs-on-the-raspberry-pi/

Open crontab (don't use sudo on raspberry pi)

	crontab -e

If above is not editable, change default edit:

	export EDITOR=nano/gedit

Add the python or shell script you want to run at reboot:

	@reboot sh /home/pi/startup.sh &

Create a shell script at the location above with the same name
The first line of the shell script should be 

	#!/bin/bash

followed by whatever else you would want to type into the terminal

Can start a terminal and execture a command

	lxterminal -e sudo python3 HW_Trigger.py

Make the shell script executable by opening terminal and running:

	sudo chmod +x /home/pi/startup.sh

To kill python script running in backgroun:

	pgrep -af python 
finds all running python scrips and lists them
	
	sudo pkill -f HW_Trigger.py
	
	
# Setting up a GUI-program to run at start up	

Note: Using crontab works really well to run background scripts at start, but it does not run program or script that requires a GUI to open. To do that, follow this guide: https://www.tomshardware.com/how-to/run-script-at-boot-raspberry-pi

Create a file called myapp.desktop

	sudo nano /etc/xdg/autostart/myapp.desktop

Enter the following in the file:

	[Desktop Entry]
	Exec=/path/to/shellscript.sh

# Setting up Jetson or Rpi? to sync with custom NTP server
Do NOT install NTP, in fact, purge NTP if already installed

	sudo apt purge ntp
Check that timesyncd service is active:

	timedatectl status
 	timedatectl set-ntp on

Configure timesyncd.conf file, add IP address of NTP server in the file (the RUT router's IP in this case)
	
	sudo gedit /etc/systemd/timesyncd.conf
	NTP=192.168.1.227
Restart the service

	systemctl restart systemd-timesyncd.service
	enter password
Now, on fresh boot with ethernet cable connected, the Jetson/EPC should sync with the RUT router


# Settig up SSH keys to transfer files	

Create an SSH key on the RPi which other Jetsons can use to transfer files: https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md

Check for previous SSH keys on the Raspberry Pi:

	ls ~/.ssh
Generate new keys if necessary

	ssh-keygen (press enter until finished)
Now, on the Jetson, from which you want to connect (or send files) to the Pi

	ssh-copy-id pi@192.168.1.225
On the pi, to which you want to connect

	ssh-copy-id blackfly@192.168.1.22#

Now set up SCP to send files from Jetson to SSDs on RPi: https://www.raspberrypi.org/documentation/remote-access/ssh/scp.md


# Settig IPTables to allow port through firewall

Update IP table with new rule

	sudo iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 5900  -j ACCEPT
Install package iptables-persistent (will prompt to save current rules)

	sudo apt-get install iptables-persistent	
Save new rules (if not saved during install)

	sudo netfilter-persistent save
To check current rules:

	sudo iptables -S
	
	
