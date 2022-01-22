---
title:  "How to develop Duckiebot for UNH robotics teams"
last_modified_at: 2021-04-18
categories: 
  - Teaching
tags:
  - Duckie
toc: true
toc_label: "Duckie"
comments: true
---

The Duckiebot hardware is a Nvidia Jetson nano and the operating system is also built based on its genetic [Jetpack system](https://developer.nvidia.com/jetson-nano-2gb-sd-card-image) (Ubuntu 18.04). Ubuntu 18.04 is associated to [ROS melodic](http://wiki.ros.org/melodic) which only supports Python 2* versions. It is doable to develop in that way but a lot of Python 3+ features will be lost. However, Ubuntu 20 is not officially supported in Nvidia Jetson nano. Costum images are available, e.g., [Xubuntu](https://forums.developer.nvidia.com/t/xubuntu-20-04-focal-fossa-l4t-r32-3-1-custom-image-for-the-jetson-nano/121768). Note that this image is not offically tested, use at your own risk. So, this is why Docker kicks in. In your host machine, you need the `dts` tool. And it only runs in Linux. So it is highly recommended to operate everything in Ubuntu 20. 

# Development in Windows
- Check the robot connection
  - Push win+R in keyboard 
  - Type in `cmd`, click `OK`. Then you will be prompt to the command line in Windows system.
  -  Ping the robot.
  ```
  ping [robot_name].local 
  ```
  - If shows the following prompt, your robot is not connected. Stop and go back to connect your robot to WIFI. 
  >Ping request could not find ...

  - If shows the following prompt, you're good to continue.
  >Reply from ...

- Install the SSH software `Putty`. [Download here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
- Input the robot name into "Host Name" and click "Open"
  ![putty1](/assets/images/putty1.png)
- The robot will be connected remotely through SSH tunnel. Input the user name and password. Default user name is `duckie`. Default password is `quackquack`. Please don't change this credential. Now you're in the robot.

- Pull the OS Docker
``` 
docker pull superkkniu/unh-robotics:basic
```

- Run the Docker
```
docker run \
  --name unh_duckie \
  -v /data:/data \
  --privileged \
  --network=host \
  -dit \
  -e ROBOT_TYPE=duckiebot \
  superkkniu/unh-robotics:basic 
```
At this point, the tool chain has been built in the `unh_duckie` Docker. You can easily go into the Docker to create a work space. Next, we're going to introduce how to use VS code as a ROS IDE.

