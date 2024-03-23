# Solved-a-problem.
Solved a problem with open 3D based on vnc in a remote ubuntu server environment.

You'll need to work around that with turbo VNC, which supports 3D acceleration. 


# background
Open 3D error issue when using SLAM architecture remotely via VNC with lab server.
Prior to this error, VNC environments were opening the server via 'tightvncserver' and accessing the server via 'tiger VNC' or 'Real VNC' to use the GUI.
I was doing well with that environment until I ran into the following error.

The error is shown below
[0pen3D WARNING] GLFW Error: GLX: GLX extension not found
[OpenD WARNING] Failed to create window

The error turned out to be an error in 3D acceleration, meaning that my VNC environment was not able to open the 3D window.

I'm posting this because a lot of people are seeing this error and not being able to resolve it.
I've spent days and days trying to figure it out, with very few actual resolutions and no clear written workarounds.
Now let's describe the solution.
This solution isn't perfect. I've tried a lot of things, and there may be things I've installed without realizing it. But I'm sure it's a good workaround. 
I apologize if this is not the best way to explain it.

# How to solve problem?


1) 
Access the server with SSH via visual code and execute

$ sudo apt update 
$ sudo apt install tightvncserver 

and enter the corresponding



2) 
Enter the following to set up the VNC server

$ tightvncserver

On the first run of this command, you will be prompted for a password to configure the VNC server. Enter the desired password and confirm! After that, the VNC server will be up and running. 

A new VNC display will be created for each client connection. 

The default port for the VNC server is 5900. Would you like to enter a view-only password (y/n)? 

This will ask you to select n (if you y it, you will only be able to view and not interact inside)

Next, enter the following

vi ~/.vnc/xstartup

As you type this, you'll see a field to fill out.
Please try to copy and paste the following!

## ======= content ======== ## 
!/bin/sh 
def 
export XKL_XMODMAP_DISABLE=1 
unset SESSION_MANAGER 
unset DBUS_SESSION_BUS_ADDRESS 
gnome-panel & 
gnome-settings-daemon & 
metacity & 
nautilus & 
gnome-terminal & 
autocutsel -fork & 
## ======================= ##

Install the VNC server: If you are installing from https://github.com/TurboVNC/turbovnc/releases 
Proceed with the installation on your Ubuntu server from that site. The version from 2024.3.23 should run fine with the following content

$ wget https://github.com/TurboVNC/turbovnc/releases/download/3.1.1/turbovnc_3.1.1_amd64.deb

$ sudo dpkg -i turbovnc_3.1.1_amd64.deb

Install the corresponding VNC on your server. 



Install Turbo VNC on your own computer as well. 
If it's a MacBook, install via iterm using home brew.

----- homebrew site -----
https://formulae.brew.sh/cask/turbovnc-viewer#default

Access the server via SSH with visual code and type

$ /opt/TurboVNC/bin/vncserver :1 -geometry 1920x1080 -rfbport 5901

and enter 
opt/TurboVNC/bin/vncserver :1 is common, but enter 

-geometry 1920x1080 can be set to whatever size you want, please refer to your computer's resolution!

-rfbport 5901 This is about which port to use! I use port 5901. 

Port 5900 is common, but it can be too vulnerable to security, so I like to use a different port!


The following will unblock that port on your Ubuntu server.

$ sudo ufw allow 5901/tcp


Open TurboVNC on your computer and enter your IP address:port number as follows.

Make sure to open the port and enter

You should see it open up nicely as shown below.

![image](https://github.com/paulsung97/Solved-a-problem./assets/63456050/b6a05003-2bc6-48a2-983e-76690b861145)


