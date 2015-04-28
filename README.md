# UR5
This document is intented to have a quick and easy setup for using the UR5 robot in ROS (with Ubuntu 14.04).

This readme assumes you have ros-indigo-desktop-full pre-installed on your laptop/PC.


## UR5 configuration
(1) Start with building a catkin workspace with a name you prefer, in this case I use "ur5_ws".

(2) Clone the UR5 repository into your workspace src folder using the following commands:
```
  cd ~/ur5_ws/src/
  git clone https://github.com/ros-industrial/universal_robot.git
```

(3) To use moveIt I needed to install a few extra packages, using the following command:
```
  sudo apt-get update
  sudo apt-get install ros-indigo-moveit-commander ros-indigo-moveit-planners ros-indigo-moveit-plugins ros-indigo-moveit-ros ros-indigo-moveit-setup-assistant
``` 

(4) Start a new terminal and build your workspace using the following commands:
```
  cd ~/ur5_ws/
  source devel/setup.bash
  catkin_make
```

(5) It will start to build the workspace. Now it's time to plug-in your ethernet cable going to the UR5 controller.

(6) You need to manually configure your wired internet connection using the following steps:
- In the top right of your UI, click the internet symbol. Press edit connections at the bottom
- This should open the "Network Connections" window. Click the Ethernet Connection and click edit
- This opens a windows called "Editing *name of connection*", go to the tab IPv4 Settings, set method to manual and add the following information:
  * Address: 172.16.0.42 (new computer IP-address)
  * Netmask: 255.255.255.0
  * Gateway: 172.16.0.1 (might be different, but this is the UR5 IP-address)

(7) If the previous steps are done correctly, at this point you should be able to ping the UR5 from your PC. You can do so by opening a terminal and enter the following command:
```
  ping 172.16.0.1
```

## Test program
To check if everything is working correctly, you can run a testprogram and send it to the UR5. Make sure you have cleared yhe workspace around your UR5 before you run a program. You can do this through a few steps:

(1) Open a terminal window and enter the following commands:
```
  cd ~/catkin_ws/
  source devel/setup.bash
  roslaunch ur_bringup ur5_bringup.launch robot_ip:=172.16.0.1 [reverse_port:=REVERSE_PORT]
  ```
(2) Open another terminal window and enter the following commands:
```
  cd ~/catkin_ws/
  source devel/setup.bash
  rosrun ur_driver test_move.py
```
(3) The UR5 should start making a test movement.






