# Marinecoin-Raspberry_Pi
Marinecore Installation Guide for Raspberry_Pi IoT Platform

The distribution version we need to flash on the SD card is the lastest Stable Raspbian Image from: https://www.raspberrypi.org/downloads/raspbian/
Setup Raspbian in the Pi
Insert the flashed SD with Raspbian in the Raspberry Pi 2/3.
Plug in the USB mouse, the USB keyboard, the HDMI screen, the network cable, and the power cable.
The Raspberry Pi will boot for the first time and you will be presented with the Raspberry Pi Software Configuration Tool (raspi-config). To navigate in this tool, the useful keys are: The up/down arrow, the Enter key, and the Tab key whenever the up/down arrow keys don’t do the job. Here, we will do next things:
Expand the Filesystem by choosing option 1 and Reboot. You will get a message Root partition has been resized.
Select your Proper Time Zone and Change the User Password by choosing option 2. Enter your new password twice. When entering the password, the characters won’t be displayed as a security feature. You will get a message Password changed successfully.
We are done with the raspi-config tool. Select Finish and reboot the Pi.
Note: If you ever need to open the raspi-config tool again, simply type sudo raspi-config at the Terminal.
Update the Raspberry Pi device.
Run the following commands:
sudo apt-get update
Setting up the Raspberry Pi for compiling Marinecore.
Change Swap Size.
Use the following command to change the default swap size:
sudo nano /etc/dphys-swapfile
Make sure it reads CONF_SWAPSIZE=1024 Use the left/right arrow keys to navigate the file. After change is done, press Ctrl+O followed by the Enter key to save the file. Then, press Ctrl+X to exit the editor.
Use the following commands to enable the swap file with its new size:
sudo dphys-swapfile setup
sudo dphys-swapfile swapon
You can check the new active swap size with next command:
free -m
Install Required Dependencies with next commands:
wget http://www.openssl.org/source/openssl-1.0.2g.tar.gz
tar -xvzf openssl-1.0.2g.tar.gz
cd openssl-1.0.2g
./config --prefix=/usr/
make
sudo make install
sudo apt-get install autoconf libevent-dev libtool libboost-all-dev libdb-dev libdb4.8++ libdb5.3++-dev git -y
Compile and Install BerkeleyDB 4.8.30 by running the following commands:
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
sudo tar -xzvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
sudo ../dist/configure --enable-cxx
sudo make
sudo make install
export CPATH="/usr/local/BerkeleyDB.4.8/include"
export LIBRARY_PATH="/usr/local/BerkeleyDB.4.8/lib"
Note: the export commands are only valid in the current Terminal session. To avoid errors, don't close the Terminal until you fully completed with Marinecoin compilation.
Clone the marinecoin Github, compile and install the client / node with following commands
git clone https://github.com/marinecoin/marinecore.git
cd db-4.8.30.NC/build_unix
sudo ../dist/configure --prefix=/usr/local --enable-cxx
cd
cd marinecore/src
"we have to make a small edit in the net.cpp file for raspberry Pi to succesfully compile Marinecore"
sudo nano net.cpp
go to line 46 array<int, THREAD_MAX> vnThreadsRunning; change it like below and save
boost::array<int, THREAD_MAX> vnThreadsRunning;
sudo make -f makefile.unix USE_UPNP=-
Start the daemon with:
./marinecoind &
you may need to create a marinecoin.conf file
cd
cd .marinecoin
sudo nano marinecoin.conf
Enter the following
rpcuser=marinecoinrpc
rpcpassword= *enter a secure password
listen=1 *if you want a fullnode
cd marinecore/src
./marinecoind &
To check if marinecoin is running
./marinecoind getinfo
You should see info on the runnig node :)


