---
layout: default
title: Swarm SITL
description: [Ardupilot | MAVProxy | Gazebo | ROS2]
permalink: /4_SwarmSITL/
---

[Back](README.md) 

---

**Content**
- Basic Swarm SITL

---

# Basic Swarm ArduPilot SITL
> **Official page**: https://ardupilot.org/dev/docs/using-sitl-for-ardupilot-testing.html#swarming-with-sitl

## About basic Swarm SITL with ArduPilot and MAVProxy
- SITL is restricted to launching vehicles of the **same type**
- When launching multiple vehicles, each one will need a unique `SysId` parameter: `SYSID_THISMAV`. 
	- The easiest method to avoid `SysId` conflicts is with the `--auto-sysid` option.
- The number of vehicles is set with `--count` option.
- To send all **mavlink data** between the vehicles add `--mcast`

To avoid vehicles spawning in the same default location, they must be spawned in unique locations. There are two primary approaches, using either 
- **Option1: a swarm offset line**
	- When using a swarm offset line, the option `--auto-offset-line 90,10` will space the vehicles out at a line with heading of 90 degrees orientation. The vehicles will be spaced 10 meters apart. Thus, they will be spread out east-west.
- **Option 2: a swarm configuration file**. 
(Both require a location to be set with `--location`)

### **Example 1**: Five Copter vehicles on an auto-offset-line at CMAC:

```shell
sim_vehicle.py -v Copter --map --console --count 5 --auto-sysid --location CMAC --auto-offset-line 90,10 --mcast
```

### **Example 2**: Five Copter vehicles at Cranfield University:

```shell
sim_vehicle.py -v Copter --map --console --count 5 --auto-sysid --custom-location=52.06758628,-0.62690711,108.5,0 --auto-offset-line 90,10 --mcast
```
> Note: `--custom-location=<lat>,<lon>,<alt>,<heading>`

![image](https://github.com/user-attachments/assets/4c1f8028-a296-4440-a7fc-5b8f97c2f593)


## Load swarm module for swarm help

On MAVProxy Prompt
```shell
module load swarm
```

![image](https://github.com/user-attachments/assets/5621234d-e91f-423e-b2d6-dfbdb3f72347)


# Other useful resources
- [Swarming experiments for the common man](https://discuss.ardupilot.org/t/swarming-experiments-for-the-common-man-part-1-introduction-and-simulation/62774)

---

[Back](README.md) 

---

**Contact**: [Komsun Tamanakijprasart](https://www.linkedin.com/in/komsun-tamanakijprasart-5a82709b/)

k.tamanakijprasart.263@cranfield.ac.uk

[>> Buy me a coffee 🤗☕ << ](https://monzo.me/komsuntamanakijprasart?h=BU-3i8) 










