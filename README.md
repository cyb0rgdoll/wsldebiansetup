# wsldebiansetup

Setting up WSL Debian - setup and GUI etc

To use WSL2, Windows 10 has to be updated to version 2004 (Build 10941) or higher.

    Run Windows update

    Check your Windows version by opening the “Run” dialog (Windows key + R) and entering winver.
    
Activate optional features

WSL1 and WSL2 use some features that aren’t activated by default, enabling those is necessary.

This is possible through a GUI, by going to “turn Windows features on or off”

##  DEBIAN ON WSL

Download and install the Debian app via Microsoft Store, this will open up the Debian command line and prompt me to enter a username and password.

I’ll then need to change this from WSL to WSL2 via a PowerShell admin panel (Windows Key+x, then a) as it’s fresh from the store it’ll be on version 1.

To list out my WSL instances I’ll use the _wsl -l -v_ command:

## GUI 

sudo apt-get purge xrdp

then
sudo apt-get install xrdp
sudo apt-get install xfce4
sudo apt-get install xfce4-goodies

configure :
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession

sudo nano /etc/xrdp/startwm.sh
comment these lines to:
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession

add these lines:
# xfce
startxfce4

sudo /etc/init.d/xrdp start

Now in Windows, use Remote Desktop Connection
localhost:3390
then login with Xorg, fill in your username and password.
