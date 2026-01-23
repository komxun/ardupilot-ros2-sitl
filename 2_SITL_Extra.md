---
layout: default
title: SITL Extra Components Installation
description: [Ardupilot Gazebo Plugin | ROS 2 Humble | MAVROS and MAVLINK]
permalink: /2_SITL_Extra/
---

[Back to Home](index.md)

---

**Contents:**
- Ardupilot Gazebo Plugin Installation
- ROS 2 Humble Installation
- MAVROS and MAVLINK Installation


> **⚠️ CAUTION**: It is strongly recommended to follow instructions from the **official page** for the most up-to-date information!

---

# ArduPilot Gazebo Harmonic Plugin (🟢Updated 2026)
> **Official page**: [ArduPilot Gazebo Plugin (main branch)](https://github.com/ArduPilot/ardupilot_gazebo)

## Prerequisites
Gazebo Garden or Harmonic is supported on Ubuntu 22.04 (Jammy). Harmonic is recommended. If you are running Ubuntu as a virtual machine you will need at least Ubuntu 20.04 in order to have the OpenGL support required for the ogre2 render engine. Gazebo and ArduPilot SITL will also run on macOS (Big Sur, Monterey and Venturua; Intel and M1 devices).

Follow the instructions for a binary install of Gazebo Garden or Gazebo Harmonic or Gazebo Ionic and verify that Gazebo is running correctly.

Set up an ArduPilot development environment. In the following it is assumed that you are able to run ArduPilot SITL using the MAVProxy GCS.

## Installation
STEP 1: Install Gazebo Harmonic development libs and rapidjson:
```shell
sudo apt update
sudo apt install libgz-sim8-dev rapidjson-dev
sudo apt install libopencv-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl
```
STEP 2: Clone the repo (at root) and build with:
```shell
cd ~
git clone https://github.com/ArduPilot/ardupilot_gazebo
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4
```

STEP 3: Set the Gazebo environment variables in your `.bashrc` (Window) or `.zshrc` (macOS) or in the terminal used to run Gazebo. 
```shell
export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:$GZ_SIM_SYSTEM_PLUGIN_PATH
export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:$GZ_SIM_RESOURCE_PATH
```

Edit the `.bashrc` so you don't have to run the `export` commands every time 
(this is down through the following command lines, you can modify it later with `nano ~/.bashrc`)
```shell
echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
```
Reload your terminal
```shell
source ~/.bashrc
```

STEP 4: Run Gazebo
```shell
gz sim -v4 -r iris_runway.sdf
```
The `-v 4` parameter is not mandatory, it shows the debug information.

---

## Run ArduPilot SITL
To run an ArduPilot simulation with Gazebo, **the frame should have** `gazebo-` in it and **have JSON as model**. Other command line parameters are the same as usual on SITL.
```shell
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
```
> **CAUTION**: `--model JSON` to refers to the model run on Gazebo, thus it is very important to **run Gazebo first** before starting the SITL

![Pasted image 20250307102426](https://github.com/user-attachments/assets/f32a926e-acc2-4593-b20c-e97a05e05f33)


---

# ROS2 Humble

> **Official page**: [Ubuntu (source) — ROS 2 Documentation: Humble documentation](https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html)

> Tutorial: [Jeremy Pedersen - Installing ROS2](https://jeremypedersen.com/posts/2024-07-17-gazebo-ros-install/#installing-ros2)

## Installation
First, we need to fiddle with Ubuntu’s locale settings a little bit. ROS 2 prefers a UTF-8 locale.

```shell
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```
> **Note:** You may have to run them one-by-one.

Install necessary prerequisites:
```shell
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Add the ROS2 GPG key:
```shell
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Add the repo to sources.list:
```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

Pull the updated package lists, make sure everything is up to date::
```shell
sudo apt update
sudo apt upgrade
```

Next, let’s install **ROS Desktop**, which includes packages like `RViz`, which we will use to visualize the outputs from sensors like cameras and LIDAR:

```shell
sudo apt install ros-humble-desktop
```

We should also fetch tools for building ROS packages, just in case (although hopefully we won’t need these):
```shell
sudo apt install ros-dev-tools
```
## Set up the environment
Whenever we want to use ROS, we will need to ensure our environment is set up properly, by running the following command:
```shell
source /opt/ros/humble/setup.bash
```

---

# MAVROS and MAVLINK

> Official page: [mavros/mavros/README.md at ros2 · mavlink/mavros](https://github.com/mavlink/mavros/blob/ros2/mavros/README.md)

> **⚠️ CAUTION**: It is recommended to install `mavros` with [source installation](https://github.com/mavlink/mavros/blob/ros2/mavros/README.md#source-installation)

## Source installation

**Step 0**: Install `vcs` utility for retrieving sources and `colcon` tool for build.
```shell
sudo apt install -y python3-vcstool python3-rosinstall-generator python3-osrf-pycommon
```

**Step 1**: Create the workspace: (*unneeded if you already has workspace*)
```shell
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```

**Step 2**: Install **MAVLink**
```shell
rosinstall_generator --format repos mavlink | tee /tmp/mavlink.repos
```

**Step 3**: Install MAVROS: get source (upstream - released)
```shell
rosinstall_generator --format repos --upstream mavros | tee -a /tmp/mavros.repos
```

**Step 4**: Create workspace & deps
```shell
vcs import src < /tmp/mavlink.repos
vcs import src < /tmp/mavros.repos
rosdep install --from-paths src --ignore-src -y
```

**Step 5**: Install GeographicLib datasets:
```shell
./src/mavros/mavros/scripts/install_geographiclib_datasets.sh
```

<font color="#00b050">Note: You may be asked for root permission to run this. Alternatively, you can run with</font>
```shell
wget https://raw.githubusercontent.com/mavlink/mavros/ros2/mavros/scripts/install_geographiclib_datasets.sh
./install_geographiclib_datasets.sh
```

**Step 6**: Build source
```shell
cd ~/ros2_ws
colcon build
```

**Step 7**: Source the underlay + overlay
```shell
source install/setup.bash
```

## Using MAVROS (basic)
**Step1**: Terminal 1: Run ArduPilot SITL
```shell
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```

**Step 2**: Terminal 2: launch `mavros`
(Don't forget to source the overlay!)
```shell
cd ~/ros2_ws
source install/setup.bash
```
Launch `mavros`
```shell
ros2 launch mavros apm.launch fcu_url:=udp://localhost:14550@
```
(Note: `apm` is for Ardupilot)

**Step 3**: Terminal 3: Check `ros2 topic`
```shell
ros2 topic list
```
Now the `ros2 topic list` can see the `mavlink` messages published from the sitl vehicle
- Additionally, you can `echo` to see the data streaming from the topic
```shell
ros2 topic echo /mavros/battery
```
![image](https://github.com/user-attachments/assets/14257bc9-7bc8-417f-b35c-0b9fa69b1f68)

- Terminal 0 (black) : ArduCopter (running)
- Terminal 1: MAVProxy GCS command prompt (running)
- Terminal 2: MAVROS (running)
- Terminal 3: List of `mavros`topics (=`mavlink` messages) available on `ros 2` network
- Terminal 4: Data streaming of `/mavros/battery` topic from the SITL vehicle

---

- [Back to Home](index.md)
- [Next: Running SITL](3_RunningSITL.md)

---

**Contact**: [Komsun Tamanakijprasart](https://www.linkedin.com/in/komsun-tamanakijprasart-5a82709b/)

k.tamanakijprasart.263@cranfield.ac.uk

[>> Buy me a coffee 🤗☕ << ](https://monzo.me/komsuntamanakijprasart?h=BU-3i8) 
