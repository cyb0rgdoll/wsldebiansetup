# wsldebiansetup

Setting up WSL Debian - setup and GUI etc

To use WSL2, Windows 10 has to be updated to version 2004 (Build 10941) or higher.

        Run Windows update

        Check your Windows version by opening the “Run” dialog (Windows key + R) and entering winver.
    
## Activate optional features

WSL1 and WSL2 use some features that aren’t activated by default, enabling those is necessary. This is possible through a GUI, by going to “turn Windows features on or off”
- To use WSL, enable the aptly named “Windows Subsystem for Linux” feature.
- Powershell snippit if you preferred
  
      Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
      Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform


##  DEBIAN ON WSL

Download and install the Debian app via Microsoft Store, this will open up the Debian command line and prompt me to enter a username and password.

I’ll then need to change this from WSL to WSL2 via a PowerShell admin panel (Windows Key+x, then a) as it’s fresh from the store it’ll be on version 1.

To list out my WSL instances I’ll use the _wsl -l -v_ command:

    # the l is for list
    # v is for verbose 🤷‍♀️
    # this is the long version => wsl --list --verbose
    PS C:Windowssystem32> wsl -l -v
      NAME            STATE           VERSION
    * Ubuntu          Running         2
      Debian          Running         1
      Ubuntu-20.04    Running         2
      

    wsl --set-version <distro-name> 2
    # in my case
    wsl --set-version Ubuntu 2

## VSCode and git for Windows

Remember to set the autocrlf setting to input for git. VSCode handles it well. Open a terminal in Windows!

    git config --global core.autocrlf input

Before beginning to install Linux tools, we’ll update our already installed packages.

From this point on, the action happens in your Linux terminal!

    sudo apt update
    sudo apt upgrade

More preparation, installing build tools for node-gyp

    sudo apt install build-essential

## Git

    This one should also be installed on the Linux side.

    sudo apt install git

After installing git, remember to configure it.
Especially setting the autocrlf setting to input is important here.

    git config --global core.autocrlf input

## Node & NPM

You can install it as a standalone package. Now we can harness all those Linux-y tools, I’ll use nvm instead to make using different versions easier.

The nvm repo has excellent installation instructions.

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

Restart your terminal after the install.
To confirm the installation was successfull:

    command -v nvm

It should return: nvm
That’s all, no version number, just that string.

To install the latest the latest stable version of node:

    nvm install node # "node" is an alias for the latest version

When node releases a new version, you can run that same command again.

You’ll need to tell nvm which version of node you want to use.
So next time you boot your Linux distro, you’ll have to use.

    nvm use node

When the project you are working on requires a different version of node, specify that one.

    nvm use v<version number>
    # or if the project has a valid .nvmrc file
    nvm use

Having to do that manually seems annoying right? Many solutions to this annoyance exist. Later in this post that annoyance will be dealt with.

## Docker

 Kick things off by updating the packages index and installing dependencies.

    sudo apt update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common

    Add Dockers’s official GPG-key.

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

 Verify this by running:

    sudo apt-key fingerprint 0EBFCD88

    You should see the full key in the output.

    9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88

Add the Docker repository to your list of repositories.

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

 Update the list of repositories again and install Docker CE.

    sudo apt update
    sudo apt install docker-ce

Normally, the docker engine starts automatically after the install.
For me that was not the case so I started it manually.

    sudo service docker start

 Verify Docker CE was installed correcly by booting up their hello-world container.

    sudo docker run hello-world

## Docker Compose

As a handy tool for managing docker containers, docker-compose is frequently installed alongside docker-ce.

 Download the current stable release and place it in the /usr/local/bin folder.

    sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

Make the file executable.

    sudo chmod +x /usr/local/bin/docker-compose

 Verify the installation.

    docker-compose --version
    # output: docker-compose version 1.24.0, build 0aa59064

## Docker Desktop

A short trip back to the Windows side!

The previous way to do Docker-y things all happened through the Linux terminal. While this is fine, the Docker Desktop for Windows application integrates with WSL2 quite well and provides a GUI.


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
