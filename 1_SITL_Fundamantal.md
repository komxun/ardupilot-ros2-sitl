---
layout: default
title: SITL Fundamental Components
description: [Ardupilot | MAVProxy | Q-GroundControl | Gazebo]
permalink: /1_SITL_Fundamantal/
---

[Back](README.md) 

---

**Contents:**

- [ArduPilot and MAVProxy Installation](https://github.com/komxun/ardupilot-ros2-sitl/blob/main/1_SITL_Fundamantal.md#ardupilot--mavproxy-installation)
- [Q-GroundControl Installation](https://github.com/komxun/ardupilot-ros2-sitl/blob/main/1_SITL_Fundamantal.md#q-groundcontrol-installation)
- [Gazebo Installation](https://github.com/komxun/ardupilot-ros2-sitl/blob/main/1_SITL_Fundamantal.md#gazebo-installation)

> **⚠️ CAUTION**: It is strongly recommended to follow instructions from the **official page** for the most up-to-date information!

---

# ArduPilot + MAVProxy Installation

> **Official page**: [Setting up the Build Environment (Linux/Ubuntu) — Dev documentation](https://ardupilot.org/dev/docs/building-setup-linux.html)

Tutorial for guidance (**⚠️ BUT already outdated**):
- [02 Installing ArduCopter for Development (SITL) - YouTube](https://www.youtube.com/watch?v=wlkoq65mM2A&list=PLy9nLDKxDN683GqAiJ4IVLquYBod_2oA6&index=2)
- [Intelligent-Quads GitHub-Installing_Ardupilot_20_04.md](https://github.com/Intelligent-Quads/iq_tutorials/blob/master/docs/Installing_Ardupilot_20_04.md)
## Step1: Install `git`
```shell
sudo apt-get update
sudo apt-get install git
```

## Step 2: Clone ArduPilot repository
In this case, we will clone it to the root directory
```shell
cd ~
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
```

Install dependencies
```shell
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y
```

Reload profile
```shell
. ~/.profile
```

## Step 3: Swap to the latest `Copter` branch version
As of 7 Mar 2025, the latest ArduCopter version is [Copter-4.5](https://github.com/ArduPilot/ardupilot/tree/Copter-4.5)
```shell
git checkout Copter-4.5
git submodule update --init --recursive
```

## Step 4: Run SITL once to set parameters
```shell
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```
> This should come with `MAVProxy` installed

![Pasted image 20250307091809](https://github.com/user-attachments/assets/43ee2f69-a2c2-4846-89a9-bf4b22aeda8e)


- **Left**: `MAVProxy` command prompt / **Right**: `ArduCopter` instance
- With **MAVProxy GCS command prompt**
	- To open the map: `module load map`
	- To open the console: `module load console`
---

# Q-GroundControl Installation

> **Official page**: https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html#ubuntu

Before installing _QGroundControl_ for the first time:
``` shell
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
sudo apt install libfuse2 -y
sudo apt install libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor-dev -y
```

Then, **Logout and login again** to enable the change to user permissions.
To install _QGroundControl_:
1. Download [QGroundControl.AppImage](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage).
2. Install (and run) using the terminal commands:
``` shell
chmod +x ./QGroundControl.AppImage
./QGroundControl.AppImage  
```
(or double click)

---

# Gazebo Installation

## Why Gazebo Fortress?
- For Ubuntu 22.04, **Gazebo Harmonic** is recommended. However, **Gazebo Fortress** is recommended for the use with **ROS2 Humble**.

![Pasted image 20250304090430](https://github.com/user-attachments/assets/213beb87-2a91-4b24-b6c8-552cd43edd7d)

(Ref: [Getting Started with Gazebo? — Gazebo ionic documentation](https://gazebosim.org/docs/latest/getstarted/))

![Pasted image 20250303155456](https://github.com/user-attachments/assets/bc543158-70a4-43b9-9195-3e19dfd52373)

(Ref: [Installing Gazebo with ROS — Gazebo ionic documentation](https://gazebosim.org/docs/latest/ros_installation/))

---

## Gazebo Fortress Installation

> **Official page**: [Binary Installation on Ubuntu — Gazebo fortress documentation](https://gazebosim.org/docs/fortress/install_ubuntu/)

First install some necessary tools:
```shell
sudo apt-get update
sudo apt-get install lsb-release gnupg
sudo apt install curl
```

Then install Ignition Fortress:
```shell
sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install ignition-fortress
```

All libraries should be ready to use and the `ign gazebo` app ready to be executed.

## Verify if Gazebo is running properly

```shell
ign gazebo sim shapes.sdf
```
![Pasted image 20250307094628](https://github.com/user-attachments/assets/40ecc4ce-3269-415b-9769-d6b81712cff7)

[Getting Started with Gazebo? — Gazebo fortress documentation](https://gazebosim.org/docs/fortress/getstarted/)

---

[Back](README.md) 

