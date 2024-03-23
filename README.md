
# Solved-a-problem
Resolved an issue with opening 3D applications in a remote Ubuntu server environment using VNC, by utilizing Turbo VNC which supports 3D acceleration.

## Background
I used a MacBook to proceed below.
Encountered an Open 3D error when using SLAM architecture remotely via VNC with a laboratory server. Previously, the server was accessed through `tightvncserver` and interacted with via `tiger VNC` or `Real VNC` for GUI operations. This setup functioned well until the following error occurred, indicating a problem with 3D acceleration in the VNC environment:

```
[Open3D WARNING] GLFW Error: GLX: GLX extension not found
[OpenD WARNING] Failed to create window
```

This post aims to help those encountering the same issue and struggling to find a solution, as I have spent considerable time with few resolutions and no clear written workarounds. The following solution may not be perfect and might include unintentional steps, but it serves as a good workaround.

## How to Solve the Problem

### Step 0: Verify that there are no issues when entering the following command on the server Ubuntu. Access VNC and type the following
```bash
$ glxinfo
$ vglrun
```
If you get an error when entering a command, do the following
It may not work because there is no screen when running with Ssh. 
If it doesn't work when running with vnc, there is a problem. 


```bash
$ wget https://github.com/VirtualGL/virtualgl/releases/download/3.1.1/virtualgl_3.1.1_amd64.deb
$ sudo dpkg -i virtualgl_3.1.1_amd64.deb
$ export DISPLAY=:1
$ export VGL_DISPLAY=:1
$ echo $DISPLAY
$ echo $VGL_DISPLAY
```
If that doesn't work, try the entire process from step 1, then try step 0 again in a VNC environment.

### Step 1: Access the Server and Install VNC

Connect to the server using SSH through Visual Code and execute:

```bash
$ sudo apt update
$ sudo apt install tightvncserver
```

### Step 2: Set Up the VNC Server

Execute the following command to setup the VNC server:

```bash
$ tightvncserver
```

During the first execution, set a password for the VNC server. A new VNC display will be created for each client connection. The default port is 5900. Choose 'n' when prompted to set a view-only password.

Next, configure the VNC session:

```bash
$ vi ~/.vnc/xstartup
```

Insert the following script:

```bash
#!/bin/sh
export XKL_XMODMAP_DISABLE=1
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
autocutsel -fork &
```

### Step 3: Install Turbo VNC

Download and install Turbo VNC on the Ubuntu server:

```bash
$ wget https://github.com/TurboVNC/turbovnc/releases/download/3.1.1/turbovnc_3.1.1_amd64.deb
$ sudo dpkg -i turbovnc_3.1.1_amd64.deb
```

Install Turbo VNC on your local machine as well. For macOS, use Homebrew:

[Homebrew TurboVNC Viewer](https://formulae.brew.sh/cask/turbovnc-viewer#default)

To start the Turbo VNC server with specific geometry and port, use:

```bash
$ /opt/TurboVNC/bin/vncserver :1 -geometry 1920x1080 -rfbport 5901
```

Adjust the `-geometry` and `-rfbport` options as needed.

### Step 4: Configure the Firewall

Allow traffic through the chosen port:

```bash
$ sudo ufw allow 5901/tcp
```

### Step 5: Connect Using Turbo VNC

Launch Turbo VNC on your computer and connect using your server's IP address and port.


### Step 6: Open a terminal via Turbo VNC

```bash
$ export DISPLAY=:1
$ echo $DISPLAY
$ export VGL_DISPLAY=:1
$ echo $VGL_DISPLAY
```

Example of successful connection:

![image](https://github.com/paulsung97/Solved-a-problem./assets/63456050/b6a05003-2bc6-48a2-983e-76690b861145)
```
