---
layout: default
title: Running SITL
description: [Ardupilot | MAVProxy | Gazebo | ROS2]
permalink: /3_RunningSITL/
---

[Back](index.md)

---

**Content**
- Basic ArduPilot SITL
- ArduPilot SITL with Gazebo
- ArduPilot SITL with Gazebo and ROS2

---

# Basic ArduPilot SITL

![image](https://github.com/user-attachments/assets/5bd17850-e183-42a2-982f-de2596bbfc99)


**Step 1**: Run ArduPilot + MAVProxy SITL

```shell
sim_vehicle.py -v ArduCopter --map --consol
```

> **Step 2** (optional): Run Q-GroundControl

---

# ArduPilot SITL with Gazebo

![image](https://github.com/user-attachments/assets/b87ede34-59fa-4751-8afa-343a7f73f52a)


**Step 1**: Launch Gazebo with Iris drone

```shell
ign gazebo -v 4 -r iris_arducopter_runway.world
```
> Note:
> `-v 4` → Sets the verbosity level to 4 (higher verbosity provides more detailed logging).
> `-r` → Runs the simulation immediately after loading the world file. Without `-r`, the world would load in a paused state.

**Step 2**: Run ArduPilot + MAVProxy SITL with `-f gazebo-iris` and `--model JSON`

```shell
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --consol
```

Check the ArduCopter terminal if the it reads the JSON model succesfully:

![image](https://github.com/user-attachments/assets/cec88c34-d04d-4803-abc6-0aad59ae5bfb)

> It should states that `JSON received:`

> **Step 3** (optional): Run Q-GroundControl

## Bash script to run ArduPilot SITL with Gazebo
To run both commands in separate terminal windows from a single Bash script, you can use gnome-terminal.
First, create the bash script:

```shell
cd ~
nano startsitl_gazebo.sh
```

Inside `startsitl_gazebo.sh`:

```shell
!/bin/bash

# Open first terminal and run Ignition Gazebo
gnome-terminal --title="Gazebo Fortress" -- bash -c "ign gazebo -v 4 -r iris_arducopter_runway.world; exec bash"

# Wait a few seconds to ensure Gazebo is initialized
sleep 2

# Open second terminal and run sim_vehicle.py
gnome-terminal --title="MAVProxy" -- bash -c "sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console; exec bash"
```

Grant the access to execute the script:

```shell
chmod +x startsitl_gazebo.sh
```

Run the bash script:

```shell
./startsitl_gazebo.sh
```

---

# ArduPilot SITL with Gazebo and ROS2

**Step 1**: Run ArduPilot SITL with/without Gazebo

```shell
./startsitl_gazebo.sh
```

**Step 2**: Launch `mavros` with correct ip for the fcu. For example, if sitl and mavros are running on the same machine:

```shell
cd ~/ros2_ws
source install/setup.bash
ros2 launch mavros apm.launch fcu_url:=udp://localhost:14550@
```

> Note: Don't forget to `source` the overlay first.

- Now open a new terminal and check `ros2 topic list`

## Bash script to run ArduPilot SITL with Gazebo and ROS2

Script: `startsitl_gazebo_ros2.sh`

```shell
#!/bin/bash

# Open first terminal for Ignition Gazebo
gnome-terminal --title="Gazebo Fortress" -- bash -c "ign gazebo -v 4 -r iris_arducopter_runway.world; exec bash"

# Wait a few seconds to ensure Gazebo is initialized
sleep 2

# Open second terminal for ArduPilot simulation
gnome-terminal --title="MAVProxy" -- bash -c "sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console; exec bash"

# Wait a few more seconds to ensure ArduPilot is initialized
sleep 2

# Open third terminal for ROS 2 and MAVROS
gnome-terminal --title="mavros" -- bash -c "cd ~/ros2_ws; source install/setup.bash; ros2 launch mavros apm.launch fcu_url:=udp://localhost:14550@; exec bash"

```

![image](https://github.com/user-attachments/assets/a69a66a9-f2b4-41ef-a303-c924b4c27ab1)


---

[Back](index.md)

---

**Contact**: [Komsun Tamanakijprasart](https://www.linkedin.com/in/komsun-tamanakijprasart-5a82709b/)

k.tamanakijprasart.263@cranfield.ac.uk

[>> Buy me a coffee 🤗☕ << ](https://monzo.me/komsuntamanakijprasart?h=BU-3i8) 




