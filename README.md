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
  cd ~/ur5_ws/
  source devel/setup.bash
  roslaunch ur_bringup ur5_bringup.launch robot_ip:=172.16.0.1 [reverse_port:=REVERSE_PORT]
  ```
(2) Open another terminal window and enter the following commands:
```
  cd ~/ur5_ws/
  source devel/setup.bash
  rosrun ur_driver test_move.py
```
(3) The UR5 should start making a test movement.

## MoveIt! with real UR5
If the previous steps have succeeded, it's time to use MoveIt! with the real hardware. We need to open 3 seperate terminal windows and enter a few commands.

(1) In terminal window 1 enter the following commands:
```
  cd ~/ur5_ws/
  source devel/setup.bash
  roslaunch ur_bringup ur5_bringup.launch limited:=true robot_ip:=172.16.0.1 [reverse_port:=REVERSE_PORT]
```
(2) In terminal window 2 enter the following commands:
```  
  cd ~/ur5_ws/
  source devel/setup.bash
  roslaunch ur5_moveit_config ur5_moveit_planning_execution.launch limited:=true robot_ip:=172.16.0.1
```
(3) In terminal window 3 enter the following commands:
```
  cd ~/ur5_ws/
  source devel/setup.bash
  roslaunch ur5_moveit_config moveit_rviz.launch config:=true robot_ip:=172.16.0.1
```

After these steps, Rviz should start and can be used to control the UR5. If it started correctly, Rviz would already display the current state of the robot. If it didn’t start correctly, the model display in Rviz will display the arm as being horizontally extended. With the plan-tab in the Rviz UI you can now plan and execute movements of the Rviz.

## MoveIt! with Gazebo
It's also possible to use the UR5 model within Gazebo environment following a few extra steps:

(1) Download and install gazebo: http://gazebosim.org/tutorials?tut=ros_installing.

(2) I used the following command from the previous tutorial:
```
  sudo apt-get install ros-indigo-gazebo-ros-pkgs ros-indigo-gazebo-ros-control
```
(3) After this, sometimes when running gazebbo I got an error "couldn't load joint_state controller", this can be fixed by entering the following commands in a terminal window:
```
  sudo apt-get update
  sudo apt-get install ros-indigo-ros-control ros-indigo-ros-controllers
```
(4) If the previous steps have succeeded, it's time to use Gazebo. We need to open 3 seperate terminal windows and enter a few commands.

(5) In terminal window 1 enter the following commands:
```
  cd ~/ur5_ws  
  source devel/setup.bash
  roslaunch ur_gazebo ur5.launch limited:=true

```
(6) In terminal window 2 enter the following commands:
```  
  cd ~/ur5_ws
  source devel/setup.bash
  roslaunch ur5_moveit_config ur5_moveit_planning_execution.launch sim:=true limited:=true

```
(7) In terminal window 3 enter the following commands:
```
  cd ~/ur5_ws
  source devel/setup.bash
  roslaunch ur5_moveit_config moveit_rviz.launch config:=true
```
(8) Once this is done, it's possible to move the simulation with Rviz and the planning tab.
